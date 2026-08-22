# ⚙️ Configuration Reference — Piotras Smart Button (v2.0.0)

Full list of all configuration options available for `custom:piotras-smart-button`.

> 🔙 Back to the [Main README](../README.md) · See also the [Changelog](./CHANGELOG.md)

---

### Card Size & Shape

| Option | Type | Default | Description |
|---|---|---|---|
| `card_width` | number / string | `140` | Card width (px) or `"auto"` |
| `card_height` | number / string | `120` | Card height (px) or `"auto"` |
| `border_radius` | number | `12` | Corner radius (px) |
| `border_width` | number | `0` | Border thickness (px) |
| `border_color` | string | `rgba(255,255,255,0.2)` | Border color |
| `box_shadow` | string | `null` | CSS box-shadow — e.g. `0px 0px 15px 0px rgba(0,0,0,0.5)` |

### Entity & Labels

| Option | Type | Default | Description |
|---|---|---|---|
| `entity` | string | — | Main entity ID |
| `name` | string | — | Display name |
| `icon` | string | auto | MDI icon **or an image** — auto-detected for `battery`, `temperature`, `humidity`. Accepts an image path/URL (`.svg .png .jpg .jpeg .gif .webp`, or `http(s)://`, `/local/`, `/hacsfiles/`, `/api/image/`, `data:image/`) instead of an mdi icon — rendered as `<img>` automatically. |
| `icon_on` | string | `null` | Alternative icon (or image) when entity is ON |
| `name_on` | string | `null` | Custom state label when ON |
| `name_off` | string | `null` | Custom state label when OFF |

### Visibility

| Option | Type | Default | Description |
|---|---|---|---|
| `show_icon` | boolean | `true` | Show / hide icon |
| `show_icon_full` | boolean | `true` | Keep icon inside card bounds |
| `show_name` | boolean | `true` | Show / hide name label |
| `show_state` | boolean | `true` | Show / hide state badge |
| `show_more` | boolean | `false` | Enable Control Zone (slider / bar / person time / thermostat buttons) |
| `visible_if` | boolean / string | `null` | 🆕 Hide or show the **whole card**. Accepts a boolean, a `customData.xxx` reference, or a `p{{{ }}}q` block resolving to truthy/falsy. Card is fully hidden (`display:none`) when falsy. |

### Colors & Icon

| Option | Type | Default | Description |
|---|---|---|---|
| `icon_color` | string | `#f0c040` | Icon color when OFF · away (person) · ring tint |
| `icon_color_on` | string | `#ffffff` | Icon color when ON · home (person) · ring tint |
| `icon_size` | number | `28` | Icon size (px) |
| `icon_wrap_size` | number | `48` | Glow ring diameter (px) |
| `icon_over_size` | number | `4` | Corner overlap divisor when `show_icon_full: false` |
| `icon_style` | string | `circle_color` | Icon style: `circle_color`, `circle`, `square_color`, `square`, `none` |
| `icon_margin` | number / string | `null` | 🆕 Fine-tune icon offset independently from the default padding. A single number applies to all sides; `"left,right,top,bottom"` sets each side individually. |
| `text_color` | string | `#ffffff` | Color for Name and State labels |
| `value_color` | string | `null` | Optional override color for the State badge only — falls back to `text_color` when not set |
| `name_size` | number | `14` | Name font size (px) — also the base size used to scale Clock Display tiles/segments/analog face when `entity: on.clock` |
| `state_size` | number | `12` | State badge font size (px) |
| `font_style` | number | `1` | `1` normal · `2` small-caps · `3` monospace · `4` uppercase + letter-spacing |

### Background

| Option | Type | Default | Description |
|---|---|---|---|
| `background_color1` | string | `#1a1a2e` | Primary background color |
| `background_color2` | string | `""` | Second gradient color |
| `background_color3` | string | `""` | Third gradient color |
| `background_gradient_angle` | number | `135` | Gradient angle (degrees) |
| `show_shadow` | boolean | `true` | Enable the inner dark overlay gradient on the card background |

### Background Images & Filters

| Option | Type | Default | Description |
|---|---|---|---|
| `show_image` | boolean | `false` | Enable background image |
| `background_image_on` | string | `null` | Image path for ON state |
| `background_image_off` | string | `null` | Image path for OFF state |
| `show_filter` | boolean | `false` | Enable CSS filters on background |
| `image_filter_on` | string | `brightness(1) saturate(1.1)` | CSS filter when ON |
| `image_filter_off` | string | `brightness(0.45) saturate(0.2) grayscale(0.5)` | CSS filter when OFF |

