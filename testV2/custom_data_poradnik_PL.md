# custom_data — jak pisać kod w karcie (Piotras Smart Button)

Ten poradnik tłumaczy krok po kroku, jak korzystać z `custom_data` — miejsca w konfiguracji karty, gdzie możesz napisać własną logikę (odczyt czujników, warunki, kolory, teksty) i wykorzystać ją w dowolnym polu karty.

---

## 1. Czym jest `custom_data`

To osobna sekcja w YAML-u karty, w której definiujesz własne "zmienne" — każda z nich to jeden klucz, a jej wartość to albo zwykły tekst, albo kawałek kodu, który coś oblicza.

```yaml
custom_data:
  mojaZmienna: |-
    p{{{
      return "Cześć świecie";
    }}}q
```

Potem w dowolnym polu karty (np. `name`, `icon`, `background_image_on`) możesz się do tego odwołać:

```yaml
name: customData.mojaZmienna
```

I tyle — karta pokaże "Cześć świecie" jako nazwę.

---

## 2. Jedyna obowiązująca składnia: `p{{{ ... }}}q`

Kod wpisujesz zawsze między `p{{{` a `}}}q`. To nie jest przypadkowy wybór — silnik karty rozpoznaje **tylko** tę składnię. Stare warianty (`${...}`, `${(() => {...})()}`) **nie działają** — jeśli je wkleisz, karta pokaże je jako zwykły, dosłowny tekst zamiast wykonać kod.

```yaml
custom_data:
  przyklad: |-
    p{{{
      return "to jest wynik";
    }}}q
```

### Ważne: `return` jest obowiązkowe

Blok `p{{{ ... }}}q` to funkcja — jeśli nie napiszesz `return`, nic nie zostanie zwrócone (pole będzie puste). To częsty błąd na start:

```yaml
# ŹLE — nic nie zwróci
p{{{ "Cześć"; }}}q

# DOBRZE
p{{{ return "Cześć"; }}}q
```

---

## 3. Format w YAML — jedna linia czy wiele?

Możesz napisać kod w jednej linii:

```yaml
custom_data: {start: "p{{{ return states['switch.gniazdko']?.state !== 'unavailable'; }}}q"}
```

**Ale nie musisz się tym przejmować** — jeśli zapiszesz kartę przez edytor YAML w Home Assistant, HA **sam** przerobi to na czytelniejszy, wieloliniowy zapis:

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.gniazdko']?.state !== 'unavailable'; }}}q
```

Oba zapisy działają identycznie — różnica jest czysto kosmetyczna. Polecam od razu pisać w formie wieloliniowej (`|-`), bo dłuższy kod w jednej linii szybko robi się nieczytelny, a i tak HA to przerobi za Ciebie przy pierwszym zapisie.

---

## 4. Skąd bierzesz dane — `states[...]`

Wewnątrz `custom_data` masz dostęp do zmiennej `states`, przez którą czytasz stan dowolnej encji:

```yaml
custom_data:
  temperatura: |-
    p{{{
      const sensor = states['sensor.temperatura_salon'];
      return sensor?.state;
    }}}q
```

- `states['sensor.temperatura_salon']` — daje Ci cały obiekt encji (stan + atrybuty)
- `.state` — sam stan jako tekst, np. `"21.5"`
- `.attributes.cos_tam` — dodatkowe dane, np. `.attributes.humidity`

### `?.` — Twój najlepszy przyjaciel

Zawsze pisz `?.` zamiast `.`, gdy sięgasz do encji, która może nie istnieć albo być tymczasowo niedostępna. Bez tego karta wywali błąd, jeśli encja zniknie:

```js
// RYZYKOWNE — wywali się, jeśli encja nie istnieje
p{{{ return states['sensor.cos'].state; }}}q

// BEZPIECZNE
p{{{ return states['sensor.cos']?.state; }}}q
```

### ⚠️ Bardzo ważna zasada: `states[...]` działa TYLKO w `custom_data`

To jest twarda reguła silnika, nie tylko dobra praktyka. Pola karty poza `custom_data` (`name`, `icon`, `background_image_on`, `show_more` itd.) **nie mają dostępu** do `states`. Jeśli spróbujesz:

```yaml
show_more: p{{{ return states['switch.gniazdko']?.state === 'on'; }}}q
```

...dostaniesz błąd w konsoli przeglądarki, a pole cicho przyjmie wartość pustą/fałszywą. **Cały odczyt encji musi się odbyć w `custom_data`**, a inne pola tylko odwołują się do już policzonego wyniku:

```yaml
custom_data:
  gniazdkoWlaczone: |-
    p{{{ return states['switch.gniazdko']?.state === 'on'; }}}q
show_more: customData.gniazdkoWlaczone
```

Dlaczego tak jest? Żeby zawsze wiedzieć, gdzie szukać, z jakich encji korzysta karta — jedno miejsce, a nie rozsiane po całym configu.

---

## 5. Jak odwołać się do `custom_data` w innych polach karty

Masz dwie opcje:

### A) Prosty skrót — `customData.klucz`

Jeśli wartość już jest gotowa (bool, tekst, liczba), po prostu ją wskaż — **bez** żadnego `p{{{ }}}q`:

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.gniazdko']?.state !== 'unavailable'; }}}q
show_more: customData.start
```

Działa też z zagnieżdżeniem, jeśli `custom_data` zwraca cały obiekt:

```yaml
custom_data:
  Configuration: |-
    p{{{
      return { temperatura: 21.5, kolor: '#22c55e' };
    }}}q
name: customData.Configuration.kolor
```

### B) Blok z logiką — gdy potrzebujesz czegoś policzyć w miejscu użycia

Jeśli samo pole (np. `name`) ma podjąć jeszcze jakąś decyzję na podstawie danych z `custom_data`:

```yaml
name: |-
  p{{{
    if (customData.Configuration.temperatura > 25) return "Gorąco";
    return "OK";
  }}}q
```

**Uwaga:** to `p{{{ }}}q` w polu `name` **nie ma dostępu do `states`** — tylko do `customData`. To ta sama zasada z punktu 4.

---

## 6. Podstawy pisania kodu w `p{{{ ... }}}q`

Kod w środku to zwykły JavaScript. Nie musisz znać go dogłębnie — poniżej najważniejsze rzeczy, które wystarczą do 95% przypadków.

### Zmienne

```js
const temperatura = 21.5;   // wartość, która się nie zmieni w tym bloku
let opis = "OK";            // wartość, którą możesz nadpisać niżej
```

### Warunki

```js
p{{{
  const temp = parseFloat(states['sensor.temperatura']?.state) || 0;

  if (temp > 25) {
    return "Gorąco";
  } else if (temp < 15) {
    return "Zimno";
  } else {
    return "OK";
  }
}}}q
```

### Wartości domyślne — `||`

```js
const poziom = Number(states['sensor.bateria']?.state) || 0;   // gdy brak/0/pusty -> 0
```

### Tekst z wstawionymi zmiennymi (template literals)

