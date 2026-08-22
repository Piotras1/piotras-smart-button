# custom_data — jak pisać kod w karcie (Piotras Smart Button)

Ten poradnik krok po kroku wyjaśnia `custom_data` — część konfiguracji karty, w której piszesz własną logikę (odczyt sensorów, warunki, kolory, teksty) i wykorzystujesz wynik w dowolnym innym polu karty.

Są dwie ścieżki korzystania z tego poradnika, zależnie od tego, co robisz:

- **Po prostu wklejasz gotowy moduł** (np. "odtwarzacz multimediów z okładką", "zegar z powitaniem") z biblioteki modułów → przeczytaj sekcje 1–5, potem przejdź od razu do **sekcji 7 — Wklejanie gotowego modułu**. Nie musisz rozumieć kodu JS w środku, tylko dwie–trzy linijki oznaczone jako ustawienia.
- **Piszesz własny `custom_data` od zera** → przeczytaj cały poradnik, w tym sekcję 8 o współdzieleniu danych między kilkoma polami.

---

## 1. Czym jest `custom_data`

To osobna sekcja w YAML karty, w której definiujesz własne "zmienne" — każda z nich to klucz, a jej wartość to albo zwykły tekst, albo kawałek kodu, który coś oblicza.

```yaml
custom_data:
  myVariable: |-
    p{{{
      return "Hello world";
    }}}q
```

Następnie w dowolnym polu karty (np. `name`, `icon`, `background_image_on`) możesz się do niej odwołać:

```yaml
name: customData.myVariable
```

I tyle — karta pokaże "Hello world" jako swoją nazwę.

---

## 2. Jedyna wspierana składnia: `p{{{ ... }}}q`

Kod zawsze umieszczasz między `p{{{` a `}}}q`. To nie jest przypadkowe — silnik karty rozpoznaje **wyłącznie** tę składnię. Starsze formy (`${...}`, `${(() => {...})()}`) **nie działają** — jeśli je wkleisz, karta wyświetli je jako zwykły tekst zamiast je uruchomić.

```yaml
custom_data:
  example: |-
    p{{{
      return "this is the result";
    }}}q
```

### Ważne: `return` jest obowiązkowy

Blok `p{{{ ... }}}q` to funkcja — jeśli nie napiszesz `return`, nic nie zostanie zwrócone (pole pozostaje puste). To częsty pierwszy błąd:

```yaml
# ŹLE — nic nie zwraca
p{{{ "Hello"; }}}q

# DOBRZE
p{{{ return "Hello"; }}}q
```

### ⚠️ Blok musi być *całą* wartością pola — nic innego wokół niego

Silnik wykonuje blok `p{{{ }}}q` tylko wtedy, gdy stanowi **całą** wartość pola (po przycięciu białych znaków). Jeśli w tym samym polu zmieszasz zwykły tekst z blokiem, silnik wprawdzie wykryje, że gdzieś w środku jest blok, ale nie potrafi go wykonać — bo cała wartość jako całość nie pasuje już do wzorca "zaczyna się od `p{{{`, kończy na `}}}q`". Efekt: ostrzeżenie w konsoli, a **cały string** — łącznie z kodem — zostaje pokazany jako zwykły, dosłowny tekst.

```yaml
# ŹLE — tekst i blok zmieszane w jednym polu; nic się nie wykonuje,
# całość (łącznie z "p{{{ ... }}}q") jest pokazana jako dosłowny tekst
name: "Wynik: p{{{ return 1 + 1; }}}q"

# DOBRZE — blok jest całą wartością pola
name: |-
  p{{{
    return "Wynik: " + (1 + 1);
  }}}q
```

Jeśli chcesz połączyć statyczny tekst z obliczoną wartością, zrób tę konkatenację *wewnątrz* bloku (za pomocą template literal, patrz sekcja 6), a nie na zewnątrz niego.

---

## 3. Formatowanie YAML — jedna linia czy wiele?

Możesz napisać kod w jednej linii:

```yaml
custom_data: {start: "p{{{ return states['switch.socket']?.state !== 'unavailable'; }}}q"}
```