### Layout (9-Grid)

| Option | Type | Default | Description |
|---|---|---|---|
| `icon_mode` | number | `5` | Icon position (1–9) |
| `name_mode` | number | `5` | Name position (1–9) |
| `value_mode` | number | `5` | State badge position (1–9) |

### Control Zone (Slider & Power)

| Option | Type | Default | Description |
|---|---|---|---|
| `show_more` | boolean | `false` | Enable Control Zone |
| `slider_height` | number | `26` | Control Zone height (px) — content scales proportionally via `√(height/26)` |
| `slider_label_color` | string | `rgba(255,255,255,0.85)` | Color for all labels and icons in the Control Zone |

Auto-detected sliders: `brightness`, `color_temp`, `volume_level`, `current_position` (cover), `percentage` (fan), `preset_mode` (fan).

### 🆕 Media Player Controls

| Option | Type | Default | Description |
|---|---|---|---|
| `show_player` | boolean | `false` | Enable playback controls for `media_player` entities — prev / play-pause / next / power. Shows only a power button when the player is `off`/`unavailable`. |
| `player_mode` | number | `1` | `1` buttons only · `2` buttons + volume slider, slider first · `3` buttons + volume slider, volume first. When combined (`2`/`3`), the volume slider replaces the standalone Control Zone slider for this card. |
| `player_height` | number | `28` | Scales button size and padding proportionally (`player_height / 28`) |

### Temperature & Humidity Comfort

| Option | Type | Default | Description |
|---|---|---|---|
| `comfort_min` | number | `null` | Lower bound of comfort range — activates comfort mode when set together with `comfort_max` |
| `comfort_max` | number | `null` | Upper bound of comfort range — activates comfort mode when set together with `comfort_min` |

> Comfort mode is active only when **both** `comfort_min` and `comfort_max` are set. Applies to `sensor` entities with `device_class: temperature` or `device_class: humidity`. Temperature sensors reporting in °F are automatically converted to °C before comparison.

### Battery

| Option | Type | Default | Description |
|---|---|---|---|
| `entity_battery_state` | string | `null` | Optional sensor reporting `charging` / `discharging` / `full` / `not_charging` — enables ON/OFF card state based on charging. Dynamic icon works without this option. |

### Power Bar

| Option | Type | Default | Description |
|---|---|---|---|
| `entity_watts` | string | `null` | Power sensor entity ID |
| `max_watts` | number | `2000` | Maximum scale (W) |
| `con_warning` | number / boolean | `80` | Pulse warning threshold (%) — `false` to disable |

### Vacuum / Custom States

| Option | Type | Default | Description |
|---|---|---|---|
| `vacuum_states_on` | array | `null` | *(legacy)* List of raw states/attributes that should turn the card ON for `vacuum` or sensor-based vacuum entities |
| `vacuum_states_labels` | object | `null` | *(legacy)* Map of raw state → custom label override |
| `custom_states_on` | array | `null` | Generalized version of `vacuum_states_on` — works with any entity domain, not just vacuum. Takes priority over `vacuum_states_on` if both are set. 🆕 Entries now also accept numeric comparisons (`">=20"`, `"<=5"`, `">10"`, `"<3"`) and ranges (`"10-20"`), matched against the entity's numeric state — in addition to plain exact-match strings. |
| `custom_states_labels` | object | `null` | Generalized version of `vacuum_states_labels`. Labels render exactly as written (no forced uppercase). Takes priority over `vacuum_states_labels` if both are set. |

### 🆕 Clock Display *(only when `entity: on.clock`)*

| Option | Type | Default | Description |
|---|---|---|---|
| `clock_mode` | number | `1` | Clock style: `1` digits *(default, same look as v1.2.5)* · `2` flip tiles · `3` analog · `4` LED (7-segment) |
| `clock_tile_bg` | string | `null` | Background color for the tile/LED box (`clock_mode: 2` / `4`) or the analog clock face (`clock_mode: 3`). Accepts any CSS color (`#fff`, `rgba(...)`) and supports templates via `customData`. Falls back to a sensible translucent default per style when unset. |

