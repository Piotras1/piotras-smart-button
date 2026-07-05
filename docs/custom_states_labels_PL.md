# `custom_states_labels` — zasady dopasowania

`custom_states_labels` pozwala podać własne etykiety tekstowe wyświetlane w karcie, w zależności od aktualnego stanu encji. Obsługiwane są trzy typy kluczy:

| Typ klucza | Przykład | Znaczenie |
|---|---|---|
| Dopasowanie dokładne | `"25": OK` | stan równy dokładnie `25` |
| Operator porównania | `">25": Ciepło` | stan większy/mniejszy od podanej liczby |
| Zakres | `"10-25": Normalnie` | stan w przedziale domkniętym `[10, 25]` |

Dozwolone operatory: `>`, `<`, `>=`, `<=`.

## ⚠️ NAJWAŻNIEJSZA ZASADA: kolejność wpisów ma znaczenie

Karta sprawdza klucze **w takiej kolejności, w jakiej są zapisane w YAML, od góry do dołu**, i zwraca etykietę dla **pierwszego dopasowania, jakie znajdzie**. Nie szuka „najlepszego" ani „najbardziej szczegółowego" warunku — bierze pierwszy z rzędu, który jest prawdziwy.

Wyjątek: dopasowanie **dokładne** (np. `"25"`) jest sprawdzane jako pierwsze, niezależnie od pozycji w pliku — dopiero gdy nie znajdzie dokładnego dopasowania, przechodzi do sprawdzania operatorów/zakresów po kolei.

ℹ️ Dopasowanie dokładne (tekstowe) jest **niezależne od wielkości liter** — klucz `open` złapie stan `open`, `Open` i `OPEN` jednakowo. Dotyczy to każdego miejsca, gdzie karta robi dopasowanie tekstowe: stanu encji, `hvac_action`, `preset_mode`, itd.

### Operatory `>` i `>=` → zapisuj progi malejąco (od najwyższego do najniższego)

```yaml
custom_states_labels:
  ">30": Gorąco      # sprawdzane jako pierwsze
  ">20": Ciepło      # sprawdzane jako drugie
  ">10": Chłodno     # sprawdzane jako trzecie
```

❌ Źle (rosnąco) — `>10` złapie wszystko od 10 w górę, zanim dojdzie do `>20` czy `>30`:
```yaml
custom_states_labels:
  ">10": Chłodno     # to złapie WSZYSTKO powyżej 10, łącznie z 25 i 35!
  ">20": Ciepło      # nigdy nie zostanie sprawdzone dla wartości > 20
  ">30": Gorąco      # nigdy nie zostanie sprawdzone dla wartości > 30
```

### Operatory `<` i `<=` → zapisuj progi rosnąco (od najniższego do najwyższego)

```yaml
custom_states_labels:
  "<10": Zimno       # sprawdzane jako pierwsze
  "<20": Chłodno     # sprawdzane jako drugie
  "<30": Ciepło      # sprawdzane jako trzecie
```

❌ Źle (malejąco) — `<30` złapie wszystko poniżej 30, łącznie z 5 i 15.

### Zakresy (`min-max`) → unikaj nakładających się granic

Jeśli zakresy się stykają na tej samej liczbie (np. `17-18` i `18-19`), wartość graniczna (tu: `18`) zawsze trafi do **zakresu wcześniejszego** w pliku, bo on jest sprawdzany pierwszy.

❌ Granice się nakładają (18 zawsze da „Zimno", nie „Chłodno"):
```yaml
custom_states_labels:
  "17-18": Zimno
  "18-19": Chłodno
```

✅ Granice się nie nakładają — każda wartość trafia tam, gdzie się tego oczekuje:
```yaml
custom_states_labels:
  "17-17.9": Zimno
  "18-18.9": Chłodno
```

## Dopasowanie dla dowolnej encji (input_select, sensor tekstowy, lock, switch itd.)

`custom_states_labels` nie jest ograniczone do `light`/`fan`/`cover`/`climate`/`weather`/`on.clock` — to są tylko domeny ze **specjalną** obsługą (procenty, druga linia value2, godzina). Dla **każdej innej** encji (np. `input_select`, `sensor` z tekstowymi stanami, `lock`, `switch`, `person`, `media_player`, `vacuum`...) `custom_states_labels` działa jako **główna etykieta** (ta sama, która domyślnie pokazuje `ON`/`OFF`/`PLAY`/`STOP` itd.) — karta sprawdza dokładne dopasowanie do surowego stanu encji **zawsze jako pierwszy krok**, przed jakąkolwiek logiką domyślną danej domeny.

