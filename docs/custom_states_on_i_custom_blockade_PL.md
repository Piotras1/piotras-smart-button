# `custom_states_on` i `custom_blockade` — zasady działania

Te dwie opcje są niezależne od `custom_states_labels` (opisanego w osobnym dokumencie) — nie zmieniają treści żadnej etykiety, tylko wizualny stan „aktywny" karty (`custom_states_on`) oraz blokowanie interakcji (`custom_blockade`). Można je swobodnie łączyć z `custom_states_labels`, ale działają w pełni samodzielnie.

## `custom_states_on` — wymuszanie wizualnego stanu „aktywny" dla dowolnej encji

`custom_states_on` to **osobna** opcja od `custom_states_labels` — nie dotyczy treści etykiety, tylko tego, czy karta uzna encję za „włączoną/aktywną" wizualnie. Wpływa na:
- wybór ikony: `icon_on` zamiast `icon`,
- kolor ikony: `icon_color_on` zamiast `icon_color`,
- podświetlenie/gradient tła karty (klasa `active`),
- filtr zdjęcia tła, jeśli używasz `background_image_on`/`background_image_off`.

```yaml
type: custom:piotras-smart-button
entity: input_select.brama_garazowa_status
name: Garaż
icon: mdi:garage
icon_on: mdi:garage-open
custom_states_on:
  - open
  - opening
```

Domyślnie (bez `custom_states_on`) karta sama zgaduje, czy stan jest „aktywny", na podstawie domeny encji — m.in.:
- `battery` → `charging`,
- `climate` → `hvac_action` w `heating`/`cooling`,
- `alarm_control_panel` → stany typu `armed_*`/`triggered`,
- `vacuum` → `cleaning`/`returning`,
- każda inna domena → stan w liście `["on", "open", "true", "home", "playing", "unlocked"]`.

Dla `input_select` (i podobnych encji bez wbudowanego on/off) stan `open` **już domyślnie** zostałby uznany za aktywny (jest na liście powyżej), ale `opening` — nie. Dlatego w powyższym przykładzie podano `custom_states_on: [open, opening]` — żeby **obie** te wartości włączały `icon_on` i podświetlenie, nadpisując domyślną listę.

⚠️ `custom_states_on` (jako niepustą listę) **w pełni i konsekwentnie zastępuje** domyślną logikę „aktywności” dla domen **bez własnej wbudowanej logiki** (np. `input_select`, `switch`, `lock`, `cover`, `media_player`, `light`, `fan`, `person`, `binary_sensor` z niestandardowymi stanami). Dla pięciu **specjalnych** domen — `climate`, `battery` (sensor z `device_class: battery`), czujnik komfortu (`temperature`/`humidity` z `comfort_min`/`comfort_max`), `alarm_control_panel` i `vacuum` — sytuacja jest bardziej skomplikowana, patrz ostrzeżenie poniżej.

### 🐛 Znany błąd karty: na `climate`/`battery`/`comfort`/`alarm`/`vacuum` tło i ikona mogą się rozjechać

To jest realna niekonsekwencja w samym kodzie karty (nie błąd w tej dokumentacji), więc opisuję ją jako ostrzeżenie. Po każdej aktualizacji stanu z Home Assistant karta liczy „czy aktywne” **dwukrotnie, dwoma różnymi funkcjami, w innej kolejności**:

1. Funkcja sterująca **tłem karty i klasą `active`** sprawdza `custom_states_on` **najpierw** — jeśli jest ustawione, wygrywa zawsze, niezależnie od domeny.
2. Funkcja sterująca **wyborem ikony** (`icon_on` vs `icon`) sprawdza domeny w innej kolejności: `battery` → `climate` → czujnik komfortu → `alarm_control_panel` → `vacuum` → **i tylko na samym końcu** `custom_states_on`. Dla tych pięciu domen Twoja lista `custom_states_on` jest więc w praktyce **ignorowana przy wyborze ikony** — nawet jeśli tło już poprawnie reaguje na Twoją listę.

