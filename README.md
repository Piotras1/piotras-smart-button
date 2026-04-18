## Piotras Smart Button
### Release v1.2.0

**Most cards go silent after calling a service. This one tells you it worked.**

### Cards are OFF
![Zrzut ekranu (1222)](https://github.com/user-attachments/assets/a7b07b54-730c-4821-8396-dc8a030987a7)
### Cards are ON
![Zrzut ekranu (1223)](https://github.com/user-attachments/assets/cb632061-915c-4bbe-90ad-bb6e1bb3665e)

A Home Assistant button card with 9-grid layout · auto-detecting sliders · dynamic backgrounds · service countdown · visual editor included.

---

## ⚙️ Installation

### Method 1: Via HACS (Recommended)

1. Click the button below to automatically add the repository to your HACS:

<a href="https://my.home-assistant.io/redirect/hacs_repository/?owner=Piotras1&repository=piotras-smart-button&category=plugin">
    <img src="https://my.home-assistant.io/badges/hacs_repository.svg" alt="Open your Home Assistant instance">
</a>

2. Click **Add** in the pop-up window.
3. Once the repository page opens, click **Download**.
4. After downloading, do a **Hard reload** of your browser.

### Method 2: Manual Installation

1. Download this repository as a ZIP file and extract it.
2. Inside your Home Assistant `config/www/` directory, create a new folder named `piotras-smart-button`.
3. Copy the compiled files (from `dist/` folder) into `config/www/piotras-smart-button/`.
4. Go to **Settings → Dashboards → Resources**.
5. Click **Add Resource** and enter:
```
/local/piotras-smart-button/piotras-smart-button-loader.js?v=1.2.0
```
- Resource type: **JavaScript Module**
6. Hard reload your browser (`Ctrl+Shift+R`).

---

## ✨ Features

- **Visual Editor** — full GUI with tabs: General, Size, Background, Icon, Text, Layout, Slider & Power, Filters, Actions, Service. No manual YAML needed.
- **9-Grid Layout System** — place Icon, Name, and State badge independently across a 3×3 matrix.
- **Adaptive Auto-Sliders** — smart detection of Brightness, Color Temp, Volume, Cover Position, Fan Speed. Zero configuration required.
- **Dynamic Backgrounds** — solid color, 2/3-color gradients, or full-resolution background images.
- **Smart Filter Engine** — CSS filters (brightness, saturation, grayscale) applied automatically per ON/OFF state.
- **Service Countdown** — animated SVG circle or progress bar after `call-service`, with optional card blockade.
- **Power Monitoring** — real-time consumption bar with configurable pulse warning threshold.
- **Person & Device Tracker** — last state change time displayed in the Control Zone. Icon color follows home/away state.
- **Battery Monitoring** — dynamic icon auto-adjusted to charge level, color bar, optional charging state entity.
- **Thermostat Control** — temperature buttons in the Control Zone, room temperature as state badge, ON when heating or cooling.
- **Dual Icon Mode** — separate icons for ON and OFF states via `icon_on`.
- **Font Styles** — 4 text presets for Name and State labels.
- **Advanced Action Engine** — Tap, Double-Tap, and Hold, optimized for both mobile and desktop.

---

## 🧩 The 9-Grid Layout System

Position Icon, Name, and State independently using a 3×3 grid (positions 1–9):

![Zrzut ekranu (1190)](https://github.com/user-attachments/assets/1f66c380-280f-4242-86f8-cd03f11fa073)

Elements sharing the same position are stacked vertically: `icon → name → state`.  
When `show_more: true` is active, elements in the bottom row shift up automatically to avoid the Control Zone.

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

## 🧭 Navigation Mode (Neumorphic Style)

The card works equally well as a navigation controller — icon-only, circular, with a tactile press-and-hold feel.

Key features:

- **Active State Glow** — the current page highlights with a colored glow, immediately visible where you are.
- **Press & Hold feedback** — button stays visually pressed ~500ms before redirecting.
- **Minimalist look** — icon-only with `border_radius: 50` for a perfect circle, no clutter.
- **Still functional** — navigation buttons can still show background statuses (`show_service`, `entity_watts`) in real-time.

![Zrzut ekranu (1174)a](https://github.com/user-attachments/assets/d4a332d1-9811-47a7-8f61-7adacb4270b9)

![Zrzut ekranu (1175)](https://github.com/user-attachments/assets/7856887a-f315-4faf-9a49-18947315f8dc)

```yaml
type: custom:piotras-smart-button
icon: mdi:folder-home
icon_color: "#5badff"
show_filter: false
show_state: false
show_name: false
show_image: false
background_color1: "#5badff"
card_width: 70
card_height: 70
border_radius: 50
icon_size: 44
icon_wrap_size: 48
tap_action:
  action: navigate
  navigation_path: /dashboard-home/0
```

---

## 🔌 Socket & Power Monitoring

Real-time power consumption bar for smart plugs and sockets. Tap toggles the socket on or off. Hold triggers an optional service call (e.g. a boiler heating script) with a countdown timer and card blockade.

![Zrzut ekranu (1177)](https://github.com/user-attachments/assets/3f273068-c126-4333-9879-cdb9f824ecc4)

![Zrzut ekranu (1178)](https://github.com/user-attachments/assets/8e362b2d-0a7b-4ae5-8a87-426c843c522a)

![Zrzut ekranu (1179)](https://github.com/user-attachments/assets/1e49a8d1-d191-4a8a-9033-f3841203eedc)

- **Dynamic Fill** — the power bar fills proportionally to `max_watts`.
- **Pulse Warning** — bar pulses when consumption exceeds `con_warning` (%). Set to `false` to disable.
- **Card Blockade** — when `blockade_card: true`, re-triggering the service call is blocked for the full timer duration. Tap actions (toggle, more-info) still work normally.

```yaml
type: custom:piotras-smart-button
entity: switch.your_socket_entity
entity_watts: sensor.your_power_sensor
name: Boiler
icon: mdi:water-boiler
icon_color_on: "#ff8080"
card_width: 180
card_height: 120
border_width: 1
icon_size: 40
icon_wrap_size: 50
icon_color: "#c0c0c0"
show_icon_full: false
icon_over_size: 4
font_style: 4
name_size: 20
state_size: 15
icon_mode: 1
name_mode: 3
value_mode: 3
show_image: true
background_image_on: /local/your_background.jpg
show_filter: true
show_more: true
con_warning: false
max_watts: 2000
show_service: true
time_service: 20
service_style: bar
blockade_card: true
tap_action:
  action: toggle
hold_action:
  action: call-service
  service: script.your_script
```

---

## 💡 Light & Auto-Dimmer Slider

When the card detects a `light` entity with brightness support, it automatically renders a brightness slider in the Control Zone — no extra configuration needed. Tap toggles the light. The slider adjusts brightness directly by dragging.

![Zrzut ekranu (1180)](https://github.com/user-attachments/assets/e15eda84-2154-4371-aa75-74099743dd0e)

![Zrzut ekranu (1181)](https://github.com/user-attachments/assets/5e8239f0-2708-419e-b991-39625f65c949)

![Zrzut ekranu (1182)](https://github.com/user-attachments/assets/5d624c7a-80d0-4f8c-8317-db58ac1be7d0)

![Zrzut ekranu (1183)](https://github.com/user-attachments/assets/859ebdb3-590f-4515-a1d6-f10f4b02c826)

- **Auto-detected** — no slider configuration needed, the card detects `brightness` automatically.
- **Live feedback** — slider position reflects current brightness in real time.
- **Countdown + slider** — `service_style: bar` renders the timer above the slider, both active at the same time.

```yaml
type: custom:piotras-smart-button
entity: light.your_light_entity
name: Living room
icon: mdi:ceiling-light
icon_color_on: "#ffff80"
card_width: 180
card_height: 120
border_width: 1
icon_size: 40
icon_wrap_size: 50
icon_color: "#c0c0c0"
font_style: 2
name_size: 20
state_size: 15
icon_mode: 1
name_mode: 5
value_mode: 3
show_image: true
show_filter: true
background_image_on: /local/your_background.jpg
show_more: true
show_icon_full: false
icon_over_size: 6.5
show_service: true
service_style: bar
time_service: 20
tap_action:
  action: toggle
hold_action:
  action: more-info
double_tap_action:
  action: call-service
  service: script.your_script
```

---

## 📜 Script Button

A dedicated layout for triggering scripts — no entity required. The card uses `name_on` and `name_off` to display custom state labels (e.g. "Ready Script" / "Progress Script") and shows an animated countdown immediately on tap.

Two countdown styles depending on layout:

- **`circle`** — SVG ring centered on the card, used when there is no entity and `show_more: false`. Ideal for icon-only or gradient background cards.
- **`bar`** — progress bar at the bottom, used alongside a background image.

Both styles can appear on different cards in the same dashboard simultaneously, each with its own `time_service` duration.

![Zrzut ekranu (1184)](https://github.com/user-attachments/assets/bad9ae77-257c-4708-9e29-15033216f324)

![Zrzut ekranu (1185)](https://github.com/user-attachments/assets/1264b8db-721d-4f09-a52a-39423bf93dd6)

```yaml
type: custom:piotras-smart-button
name: Script
icon: mdi:script
card_width: 180
card_height: 120
border_width: 1
icon_size: 45
icon_wrap_size: 60
icon_color: "#c0c0c0"
font_style: 2
name_size: 20
state_size: 15
icon_mode: 1
name_mode: 5
value_mode: 5
show_service: true
time_service: 20
service_style: circle
show_filter: true
name_off: "Ready Script"
name_on: "Progress Script"
show_image: true
background_image_on: /local/your_image_on.png
background_image_off: /local/your_image_off.png
show_icon_full: false
tap_action:
  action: call-service
  service: script.your_script
```

---

## 🔊 Media Player

When the card detects a `media_player` entity, it automatically renders a volume slider in the Control Zone. Tap toggles playback (PLAY/STOP). The slider adjusts volume level directly by dragging.

![Zrzut ekranu (1186)](https://github.com/user-attachments/assets/5f67ec1f-95eb-4514-9d24-671c1b00d6b7)![Zrzut ekranu (1188)](https://github.com/user-attachments/assets/13220e11-c6db-43c1-acb2-8e1f64f86b44)


- **Auto-detected** — `volume_level` slider requires no extra configuration.
- **Different backgrounds per state** — `background_image_off` and `background_image_on` show a different image when stopped vs playing.
- **Full toggle** — tap starts or stops playback; hold opens more-info for full media controls.

```yaml
type: custom:piotras-smart-button
entity: media_player.your_media_player
name: Speaker Bedroom
icon: mdi:radio
icon_color_on: "#ff8000"
card_width: 180
card_height: 120
border_width: 1
icon_size: 40
icon_wrap_size: 50
icon_over_size: 5
icon_color: "#c0c0c0"
font_style: 2
name_size: 20
state_size: 20
icon_mode: 1
name_mode: 5
value_mode: 3
show_image: true
show_filter: true
show_more: true
show_icon_full: false
background_image_off: /local/your_image_off.png
background_image_on: /local/your_image_on.png
tap_action:
  action: toggle
hold_action:
  action: more-info
```

---

## 👤 Person & Device Tracker

When the card detects a `person` or `device_tracker` entity, the Control Zone displays the **last state change time** (HH:MM format) alongside a dynamic icon — home or away. No extra configuration needed beyond enabling `show_more: true`.

Icon colors follow the existing color variables — `icon_color_on` when home, `icon_color` when away — so the bar stays visually consistent with the rest of the card.

![Zrzut ekranu (1292)](https://github.com/user-attachments/assets/51bf40ea-e274-4534-bb04-ab9b0d294421)![Zrzut ekranu (1281)](https://github.com/user-attachments/assets/9805f891-8e9c-409d-941c-173006059b9a)

- `mdi:home` in `icon_color_on` when state is `home`
- `mdi:walk` in `icon_color` when state is `away` / `not_home`
- Time shown as `HH:MM` — language-neutral, no translation needed
- `name_on` / `name_off` and `tap_action` work normally

```yaml
type: custom:piotras-smart-button
entity: person.jan
name: Jan
icon: mdi:account
icon_color: "#aaaaaa"
icon_color_on: "#43d14a"
card_width: 140
card_height: 140
show_image: true
background_image_on: /local/persons/jan.jpg
show_filter: true
show_more: true
slider_height: 26
name_on: In Home
name_off: Outside
tap_action:
  action: more-info
```

---

## 🔋 Battery

When the card detects a `sensor` entity with `device_class: battery`, the Control Zone displays a **color-coded charge bar** (red → yellow → green) and the current charge percentage. The main card icon is automatically replaced with a dynamic battery icon matching the charge level.

For full charging state support, add an optional `entity_battery_state` pointing to a sensor that reports `charging`, `discharging`, `full`, or `not_charging`.

![Zrzut ekranu (1293)](https://github.com/user-attachments/assets/03390d5f-f975-499e-b2b9-e2fdd25e1045)![Zrzut ekranu (1294)](https://github.com/user-attachments/assets/a9a00304-aacd-475c-9cf8-df29503ec3b9)

- Icon auto-adjusts: `mdi:battery-10` through `mdi:battery`, `mdi:battery-charging` when charging
- Card turns **ON** (active color) when `entity_battery_state` reports `charging`
- Card turns **OFF** when `discharging`, `not_charging`, or `full`
- `⚡` suffix appears next to percentage when charging
- `icon_color` and `icon_color_on` still control the icon ring color
- `name_on` / `name_off` work normally — e.g. `name_on: Charging` / `name_off: Discharging`

```yaml
type: custom:piotras-smart-button
entity: sensor.lenovo_battery
entity_battery_state: sensor.lenovo_battery_state
name: Lenovo Battery
icon_color: "#aaaaaa"
icon_color_on: "#43d14a"
card_width: 140
card_height: 120
show_more: true
slider_height: 26
slider_label_color: "#ffffff"
name_on: Charging
name_off: Discharging
tap_action:
  action: more-info
```

---

## 🌡️ Thermostat

When the card detects a `climate` entity, the Control Zone displays **temperature control buttons** — `−` on the left, target temperature in the center, `+` on the right. Each press adjusts the target temperature by **0.5°**. The state badge on the main card automatically shows the **current room temperature** instead of ON/OFF.

The card turns **ON** (active color) when `hvac_action` is `heating` or `cooling`, and **OFF** when `idle` or `off`. When the thermostat is fully off, the Control Zone shows a red **OFF** label instead of the temperature buttons.

![Zrzut ekranu (1319)](https://github.com/user-attachments/assets/54d1be45-ab9f-4aa7-b03a-40dc6e4e1288)![Zrzut ekranu (1319)](https://github.com/user-attachments/assets/26ac69de-5c2b-4503-b231-078f4f28d805)![Zrzut ekranu (1319)](https://github.com/user-attachments/assets/ad4757da-9654-4630-9416-572c2c9a316e)


- Target temperature displayed `2px` larger than other bar labels for quick readability
- `−` / `+` buttons styled as small circles, spread to card edges
- Room temperature shown as state badge (e.g. `21.5°`) — replaces ON/OFF labels
- `icon` and icon colors set freely — the card does not override them for climate
- `slider_height` and `slider_label_color` apply normally

```yaml
type: custom:piotras-smart-button
entity: climate.salon
name: Salon
icon: mdi:sofa
icon_color: "#aaaaaa"
icon_color_on: "#ff6b35"
card_width: 160
card_height: 140
show_image: true
background_image_on: /local/rooms/salon.jpg
show_filter: true
show_more: true
slider_height: 32
slider_label_color: "#ffffff"
tap_action:
  action: more-info
```

---

## ⚡ Service Countdown

When any action is set to `call-service` and `show_service: true` is enabled, the card displays an animated countdown for the duration set by `time_service`.

Two display styles:

| `service_style` | Description |
|---|---|
| `circle` *(default)* | Animated SVG ring centered on the card in `icon_color_on`. Only available when `entity` is not set and `show_more: false`. Ring diameter = `icon_wrap_size + 5px`. |
| `bar` | Progress bar docked at the bottom of the card. Works with any entity and is compatible with `show_more: true`. |

> **Note:** When using `entity` (e.g. a switch or socket) together with `show_more: true`, always use `service_style: bar`. The `circle` style requires an empty card without a power bar.

`time_service` supports two formats:
- `10` — countdown from 10 s, bar scale = 10 s
- `"10/20"` — countdown from 10 s, visual scale is 20 s (bar starts at 50%)

When `blockade_card: true` is set, re-triggering the service call is blocked for the entire countdown duration. All other tap actions (toggle, navigate, more-info) remain fully functional during this time.

```yaml
type: custom:piotras-smart-button
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
| `icon` | string | `mdi:lightning-bolt` | MDI icon (not used for `battery` — auto-dynamic) |
| `icon_on` | string | `null` | Alternative icon when entity is ON (not used for `battery`) |
| `name_on` | string | `null` | Custom state label when ON (not used for `climate`) |
| `name_off` | string | `null` | Custom state label when OFF (not used for `climate`) |

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

### Control Zone (Slider & Power)

| Option | Type | Default | Description |
|---|---|---|---|
| `show_more` | boolean | `false` | Enable Control Zone |
| `slider_height` | number | `26` | Control Zone height (px) — content scales proportionally via `√(height/26)` |
| `slider_label_color` | string | `rgba(255,255,255,0.85)` | Color for all labels and icons in the Control Zone |

Auto-detected sliders: `brightness`, `color_temp`, `volume_level`, `current_position` (cover), `percentage` (fan), `preset_mode` (fan).

### Battery

| Option | Type | Default | Description |
|---|---|---|---|
| `entity_battery_state` | string | `null` | Optional sensor reporting `charging` / `discharging` / `full` / `not_charging` — enables dynamic icon and ON/OFF card state |

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
| `service_style` | string | `"circle"` | `"circle"` SVG ring · `"bar"` bottom bar |
| `blockade_card` | boolean | `false` | Block re-triggering the service call while countdown is active |

### Actions

| Option | Type | Default | Description |
|---|---|---|---|
| `tap_action` | object | `{action: "toggle"}` | Single tap |
| `double_tap_action` | object | `{action: "more-info"}` | Double tap |
| `hold_action` | object | `{action: "more-info"}` | Long press |

Supported actions: `toggle`, `more-info`, `navigate`, `call-service`.

---

## 🖥 Visual Editor

The card ships with a full visual editor accessible directly in the Home Assistant dashboard UI. The editor automatically detects the entity domain and adjusts available options — for example, showing battery-specific fields for `sensor` with `device_class: battery`, thermostat controls for `climate`, or person tips for `person` / `device_tracker`.

![Zrzut ekranu (1191)](https://github.com/user-attachments/assets/9d9b206d-59af-4a94-b9c5-d6a00211ffae)

Tabs available: **General · Size · Background · Icon · Text · Layout · Slider & Power · Filters · Actions · Service**

---

## 📄 License

MIT — free to use, modify, and share.

---

*Created by Piotras. Strictly engineered for reliability.*