```yaml
type: custom:piotras-smart-button
entity: input_select.brama_garazowa_status
name: Garaż
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

ℹ️ `custom_states_on` widoczne w powyższym przykładzie to osobna opcja (steruje ikoną/podświetleniem „aktywności", nie treścią etykiety) — opisana szczegółowo w dokumencie **`custom_states_on i custom_blockade`**.

W tym przykładzie `custom_states_labels` zwyczajnie podmienia tekst etykiety w zależności od dokładnego stanu `input_select` (`closed`/`open`/`opening`/`closing`) — i, jak widać, treść etykiety może zawierać HTML (`<b>`, `<br>`, emoji) tak jak opisano w sekcji „Pełny HTML w etykietach" wyżej.

Ten sam mechanizm działa identycznie dla `alarm_control_panel` — encja ma swoje własne nazwy stanów (`disarmed`, `armed_home`, `arming`, `armed_away`, `armed_night`, `triggered`...), więc można im po prostu nadać dowolne, klimatyczne etykiety:

```yaml
type: custom:piotras-smart-button
entity: alarm_control_panel.dom
name: Alarm
custom_states_labels:
  disarmed: Free to roam! 🔓
  armed_home: Fortress Mode Active 🏰
  arming: Fortress Mode Arming
  armed_away: Empty Castle, Lasers ON! 🚨
  armed_night: Ghost busters enabled 👻
  triggered: INTRUDER! RELEASE THE HOUNDS! 🐕💥
```

Wizualny stan „aktywny" (podświetlenie, `icon_on`) dla alarmu ustawia się automatycznie na podstawie stanów `armed_*`/`pending`/`arming`/`triggered` — bez potrzeby dodawania `custom_states_on`, choć możesz go nadpisać, jeśli chcesz inną logikę.

## Encje `light` — etykiety zależne od jasności (brightness)

Stan encji `light` to zawsze tylko `on` albo `off` — żeby rozróżnić poziomy jasności (np. „Półmrok", „Ciemno", „Jasno", „Max"), karta dodatkowo sprawdza atrybut `brightness` (przeliczony na %), jeśli samo dopasowanie po stanie `on`/`off` nie zwróci etykiety.

```yaml
type: custom:piotras-smart-button
entity: light.lsc_outdoor_neon_strip_3m
name: living room
custom_states_labels:
  off: Wyłączony
  "<10": Ciemno
  "10-40": Półmrok
  "40-80": Jasno
  ">80": Max
```

Kolejność sprawdzania dla `light`:
1. Dopasowanie **dokładne** po stanie (`off`, `on` — jeśli podane jako klucz) — ma absolutny priorytet.
2. Jeśli nie znaleziono i światło jest `on` z atrybutem `brightness`, przeliczenie na % i dopasowanie do zakresów/operatorów `custom_states_labels` (te same zasady kolejności co wyżej: `>` malejąco, `<` rosnąco, zakresy bez nakładających się granic).
3. Jeśli nic nie dopasowano — domyślne `DIM`/`ON`/`OFF`.

⚠️ Jeśli podasz klucz `on: cokolwiek`, **zawsze wygra** on nad dopasowaniem procentowym jasności (dopasowanie dokładne ma najwyższy priorytet) — do rozróżniania poziomów jasności nie dodawaj klucza `on`, tylko same zakresy/operatory liczbowe + `off`.

## Domeny z procentami: `light`, `fan`, `cover`

Ten sam mechanizm (dopasowanie po stanie, a jeśli nie znaleziono — po wartości procentowej) działa dla trzech domen:

| Domena | Atrybut źródłowy | Przeliczanie na % |
|---|---|---|
| `light` | `brightness` (0–255) | `brightness / 255 * 100` |
| `fan` | `percentage` (0–100) | bez przeliczania |
| `cover` | `current_position` (0–100) | bez przeliczania |

Przykład dla wentylatora:
```yaml
type: custom:piotras-smart-button
entity: fan.wentylator_salon
name: Wentylator
custom_states_labels:
  off: Wyłączony
  "<20": Wolno
  "20-60": Średnio
  ">60": Mocno