**Skutek praktyczny:** jeśli ustawisz `custom_states_on` na encji `climate`/`battery`/`alarm_control_panel`/`vacuum`/czujniku temperatury-humidity z `comfort_min`/`comfort_max` — tło karty (poświata, kolor obwódki ikony) zareaguje na Twoją listę, ale **`icon_on` może się nie pojawić**, bo wybór ikony nadal liczony jest po staremu (`hvac_action`, `charging`, stan alarmu itd.), ignorując Twoje `custom_states_on`.

➡️ Dla tych pięciu domen `custom_states_on` działa więc **połowicznie** (tło tak, ikona nie) — nie jest w pełni wiarygodne. Dla wszystkich innych domen obie funkcje liczą to samo i `custom_states_on` działa w 100% konsekwentnie, tak jak w przykładach w tym dokumencie (np. `input_select` z garażem).


ℹ️ Dopasowanie w `custom_states_on`, tak jak w `custom_states_labels`, jest **niezależne od wielkości liter**.

`custom_states_on` i `custom_states_labels` to dwie niezależne opcje — możesz użyć jednej bez drugiej (np. tylko zmienić tekst etykiety bez zmiany ikony, albo odwrotnie), ale w praktyce dla encji typu `input_select` najczęściej ustawia się obie naraz, jak w przykładzie powyżej.

## `custom_blockade` — blokowanie karty dla wybranych stanów

`custom_blockade` to lista stanów encji (dopasowanie dokładne, **bez rozróżniania wielkości liter** — tak jak `custom_states_on`), które, gdy aktualny stan encji znajdzie się na liście, **całkowicie wyłączają interakcję z kartą**: `tap_action`, `hold_action` i `double_tap_action` przestają działać, dopóki encja jest w jednym z podanych stanów.

```yaml
type: custom:piotras-smart-button
entity: cover.brama_garazowa
name: Brama
icon: mdi:garage
icon_on: mdi:garage-open
custom_states_on:
  - open
  - opening
custom_blockade:
  - opening
  - closing
```

W tym przykładzie, gdy brama jest w trakcie ruchu (`opening`/`closing`), kliknięcie/przytrzymanie karty **nie wywoła żadnej akcji** — dopóki brama nie dokończy ruchu i nie przejdzie w stan `open` albo `closed`. To zabezpieczenie przed np. wielokrotnym wysłaniem komendy w trakcie animacji bramy, czy przed przerwaniem cyklu alarmu w trakcie `arming`.

Inny typowy przykład — blokada podczas `triggered` alarmu (żeby przypadkowe stuknięcie karty nie próbowało nic zmieniać, gdy alarm faktycznie się uruchomił):

```yaml
type: custom:piotras-smart-button
entity: alarm_control_panel.dom
name: Alarm
custom_blockade:
  - triggered
  - arming
```

Ważne, jak to działa technicznie:
- Karta **wizualnie** sygnalizuje blokadę kursorem `default` (zamiast normalnego "tapnięcia" ze zmniejszeniem) — to subtelna wskazówka, nie wyraźny komunikat czy szary nakładka.
- Blokada dotyczy **tylko** interakcji (kliknięć), nie wpływa na wyświetlaną etykietę (`custom_states_labels`) ani na wizualny stan „aktywny" (`custom_states_on`) — te trzy opcje są od siebie całkowicie niezależne i można je swobodnie łączyć.
- `custom_blockade` to **inna opcja** niż `blockade_card` (osobny mechanizm — tymczasowa blokada po wywołaniu `call-service` na czas trwania countdownu, niezwiązana ze stanem encji). Nie mylić tych dwóch.
- Jeśli stan encji nie jest na liście `custom_blockade` — karta działa normalnie, bez żadnych zmian.

## Sztuczki: nietypowe zastosowania `custom_states_on`

### 1. Wymuszenie wyświetlania surowej treści encji (`input_text`, `input_number`)

Domyślnie, bez żadnych opcji `custom_*`, nieznana karcie domena (np. `input_text`, `input_number`) wyświetla tylko generyczne `ON`/`OFF` jako etykietę — bo karta nie wie, co innego pokazać. Jeśli jednak dodasz `custom_states_on` (lub starszy alias `vacuum_states_on`) jako **niepustą tablicę**, nawet z jedną „śmieciową", niemożliwą do dopasowania wartością — karta przełącza się na inną ścieżkę wewnętrzną, która jako fallback wypisuje **dosłowną treść stanu encji** (pisaną WIELKIMI LITERAMI):

