# Piotras Smart Button Card
### Release v1.0.0

A high-performance Home Assistant button card designed for ultimate control and deep customization.  
Features a unique 9-grid layout system, dynamic background effects, auto-detecting control sliders, and a service countdown engine.

## ✨ Features

- **9-Grid Element Placement** – independently position Icon, Name, and State across a 3×3 matrix.
- **Adaptive Auto-Sliders** – smart detection of Brightness, Color Temp, Volume, Cover Position, and Fan Speed.
- **Dynamic Backgrounds** – solid colors, 2/3-color gradients, and filtered background images.
- **Advanced Action Engine** – optimized Tap, Double-Tap, and Hold for mobile and desktop.
- **Power Monitoring** – integrated bar with configurable safety pulse warning.
- **Service Countdown** – animated circle or progress bar countdown after `call-service`, with optional card blockade.
- **Font Styles** – 4 text style presets for Name and State labels.

---

<!-- 
  ZDJĘCIA DO WSTAWIENIA:
  1. Kilka kart obok siebie w różnych konfiguracjach kolorystycznych (gradient tła, różne ikony)
     – pokaż różnorodność wyglądu: światło, gniazdko, głośnik
  2. Karta z włączonym show_more: true i widocznym sliderem jasności (światło ON)
  3. Karta bez entity z aktywnym odliczaniem w trybie "circle" – widoczne animowane kółko w środku
  4. Karta z paskiem mocy (entity_watts) i migającym ostrzeżeniem con_warning
-->

---

## 🧩 Layout: The 9-Grid System

The card uses a 3×3 grid (positions 1–9) to place each element independently.  
You can overlap elements, push them into corners, or create any custom composition.

```
┌─────────────────────┐
│  1 (TL) │ 2 (TC) │ 3 (TR) │
│─────────┼────────┼────────│
│  4 (ML) │ 5 (MC) │ 6 (MR) │  ← default: 5 (center)
│─────────┼────────┼────────│
│  7 (BL) │ 8 (BC) │ 9 (BR) │
└─────────────────────┘
```

Assign `icon_mode`, `name_mode`, and `value_mode` to any of the 9 positions.  
When `show_more: true` is active, elements in the bottom row (7/8/9) automatically shift one row up to avoid the Control Zone.

---

<!-- 
  ZDJĘCIA DO WSTAWIENIA:
  5. Schemat / diagram wizualny siatki 3×3 z numerami – może być zrzut karty z oznaczonymi pozycjami
  6. Przykład kart z różnym rozmieszczeniem elementów: np. ikona w 1, nazwa w 9 vs ikona w 5, nazwa w 8
-->

---

### Configuration Example

```yaml
type: custom:smart-button-card
entity: light.living_room
name: Salon
icon: mdi:lightbulb
card_width: 140
card_height: 140
show_more: true
icon_mode: 5
name_mode: 8
value_mode: 9
```

---

## 🎨 Visual Style: Layered Architecture

The card is built on a layered rendering system to ensure maximum readability with any background:

- **Background Layer** — solid color, 2/3-color gradient, or full-resolution image.
- **Smart Filter Engine** — when `show_filter: true`, CSS filters (brightness, saturation, grayscale) are applied to the background image based on the entity ON/OFF state.
- **Glassmorphic Overlays** — Icon Wrap and Control Zone use `backdrop-blur` and semi-transparent backgrounds to remain legible over complex images.
- **Glow & Shadow Effects** — icons and text use multi-layered drop shadows tinted by `icon_color` / `icon_color_on` for high contrast on any background.
- **Font Styles** — 4 text presets (`font_style`) applied to both Name and State labels simultaneously.

---

## ⚡ Service Countdown

When `tap_action`, `hold_action`, or `double_tap_action` is set to `call-service` and `show_service: true` is enabled, the card displays an animated countdown for the duration defined by `time_service`.

Two display styles are available:

| `service_style` | Behavior |
|---|---|
| `circle` *(default)* | Full-card SVG circle in `icon_color_on` — only when `entity` is empty and `show_more: false` |
| `bar` | Progress bar at the bottom of the card |

The `time_service` value supports two formats:
- `10` — countdown from 10 seconds, bar scale = 10s
- `"10/20"` — countdown from 10s, but the visual scale is 20s (bar starts at 50%)

Optional `blockade_card: true` prevents re-triggering the service while countdown is active.

### Configuration Example

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

Designed for smart plugs and sockets — provides immediate visual feedback on current power consumption.

- **Dynamic Fill** — bar fills proportionally to `max_watts`.
- **Visual Alert** — when consumption exceeds `con_warning` (%), a pulse animation warns the user.
- Set `con_warning: false` to disable the warning entirely.