Zamiast sklejać teksty `+`, użyj apostrofów wstecznych (`` ` ``) i `${}`:

```js
const temp = 21.5;
return `Temperatura: ${temp}°C`;   // -> "Temperatura: 21.5°C"
```

To działa **wewnątrz** kodu JS — to nie to samo, co stare `${}` na poziomie pól karty (które jest martwe). Wewnątrz template literala w Twoim kodzie `${}` działa normalnie, bo to zwykła składnia JavaScriptu.

### Obiekty i tablice

```js
return {
  temperatura: 21.5,
  kolor: '#22c55e',
  lista: [1, 2, 3]
};
```

### Przydatne wbudowane funkcje

| Funkcja | Do czego |
|---|---|
| `parseFloat(tekst)` | zamienia tekst na liczbę z częścią dziesiętną, np. `"21.5"` → `21.5` |
| `Number(tekst)` | podobnie, zamienia na liczbę |
| `wartosc.toFixed(1)` | zaokrągla liczbę do 1 miejsca po przecinku (jako tekst) |
| `Math.min(...tablica)` / `Math.max(...tablica)` | najmniejsza/największa wartość z listy |
| `tekst.includes("coś")` | sprawdza, czy tekst zawiera dany fragment |
| `tablica.map(x => x * 2)` | przetwarza każdy element listy |
| `tablica.filter(x => x > 0)` | zostawia tylko elementy spełniające warunek |
| `(warunek) ? "tak" : "nie"` | skrócony zapis if/else w jednej linii |

### Komentarze

Wszystko po `//` w danej linii jest ignorowane — używaj do opisywania co robi kod:

```js
// To liczy temperaturę w Celsjuszu
const temp = parseFloat(states['sensor.x']?.state) || 0;
```

---

## 7. Wzorzec: jeden blok z danymi + moduły, które z nich korzystają

Jeśli karta ma więcej niż jedno pole zależne od encji, nie czytaj tej samej encji kilka razy w kilku miejscach. Zrób to raz, w jednym bloku, i podaj dalej.

```yaml
custom_data:

  # 1. Tu i tylko tu czytamy states[...]
  Configuration: |-
    p{{{
      // === ZMIENNE (zmieniaj przy kopiowaniu karty) ===
      const entityId = 'sensor.temperatura_salon';

      // === ENCJE ===
      const sensor = states[entityId];

      // === LOGIKA ===
      const temp = parseFloat(sensor?.state) || 0;
      const kolor = temp > 25 ? '#fb923c' : '#22c55e';

      return { temp, kolor };
    }}}q

  # 2. Moduł prezentacji — korzysta z Configuration, nie czyta states
  opisHtml: |-
    p{{{
      const d = customData.Configuration;
      return `<span style="color:${d.kolor};">${d.temp.toFixed(1)}°C</span>`;
    }}}q

name: customData.opisHtml
```

**Zasada:** jeśli blok tylko przekazuje jedną wartość dalej bez żadnej dodatkowej roboty (`return customData.Configuration.temp;` i nic więcej) — nie twórz go. Odwołaj się od razu: `customData.Configuration.temp`.

---

## 8. Najczęstsze błędy

| Błąd | Co się dzieje | Jak naprawić |
|---|---|---|
| Brak `return` w bloku | Pole jest puste | Zawsze kończ blok `return ...;` |
| `states[...]` poza `custom_data` | Błąd w konsoli, pole puste/fałszywe | Przenieś odczyt do `custom_data` |
| Brak `?.` przy odczycie encji | Karta się wywala, gdy encja zniknie | Zawsze `states['x']?.state` |
| Stara składnia `${...}` | Traktowana jak zwykły tekst, nic się nie liczy | Użyj `p{{{ ... }}}q` |
| Osobny blok tylko po to, żeby przekazać jedną wartość | Niepotrzebny bałagan w configu | Odwołaj się bezpośrednio, np. `customData.Configuration.pole` |

---

## 9. Gotowe przykłady do skopiowania

**Prosty warunek bool (włącz/wyłącz coś w karcie na podstawie stanu encji):**

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.gniazdko']?.state === 'on'; }}}q
show_more: customData.start
```

**Tekst z wartością liczbową i jednostką:**

```yaml
custom_data:
  info: |-
    p{{{
      const val = parseFloat(states['sensor.moc']?.state) || 0;
      return `Pobór: ${val.toFixed(0)} W`;
    }}}q
name: customData.info
```

**Kolor zależny od progu:**

```yaml
custom_data:
  kolor: |-
    p{{{
      const val = parseFloat(states['sensor.moc']?.state) || 0;
      return val > 1500 ? '#fb923c' : '#22c55e';
    }}}q
text_color: customData.kolor
```

---

Jeśli utkniesz — sprawdź konsolę przeglądarki (F12 → Console). Silnik karty zawsze wypisze tam ostrzeżenie z opisem, co poszło nie tak, zamiast po cichu zepsuć całą kartę.
