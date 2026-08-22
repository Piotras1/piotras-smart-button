# custom_data — how to write code in the card (Piotras Smart Button)

> 🔙 Back to the [Main README](../README.md)

This guide walks through `custom_data` — the part of the card's configuration where you write your own logic (reading sensors, conditions, colors, text) and use the result in any other field of the card.

There are two ways to use this guide, depending on what you're doing:

- **Just pasting a ready-made module** (e.g. "media player with cover art", "greeting clock") from the module library → read sections 1–5, then jump straight to **section 7 — Pasting a ready-made module**. You don't need to understand the JS inside, just the two or three lines marked as settings.
- **Writing your own `custom_data` from scratch** → read the whole guide, including section 8 on sharing data between several fields.

---

## 1. What `custom_data` is

It's a separate section in the card's YAML where you define your own "variables" — each one is a key, and its value is either plain text, or a piece of code that computes something.

```yaml
custom_data:
  myVariable: |-
    p{{{
      return "Hello world";
    }}}q
```

Then, in any field of the card (e.g. `name`, `icon`, `background_image_on`), you can reference it:

```yaml
name: customData.myVariable
```

That's it — the card will show "Hello world" as its name.

---

## 2. The only supported syntax: `p{{{ ... }}}q`

Code always goes between `p{{{` and `}}}q`. This isn't arbitrary — the card's engine recognizes **only** this syntax. Older forms (`${...}`, `${(() => {...})()}`) **do not work** — if you paste them in, the card will display them as literal text instead of running them.

```yaml
custom_data:
  example: |-
    p{{{
      return "this is the result";
    }}}q
```

### Important: `return` is mandatory

A `p{{{ ... }}}q` block is a function — if you don't write `return`, nothing is returned (the field stays empty). This is a common first mistake:

```yaml
# WRONG — returns nothing
p{{{ "Hello"; }}}q

# CORRECT
p{{{ return "Hello"; }}}q
```

### ⚠️ The block must be the *entire* field value — nothing else around it

The engine only evaluates a `p{{{ }}}q` block when it's the **whole** value of the field (after trimming whitespace). If you mix plain text with a block in the same field, the engine still detects that a block is present somewhere inside the string, but it can't actually run it — because the value as a whole no longer matches "start with `p{{{`, end with `}}}q`". The result: a console warning, and the **entire string** — code included — gets shown as literal text instead of a computed value.

```yaml
# WRONG — text and block mixed in one field; nothing gets executed,
# the whole thing (including "p{{{ ... }}}q") is shown as literal text
name: "Wynik: p{{{ return 1 + 1; }}}q"

# CORRECT — the block is the entire field value
name: |-
  p{{{
    return "Wynik: " + (1 + 1);
  }}}q
```

If you need to combine static text with a computed value, do the concatenation *inside* the block (with a template literal, see section 6), not outside it.

---

## 3. YAML formatting — one line or many?

You can write the code on a single line:

```yaml
custom_data: {start: "p{{{ return states['switch.socket']?.state !== 'unavailable'; }}}q"}
```

**But you don't have to worry about this** — if you save the card through Home Assistant's YAML editor, HA will **automatically** rewrite it into the more readable, multi-line form:

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.socket']?.state !== 'unavailable'; }}}q
```

Both forms behave identically — the difference is purely cosmetic. I'd recommend writing it in multi-line form (`|-`) from the start, since longer code on one line gets hard to read fast, and HA will convert it for you anyway on first save.

---

## 4. Where your data comes from — `states[...]`

Inside `custom_data` you have access to the `states` variable, through which you read the state of any entity:

```yaml
custom_data:
  temperature: |-
    p{{{
      const sensor = states['sensor.living_room_temp'];
      return sensor?.state;
    }}}q
```

- `states['sensor.living_room_temp']` — gives you the full entity object (state + attributes)
- `.state` — the state itself as text, e.g. `"21.5"`
- `.attributes.something` — additional data, e.g. `.attributes.humidity`

### `?.` — your best friend

Always write `?.` instead of `.` when reaching into an entity that might not exist or might be temporarily unavailable. Without it, the card throws an error if the entity disappears:

```js
// RISKY — throws if the entity doesn't exist
p{{{ return states['sensor.x'].state; }}}q

// SAFE
p{{{ return states['sensor.x']?.state; }}}q
```

### ⚠️ Very important rule: `states[...]` only works inside `custom_data`

This is a hard engine restriction, not just a style guideline. Fields outside `custom_data` (`name`, `icon`, `background_image_on`, `show_more`, etc.) **do not have access** to `states`. If you try:

```yaml
show_more: p{{{ return states['switch.socket']?.state === 'on'; }}}q
```

...you'll get a warning in the browser console, and the field will silently fall back to empty/falsy. **All entity reads must happen inside `custom_data`**, and other fields only reference the already-computed result:

```yaml
custom_data:
  socketOn: |-
    p{{{ return states['switch.socket']?.state === 'on'; }}}q