### Configuration Example

```yaml
type: custom:smart-button-card
entity: switch.coffee_maker
name: Coffee Machine
icon: mdi:coffee
entity_watts: sensor.coffee_machine_power
max_watts: 2500
con_warning: 85
show_more: true
icon_color_on: "#ffffff"
icon_color: "#f0c040"
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
| `border_color` | string | `"rgba(255,255,255,0.2)"` | Border color |

### Entity & Labels

| Option | Type | Default | Description |
|---|---|---|---|
| `entity` | string | — | Main entity ID |
| `name` | string | — | Display name |
| `icon` | string | `mdi:lightning-bolt` | MDI icon identifier |
| `name_on` | string | `null` | Custom state label when ON |
| `name_off` | string | `null` | Custom state label when OFF |

### Visibility

| Option | Type | Default | Description |
|---|---|---|---|
| `show_icon` | boolean | `true` | Show/hide icon |
| `show_icon_full` | boolean | `true` | Full-size icon wrap vs. corner overlap mode |
| `show_name` | boolean | `true` | Show/hide name label |
| `show_state` | boolean | `true` | Show/hide state label |
| `show_more` | boolean | `false` | Enable Sliders / Power Bar Control Zone |

### Colors & Icon

| Option | Type | Default | Description |
|---|---|---|---|
| `icon_color` | string | `#f0c040` | Icon color when OFF (also tints wrap ring) |
| `icon_color_on` | string | `#ffffff` | Icon color when ON (also tints wrap ring) |
| `icon_size` | number | `28` | Icon size (px) |
| `icon_wrap_size` | number | `48` | Icon wrap / glow ring diameter (px) |
| `icon_over_size` | number | `4` | Corner overlap divisor when `show_icon_full: false` |
| `text_color` | string | `#ffffff` | Color for name and state labels |
| `name_size` | number | `14` | Name font size (px) |
| `state_size` | number | `12` | State badge font size (px) |
| `font_style` | number | `1` | Text style: `1`=normal `2`=small-caps `3`=monospace `4`=uppercase+spacing |

### Background

| Option | Type | Default | Description |
|---|---|---|---|
| `background_color1` | string | `#1a1a2e` | Primary background color |
| `background_color2` | string | `""` | Second color for gradient |
| `background_color3` | string | `""` | Third color for gradient |
| `background_gradient_angle` | number | `135` | Gradient angle (degrees) |

### Background Images & Filters

| Option | Type | Default | Description |
|---|---|---|---|
| `show_image` | boolean | `false` | Enable background image |
| `background_image_on` | string | `null` | Image path for ON state |
| `background_image_off` | string | `null` | Image path for OFF state |
| `show_filter` | boolean | `false` | Enable CSS filters on background image |
| `image_filter_on` | string | `"brightness(1) saturate(1.1)"` | CSS filter when ON |
| `image_filter_off` | string | `"brightness(0.45) saturate(0.2) grayscale(0.5)"` | CSS filter when OFF |

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

### Power Bar Monitoring

| Option | Type | Default | Description |
|---|---|---|---|
| `entity_watts` | string | `null` | Power sensor entity ID |
| `max_watts` | number | `2000` | Maximum scale for the bar (W) |
| `con_warning` | number / boolean | `80` | Pulse warning threshold (%) — `false` to disable |

### Service Countdown

| Option | Type | Default | Description |
|---|---|---|---|
| `show_service` | boolean | `false` | Enable countdown after `call-service` action |
| `time_service` | number / string | `10` | Seconds, or `"remaining/scale"` e.g. `"10/20"` |
| `service_style` | string | `"circle"` | `"circle"` — SVG ring (no entity + show_more:false) / `"bar"` — bottom bar |
| `blockade_card` | boolean | `false` | Block re-triggering while countdown is active |

### Actions

| Option | Type | Default | Description |
|---|---|---|---|
| `tap_action` | object | `{action: "toggle"}` | Single tap |
| `double_tap_action` | object | `{action: "more-info"}` | Double tap |
| `hold_action` | object | `{action: "more-info"}` | Long press |

Supported actions: `toggle`, `more-info`, `navigate`, `call-service`.

---

## 📦 Installation

1. Copy `piotras-smart-button-card.js` to `/config/www/` in your Home Assistant instance.
2. Add the resource in **Settings → Dashboards → Resources**:

```yaml
url: /local/piotras-smart-button-card.js
type: module
```

3. Add the card to your dashboard:

```yaml
type: custom:smart-button-card
entity: light.your_light
name: My Light
icon: mdi:lightbulb
```

---

*Created by Piotras. Strictly engineered for reliability.*