```

Kolejność sprawdzania jest identyczna jak dla `light` opisana wyżej (dopasowanie dokładne po stanie ma priorytet, potem wartość procentowa, te same zasady malejąco/rosnąco dla operatorów).

## `climate` — druga linia wartości (value2) z custom_states_labels

Dla encji `climate` główna wartość (`state-badge`) **zawsze pokazuje aktualną temperaturę** (`current_temperature`) — to się nie zmienia. Jeśli jednak podasz `custom_states_labels`, pod temperaturą pojawi się **dodatkowa, mniejsza linia** z dopasowaną etykietą.

```yaml
type: custom:piotras-smart-button
entity: climate.salon
name: Salon
custom_states_labels:
  off: Wyłączony
  heating: Ogrzewa
  idle: Bezczynny
  comfort: Komfort
  eco: Eco
  none: Ręczny
```

Zasady działania:
- Dopasowanie sprawdza po kolei trzy źródła i zwraca etykietę z **pierwszego, które ma dopasowanie** w `custom_states_labels`:
  1. `hvac_action` — co urządzenie faktycznie robi w tej chwili (`heating`/`cooling`/`idle`/`drying`/`fan`/`off`) — najwyższy priorytet, bo to najdokładniejsza informacja o stanie urządzenia.
  2. `state` — aktualny tryb HVAC, jeden z `hvac_modes` (`heat`/`cool`/`auto`/`off`) — fallback, gdy `hvac_action` nie jest dostępny lub nie ma dopasowania.
  3. `preset_mode` — aktualnie wybrany tryb z `preset_modes` (np. `none`/`away`/`comfort`/`eco`/`home`/`sleep`/`activity`) — fallback na końcu, gdy ani `hvac_action` ani `state` nie dają dopasowania.
- Jeśli nie ma dopasowania w żadnym z trzech źródeł (np. zdefiniowałeś tylko `heating`/`cooling`, a urządzenie jest w stanie `drying`) — druga linia po prostu **się nie wyświetla**, temperatura jest pokazywana normalnie.
- Jeśli nie podasz `custom_states_labels` w ogóle — nic się nie zmienia, dostajesz tylko temperaturę jak dotychczas.
- Rozmiar czcionki drugiej linii domyślnie to `state_size - 5` (px), z minimum 8px. Możesz go nadpisać opcją `value2_size`:

```yaml
custom_states_labels:
  eco: Eco
value2_size: 10
```

⚠️ **Dla `climate` dopasowanie jest wyłącznie dokładne (tekstowe) — nie numeryczne.** Karta porównuje wartości `hvac_action`/`state`/`preset_mode` jako stringi z kluczami w `custom_states_labels`. Zakresy (`"10-25"`) i operatory (`">25"`, `"<10"`) **nie działają** dla `climate` — to nie ma sensu, bo te trzy źródła to nazwy stanów/trybów (`heating`, `eco`, `comfort`...), a nie liczby. Jeśli chcesz reagować na temperaturę, robisz to dla `sensor` z aktualną temperaturą jako osobnej encji, nie dla samej `climate`.

Inny przykład — termostat z trzema warstwami dopasowania i pustą etykietą dla `off` (żeby druga linia po prostu się nie wyświetlała, gdy ogrzewanie jest wyłączone):

```yaml
type: custom:piotras-smart-button
entity: climate.termostat_salon_gn
name: living room
icon: mdi:thermostat
custom_states_labels:
  "off": ""
  eco: eco
  comfort: comfort
