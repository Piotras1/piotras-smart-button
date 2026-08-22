# `custom_states_labels` — zasady dopasowania

`custom_states_labels` pozwala podać własne etykiety tekstowe pokazywane na karcie, zależnie od aktualnego stanu encji. Obsługiwane są trzy typy kluczy:

| Typ klucza | Przykład | Znaczenie |
|---|---|---|
| Dopasowanie dokładne | `"25": OK` | stan równy dokładnie `25` |
| Operator porównania | `">25": Warm` | stan większy/mniejszy od podanej liczby |
| Zakres | `"10-25": Normal` | stan w przedziale domkniętym `[10, 25]` |

Dozwolone operatory: `>`, `<`, `>=`, `<=`.

## ⚠️ NAJWAŻNIEJSZA ZASADA: kolejność wpisów ma znaczenie

Karta sprawdza klucze **w kolejności, w jakiej są zapisane w YAML, od góry do dołu**, i zwraca etykietę dla **pierwszego znalezionego dopasowania**. Nie szuka "najlepszego" ani "najbardziej precyzyjnego" warunku — bierze pierwszy z brzegu, który jest prawdziwy.

Wyjątek: dopasowanie **dokładne** (np. `"25"`) jest sprawdzane jako pierwsze, niezależnie od jego pozycji w pliku — dopiero gdy nie znajdzie się dopasowanie dokładne, karta przechodzi do sprawdzania operatorów/zakresów po kolei.

ℹ️ Dopasowanie dokładne (tekstowe) jest **niewrażliwe na wielkość liter** — klucz `open` dopasuje jednakowo stany `open`, `Open` i `OPEN`. Dotyczy to wszędzie tam, gdzie karta wykonuje dopasowanie tekstowe: stan encji, `hvac_action`, `preset_mode` itd.

### Operatory `>` i `>=` → zapisuj progi w kolejności malejącej (od najwyższego do najniższego)

```yaml
custom_states_labels:
  ">30": Hot       # sprawdzane jako pierwsze
  ">20": Warm      # sprawdzane jako drugie
  ">10": Cool      # sprawdzane jako trzecie
```

❌ Źle (rosnąco) — `>10` złapie wszystko od 10 wzwyż, zanim karta w ogóle dotrze do `>20` czy `>30`:
```yaml
custom_states_labels:
  ">10": Cool      # to łapie WSZYSTKO powyżej 10, w tym 25 i 35!
  ">20": Warm      # nigdy nie sprawdzane dla wartości > 20
  ">30": Hot       # nigdy nie sprawdzane dla wartości > 30
```

### Operatory `<` i `<=` → zapisuj progi w kolejności rosnącej (od najniższego do najwyższego)

```yaml
custom_states_labels:
  "<10": Cold      # sprawdzane jako pierwsze
  "<20": Cool      # sprawdzane jako drugie
  "<30": Warm      # sprawdzane jako trzecie
```

❌ Źle (malejąco) — `<30` złapie wszystko poniżej 30, w tym 5 i 15.

### Zakresy (`min-max`) → unikaj nachodzących na siebie granic

Jeśli dwa zakresy stykają się na tej samej liczbie (np. `17-18` i `18-19`), wartość graniczna (tutaj: `18`) zawsze trafi do **wcześniejszego** zakresu w pliku, bo jest sprawdzany jako pierwszy.

❌ Granice nachodzą na siebie (18 zawsze da "Cold", nie "Cool"):
```yaml
custom_states_labels:
  "17-18": Cold
  "18-19": Cool
```

✅ Granice się nie nakładają — każda wartość trafia tam, gdzie się spodziewasz:
```yaml
custom_states_labels:
  "17-17.9": Cold
  "18-18.9": Cool
```

## Dopasowanie dla dowolnej encji (input_select, sensor tekstowy, lock, switch itd.)