> All four clock styles render their DOM once and update only the changing parts on each tick (digit text, hand rotation, LED segment fill) — no full re-render every second, so there's no animation jank even with many cards on one dashboard. The analog second hand runs on a continuous CSS animation synced to the real time at render, rather than being recalculated on each tick.
>
> Exposed in the visual editor under **General → Clock display** — a style dropdown and a color picker, shown only when the card's entity is `on.clock`.

### 🆕 Templated Keys & Custom Data

Most visual/behavioral options above can be set to a **static value**, a **reference to `custom_data`** (`customData.someKey` or a nested path like `customData.Group.value`), or a **`p{{{ ... }}}q` block** — a small piece of JavaScript that returns a value, re-evaluated whenever the card's state updates.

```yaml
custom_data:
  isNight: |-
    p{{{
      const h = new Date().getHours();
      return h >= 22 || h < 6;
    }}}q
text_color: customData.isNight ? "#cfd6ff" : "#2f2f2f"   # example only — see note below
```

> `p{{{ }}}q` blocks have access to `states` (all HA entity states), `user` (current HA user), and `customData` (previously resolved `custom_data` values). Blocks assigned directly to `custom_data.*` keys run in **unrestricted mode**; when referenced elsewhere via `customData.xxx`, only the *already-resolved* value is used (not re-evaluated), so keep the actual logic inside `custom_data`.

The following keys accept templates / `customData` references:

`show_more`, `show_player`, `show_state`, `show_name`, `show_icon`, `background_image_on`, `background_image_off`, `icon`, `icon_on`, `icon_off`, `icon_color`, `icon_color_on`, `name`, `name_on`, `name_off`, `background_color1`, `background_color2`, `background_color3`, `background_gradient`, `background_gradient_angle`, `max_watts`, `text_color`, `value_color`, `name_size`, `state_size`, `font_style`, `border_width`, `border_color`, `border_radius`, `box_shadow`, `image_filter_on`, `image_filter_off`, `icon_size`, `icon_wrap_size`, `icon_over_size`, `icon_style`, `card_width`, `card_height`, `slider_height`, `slider_label_color`, `con_warning`, `entity_watts`, `comfort_min`, `comfort_max`, `time_service`, `service_style`, `clock_tile_bg`

`custom_states_labels` and `vacuum_states_labels` also support per-value templates/references inside their maps. `visible_if` supports templates independently (see **Visibility** above).

> Templating a field that's normally static (e.g. `border_color`, `text_color`) causes the card to do a full re-render on every state update instead of a lightweight partial update — fine for most dashboards, but worth knowing if you're chasing performance with many cards.

### Interaction Lock

| Option | Type | Default | Description |
|---|---|---|---|
| `custom_blockade` | array | `null` | List of entity states during which the card ignores all interaction (tap, hold, double-tap) — both the action and the pressed/scale visual feedback are disabled |

### Service Countdown

| Option | Type | Default | Description |
|---|---|---|---|
| `show_service` | boolean | `false` | Enable countdown after `call-service` |
| `time_service` | number / string | `10` | Seconds, or `"remaining/scale"` e.g. `"10/20"` |
| `service_style` | string | `"circle"` | `"circle"` SVG ring · `"bar"` bottom bar |
| `blockade_card` | boolean | `false` | Block re-triggering the service call while countdown is active |

### Actions

| Option | Type | Default | Description |
|---|---|---|---|
| `tap_action` | object | `{action: "toggle"}` | Single tap |
| `double_tap_action` | object | `{action: "none"}` | Double tap |
| `hold_action` | object | `{action: "none"}` | Long press |

Supported actions: `toggle`, `more-info`, `navigate`, `call-service`, `none`.

> If `tap_action`, `hold_action`, and `double_tap_action` are all set to `none` (or left at their defaults of `none` for hold/double-tap), the card also disables the pressed/scale visual feedback — it stops looking clickable.

> **Advanced / YAML-only options:** `custom_blockade`, `custom_states_on`, `custom_states_labels`, `custom_data`, and `visible_if` are intentionally not exposed in the visual editor (except `clock_mode` / `clock_tile_bg`, which do have GUI controls). Using them effectively requires knowing your entity's exact state values (check **Developer Tools → States** in Home Assistant) — these options are aimed at advanced users who are already comfortable editing YAML directly.

> 🔙 Back to the [Main README](../README.md) · See also the [Changelog](./CHANGELOG.md)