```

Można dodać tyle stanów, ile potrzeba — termostat ma swoje własne `hvac_modes` i `preset_modes`, więc lista kluczy zależy od konkretnego urządzenia (np. `away`, `home`, `sleep`, `boost`, `manual` itd.). Mechanizm sprawdzania zostaje ten sam — trzy „pętle" po kolei:
1. `hvac_action` — jeśli nie znajdzie dopasowania w `custom_states_labels`, przechodzi do (2).
2. `state` (czyli `hvac_modes`) — jeśli nie znajdzie, przechodzi do (3).
3. `preset_mode` — ostatnia szansa na dopasowanie.

Pusty string (`""`) jako wartość klucza jest w pełni poprawny i powoduje, że dla tego konkretnego stanu druga linia po prostu nie pojawia się na karcie (tak jakby nie było dopasowania), mimo że technicznie dopasowanie zostało znalezione już w pierwszej pętli.

## `weather` — druga linia wartości (value2) na podstawie temperatury

Działa tym samym mechanizmem co `climate`, tylko dopasowanie odbywa się po atrybucie `temperature` encji `weather`. Główna wartość (przetłumaczony stan pogody, np. "Słonecznie") zostaje bez zmian — dodaje się tylko druga, mniejsza linia pod nią.

```yaml
type: custom:piotras-smart-button
entity: weather.forecast_dom
name: Speaker Bedroom
font_style: 2
custom_states_labels:
  < -5: Syberia! Kurtka zimowa, czapka, rękawice i szalik obligatoryjnie.
  "-5 - 5": Zimno! Gruby płaszcz/kurtka, warstwy i coś na głowę.
  5 - 12: Chłodno. Kurtka przejściowa lub grubszy płaszcz, lekki szalik.
  12 - 17: Wiosennie/Jesiennie. Lekka kurtka, bomberka lub grubsza bluza.
  17 - 22: Komfortowo. Długi rękaw, lekki sweter lub t-shirt + bluza.
  22 - 26: Ciepło! T-shirt i krótkie spodenki lekka sukienka.
  "> 26": Upał! Lekko i przewiewnie, okulary przeciwsłoneczne i czapka z daszkiem.
```

Jeśli nie podasz `custom_states_labels` — nic się nie zmienia, dostajesz tylko przetłumaczony stan pogody jak dotychczas. Jeśli temperatura nie ma dopasowania w żadnym z zakresów — druga linia po prostu się nie wyświetla.

## Tryb zegara (`entity: on.clock`) — powitanie na podstawie godziny

`on.clock` to nie jest realna encja Home Assistant — to wbudowany w kartę zegar, liczący czas po stronie przeglądarki (aktualizowany co sekundę). Jeśli podasz `custom_states_labels` z zakresami godzin, pod cyframi zegara pojawi się powitanie dopasowane do aktualnej godziny — i **aktualizuje się na żywo**, bez przeładowania karty.

```yaml
type: custom:piotras-smart-button
entity: on.clock
name: Zegar
custom_states_labels:
  "0-5": "Cisza nocna... 🌙"
  "5-9": "Dzień dobry! ☕"
  "9-17": "Miłego dnia! 👋"
  "17-22": "Dobry wieczór! 🌆"
  "22-0": "Dobrej nocy! ✨"
```

Dopasowanie odbywa się po aktualnej godzinie (`0–23`, bez minut), z tymi samymi zasadami co wszędzie (kolejność zapisu, zakresy przez północ jak `"22-0"`, auto-zawijanie tekstu, `<br>`). Jeśli żaden zakres nie pasuje do bieżącej godziny — powitanie po prostu się nie wyświetla. Jeśli nie podasz `custom_states_labels` — zegar działa jak dotychczas, bez zmian.

## Pełny przykład — karta temperatury

```yaml
type: custom:piotras-smart-button
name: living room
entity: sensor.sonoff_termometr_salon_temperature
custom_states_labels:
  "<17": Włącz Ogrzewanie
  "17-17.9": Zimno
  "18-18.9": Chłodno
  "19-24.9": Komfortowo
  "25-25.9": Ciepło
  "26-26.9": Gorąco
  ">=27": Włącz Klimę
```

## Łamanie linii — automatyczne i ręczne

Tekst etykiety **zawija się automatycznie**, jeśli nie mieści się w szerokości karty — nie trzeba już niczego specjalnego robić. Jeśli chcesz **wymusić** złamanie linii w konkretnym miejscu (np. żeby nie urywało w nieładnym momencie), użyj `<br>` w treści etykiety:

```yaml
custom_states_labels:
  "22 - 26": "Ciepło! T-shirt i krótkie spodenki <br> lekka sukienka."