`custom_states_labels` nie jest ograniczone do `light`/`fan`/`cover`/`climate`/`weather`/`on.clock` — to tylko domeny ze **specjalną** obsługą (procenty, druga linia value2, godzina). Dla **wszystkich innych** encji (np. `input_select`, `sensor` ze stanami tekstowymi, `lock`, `switch`, `person`, `media_player`, `vacuum`...) `custom_states_labels` działa jako **główna etykieta** (ta sama, która domyślnie pokazuje `ON`/`OFF`/`PLAY`/`STOP` itd.) — karta sprawdza dopasowanie dokładne do surowego stanu encji **zawsze jako pierwszy krok**, przed jakąkolwiek domyślną logiką danej domeny.

```yaml
type: custom:piotras-smart-button
entity: input_select.garage_door_status
name: Garage
icon: mdi:garage
icon_on: mdi:garage-open
custom_states_on:
  - open
  - opening
custom_states_labels:
  closed: <b>Garage Closed <br>🚪
  open: <b>Garage Open <br>🔓
  opening: <b>Going up! <br>🚀
  closing: <b>Going down! <br>🛑
```

ℹ️ `custom_states_on` pokazane w przykładzie powyżej to osobna opcja (kontroluje ikonę/podświetlenie "aktywne", nie tekst etykiety) — opisana szczegółowo w dokumencie **`custom_states_on and custom_blockade`**.

W tym przykładzie `custom_states_labels` po prostu zamienia tekst etykiety w zależności od dokładnego stanu `input_select` (`closed`/`open`/`opening`/`closing`) — i jak widać, zawartość etykiety może zawierać HTML (`<b>`, `<br>`, emoji), dokładnie tak jak opisano w sekcji "Pełny HTML w etykietach" poniżej.

Ten sam mechanizm działa identycznie dla `alarm_control_panel` — encja ma własne nazwy stanów (`disarmed`, `armed_home`, `arming`, `armed_away`, `armed_night`, `triggered`...), więc możesz po prostu nadać im dowolne, tematyczne etykiety:

```yaml
type: custom:piotras-smart-button
entity: alarm_control_panel.home
name: Alarm
custom_states_labels:
  disarmed: Free to roam! 🔓
  armed_home: Fortress Mode Active 🏰
  arming: Fortress Mode Arming
  armed_away: Empty Castle, Lasers ON! 🚨
  armed_night: Ghost busters enabled 👻
  triggered: INTRUDER! RELEASE THE HOUNDS! 🐕💥
```

"Aktywny" stan wizualny (podświetlenie, `icon_on`) dla alarmu jest ustawiany automatycznie na podstawie stanów `armed_*`/`pending`/`arming`/`triggered` — nie trzeba dodawać `custom_states_on`, choć nadal możesz to nadpisać, jeśli chcesz innej logiki.

## Encje `light` — etykiety zależne od jasności

Stan encji `light` to zawsze tylko `on` albo `off` — żeby rozróżnić poziomy jasności (np. "Dim", "Dark", "Bright", "Max"), karta dodatkowo sprawdza atrybut `brightness` (przeliczony na %), jeśli dopasowanie do samego stanu `on`/`off` nie zwróci etykiety.

```yaml
type: custom:piotras-smart-button
entity: light.lsc_outdoor_neon_strip_3m
name: living room
custom_states_labels:
  off: Off
  "<10": Dark
  "10-40": Dim
  "40-80": Bright
  ">80": Max
```

Kolejność sprawdzania dla `light`:
1. Dopasowanie **dokładne** do stanu (`off`, `on` — jeśli podane jako klucz) — ma absolutny priorytet.
2. Jeśli nie znaleziono i światło jest `on` z atrybutem `brightness`, przelicz na % i dopasuj do zakresów/operatorów w `custom_states_labels` (te same zasady kolejności co powyżej: `>` malejąco, `<` rosnąco, niezachodzące na siebie zakresy).
3. Jeśli nic nie pasuje — domyślne `DIM`/`ON`/`OFF`.

⚠️ Jeśli podasz klucz `on: coś`, **zawsze wygra** on nad dopasowaniem procentowym jasności (dopasowanie dokładne ma najwyższy priorytet) — żeby rozróżnić poziomy jasności, nie dodawaj klucza `on`, tylko liczbowe zakresy/operatory + `off`.

## Domeny oparte na procentach: `light`, `fan`, `cover`

Ten sam mechanizm (dopasowanie do stanu, a jeśli nie znaleziono — do wartości procentowej) działa dla trzech domen:

| Domena | Atrybut źródłowy | Przeliczenie na % |
|---|---|---|
| `light` | `brightness` (0–255) | `brightness / 255 * 100` |
| `fan` | `percentage` (0–100) | brak przeliczenia |
| `cover` | `current_position` (0–100) | brak przeliczenia |

Przykład dla wentylatora:
```yaml
type: custom:piotras-smart-button
entity: fan.living_room_fan
name: Fan
custom_states_labels:
  off: Off
  "<20": Slow
  "20-60": Medium
  ">60": Strong
```

Kolejność sprawdzania jest identyczna jak dla `light` opisanego powyżej (dopasowanie dokładne do stanu ma priorytet, potem wartość procentowa, te same zasady malejąco/rosnąco dla operatorów).

## `climate` — druga linia wartości (value2) z custom_states_labels

Dla encji `climate` główna wartość (`state-badge`) **zawsze pokazuje aktualną temperaturę** (`current_temperature`) — to się nie zmienia. Jeśli jednak podasz `custom_states_labels`, pod temperaturą pojawia się **dodatkowa, mniejsza linia** z dopasowaną etykietą.

```yaml
type: custom:piotras-smart-button
entity: climate.living_room
name: Living Room
custom_states_labels:
  off: Off
  heating: Heating
  idle: Idle
  comfort: Comfort
  eco: Eco
  none: Manual
```

Jak to działa:
- Dopasowanie sprawdza trzy źródła po kolei i zwraca etykietę z **pierwszego, które ma dopasowanie** w `custom_states_labels`:
  1. `hvac_action` — co urządzenie faktycznie teraz robi (`heating`/`cooling`/`idle`/`drying`/`fan`/`off`) — najwyższy priorytet, bo to najdokładniejsza informacja o stanie urządzenia.
  2. `state` — aktualny tryb HVAC, jeden z `hvac_modes` (`heat`/`cool`/`auto`/`off`) — awaryjne rozwiązanie, gdy `hvac_action` jest niedostępny lub nie ma dopasowania.
  3. `preset_mode` — aktualnie wybrany tryb z `preset_modes` (np. `none`/`away`/`comfort`/`eco`/`home`/`sleep`/`activity`) — ostatnia deska ratunku, gdy ani `hvac_action`, ani `state` nie pasują.
- Jeśli żadne z trzech źródeł nie pasuje (np. zdefiniowano tylko `heating`/`cooling`, a urządzenie jest w stanie `drying`) — druga linia po prostu **nie pokazuje się**, a temperatura wyświetla się normalnie.
- Jeśli w ogóle nie podasz `custom_states_labels` — nic się nie zmienia, dostajesz temperaturę jak dotychczas.
- Domyślny rozmiar czcionki drugiej linii to `state_size - 5` (px), z minimum 8px. Możesz go nadpisać opcją `value2_size`:

```yaml
custom_states_labels:
  eco: Eco
value2_size: 10
```

⚠️ **Dla `climate` dopasowanie jest wyłącznie dokładne (tekstowe) — nie liczbowe.** Karta porównuje wartości `hvac_action`/`state`/`preset_mode` jako stringi z kluczami w `custom_states_labels`. Zakresy (`"10-25"`) i operatory (`">25"`, `"<10"`) **nie działają** dla `climate` — co ma sens, bo te trzy źródła to nazwy stanów/trybów (`heating`, `eco`, `comfort`...), nie liczby. Jeśli chcesz reagować na temperaturę, zrób to na encji `sensor` z aktualną temperaturą, a nie na samej encji `climate`.

Kolejny przykład — termostat z trzema warstwami dopasowania i pustą etykietą dla `off` (żeby druga linia po prostu nie pokazywała się, gdy ogrzewanie jest wyłączone):

```yaml
type: custom:piotras-smart-button
entity: climate.living_room_thermostat
name: living room
icon: mdi:thermostat
custom_states_labels:
  "off": ""
  eco: eco
  comfort: comfort
```

