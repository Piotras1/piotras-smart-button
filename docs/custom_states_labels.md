# `custom_states_labels` — matching rules

> 🔙 Back to the [Main README](../README.md)

`custom_states_labels` lets you provide custom text labels shown on the card, depending on the entity's current state. Three key types are supported:

| Key type | Example | Meaning |
|---|---|---|
| Exact match | `"25": OK` | state equal to exactly `25` |
| Comparison operator | `">25": Warm` | state greater/less than the given number |
| Range | `"10-25": Normal` | state within the closed interval `[10, 25]` |

Allowed operators: `>`, `<`, `>=`, `<=`.

## ⚠️ MOST IMPORTANT RULE: entry order matters

The card checks keys **in the order they are written in YAML, top to bottom**, and returns the label for the **first match it finds**. It does not look for the "best" or "most specific" condition — it takes the first one in line that is true.

Exception: an **exact** match (e.g. `"25"`) is checked first, regardless of its position in the file — only when no exact match is found does the card move on to checking operators/ranges in order.

ℹ️ Exact (text) matching is **case-insensitive** — the key `open` will match the states `open`, `Open`, and `OPEN` equally. This applies everywhere the card does text matching: entity state, `hvac_action`, `preset_mode`, etc.

### `>` and `>=` operators → write thresholds in descending order (highest to lowest)

```yaml
custom_states_labels:
  ">30": Hot       # checked first
  ">20": Warm      # checked second
  ">10": Cool      # checked third
```

❌ Wrong (ascending) — `>10` will catch everything from 10 upward, before the card ever reaches `>20` or `>30`:
```yaml
custom_states_labels:
  ">10": Cool      # this catches EVERYTHING above 10, including 25 and 35!
  ">20": Warm      # never checked for values > 20
  ">30": Hot       # never checked for values > 30
```

### `<` and `<=` operators → write thresholds in ascending order (lowest to highest)

```yaml
custom_states_labels:
  "<10": Cold      # checked first
  "<20": Cool      # checked second
  "<30": Warm      # checked third
```

❌ Wrong (descending) — `<30` will catch everything below 30, including 5 and 15.

### Ranges (`min-max`) → avoid overlapping boundaries

If two ranges touch at the same number (e.g. `17-18` and `18-19`), the boundary value (here: `18`) will always fall into the **earlier range** in the file, because it's checked first.

❌ Boundaries overlap (18 will always give "Cold", not "Cool"):
```yaml
custom_states_labels:
  "17-18": Cold
  "18-19": Cool
```

✅ Boundaries don't overlap — every value lands where you'd expect:
```yaml
custom_states_labels:
  "17-17.9": Cold
  "18-18.9": Cool
```

## Matching for any entity (input_select, text sensor, lock, switch, etc.)

`custom_states_labels` isn't limited to `light`/`fan`/`cover`/`climate`/`weather`/`on.clock` — those are just the domains with **special** handling (percentages, the value2 second line, the hour). For **every other** entity (e.g. `input_select`, a `sensor` with text states, `lock`, `switch`, `person`, `media_player`, `vacuum`...) `custom_states_labels` acts as the **main label** (the same one that, by default, shows `ON`/`OFF`/`PLAY`/`STOP`, etc.) — the card checks for an exact match against the entity's raw state **always as the first step**, before any of that domain's default logic.

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

ℹ️ `custom_states_on` shown in the example above is a separate option (it controls the icon/"active" highlight, not the label text) — covered in detail in the **`custom_states_on and custom_blockade`** document.

In this example, `custom_states_labels` simply swaps the label text depending on the exact `input_select` state (`closed`/`open`/`opening`/`closing`) — and, as you can see, the label content can include HTML (`<b>`, `<br>`, emoji), exactly as described in the "Full HTML in labels" section above.

The same mechanism works identically for `alarm_control_panel` — the entity has its own state names (`disarmed`, `armed_home`, `arming`, `armed_away`, `armed_night`, `triggered`...), so you can simply give them any custom, themed labels:

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