```

`<br>` działa, bo etykieta trafia na kartę jako prawdziwy HTML, a nie zwykły tekst. Znak `\n` (nowa linia w YAML) **nie zadziała** — przeglądarka go zignoruje.

## Pełny HTML w etykietach ✅ (potwierdzone działanie)

Etykieta z `custom_states_labels` nie musi być samym tekstem — może zawierać **dowolny znacznik HTML wraz z inline-stylami** (`<span style="...">`, `<br>`, emoji, zagnieżdżone elementy itd.), bo karta wstawia ją bezpośrednio jako HTML, a nie jako plain text. Dzięki temu można np. zrobić „plakietkę" z kolorowym tłem dla głównej etykiety i osobno wystylizować drugą linię z opisem — dokładnie tak jak na poniższym, realnym przykładzie z karty pogodowej:

> Efekt: czerwona plakietka **"Hot! 🔥"**, pod nią jasnoszary podpis "Light and breezy, sunglasses and baseball cap.", a temperatura (28.5°C) wyświetlana osobno, jak zwykle, na dole karty.

```yaml
custom_states_labels:
  "< -5": >-
    <span style='background: rgba(0,0,0,0.5); padding: 4px 10px;
    border-radius: 6px; display: inline-block; font-size: 19px;
    color: #00ffff; font-weight: bold;'>Syberia! 🥶</span><br>
    <span style='font-size: 15px; opacity: 0.85;'>Kurtka zimowa, czapka,
    rękawice i szalik obligatoryjnie.</span>
  "> 26": >-
    <span style='background: rgba(0,0,0,0.5); padding: 4px 10px;
    border-radius: 6px; display: inline-block; font-size: 19px;
    color: #ef4444; font-weight: bold;'>Hot! 🔥</span><br>
    <span style='font-size: 15px; opacity: 0.85;'>Light and breezy,
    sunglasses and baseball cap.</span>
```

Kilka uwag praktycznych:

- **Cudzysłowy w `style`** — używaj apostrofów (`style='...'`) wewnątrz HTML-a, żeby nie kolidowały z cudzysłowami otaczającymi klucz YAML (np. `"< -5"`). Mieszanie `"` w kluczu i `"` w stylu psuje parsowanie YAML.
- Wielowierszowy HTML w YAML wygodnie zapisać blokiem składanym ze spłaszczeniem `>-` (folded block scalar) — łamie linie wewnątrz pliku, ale skleja je w jeden ciąg przed wysłaniem do karty. `<br>` w treści wciąż działa normalnie, bo to jawny znacznik HTML, niezależny od łamania linii w samym YAML.
- Działa to identycznie dla wszystkich typów encji opisanych wyżej (`climate`, `weather`, `light`/`fan`/`cover`, `on.clock`, dopasowanie dokładne/operatory/zakresy) — HTML jest tylko treścią etykiety, nie wpływa na logikę dopasowania.
- Jak zawsze: znak `\n` nie zadziała jako łamanie linii — tylko `<br>`.

## Zakresy przez północ / „zawijane" (np. godziny 22-0)

Jeśli dolna granica zakresu jest **większa** od górnej (np. `"22-0"`), karta automatycznie interpretuje to jako zakres przechodzący przez punkt zawijania (np. przez północ) — dopasowanie działa jako „wartość ≥ dolna **LUB** wartość ≤ górna", a nie standardowo „pomiędzy".

```yaml
type: custom:piotras-smart-button
entity: sensor.godzina
custom_states_labels:
  "0-5": "Cisza nocna... 🌙"
  "5-9": "Dzień dobry! ☕"
  "9-17": "Miłego dnia! 👋"
  "17-22": "Dobry wieczór! 🌆"
  "22-0": "Dobrej nocy! ✨"
```

Przy sensorze typu `{{ now().strftime('%H:%M') }}` dopasowanie odbywa się po **godzinie** (minuty są ignorowane przy parsowaniu liczby) — czyli `23:45` i `23:05` dają ten sam wynik.

## Ważne przy zapisie w YAML

- Klucze zaczynające się od `>` **muszą** być w cudzysłowie (`">27"`), bo `>` jest zarezerwowanym znakiem YAML (wskaźnik bloku składanego) i bez cudzysłowu konfiguracja się nie wczyta poprawnie.
- Klucze zaczynające się od `<` **mogą** być bez cudzysłowu (`<17`), ale dla czytelności i konsekwencji lepiej cytować wszystkie klucze z operatorami: `"<17"`, `">27"`, `">=30"`, `"<=5"`.
- Klucze tekstowe (np. dla `vacuum`, `media_player`: `playing`, `paused`, `off`) nie wymagają cudzysłowu.