**Ale nie musisz się tym przejmować** — jeśli zapiszesz kartę przez edytor YAML w Home Assistant, HA **automatycznie** przepisze to na bardziej czytelną, wielolinijkową formę:

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.socket']?.state !== 'unavailable'; }}}q
```

Obie formy zachowują się identycznie — różnica jest czysto kosmetyczna. Polecam pisać od razu w formie wielolinijkowej (`|-`), bo dłuższy kod w jednej linii szybko robi się nieczytelny, a HA i tak go przekonwertuje przy pierwszym zapisie.

---

## 4. Skąd biorą się dane — `states[...]`

Wewnątrz `custom_data` masz dostęp do zmiennej `states`, dzięki której odczytujesz stan dowolnej encji:

```yaml
custom_data:
  temperature: |-
    p{{{
      const sensor = states['sensor.living_room_temp'];
      return sensor?.state;
    }}}q
```

- `states['sensor.living_room_temp']` — daje pełny obiekt encji (stan + atrybuty)
- `.state` — sam stan jako tekst, np. `"21.5"`
- `.attributes.something` — dodatkowe dane, np. `.attributes.humidity`

### `?.` — twój najlepszy przyjaciel

Zawsze pisz `?.` zamiast `.`, gdy sięgasz do encji, która może nie istnieć lub być tymczasowo niedostępna. Bez tego karta rzuci błędem, jeśli encja zniknie:

```js
// RYZYKOWNE — rzuca błąd, jeśli encja nie istnieje
p{{{ return states['sensor.x'].state; }}}q

// BEZPIECZNE
p{{{ return states['sensor.x']?.state; }}}q
```

### ⚠️ Bardzo ważna zasada: `states[...]` działa tylko wewnątrz `custom_data`

To twarde ograniczenie silnika, nie tylko zalecenie stylistyczne. Pola poza `custom_data` (`name`, `icon`, `background_image_on`, `show_more` itd.) **nie mają dostępu** do `states`. Jeśli spróbujesz:

```yaml
show_more: p{{{ return states['switch.socket']?.state === 'on'; }}}q
```

...dostaniesz ostrzeżenie w konsoli przeglądarki, a pole po cichu przyjmie wartość pustą/fałszywą. **Wszystkie odczyty encji muszą odbywać się wewnątrz `custom_data`**, a inne pola tylko odwołują się do już obliczonego wyniku:

```yaml
custom_data:
  socketOn: |-
    p{{{ return states['switch.socket']?.state === 'on'; }}}q
show_more: customData.socketOn
```

Dlaczego? Żebyś zawsze wiedział dokładnie, gdzie szukać, od jakich encji zależy karta — w jednym miejscu, a nie rozrzucone po całej konfiguracji.

---

## 5. Jak odwołać się do `custom_data` z innych pól karty

Masz dwie opcje:

### A) Prosty skrót — `customData.klucz`

Jeśli wartość jest już obliczona (bool, tekst, liczba), po prostu wskaż na nią — **bez** `p{{{ }}}q`:

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.socket']?.state !== 'unavailable'; }}}q
show_more: customData.start
```

To działa też z zagnieżdżeniem, jeśli `custom_data` zwraca cały obiekt:

```yaml
custom_data:
  Configuration: |-
    p{{{
      return { temperature: 21.5, color: '#22c55e' };
    }}}q
name: customData.Configuration.color
```

#### ⚠️ Obsługiwane są tylko proste ścieżki kropkowe

Skrót rozumie wyłącznie zwykłe identyfikatory połączone kropkami — `customData.klucz`, `customData.klucz.zagnieżdżony` i tak dalej. **Nie** rozumie natomiast składni z indeksem tablicy ani nawiasami kwadratowymi:

```yaml
# DZIAŁA
name: customData.Configuration.color

# NIE DZIAŁA — składnia z nawiasem/indeksem nie jest rozpoznawana jako odwołanie do customData
name: customData.list[0]
name: customData['klucz ze spacją']
```

Jeśli potrzebujesz elementu z tablicy albo klucza ze znakami specjalnymi, zrób to wydobycie *wewnątrz* bloku `p{{{ }}}q` (patrz opcja B poniżej), np. `p{{{ return customData.list[0]; }}}q`.

### B) Blok logiki — gdy trzeba coś dodatkowo zdecydować w miejscu użycia

Jeśli samo pole (np. `name`) musi podjąć dodatkową decyzję na podstawie danych z `custom_data`:

```yaml
name: |-
  p{{{
    if (customData.Configuration.temperature > 25) return "Hot";
    return "OK";
  }}}q
```