show_more: customData.socketOn
```

Why? So you always know exactly where to look for which entities the card depends on — one place, not scattered across the whole config.

---

## 5. How to reference `custom_data` from other card fields

You have two options:

### A) Simple shorthand — `customData.key`

If the value is already computed (bool, text, number), just point at it — **no** `p{{{ }}}q` needed:

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.socket']?.state !== 'unavailable'; }}}q
show_more: customData.start
```

This also works with nesting, if `custom_data` returns a whole object:

```yaml
custom_data:
  Configuration: |-
    p{{{
      return { temperature: 21.5, color: '#22c55e' };
    }}}q
name: customData.Configuration.color
```

#### ⚠️ Only simple dotted paths are supported

The shorthand only understands plain identifiers chained with dots — `customData.key`, `customData.key.nested`, and so on. It does **not** understand array-index or bracket syntax:

```yaml
# WORKS
name: customData.Configuration.color

# DOES NOT WORK — bracket/index syntax is not recognized as a customData reference
name: customData.list[0]
name: customData['key with spaces']
```

If you need an item from an array or a key with special characters, do the extraction *inside* a `p{{{ }}}q` block instead (see option B below), e.g. `p{{{ return customData.list[0]; }}}q`.

### B) A logic block — when you need to decide something at the point of use

If the field itself (e.g. `name`) needs to make an additional decision based on data from `custom_data`:

```yaml
name: |-
  p{{{
    if (customData.Configuration.temperature > 25) return "Hot";
    return "OK";
  }}}q
```

**Note:** this `p{{{ }}}q` inside `name` **does not have access to `states`** — only to `customData`. Same rule as in section 4.

---

## 6. Writing code inside `p{{{ ... }}}q`

The code inside is plain JavaScript. You don't need to know it deeply — below is everything that covers 95% of cases.

### Variables

```js
const temperature = 21.5;   // a value that won't change within this block
let description = "OK";     // a value you might overwrite below
```

### Conditions

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

### Default values — `||`

```js
const level = Number(states['sensor.battery']?.state) || 0;   // missing/0/empty -> 0
```

### Text with embedded variables (template literals)

Instead of concatenating strings with `+`, use backticks (`` ` ``) and `${}`:

```js
const temp = 21.5;
return `Temperature: ${temp}°C`;   // -> "Temperature: 21.5°C"
```

This works **inside** your JS code — it's not the same thing as the old card-level `${}` (which is dead). Inside a template literal in your own code, `${}` works normally, because it's plain JavaScript syntax.

### Objects and arrays

```js
return {
  temperature: 21.5,
  color: '#22c55e',
  list: [1, 2, 3]
};
```

### Useful built-in functions

| Function | What it does |
|---|---|
| `parseFloat(text)` | converts text to a decimal number, e.g. `"21.5"` → `21.5` |
| `Number(text)` | similarly, converts to a number |
| `value.toFixed(1)` | rounds a number to 1 decimal place (as text) |
| `Math.min(...array)` / `Math.max(...array)` | smallest/largest value in a list |
| `text.includes("something")` | checks whether text contains a given fragment |
| `array.map(x => x * 2)` | transforms every element of a list |
| `array.filter(x => x > 0)` | keeps only elements matching a condition |
| `(condition) ? "yes" : "no"` | shorthand if/else in one line |

### Comments

Everything after `//` on a line is ignored — use it to describe what the code does:

```js
// This computes the temperature in Celsius
const temp = parseFloat(states['sensor.x']?.state) || 0;
```

---

## 7. Pasting a ready-made module

The module library (media players, greeting clocks, radio buttons with cover art, etc.) is built around one rule, on purpose:

> **Each module is a single, self-contained block.** Everything it needs — the entity ID, colors, default images, the logic, and the final result — lives inside that one block. Nothing is split across several `custom_data` keys, and nothing references another module.

This is a deliberate design choice, even though it means that if you paste two modules on the same card, both might read the same entity separately. That's fine — the small extra cost is worth it because:

- **You can copy just one block and it works**, without hunting for two or three other blocks it silently depends on.
- **You can delete or replace a module** without breaking anything else on the card.
- **You can't accidentally break a working module** by editing a "shared" block that something else also depends on.

### What a ready-made module looks like

```yaml
custom_data:
  RadioButtonCover: |-
    p{{{
      // ===== SETTINGS — this is the only part you edit =====
      const entityId = 'media_player.living_room';
      const defaultImage = '/local/speaker.png';
      // =======================================================

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

To use a module like this:

1. Copy the whole block, from `custom_data:` down to the closing `}}}q` (plus any card fields underneath it that reference it, like `background_image_on` above).
2. Only change what's between `// ===== SETTINGS ===== ` and `// =======================` — usually just the entity ID and maybe a default image path or a color.
3. Leave everything below that line alone, even if you don't understand it.

### Module block names must be unique on the same card

If you paste two modules on the same card and both happen to be named `Configuration`, the second one will silently overwrite the first — you'll lose the first module's data without any error message.