The "active" visual state (highlight, `icon_on`) for an alarm is set automatically based on the `armed_*`/`pending`/`arming`/`triggered` states — no need to add `custom_states_on`, though you can still override it if you want different logic.

## `light` entities — brightness-dependent labels

A `light` entity's state is always just `on` or `off` — to distinguish brightness levels (e.g. "Dim", "Dark", "Bright", "Max"), the card additionally checks the `brightness` attribute (converted to %) if matching against the `on`/`off` state alone returns no label.

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

Check order for `light`:
1. **Exact** match on state (`off`, `on` — if provided as a key) — has absolute priority.
2. If not found and the light is `on` with a `brightness` attribute, convert to % and match against `custom_states_labels` ranges/operators (same ordering rules as above: `>` descending, `<` ascending, non-overlapping ranges).
3. If nothing matches — default `DIM`/`ON`/`OFF`.

⚠️ If you provide an `on: something` key, it will **always win** over the brightness percentage match (exact match has the highest priority) — to distinguish brightness levels, don't add an `on` key, only numeric ranges/operators + `off`.

## Percentage-based domains: `light`, `fan`, `cover`

The same mechanism (match on state, and if not found — on the percentage value) works for three domains:

| Domain | Source attribute | % conversion |
|---|---|---|
| `light` | `brightness` (0–255) | `brightness / 255 * 100` |
| `fan` | `percentage` (0–100) | no conversion |
| `cover` | `current_position` (0–100) | no conversion |

Fan example:
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

The check order is identical to `light` described above (exact match on state takes priority, then the percentage value, same descending/ascending rules for operators).

## `climate` — second value line (value2) from custom_states_labels

For `climate` entities, the main value (`state-badge`) **always shows the current temperature** (`current_temperature`) — this doesn't change. However, if you provide `custom_states_labels`, an **additional, smaller line** with the matched label appears below the temperature.

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

How it works:
- The matching checks three sources in order and returns the label from the **first one that has a match** in `custom_states_labels`:
  1. `hvac_action` — what the device is actually doing right now (`heating`/`cooling`/`idle`/`drying`/`fan`/`off`) — highest priority, since it's the most accurate info about the device's state.
  2. `state` — the current HVAC mode, one of `hvac_modes` (`heat`/`cool`/`auto`/`off`) — fallback when `hvac_action` is unavailable or has no match.
  3. `preset_mode` — the currently selected mode from `preset_modes` (e.g. `none`/`away`/`comfort`/`eco`/`home`/`sleep`/`activity`) — final fallback, when neither `hvac_action` nor `state` match.
- If none of the three sources match (e.g. you only defined `heating`/`cooling`, but the device is in `drying` state) — the second line simply **doesn't show**, and the temperature is displayed normally.
- If you don't provide `custom_states_labels` at all — nothing changes, you just get the temperature as before.
- The default font size for the second line is `state_size - 5` (px), with a minimum of 8px. You can override it with the `value2_size` option:

```yaml
custom_states_labels:
  eco: Eco
value2_size: 10
```

⚠️ **For `climate`, matching is exact (text-based) only — not numeric.** The card compares `hvac_action`/`state`/`preset_mode` values as strings against the keys in `custom_states_labels`. Ranges (`"10-25"`) and operators (`">25"`, `"<10"`) **do not work** for `climate` — which makes sense, since these three sources are state/mode names (`heating`, `eco`, `comfort`...), not numbers. If you want to react to temperature, do it on a `sensor` entity with the current temperature, not on the `climate` entity itself.

