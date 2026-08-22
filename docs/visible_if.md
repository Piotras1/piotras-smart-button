# `visible_if` — showing or hiding the whole card

> 🔙 Back to the [Main README](../README.md) · See also the [Custom Data guide](./custom_data_guide.md)

`visible_if` lets you hide or show the **entire card** dynamically — the whole thing, not just a field. When the condition is falsy, the card is fully removed from layout (`display:none`); when it's truthy, it's shown normally.

Unlike most templated options on the card, `visible_if` isn't about *what* the card shows — it's about *whether* the card shows at all.

---

## 1. Accepted value types

`visible_if` accepts exactly three kinds of values:

| Type | Example | Behavior |
|---|---|---|
| Plain boolean | `visible_if: true` / `visible_if: false` | Static — the card is always visible / always hidden |
| `custom_data` reference | `visible_if: customData.showCard` | Resolved from an already-computed `custom_data` key |
| Template block | `visible_if: |- p{{{ return ...; }}}q` | Re-evaluated live every time the card's state updates |

If `visible_if` isn't set at all (or is explicitly `null`), the card behaves as if it were always `true` — fully visible, no change from how the card worked before this option existed.

```yaml
# Always hidden — useful for temporarily disabling a card without deleting it
type: custom:piotras-smart-button
entity: switch.test_socket
visible_if: false
```

```yaml
# Hide the card entirely unless the linked switch exists and isn't unavailable
type: custom:piotras-smart-button
entity: switch.garden_pump
custom_data:
  pumpAvailable: |-
    p{{{
      return states['switch.garden_pump']?.state !== 'unavailable';
    }}}q
visible_if: customData.pumpAvailable
```

---

## 2. ⚠️ `states` is not available directly inside `visible_if`

Same rule as every other field outside `custom_data` (see the [Custom Data guide](./custom_data_guide.md), section 4): a `p{{{ }}}q` block written directly as the value of `visible_if` only has access to `customData` — **not** to `states`. If your condition needs to read an entity, do the read inside `custom_data` first, and reference the result from `visible_if`.

```yaml
# WRONG — states is not available here
visible_if: |-
  p{{{ return states['binary_sensor.someone_home']?.state === 'on'; }}}q

# CORRECT — read the entity in custom_data, then reference the result
custom_data:
  someoneHome: |-
    p{{{ return states['binary_sensor.someone_home']?.state === 'on'; }}}q
visible_if: customData.someoneHome
```

You can put the whole condition inside `custom_data` and reference it with the plain `customData.key` shorthand (recommended for simple booleans), or, if you need extra logic at the point of use, write a `p{{{ }}}q` block directly in `visible_if` that only reads `customData` — never `states`.

```yaml
# A logic block directly in visible_if — allowed, as long as it only touches customData
custom_data:
  temp: |-
    p{{{ return parseFloat(states['sensor.living_room_temp']?.state) || 0; }}}q
visible_if: |-
  p{{{ return customData.temp > 18 && customData.temp < 28; }}}q
```

---

## 3. ⚠️ Quoted strings are always truthy — even `"false"`

If you give `visible_if` a value that is neither a real boolean, nor a recognized `p{{{ }}}q` template, nor a `customData.key` reference, the card falls back to plain JavaScript truthiness (`!!value`). In JavaScript, **any non-empty string is truthy** — including the string `"false"`.

```yaml
# WRONG — "false" here is a YAML STRING, not the boolean false.
# A non-empty string is truthy in JS, so the card stays VISIBLE.
visible_if: "false"

# CORRECT — an unquoted true/false in YAML is parsed as a real boolean
visible_if: false
```

**Rule of thumb:** for a static value, always write `visible_if: true` or `visible_if: false` **without quotes**. If you quote it, YAML treats it as text, and the card will almost certainly end up visible when you meant to hide it — with no warning in the console, since this isn't an error, just unintended truthiness.

---

## 4. Live re-evaluation, and a note on timing with `custom_data`

`visible_if` is recalculated every time the card receives a Home Assistant state update (i.e. on every `hass` update the card receives) — so a card can appear or disappear live on the dashboard as conditions change, without a page reload.

⚠️ **Advanced timing note:** when `visible_if` references a `custom_data` key, it's resolved using the `custom_data` values computed on the **previous** update — not the ones computed during the same update cycle that changed the underlying entity. In practice this is invisible almost all the time, because Home Assistant dashboards receive frequent state updates and the card catches up immediately on the very next one. But if you're chasing a visibility change that seems to lag by a fraction of a second (or doesn't update until *something else* on the dashboard changes), this one-tick lag is why. It's not a bug to work around in normal use — just something to be aware of if you're debugging fast-changing, timing-sensitive conditions.

---

## 5. Practical examples

**Hide a card until an entity has a valid (non-`unavailable`/`unknown`) state:**

```yaml
type: custom:piotras-smart-button
entity: sensor.outdoor_temperature
custom_data:
  hasData: |-
    p{{{
      const s = states['sensor.outdoor_temperature']?.state;
      return s !== undefined && s !== 'unavailable' && s !== 'unknown';
    }}}q
visible_if: customData.hasData
```

**Show a card only during certain hours (e.g. a "goodnight" button only visible at night):**

```yaml
type: custom:piotras-smart-button
entity: script.goodnight_routine
name: Goodnight
custom_data:
  isNight: |-
    p{{{
      const h = new Date().getHours();
      return h >= 21 || h < 6;
    }}}q
visible_if: customData.isNight
```

**Combine two entity conditions (show only when a room is occupied AND the light is off):**

```yaml
type: custom:piotras-smart-button
entity: light.office
custom_data:
  showPrompt: |-
    p{{{
      const occupied = states['binary_sensor.office_occupancy']?.state === 'on';
      const lightOff = states['light.office']?.state === 'off';
      return occupied && lightOff;
    }}}q
visible_if: customData.showPrompt
```

**Hide a card entirely for a specific user (e.g. a guest-only view where an admin card should stay hidden):**

```yaml
custom_data:
  isAdmin: |-
    p{{{
      return user?.name === 'Piotr';
    }}}q
visible_if: customData.isAdmin
```

---

## 6. Most common mistakes

| Mistake | What happens | Fix |
|---|---|---|
| `visible_if: "false"` (quoted string) | Non-empty string is truthy — card stays **visible** | Write `visible_if: false` without quotes |
| `states[...]` used directly inside a `p{{{ }}}q` block in `visible_if` | No access to `states` outside `custom_data` — condition silently fails/falls back | Read the entity inside `custom_data`, reference the result from `visible_if` |
| Expecting instant hide/show the same tick an entity changes, when going through `custom_data` | Can lag by one `hass` update cycle (see section 4) | Usually self-corrects on the very next update; not something to design around in normal use |
| Forgetting `visible_if` hides the **whole card**, not a field | Confusing it with `show_name`/`show_icon`/etc., which hide only parts of the card | Use `visible_if` only when you want the entire card gone from the dashboard |
| Setting `visible_if` to a `custom_data` key that's defined *below* it in the YAML | The `custom_data` key ordering rule still applies — resolves to `undefined`, treated as falsy | Define the referenced `custom_data` key earlier in the YAML (see the Custom Data guide, section 8) |

---

> 🔙 Back to the [Main README](../README.md) · See also the [Custom Data guide](./custom_data_guide.md)