Official modules from the library each get a distinct, descriptive name (`RadioButtonCover`, `PlayerCompact`, `ClockGreeting`, and so on), specifically so this can't happen when you combine several of them. If you ever write your own module to share, give it a specific name too — avoid generic ones like `Configuration`, `Logic`, or `Data`.

---

## 8. Advanced: sharing one entity read across several fields

*Skip this section if you're only pasting ready-made modules from the library — it's for when you're writing several custom fields yourself and want to avoid reading the same entity more than once.*

If a card has more than one field depending on the same entity, you can read it once, in one block, and pass the result down to the others instead of repeating `states[...]` in each one:

```yaml
custom_data:

  # 1. This is the only place reading states[...]
  Configuration: |-
    p{{{
      // === VARIABLES (change these when copying the card) ===
      const entityId = 'sensor.living_room_temp';

      // === ENTITIES ===
      const sensor = states[entityId];

      // === LOGIC ===
      const temp = parseFloat(sensor?.state) || 0;
      const color = temp > 25 ? '#fb923c' : '#22c55e';

      return { temp, color };
    }}}q

  # 2. Presentation module — consumes Configuration, doesn't read states
  descriptionHtml: |-
    p{{{
      const d = customData.Configuration;
      return `<span style="color:${d.color};">${d.temp.toFixed(1)}°C</span>`;
    }}}q

name: customData.descriptionHtml
```

**Rule:** if a block only passes a single value along without doing any real work (`return customData.Configuration.temp;` and nothing else) — don't create it. Reference it directly instead: `customData.Configuration.temp`.

This pattern is a good idea for a card you're building and maintaining yourself. It's a bad idea to publish as a copy-paste module for other users, for the reasons explained in section 7.

### ⚠️ Keys are resolved top-to-bottom — order in the YAML matters

`custom_data` keys are evaluated **in the order they appear in the YAML file**, one after another. Each key can only see the keys that were **already resolved before it** — i.e. the ones written *above* it. If a key references another key that is defined *below* it, that reference will resolve to `undefined`, because the referenced key hasn't been computed yet at that point.

```yaml
# WRONG — descriptionHtml is defined BEFORE Configuration, so
# customData.Configuration is still undefined when descriptionHtml runs
custom_data:
  descriptionHtml: |-
    p{{{
      const d = customData.Configuration;   # undefined here!
      return `${d.temp}°C`;
    }}}q
  Configuration: |-
    p{{{
      return { temp: 21.5 };
    }}}q

# CORRECT — Configuration is defined first, so descriptionHtml can use it
custom_data:
  Configuration: |-
    p{{{
      return { temp: 21.5 };
    }}}q
  descriptionHtml: |-
    p{{{
      const d = customData.Configuration;   # works — already resolved
      return `${d.temp}°C`;
    }}}q
```

**Rule of thumb:** always put the block that *reads an entity* above the block(s) that *consume its result*.

---

## 9. Most common mistakes

| Mistake | What happens | Fix |
|---|---|---|
| Missing `return` in the block | Field ends up empty | Always end the block with `return ...;` |
| `states[...]` outside `custom_data` | Console warning, field falls back to empty/falsy | Move the read into `custom_data` |
| No `?.` when reading an entity | Card crashes if the entity disappears | Always `states['x']?.state` |
| Old `${...}` syntax | Treated as literal text, nothing is computed | Use `p{{{ ... }}}q` |
| Mixing plain text with a `p{{{ }}}q` block in the same field | The block isn't recognized as executable — the entire string, code included, is shown as literal text | The block must be the field's *only* content; build any surrounding text inside the block with a template literal |
| `customData.list[0]` or `customData['key with spaces']` used as a shorthand reference | Not recognized as a `customData` reference — treated as plain text | Do the array/bracket access inside a `p{{{ }}}q` block instead |
| A `custom_data` key referencing another key defined *below* it in the YAML | The reference resolves to `undefined` | Reorder so the key being read comes first in the YAML |
| A separate block that only forwards one value | Unnecessary clutter in the config | Reference it directly, e.g. `customData.Configuration.field` |
| Pasting only part of a multi-block module (e.g. `Logic` but not `Configuration`) | `customData.Configuration is undefined` error, module breaks | Prefer single-block modules (section 7); if using a multi-block one, copy every block it depends on |
| Two pasted modules using the same block name (e.g. both `Configuration`) | The second silently overwrites the first — no error, just missing data | Give each module's block a distinct, descriptive name |

---

## 10. Ready-to-copy examples

**Simple boolean condition (toggle something on the card based on an entity's state):**

```yaml
custom_data:
  start: |-
    p{{{ return states['switch.socket']?.state === 'on'; }}}q
show_more: customData.start
```

**Text with a numeric value and unit:**

```yaml
custom_data:
  info: |-
    p{{{
      const val = parseFloat(states['sensor.power']?.state) || 0;
      return `Draw: ${val.toFixed(0)} W`;
    }}}q
name: customData.info
```

**Threshold-based color:**

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

If you get stuck — check your browser's console (F12 → Console). The card's engine always logs a warning describing exactly what went wrong, instead of silently breaking the whole card.

> 🔙 Back to the [Main README](../README.md)
