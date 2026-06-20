# ⚙️ Configuration Reference — Piotras Smart Button

Full list of all configuration options available for `custom:piotras-smart-button`, matching the **v1.2.5** release.

> Looking for the main overview, installation steps, or entity-mode examples (light, switch, climate, vacuum, etc.)? Go back to the [main README](../README.md).

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
| `icon` | string | auto | MDI icon — auto-detected for `battery`, `temperature`, `humidity` |
| `icon_on` | string | `null` | Alternative icon when entity is ON |
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

### Colors & Icon

| Option | Type | Default | Description |
|---|---|---|---|
| `icon_color` | string | `#f0c040` | Icon color when OFF · away (person) · ring tint |
| `icon_color_on` | string | `#ffffff` | Icon color when ON · home (person) · ring tint |
| `icon_size` | number | `28` | Icon size (px) |
| `icon_wrap_size` | number | `48` | Glow ring diameter (px) |
| `icon_over_size` | number | `4` | Corner overlap divisor when `show_icon_full: false` |
| `icon_style` | string | `circle_color` | Icon style: `circle_color`, `circle`, `square_color`, `square`, `none` |
| `text_color` | string | `#ffffff` | Color for Name and State labels |
| `value_color` | string | `null` | Optional override color for the State badge only — falls back to `text_color` when not set |
| `name_size` | number | `14` | Name font size (px) |
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
| `custom_states_on` | array | `null` | Generalized version of `vacuum_states_on` — works with any entity domain, not just vacuum. Takes priority over `vacuum_states_on` if both are set. |
| `custom_states_labels` | object | `null` | Generalized version of `vacuum_states_labels`. Labels render exactly as written (no forced uppercase). Takes priority over `vacuum_states_labels` if both are set. |

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

> **Advanced / YAML-only options:** `custom_blockade`, `custom_states_on`, and `custom_states_labels` are intentionally not exposed in the visual editor. Using them effectively requires knowing your entity's exact state values (check **Developer Tools → States** in Home Assistant) — these options are aimed at advanced users who are already comfortable editing YAML directly.