Możesz dodać tyle stanów, ile potrzeba — termostat ma własne `hvac_modes` i `preset_modes`, więc lista kluczy zależy od konkretnego urządzenia (np. `away`, `home`, `sleep`, `boost`, `manual` itd.). Mechanizm sprawdzania pozostaje ten sam — trzy "pętle" po kolei:
1. `hvac_action` — jeśli nie znaleziono dopasowania w `custom_states_labels`, przejdź do (2).
2. `state` (czyli `hvac_modes`) — jeśli brak dopasowania, przejdź do (3).
3. `preset_mode` — ostatnia szansa na dopasowanie.

Pusty string (`""`) jako wartość klucza jest w pełni poprawny i sprawia, że druga linia po prostu nie pojawia się na karcie dla tego konkretnego stanu (tak jakby nie było dopasowania), mimo że technicznie dopasowanie już zostało znalezione w pierwszej pętli.

## `weather` — druga linia wartości (value2) na podstawie temperatury

Działa przez ten sam mechanizm co `climate`, z tą różnicą, że dopasowanie odbywa się do atrybutu `temperature` encji `weather`. Główna wartość (przetłumaczony stan pogody, np. "Słonecznie") pozostaje bez zmian — pod nią dodawana jest tylko druga, mniejsza linia.

```yaml
type: custom:piotras-smart-button
entity: weather.home_forecast
name: Speaker Bedroom
font_style: 2
custom_states_labels:
  < -5: Siberia! Winter jacket, beanie, gloves, and scarf are mandatory.
  "-5 - 5": Cold! Thick coat/jacket, layers, and headwear recommended.
  5 - 12: Chilly. Transitional jacket or thicker coat, light scarf.
  12 - 17: Mild weather. Light jacket, bomber, or thick hoodie.
  17 - 22: Comfortable. Long sleeve, light sweater, or t-shirt + hoodie.
  22 - 26: Warm! T-shirt and shorts, light dress.
  "> 26": Hot! Light and breezy, sunglasses and baseball cap.
```

Jeśli nie podasz `custom_states_labels` — nic się nie zmienia, dostajesz przetłumaczony stan pogody jak dotychczas. Jeśli temperatura nie ma dopasowania w żadnym zakresie — druga linia po prostu się nie pokazuje.

## Tryb zegara (`entity: on.clock`) — powitanie zależne od pory dnia

`on.clock` nie jest prawdziwą encją Home Assistant — to zegar wbudowany w kartę, działający na czasie po stronie przeglądarki (aktualizowany co sekundę). Jeśli podasz `custom_states_labels` z zakresami godzin, pod cyframi zegara pojawia się powitanie pasujące do aktualnej godziny — i **aktualizuje się na żywo**, bez przeładowania karty.

```yaml
type: custom:piotras-smart-button
entity: on.clock
name: Clock
custom_states_labels:
  "0-5": "Quiet night... 🌙"
  "5-9": "Good morning! ☕"
  "9-17": "Have a great day! 👋"
  "17-22": "Good evening! 🌆"
  "22-0": "Good night! ✨"
```

Dopasowanie odbywa się do aktualnej godziny (`0–23`, minuty są ignorowane), z tymi samymi zasadami co wszędzie indziej (kolejność wpisów, zakresy przechodzące przez północ jak `"22-0"`, automatyczne zawijanie tekstu, `<br>`). Jeśli żaden zakres nie pasuje do aktualnej godziny — powitanie po prostu się nie pokazuje. Jeśli nie podasz `custom_states_labels` — zegar działa dokładnie tak jak wcześniej, bez zmian.

## Pełny przykład — karta z temperaturą

```yaml
type: custom:piotras-smart-button
name: living room
entity: sensor.living_room_thermometer_temperature
custom_states_labels:
  "<17": Turn On Heating
  "17-17.9": Cold
  "18-18.9": Cool
  "19-24.9": Comfortable
  "25-25.9": Warm
  "26-26.9": Hot
  ">=27": Turn On AC
```

## Łamanie linii — automatyczne i ręczne

Tekst etykiety **zawija się automatycznie**, jeśli nie mieści się w szerokości karty — bez dodatkowych działań. Jeśli chcesz **wymusić** złamanie linii w konkretnym miejscu (np. żeby uniknąć niezgrabnego zawinięcia), użyj `<br>` w treści etykiety:

```yaml
custom_states_labels:
  "22 - 26": "Warm! T-shirt and shorts <br> light dress."
```

`<br>` działa, ponieważ etykieta jest wstawiana do karty jako prawdziwy HTML, a nie zwykły tekst. Znak `\n` (nowa linia w YAML) **nie zadziała** — przeglądarka go ignoruje.

## Pełny HTML w etykietach ✅ (potwierdzone działanie)

Etykieta w `custom_states_labels` nie musi być zwykłym tekstem — może zawierać **dowolny tag HTML ze stylami inline** (`<span style="...">`, `<br>`, emoji, zagnieżdżone elementy itd.), ponieważ karta wstawia ją bezpośrednio jako HTML, a nie jako zwykły tekst. Dzięki temu możesz np. zbudować kolorową "plakietkę" dla głównej etykiety i osobno stylować drugą linię z opisem — dokładnie jak w tym rzeczywistym przykładzie z karty pogodowej:

> Wynik: czerwona plakietka **"Hot! 🔥"**, jasnoszary podpis pod nią z tekstem "Light and breezy, sunglasses and baseball cap.", oraz temperatura (28.5°C) pokazana osobno, jak zwykle, na dole karty.

```yaml
custom_states_labels:
  "< -5": >-
    <span style='background: rgba(0,0,0,0.5); padding: 4px 10px;
    border-radius: 6px; display: inline-block; font-size: 19px;
    color: #00ffff; font-weight: bold;'>Siberia! 🥶</span><br>
    <span style='font-size: 15px; opacity: 0.85;'>Winter jacket, beanie,
    gloves, and scarf are mandatory.</span>
  "> 26": >-
    <span style='background: rgba(0,0,0,0.5); padding: 4px 10px;
    border-radius: 6px; display: inline-block; font-size: 19px;
    color: #ef4444; font-weight: bold;'>Hot! 🔥</span><br>
    <span style='font-size: 15px; opacity: 0.85;'>Light and breezy,
    sunglasses and baseball cap.</span>
```

Kilka praktycznych uwag:

- **Cudzysłowy wewnątrz `style`** — użyj pojedynczych cudzysłowów (`style='...'`) wewnątrz HTML, żeby nie kolidowały z podwójnymi cudzysłowami otaczającymi klucz YAML (np. `"< -5"`). Mieszanie `"` w kluczu i `"` w stylu psuje parsowanie YAML.
- Wieloliniowy HTML w YAML wygodnie zapisuje się jako złożony blok skalarny (folded) za pomocą `>-` — łamie linie w pliku, ale przed wysłaniem do karty zwija je w jeden string. `<br>` w treści nadal działa normalnie, bo to jawny tag HTML, niezależny od łamania linii w samym YAML.
- To działa identycznie dla każdego typu encji opisanego powyżej (`climate`, `weather`, `light`/`fan`/`cover`, `on.clock`, dopasowanie dokładne/operatory/zakresy) — HTML to po prostu zawartość etykiety, nie wpływa na logikę dopasowania.
- Jak zawsze: `\n` nie zadziała jako złamanie linii — działa tylko `<br>`.

## 🆕 Użycie `custom_data` / szablonów jako wartości etykiet

**Klucz** w `custom_states_labels` (to, co jest dopasowywane — dokładny stan, operator albo zakres) zawsze jest statycznym stringiem, ocenianym tak jak opisano powyżej. Ale **wartość** (tekst/HTML etykiety faktycznie pokazywany) nie musi być statyczna — każda pojedyncza wartość może zamiast tego być:

- **odwołaniem do `custom_data`** (`customData.jakisKlucz`, w tym zagnieżdżonymi ścieżkami jak `customData.Configuration.label`), albo
- **blokiem szablonu `p{{{ ... }}}q`**, ponownie obliczanym za każdym razem, gdy aktualizuje się stan karty.

Dzięki temu tekst, kolor lub zawartość etykiety mogą reagować na coś innego niż sam dopasowany klucz — np. pokazywać inne powitanie w zależności od dnia tygodnia albo pobierać przetłumaczony tekst z tabeli w `custom_data` — podczas gdy klucz nadal dopasowuje wyłącznie do surowego stanu encji/godziny/procentu, tak jak dotychczas.

```yaml
custom_data:
  weekendGreeting: |-
    p{{{
      const day = new Date().getDay();   // 0 = niedziela, 6 = sobota
      return (day === 0 || day === 6) ? "Leniwy weekendowy poranek ☕" : "Dzień dobry! ☕";
    }}}q

custom_states_labels:
  "5-9": customData.weekendGreeting
  "9-17": "Miłego dnia! 👋"
```

```yaml
custom_data:
  hotLabel: |-
    p{{{
      return `<span style="color:#ef4444;font-weight:bold;">Gorąco! 🔥</span>`;
    }}}q

custom_states_labels:
  "> 26": customData.hotLabel
  "22-26": "Ciepło! Koszulka i szorty."
```

Kilka ważnych punktów:

- **Tylko wartość może być szablonem/odwołaniem — nigdy klucz.** Klucz (`"5-9"`, `">26"`, `off` itd.) zawsze jest dopasowywany jako zwykły string do stanu/godziny/procentu encji; nie da się zrobić samego klucza dynamicznym.
- **`states` nie jest dostępne wewnątrz szablonu użytego jako wartość etykiety.** Dokładnie tak jak w każdym innym polu poza `custom_data` (patrz sekcja 4 poradnika `custom_data`), blok `p{{{ }}}q` umieszczony bezpośrednio jako wartość etykiety ma dostęp tylko do `customData` — nie do `states`. Jeśli etykieta ma odczytać inną encję, zrób ten odczyt najpierw wewnątrz `custom_data`, a potem odwołaj się do wyniku.
- To dotyczy identycznie `vacuum_states_labels` (starszego odpowiednika `custom_states_labels`).
- Obowiązują tu też wszystkie te same zasady z poradnika `custom_data` — przede wszystkim, jeśli odwoływany klucz `custom_data` sam jest obliczany na podstawie innego klucza `custom_data`, musi być **zdefiniowany wcześniej w YAML**, inaczej zwróci `undefined` (patrz poradnik `custom_data`, sekcja 8).
- Zmieszanie statycznego tekstu z szablonem/odwołaniem w **tej samej** wartości etykiety nie działa — tak jak w każdym innym templatowalnym polu, wartość musi być albo zwykłym statycznym stringiem, albo pełnym odwołaniem `customData.klucz`, albo pełnym blokiem `p{{{ }}}q`, a nie ich kombinacją w jednym stringu.

## Zakresy przechodzące przez północ (np. godziny 22-0)

Jeśli dolna granica zakresu jest **większa** niż górna (np. `"22-0"`), karta automatycznie interpretuje to jako zakres przechodzący przez punkt graniczny (np. północ) — dopasowanie działa jako "wartość ≥ dolna **LUB** wartość ≤ górna", a nie jako standardowe "pomiędzy".

```yaml
type: custom:piotras-smart-button
entity: sensor.hour
custom_states_labels:
  "0-5": "Quiet night... 🌙"
  "5-9": "Good morning! ☕"
  "9-17": "Have a great day! 👋"
  "17-22": "Good evening! 🌆"
  "22-0": "Good night! ✨"
```

Przy sensorze takim jak `{{ now().strftime('%H:%M') }}`, dopasowanie odbywa się do **godziny** (minuty są ignorowane przy parsowaniu liczby) — więc `23:45` i `23:05` dają ten sam wynik.

## Ważne uwagi dotyczące pisania YAML

- Klucze zaczynające się od `>` **muszą** być w cudzysłowach (`">27"`), ponieważ `>` to zarezerwowany znak YAML (wskaźnik złożonego bloku skalarnego typu folded), a bez cudzysłowów konfiguracja nie wczyta się poprawnie.
- Klucze zaczynające się od `<` **mogą** być bez cudzysłowów (`<17`), ale dla czytelności i spójności lepiej ująć w cudzysłów wszystkie klucze operatorowe: `"<17"`, `">27"`, `">=30"`, `"<=5"`.
- Klucze tekstowe (np. dla `vacuum`, `media_player`: `playing`, `paused`, `off`) nie wymagają cudzysłowów.