**Uwaga:** ten blok `p{{{ }}}q` wewnątrz `name` **nie ma dostępu do `states`** — tylko do `customData`. Ta sama zasada co w sekcji 4.

---

## 6. Pisanie kodu wewnątrz `p{{{ ... }}}q`

Kod w środku to zwykły JavaScript. Nie musisz go znać dogłębnie — poniżej jest wszystko, co pokrywa 95% przypadków.

### Zmienne

```js
const temperature = 21.5;   // wartość, która nie zmieni się w tym bloku
let description = "OK";     // wartość, którą możesz nadpisać poniżej
```

### Warunki

```js
p{{{
  const temp = parseFloat(states['sensor.temperature']?.state) || 0;

  if (temp > 25) {
    return "Hot";
  } else if (temp < 15) {
    return "Cold";
  } else {
    return "OK";
  }
}}}q
```

### Wartości domyślne — `||`

```js
const level = Number(states['sensor.battery']?.state) || 0;   // brak/0/puste -> 0
```

### Tekst z osadzonymi zmiennymi (template literals)

Zamiast sklejać teksty za pomocą `+`, użyj backticków (`` ` ``) i `${}`:

```js
const temp = 21.5;
return `Temperature: ${temp}°C`;   // -> "Temperature: 21.5°C"
```

To działa **wewnątrz** twojego kodu JS — to nie to samo co stary, martwy `${}` na poziomie karty. Wewnątrz template literal w twoim własnym kodzie `${}` działa normalnie, bo to zwykła składnia JavaScript.

### Obiekty i tablice

```js
return {
  temperature: 21.5,
  color: '#22c55e',
  list: [1, 2, 3]
};
```

### Przydatne funkcje wbudowane

| Funkcja | Co robi |
|---|---|
| `parseFloat(text)` | zamienia tekst na liczbę dziesiętną, np. `"21.5"` → `21.5` |
| `Number(text)` | podobnie, zamienia na liczbę |
| `value.toFixed(1)` | zaokrągla liczbę do 1 miejsca po przecinku (jako tekst) |
| `Math.min(...array)` / `Math.max(...array)` | najmniejsza/największa wartość na liście |
| `text.includes("something")` | sprawdza, czy tekst zawiera dany fragment |
| `array.map(x => x * 2)` | przekształca każdy element listy |
| `array.filter(x => x > 0)` | zostawia tylko elementy spełniające warunek |
| `(condition) ? "yes" : "no"` | skrócone if/else w jednej linii |

### Komentarze

Wszystko po `//` w danej linii jest ignorowane — użyj tego, żeby opisać, co robi kod:

```js
// To oblicza temperaturę w stopniach Celsjusza
const temp = parseFloat(states['sensor.x']?.state) || 0;
```

---

## 7. Wklejanie gotowego modułu

Biblioteka modułów (odtwarzacze multimediów, zegary z powitaniem, przyciski radiowe z okładką itd.) jest celowo zbudowana wokół jednej zasady:

> **Każdy moduł to jeden, samodzielny blok.** Wszystko, czego potrzebuje — ID encji, kolory, domyślne obrazki, logika i końcowy wynik — mieści się w tym jednym bloku. Nic nie jest rozbite na kilka kluczy `custom_data` i nic nie odwołuje się do innego modułu.

To celowa decyzja projektowa, nawet jeśli oznacza to, że jeśli wkleisz dwa moduły na tej samej karcie, oba mogą osobno odczytywać tę samą encję. To w porządku — ten mały dodatkowy koszt się opłaca, ponieważ:

- **Możesz skopiować tylko jeden blok i on działa**, bez szukania dwóch–trzech innych bloków, od których po cichu zależy.
- **Możesz usunąć lub zastąpić moduł**, nie psując niczego innego na karcie.
- **Nie możesz przypadkowo zepsuć działającego modułu**, edytując "współdzielony" blok, od którego zależy też coś innego.

### Jak wygląda gotowy moduł

```yaml
custom_data:
  RadioButtonCover: |-
    p{{{
      // ===== USTAWIENIA — to jedyna część, którą edytujesz =====
      const entityId = 'media_player.living_room';
      const defaultImage = '/local/speaker.png';
      // ============================================================

      const p = states[entityId];
      const isOnline = p && ['playing', 'paused', 'idle'].includes(p.state);
      const cover = isOnline
        ? (p.attributes.entity_picture_local || p.attributes.entity_picture || defaultImage)
        : defaultImage;

      return { isOnline, cover, attributes: p ? p.attributes : {} };
    }}}q
background_image_on: customData.RadioButtonCover.cover
show_state: false
```

Żeby użyć takiego modułu:

1. Skopiuj cały blok, od `custom_data:` aż do zamykającego `}}}q` (plus wszelkie pola karty poniżej, które się do niego odwołują, jak `background_image_on` powyżej).
2. Zmieniaj tylko to, co jest między `// ===== USTAWIENIA ===== ` a `// ============================================================` — zwykle tylko ID encji i ewentualnie ścieżkę domyślnego obrazka lub kolor.
3. Wszystko poniżej tej linii zostaw bez zmian, nawet jeśli tego nie rozumiesz.

### Nazwy bloków modułów muszą być unikalne na tej samej karcie

Jeśli wkleisz dwa moduły na tej samej karcie i oba nazwiesz np. `Configuration`, drugi po cichu nadpisze pierwszy — stracisz dane pierwszego modułu bez żadnego komunikatu o błędzie.

Oficjalne moduły z biblioteki mają odrębne, opisowe nazwy (`RadioButtonCover`, `PlayerCompact`, `ClockGreeting` itd.), właśnie po to, żeby to się nie zdarzyło przy łączeniu kilku z nich. Jeśli kiedykolwiek napiszesz własny moduł, żeby się nim podzielić, nadaj mu też konkretną nazwę — unikaj ogólnych, jak `Configuration`, `Logic` czy `Data`.

---

## 8. Zaawansowane: współdzielenie jednego odczytu encji między kilkoma polami

*Pomiń tę sekcję, jeśli tylko wklejasz gotowe moduły z biblioteki — jest ona dla sytuacji, gdy piszesz kilka własnych pól i chcesz uniknąć wielokrotnego odczytu tej samej encji.*

Jeśli karta ma więcej niż jedno pole zależne od tej samej encji, możesz odczytać ją raz, w jednym bloku, i przekazać wynik dalej zamiast powtarzać `states[...]` w każdym z nich:

```yaml
custom_data:

  # 1. To jedyne miejsce czytające states[...]
  Configuration: |-
    p{{{
      // === ZMIENNE (zmień to przy kopiowaniu karty) ===
      const entityId = 'sensor.living_room_temp';

      // === ENCJE ===
      const sensor = states[entityId];

      // === LOGIKA ===
      const temp = parseFloat(sensor?.state) || 0;
      const color = temp > 25 ? '#fb923c' : '#22c55e';

      return { temp, color };
    }}}q

  # 2. Moduł prezentacyjny — konsumuje Configuration, nie czyta states
  descriptionHtml: |-
    p{{{
      const d = customData.Configuration;
      return `<span style="color:${d.color};">${d.temp.toFixed(1)}°C</span>`;
    }}}q

name: customData.descriptionHtml
```

**Zasada:** jeśli blok tylko przekazuje jedną wartość dalej, bez wykonywania żadnej realnej pracy (`return customData.Configuration.temp;` i nic więcej) — nie twórz go. Odwołaj się bezpośrednio: `customData.Configuration.temp`.

Ten wzorzec jest dobrym pomysłem dla karty, którą sam budujesz i utrzymujesz. Jest złym pomysłem do publikacji jako moduł do wklejenia dla innych użytkowników, z powodów opisanych w sekcji 7.

### ⚠️ Klucze są rozwiązywane od góry do dołu — kolejność w YAML ma znaczenie

Klucze `custom_data` są obliczane **w kolejności, w jakiej występują w pliku YAML**, jeden po drugim. Każdy klucz widzi tylko te klucze, które zostały **już obliczone przed nim** — czyli te zapisane *powyżej* niego. Jeśli klucz odwołuje się do innego klucza zdefiniowanego *poniżej*, to odwołanie zwróci `undefined`, bo ten klucz nie został jeszcze obliczony w tym momencie.

```yaml
# ŹLE — descriptionHtml jest zdefiniowany PRZED Configuration, więc
# customData.Configuration jest wciąż undefined, gdy descriptionHtml się wykonuje
custom_data:
  descriptionHtml: |-
    p{{{
      const d = customData.Configuration;   # tutaj undefined!
      return `${d.temp}°C`;
    }}}q
  Configuration: |-
    p{{{
      return { temp: 21.5 };
    }}}q

# DOBRZE — Configuration jest zdefiniowany jako pierwszy, więc descriptionHtml może go użyć
custom_data:
  Configuration: |-
    p{{{
      return { temp: 21.5 };
    }}}q
  descriptionHtml: |-
    p{{{
      const d = customData.Configuration;   # działa — już obliczone
      return `${d.temp}°C`;
    }}}q
```

**Zasada praktyczna:** zawsze umieszczaj blok, który *czyta encję*, powyżej bloku (bloków), który *konsumuje jego wynik*.

---

## 9. Najczęstsze błędy

| Błąd | Co się dzieje | Poprawka |
|---|---|---|
| Brak `return` w bloku | Pole zostaje puste | Zawsze kończ blok `return ...;` |
| `states[...]` poza `custom_data` | Ostrzeżenie w konsoli, pole przyjmuje wartość pustą/fałszywą | Przenieś odczyt do `custom_data` |
| Brak `?.` przy odczycie encji | Karta się wysypuje, jeśli encja zniknie | Zawsze `states['x']?.state` |
| Stara składnia `${...}` | Traktowana jako dosłowny tekst, nic nie jest obliczane | Użyj `p{{{ ... }}}q` |
| Zmieszanie zwykłego tekstu z blokiem `p{{{ }}}q` w tym samym polu | Blok nie jest rozpoznawany jako wykonywalny — cały string, wraz z kodem, jest pokazany jako dosłowny tekst | Blok musi być *jedyną* zawartością pola; wszelki otaczający tekst buduj wewnątrz bloku przez template literal |
| Użycie `customData.list[0]` lub `customData['klucz ze spacją']` jako skrótu | Nierozpoznane jako odwołanie do customData — potraktowane jako zwykły tekst | Zrób dostęp do tablicy/nawiasu wewnątrz bloku `p{{{ }}}q` |
| Klucz `custom_data` odwołujący się do innego klucza zdefiniowanego *poniżej* w YAML | Odwołanie zwraca `undefined` | Zmień kolejność, żeby czytany klucz był zapisany wcześniej w YAML |
| Osobny blok, który tylko przekazuje jedną wartość dalej | Niepotrzebny bałagan w konfiguracji | Odwołaj się bezpośrednio, np. `customData.Configuration.field` |
| Wklejenie tylko części wieloblokowego modułu (np. `Logic` bez `Configuration`) | Błąd `customData.Configuration is undefined`, moduł się psuje | Preferuj moduły jednoblokowe (sekcja 7); jeśli używasz wieloblokowego, skopiuj każdy blok, od którego zależy |
| Dwa wklejone moduły używające tej samej nazwy bloku (np. oba `Configuration`) | Drugi po cichu nadpisuje pierwszy — bez błędu, po prostu brakujące dane | Nadaj każdemu blokowi modułu odrębną, opisową nazwę |

---

## 10. Gotowe do skopiowania przykłady

**Prosty warunek logiczny (przełączanie czegoś na karcie na podstawie stanu encji):**

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.socket']?.state === 'on'; }}}q
show_more: customData.start
```

**Tekst z wartością liczbową i jednostką:**

```yaml
custom_data:
  info: |-
    p{{{
      const val = parseFloat(states['sensor.power']?.state) || 0;
      return `Draw: ${val.toFixed(0)} W`;
    }}}q
name: customData.info
```

**Kolor zależny od progu:**

```yaml
custom_data:
  color: |-
    p{{{
      const val = parseFloat(states['sensor.power']?.state) || 0;
      return val > 1500 ? '#fb923c' : '#22c55e';
    }}}q
text_color: customData.color
```

---

Jeśli utkniesz — sprawdź konsolę przeglądarki (F12 → Console). Silnik karty zawsze loguje ostrzeżenie opisujące dokładnie, co poszło nie tak, zamiast po cichu psuć całą kartę.

> Biblioteka [**Custom Data & Modules**](https://github.com/Piotras1/piotras-smart-button/discussions/categories/custom-data-modules) (odtwarzacze multimedialne, zegary powitalne, przyciski radiowe z okładkami itp.) jest celowo oparta na jednej zasadzie:
