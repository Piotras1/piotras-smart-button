# Piotras Smart Button Card
### Release v1.0.0

**Most cards go silent after calling a service. This one tells you it worked.**

A Home Assistant button card with 9-grid layout · auto-detecting sliders · dynamic backgrounds · service countdown · visual editor included.

---

![GitHub release](https://img.shields.io/github/v/release/piotras/smart-button-card?style=flat-square)
![HACS](https://img.shields.io/badge/HACS-Custom-orange?style=flat-square)
![HA min version](https://img.shields.io/badge/HA-2023.x%2B-blue?style=flat-square)

---

<!--
  ZDJĘCIE 1: Kilka kart obok siebie — różne kolory, ikony, stany ON/OFF.
  Pokazuje różnorodność wyglądu. Najlepiej 4–6 kart w rzędzie.
-->
<!-- ![Preview](https://github.com/user-attachments/assets/REPLACE_ME) -->

---

## ✨ Features

- **Visual Editor** — full GUI with tabs: General, Size, Background, Icon, Text, Layout, Slider & Power, Filters, Actions, Service. No manual YAML needed.
- **9-Grid Layout System** — place Icon, Name, and State badge independently across a 3×3 matrix.
- **Adaptive Auto-Sliders** — smart detection of Brightness, Color Temp, Volume, Cover Position, Fan Speed. Zero configuration required.
- **Dynamic Backgrounds** — solid color, 2/3-color gradients, or full-resolution background images.
- **Smart Filter Engine** — CSS filters (brightness, saturation, grayscale) applied automatically per ON/OFF state.
- **Service Countdown** — animated SVG circle or progress bar after `call-service`, with optional card blockade.
- **Power Monitoring** — real-time consumption bar with configurable pulse warning threshold.
- **Font Styles** — 4 text presets for Name and State labels.
- **Advanced Action Engine** — Tap, Double-Tap, and Hold, optimized for both mobile and desktop.

---

## 📦 Installation

### Manual

1. Download `piotras-smart-button-card.js` and `smart-button-card-editor.js`.
2. Copy both files to `/config/www/` in your Home Assistant instance.
3. Add the resource in **Settings → Dashboards → Resources**:

```yaml
url: /local/piotras-smart-button-card.js
type: module
```

4. Reload the browser. Add the card via the UI or YAML:

```yaml
type: custom:smart-button-card
entity: light.living_room
name: Living Room
icon: mdi:lightbulb
```

### HACS

> Add this repository as a custom repository in HACS → Frontend.

---

## 🧩 The 9-Grid Layout System

Position Icon, Name, and State independently using a 3×3 grid (positions 1–9):

```
┌───────┬───────┬───────┐
│ 1 ↖   │  2 ↑  │ 3 ↗   │
├───────┼───────┼───────┤
│ 4 ←   │  5 ·  │ 6 →   │   ← default center
├───────┼───────┼───────┤
│ 7 ↙   │  8 ↓  │ 9 ↘   │
└───────┴───────┴───────┘
```

Elements sharing the same position are stacked vertically: `icon → name → state`.  
When `show_more: true` is active, elements in the bottom row shift up automatically to avoid the Control Zone.

<!--
  ZDJĘCIE 2: Schemat siatki 3×3 lub zrzut z edytora (zakładka Layout z widocznymi gridami).
-->
<!-- ![Layout Grid](https://github.com/user-attachments/assets/REPLACE_ME) -->

```yaml
# Classic: icon top-left, name + state bottom-center
icon_mode: 1
name_mode: 8
value_mode: 8

# Centered stack (default)
icon_mode: 5
name_mode: 5
value_mode: 5
```

---

## ⚡ Service Countdown

When any action is set to `call-service` and `show_service: true` is enabled, the card displays an animated countdown for the duration set by `time_service`.

Two display styles:

| `service_style` | Description |
|---|---|
| `circle` *(default)* | Animated SVG ring centered on the card in `icon_color_on`. Available when `entity` is empty and `show_more: false`. Size follows `icon_wrap_size`. |
| `bar` | Classic progress bar docked at the bottom of the card. |

`time_service` supports two formats:
- `10` — countdown from 10 s, bar scale = 10 s
- `"10/20"` — countdown from 10 s, visual scale is 20 s (bar starts at 50%)

<!--
  ZDJĘCIE 3: Karta bez entity z aktywnym kółkiem SVG na środku (tryb circle).
  Najlepiej GIF pokazujący animację odliczania.
-->
<!-- ![Circle Countdown](https://github.com/user-attachments/assets/REPLACE_ME) -->

```yaml
type: custom:smart-button-card
name: Boiler
icon: mdi:water-boiler
icon_color_on: "#ff6b35"
show_service: true
time_service: "30/60"
service_style: circle
blockade_card: true
tap_action:
  action: call-service
  service: switch.turn_on
  service_data:
    entity_id: switch.boiler
```

---

## 🔌 Power & Safety Monitor

Real-time power consumption bar for smart plugs and sockets.

- **Dynamic Fill** — bar fills proportionally to `max_watts`.
- **Pulse Warning** — bar pulses when consumption exceeds `con_warning` (%). Set to `false` to disable.

<!--
  ZDJĘCIE 4: Karta z paskiem mocy wypełnionym wysoko + widoczne miganie ostrzeżenia.
-->
<!-- ![Power Bar](https://github.com/user-attachments/assets/REPLACE_ME) -->

```yaml
type: custom:smart-button-card
entity: switch.coffee_maker
name: Coffee Machine
icon: mdi:coffee
entity_watts: sensor.coffee_machine_power
max_watts: 2500
con_warning: 85
show_more: true
```

---

## ⚙️ Configuration Reference

### Card Size & Shape

| Option | Type | Default | Description |
|---|---|---|---|
| `card_width` | number | `140` | Card width (px) |
| `card_height` | number / string | `120` | Card height (px) or `"auto"` |
| `border_radius` | number | `12` | Corner radius (px) |
| `border_width` | number | `0` | Border thickness (px) |
| `border_color` | string | `rgba(255,255,255,0.2)` | Border color |

### Entity & Labels

| Option | Type | Default | Description |
|---|---|---|---|
| `entity` | string | — | Main entity ID |
| `name` | string | — | Display name |
| `icon` | string | `mdi:lightning-bolt` | MDI icon |
| `name_on` | string | `null` | Custom state label when ON |
| `name_off` | string | `null` | Custom state label when OFF |

### Visibility

| Option | Type | Default | Description |
|---|---|---|---|
| `show_icon` | boolean | `true` | Show / hide icon |
| `show_icon_full` | boolean | `true` | Keep icon inside card bounds |
| `show_name` | boolean | `true` | Show / hide name label |
| `show_state` | boolean | `true` | Show / hide state badge |
| `show_more` | boolean | `false` | Enable Slider / Power Bar zone |

### Colors & Icon

| Option | Type | Default | Description |
|---|---|---|---|
| `icon_color` | string | `#f0c040` | Icon color when OFF (also tints wrap ring) |
| `icon_color_on` | string | `#ffffff` | Icon color when ON (also tints wrap ring) |
| `icon_size` | number | `28` | Icon size (px) |
| `icon_wrap_size` | number | `48` | Glow ring diameter (px) |
| `icon_over_size` | number | `4` | Corner overlap divisor when `show_icon_full: false` |
| `text_color` | string | `#ffffff` | Color for Name and State labels |
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

### Sliders & Control Zone

| Option | Type | Default | Description |
|---|---|---|---|
| `show_more` | boolean | `false` | Show auto-slider or power bar zone |
| `slider_height` | number | `26` | Control Zone height (px) |

Auto-detected sliders: `brightness`, `color_temp`, `volume_level`, `current_position` (cover), `percentage` (fan), `preset_mode` (fan).

### Power Bar

| Option | Type | Default | Description |
|---|---|---|---|
| `entity_watts` | string | `null` | Power sensor entity ID |
| `max_watts` | number | `2000` | Maximum scale (W) |
| `con_warning` | number / boolean | `80` | Pulse warning threshold (%) — `false` to disable |

### Service Countdown

| Option | Type | Default | Description |
|---|---|---|---|
| `show_service` | boolean | `false` | Enable countdown after `call-service` |
| `time_service` | number / string | `10` | Seconds, or `"remaining/scale"` e.g. `"10/20"` |
| `service_style` | string | `"circle"` | `"circle"` SVG ring (requires no entity + `show_more: false`) · `"bar"` bottom bar |
| `blockade_card` | boolean | `false` | Block re-triggering while countdown is active |

### Actions

| Option | Type | Default | Description |
|---|---|---|---|
| `tap_action` | object | `{action: "toggle"}` | Single tap |
| `double_tap_action` | object | `{action: "more-info"}` | Double tap |
| `hold_action` | object | `{action: "more-info"}` | Long press |

Supported actions: `toggle`, `more-info`, `navigate`, `call-service`.

---

## 🖥 Visual Editor

The card ships with a full visual editor accessible directly in the Home Assistant dashboard UI.

<!--
  ZDJĘCIE 5: Zrzut edytora — np. zakładka Service z widocznym podglądem kółka na karcie po prawej.
-->
<!-- ![Visual Editor](https://github.com/user-attachments/assets/REPLACE_ME) -->

Tabs available: **General · Size · Background · Icon · Text · Layout · Slider & Power · Filters · Actions · Service**

---

## 📄 License

MIT — free to use, modify, and share.

---

*Created by Piotras. Strictly engineered for reliability.*