Another example — a thermostat with the three matching layers, and an empty label for `off` (so the second line simply doesn't show when heating is off):

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

You can add as many states as needed — a thermostat has its own `hvac_modes` and `preset_modes`, so the key list depends on the specific device (e.g. `away`, `home`, `sleep`, `boost`, `manual`, etc.). The check mechanism stays the same — three "loops" in order:
1. `hvac_action` — if no match is found in `custom_states_labels`, move to (2).
2. `state` (i.e. `hvac_modes`) — if no match, move to (3).
3. `preset_mode` — last chance for a match.

An empty string (`""`) as a key's value is fully valid and causes the second line to simply not appear on the card for that particular state (as if there were no match), even though technically a match was already found in the first loop.

## `weather` — second value line (value2) based on temperature

Works via the same mechanism as `climate`, except the match is done against the `weather` entity's `temperature` attribute. The main value (the translated weather state, e.g. "Sunny") stays unchanged — only a second, smaller line is added below it.

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

If you don't provide `custom_states_labels` — nothing changes, you just get the translated weather state as before. If the temperature has no match in any range — the second line simply doesn't show.

## Clock mode (`entity: on.clock`) — time-based greeting

`on.clock` is not a real Home Assistant entity — it's a clock built into the card, running on browser-side time (updated every second). If you provide `custom_states_labels` with hour ranges, a greeting matching the current hour appears below the clock digits — and **updates live**, without reloading the card.

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

The match is done against the current hour (`0–23`, minutes ignored), with the same rules as everywhere else (entry order, midnight-wrapping ranges like `"22-0"`, auto text wrap, `<br>`). If no range matches the current hour — the greeting simply doesn't show. If you don't provide `custom_states_labels` — the clock works exactly as before, no changes.

## Full example — temperature card

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

## Line breaks — automatic and manual

Label text **wraps automatically** if it doesn't fit the card's width — no extra steps needed. If you want to **force** a line break at a specific point (e.g. to avoid an awkward wrap), use `<br>` in the label content:

```yaml
custom_states_labels:
  "22 - 26": "Warm! T-shirt and shorts <br> light dress."
```

`<br>` works because the label is inserted into the card as real HTML, not plain text. The `\n` character (a newline in YAML) **won't work** — the browser ignores it.

## Full HTML in labels ✅ (confirmed working)

A label in `custom_states_labels` doesn't have to be plain text — it can contain **any HTML tag with inline styles** (`<span style="...">`, `<br>`, emoji, nested elements, etc.), because the card inserts it directly as HTML, not as plain text. This lets you, for example, build a colored "badge" for the main label and separately style a second line with a description — exactly as in this real example from a weather card:

> Result: a red **"Hot! 🔥"** badge, a light gray caption below it reading "Light and breezy, sunglasses and baseball cap.", and the temperature (28.5°C) shown separately, as usual, at the bottom of the card.

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

A few practical notes:

- **Quotes inside `style`** — use single quotes (`style='...'`) inside the HTML so they don't collide with the double quotes wrapping the YAML key (e.g. `"< -5"`). Mixing `"` in the key and `"` in the style breaks YAML parsing.
- Multi-line HTML in YAML is conveniently written as a folded block scalar with `>-` — it breaks lines within the file, but folds them into a single string before sending it to the card. `<br>` inside the content still works normally, since it's an explicit HTML tag, independent of line breaks in the YAML itself.
- This works identically for every entity type described above (`climate`, `weather`, `light`/`fan`/`cover`, `on.clock`, exact match/operators/ranges) — HTML is just the label's content, it doesn't affect the matching logic.
- As always: `\n` won't work as a line break — only `<br>` does.

## Midnight-wrapping ranges (e.g. hours 22-0)

If a range's lower bound is **greater** than its upper bound (e.g. `"22-0"`), the card automatically interprets it as a range that wraps around a boundary point (e.g. midnight) — the match works as "value ≥ lower **OR** value ≤ upper", rather than a standard "in between".

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

With a sensor like `{{ now().strftime('%H:%M') }}`, the match is done against the **hour** (minutes are ignored when parsing the number) — so `23:45` and `23:05` give the same result.

## Important notes for writing YAML

- Keys starting with `>` **must** be quoted (`">27"`), because `>` is a reserved YAML character (folded block scalar indicator), and without quotes the configuration won't load correctly.
- Keys starting with `<` **may** be unquoted (`<17`), but for readability and consistency it's better to quote all operator keys: `"<17"`, `">27"`, `">=30"`, `"<=5"`.
- Text keys (e.g. for `vacuum`, `media_player`: `playing`, `paused`, `off`) don't require quotes.

> 🔙 Back to the [Main README](../README.md)
