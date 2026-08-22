# Changelog — Piotras Smart Button

> 🔙 Back to the [Main README](../README.md)

---

## v2.0.0 — Templates, Media Player Controls & Clock Module v2

A major release. No breaking changes — every v1.2.5 config keeps working unmodified.

### ✨ Added

**Templating engine (`custom_data` + `p{{{ }}}q` blocks)**
Write small JS-like blocks inside `custom_data`, reference the result anywhere in the card config via `customData.xxx`. Over 45 config keys now accept either a raw value, a `customData.path` reference, or a full `p{{{ ... }}}q` block — letting colors, backgrounds, labels, sizes, and more react to time, entity states, or any logic you write, without Jinja/HA templates. See the full list under **Templated Keys** below.

**`visible_if`**
Hide or show the entire card based on a boolean, a `customData` reference, or a `p{{{ }}}q` block.

**`icon_margin`**
Fine-tune icon offset independently from the card's default padding. Accepts a single number (applied to all four sides) or a `"left,right,top,bottom"` string.

**Image icons**
`icon` / `icon_on` can now point to an image instead of an mdi icon — any `.svg .png .jpg .jpeg .gif .webp` file, or a URL starting with `http(s)://`, `/local/`, `/hacsfiles/`, `/api/image/`, or `data:image/`. Detected automatically and rendered as `<img>`.

**Range syntax for `custom_states_on` / `vacuum_states_on`**
Beyond exact state strings, entries now accept numeric comparisons and ranges, matched against the entity's numeric state:
- `">=20"`, `"<=5"`, `">10"`, `"<3"`
- `"10-20"` (inclusive range)

**Media player controls**
- `show_player` — enable prev / play-pause / next / power buttons for `media_player` entities (just a power button when the player is off).
- `player_mode` — `1` buttons only · `2` buttons + volume slider (slider first) · `3` buttons + volume slider (volume first).
- `player_height` — scales button and slider size.

**Clock module v2**
- `clock_mode` — `1` digits *(default, same as v1.2.5)* · `2` flip tiles · `3` analog · `4` LED.
- `clock_tile_bg` — background color for the tile/LED box or the analog clock face. Supports templates.
- All four styles are built once and updated in place every tick (no full re-render), so there's no animation jank. The analog second hand runs on a continuous CSS animation instead of being recalculated every second.
- Available only when `entity: on.clock`. Exposed in the visual editor under **General → Clock display** (style dropdown + color picker), shown only for `on.clock` entities.

### 🔧 Changed
- `_updateState()` is now a lightweight dispatcher that only triggers a full DOM rebuild when a templated field affecting static markup actually changed — reduces unnecessary re-renders on frequent state updates.
- `custom_states_on` / `vacuum_states_on` matching now goes through a shared helper that also powers the new range syntax above.

### 📝 Upgrade notes
- To try the new clock styles, add `clock_mode: 2` (or `3` / `4`) to any card with `entity: on.clock`.
- File size grew ~9% (new modules are string-built once, not evaluated every tick, so runtime cost is minimal even with many cards on one dashboard).

---

> 🔙 Back to the [Main README](../README.md)