```yaml
type: custom:piotras-smart-button
entity: input_text.test_textu
vacuum_states_on:
  - null
```

Tutaj `- null` nigdy nie dopasuje się do żadnego realnego stanu `input_text` — ale samo podanie tablicy wystarczy, żeby karta zaczęła pokazywać aktualną treść pola tekstowego jako główną etykietę, zamiast sztywnego `ON`/`OFF`. To wygodne dla encji z dowolną, zmienną treścią (notatki, liczniki, statusy tekstowe), gdzie wypisanie wszystkich możliwych wartości w `custom_states_labels` byłoby niewykonalne.

⚠️ **Pułapka:** ta ścieżka w kodzie najpierw sprawdza, czy stan encji nie pokrywa się (bez rozróżniania wielkości liter) z jednym z kilku stałych słów-kluczy ze słownika odkurzacza (`cleaning`, `docked`, `returning`, `paused`, `idle`, `error`, `charging`). Jeśli Twój `input_text` przypadkiem zawiera akurat jedno z tych słów (np. literalnie wpiszesz „idle" albo „error"), karta pokaże przetłumaczone słowo ze słownika odkurzacza (np. „GOTOWY"/„BŁĄD") **zamiast** Twojej rzeczywistej treści. W praktyce rzadko to ma znaczenie (mała szansa kolizji dla treści dowolnej), ale warto wiedzieć, skąd taki ewentualny „dziwny" wynik się bierze.

### 2. „Przycisk-wyzwalacz" — custom_states_on jako wskaźnik gotowości, nie dosłowny on/off

`custom_states_on` nie musi odpowiadać dosłownemu „włączeniu" encji — możesz go użyć semantycznie, jako wskaźnik „ten przycisk jest aktywny/gotowy do kliknięcia w tym momencie". Przykład: dedykowany przycisk **tylko do otwierania** bramy, który podświetla się (jako gotowy do użycia), gdy brama jest zamknięta, a blokuje się całkowicie, gdy brama jest w ruchu lub już otwarta (żeby nie próbować otwierać czegoś, co już jest otwarte albo w trakcie ruchu):

```yaml
type: custom:piotras-smart-button
entity: cover.testowa_brama_garazowa
custom_states_on:
  - closed
custom_blockade:
  - open
  - opening
  - closing
```

Tu `custom_states_on: [closed]` nie oznacza „brama jest włączona, gdy zamknięta" w sensie fizycznym — to po prostu sygnał wizualny „ten przycisk czeka na Twoje kliknięcie" (podświetlony `icon_on`), podczas gdy `custom_blockade` całkowicie wyłącza reakcję na dotyk, gdy brama jest już otwarta/w ruchu. Razem dają w efekcie samoblokujący się przycisk jednorazowej akcji.

### 3. Podświetlanie przycisku na podstawie dokładnej wartości `input_number`

`custom_states_on` może też dopasowywać dokładną wartość liczbową zapisaną jako stan encji `input_number` — przydatne np. do zbudowania zestawu przycisków „odkurz pokój nr X", gdzie każdy przycisk podświetla się tylko, gdy aktualnie wybrany numer pokoju zgadza się z jego własnym numerem:

```yaml
type: custom:piotras-smart-button
entity: input_number.numer_pokoju
custom_states_on:
  - "3.0"
```

⚠️ Stan encji `input_number` to **string**, a nie liczba — i często zawiera końcowe zero po przecinku (np. `"3.0"`, nie `"3"`), zależnie od kroku (`step`) ustawionego w konfiguracji `input_number`. Dopasowanie jest dokładne (choć bez rozróżniania wielkości liter, co przy liczbach nie ma znaczenia) — `"3.0"` nie złapie stanu `"3"` ani odwrotnie. Sprawdź dokładny format w Developer Tools → States w Home Assistant, zanim wpiszesz wartość w konfiguracji karty.

