![HACS](https://img.shields.io/badge/HACS-Default-orange?style=flat-square)
[![HACS Downloads](https://img.shields.io/github/downloads/Piotras1/piotras-smart-button/piotras-smart-button-loader.js?logo=homeassistant&color=41BDF5&displayAssetName=false)](https://github.com/Piotras1/piotras-smart-button)
[![GitHub Stars](https://img.shields.io/github/stars/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=stars&color=brightgreen)](https://github.com/Piotras1/piotras-smart-button/stargazers)
[![GitHub Issues](https://img.shields.io/github/issues/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=issues)](https://github.com/Piotras1/piotras-smart-button/issues)
[![GitHub Release](https://img.shields.io/github/v/release/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=released)](https://github.com/Piotras1/piotras-smart-button/releases)
[![GitHub Discussions](https://img.shields.io/github/discussions/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=discussions&color=blueviolet)](https://github.com/Piotras1/piotras-smart-button/discussions)

## Piotras Smart Button

### A powerful, all-in-one Lovelace card for Home Assistant — combining control, monitoring, visualization and custom logic in one highly configurable card.

<img width="1200" height="600" alt="piotras-smart-button" src="https://github.com/user-attachments/assets/2e6c1c86-8ee6-4e69-b357-ce285ea1fbc5" />

### 📚 Why another button card?

**Piotras Smart Button is not designed for just one type of entity.
It combines controls, status displays, visualizations and custom logic into a single card. Set it up in the visual editor — no YAML needed — or build your own panels with Custom Data.**

**🆕 v3.0 adds `custom_tap`: make any element you generate clickable, keep your own state on the card and build complete panels — keypads, charts, control boards — from a single card.**

---

## 🚀 Quick start

1. Install through HACS and hard reload your browser.
2. Add the card to your dashboard and pick an entity — light, cover, fan, vacuum, climate…
3. Adjust the look and controls in the visual editor. Sliders and icons adapt to the entity.

Not sure where to start? Find your device in the table below. Levels 2 and 3 are optional.

---

## ✨ Features

One card, three levels — start simple and go as deep as you like. Each level builds on the previous one, and cards from earlier versions keep working.

### 🖥 Level 1 — Visual editor, no YAML
Pick an entity and set everything up from the Home Assistant UI: layout, size, icon, text, background, sliders, actions and services. Sliders for brightness, color temperature, volume, position and fan speed are detected automatically. The table below lists the ready-made cards you get this way — lights, media players, thermostats, gates and blinds, vacuums, alarms, weather, clock, calendar, batteries, sockets, chip buttons and more.

### 🏷️ Level 2 — Small customizations, a few lines of YAML
No programming needed. Write your own text for each state (**Custom State Labels**, including HTML and time-of-day greetings), decide which states count as "on" and lock the card while something is running (**Custom States On & Blockade**), or hide the whole card when it doesn't matter (**Conditional Visibility**).

### 🧩 Level 3 — Pro: `custom_data` and `custom_tap` (v3.0)
Write your own JavaScript to read any entities and compute values, text and colors (**Custom Data**), then make anything you generate clickable and keep your own state on the card (**Custom Tap**). No main entity is needed, so the card becomes a blank canvas for keypads, charts and multi-device control panels — all inside a single card.

*Works with lights, media players, climate, covers, fans, vacuums, alarms, weather, sensors, scripts and more.*

---

## ⚙️ Installation

<details>
<summary><b>📦 Click here to view Installation Instructions (HACS & Manual)</b></summary>

### Method 1: Via HACS Store (Recommended)
1. Open HACS in Home Assistant
2. Search for **"Piotras Smart Button"** in the store
3. Click **Download**
4. Hard reload your browser (`Ctrl+Shift+R`)

### Method 2: Via HACS Link
1. Click the button below:

<a href="https://my.home-assistant.io/redirect/hacs_repository/?owner=Piotras1&repository=piotras-smart-button&category=plugin">
    <img src="https://my.home-assistant.io/badges/hacs_repository.svg" alt="Open your Home Assistant instance">
</a>

2. Click **Add** → **Download**
3. Hard reload your browser

### Method 3: Manual Installation

1. Download this repository as a ZIP file and extract it.
2. Inside your Home Assistant `config/www/` directory, create a new folder named `piotras-smart-button`.
3. Copy the compiled files (from `dist/` folder) into `config/www/piotras-smart-button/`.
4. Go to **Settings → Dashboards → Resources**.
5. Click **Add Resource** and enter:
```
/local/piotras-smart-button/piotras-smart-button-loader.js?v=3.0.0
```
- Resource type: **JavaScript Module**
6. Hard reload your browser (`Ctrl+Shift+R`).

</details>

---

## 🃏 Supported entities & use cases

Each guide has screenshots and ready-to-use YAML.

| What you control | Entity type | Guide |
|---|---|---|
| 💡 Lights (brightness, color temp, auto icon color) | `light` | [light](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/light_3.md) |
| 🔊 Media players | `media_player` | [media](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/media_3.md) |
| 🌡️ Thermostats & climate | `climate` | [thermostat](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/thermostat_3.md) |
| 🚪 Gates & garage doors | `cover` | [garage](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/garage_3.md) |
| 🪟 Blinds & shutters (position slider) | `cover` | [configuration](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/configuration_3.md) |
| 🌀 Fans (speed slider) | `fan` | [configuration](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/configuration_3.md) |
| 🧹 Vacuums | `vacuum` | [vacuum](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/vacuum_3.md) |
| 🚨 Alarm panels | `alarm_control_panel` | [alarm](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/alarm_3.md) |
| 🌤️ Weather | `weather` | [weather](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/weather_3.md) |
| 👤 People & devices | `person`, `device_tracker` | [person](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/person_3.md) |
| 🔋 Batteries (level bar, auto icon, charging state) | `sensor` + any entity you pick for charging state | [battery](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/battery_3.md) |
| 🎛️ Sockets & power monitoring | `switch` + power sensor | [socket](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/socket_3.md) |
| 🌡️ Temperature & humidity comfort | `sensor` | [temperature](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/temperature_3.md) |
| 📜 Scripts | `script` | [script](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/script_3.md) |
| 🕒 Clock | `on.clock` | [clock](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/clock_3.md) |
| 📅 Calendar | `on.calendar` | [calendar](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/calendar_3.md) |
| 🧭 Navigation menu | — | [navigation](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/navigation_3.md) |
| 🔘 Chip buttons (icon only, icon + name, or with slider) | any entity | [chip](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/chip_3.md) |
| 🧩 Anything else | no entity needed | [Custom Data](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/custom_data_guide_3.md) |

---

## 🧰 Build & configure

| Topic | Guide |
|---|---|
| 🖥 Visual Editor | [Editor Guide](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/edytor_3.md) |
| ⚙️ Configuration Reference | [Configuration](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/configuration_3.md) |
| 🧩 Custom Data & Templates | [Custom Data](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/custom_data_guide_3.md) |
| 🖱️ Custom Tap (v3.0) | [Custom Tap](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/custom_tap_3.md) |
| 🏷️ Custom State Labels | [Labels](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/custom_states_labels_3.md) |
| 🎛️ Custom States On & Blockade | [States & Blockade](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/custom_states_on_and_custom_blockade_3.md) |
| 👁️ Conditional Visibility | [visible_if](https://github.com/Piotras1/piotras-smart-button/blob/main/docs/visible_if_3.md) |

---

## 🌐 Community & tools

| | |
|---|---|
| 💬 [Show & Tell](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell) | Dashboards and setups shared by the community — good for ideas. |
| 🧩 [Custom Data Modules](https://github.com/Piotras1/piotras-smart-button/discussions/categories/custom-data-modules) | Ready-made `custom_data` modules to copy and paste. |
| 📈 [PSB History Engine](https://github.com/Piotras1/psb-history-engine) | Companion HACS integration that keeps a rolling history of any numeric entity, so you can build fast charts with `custom_data`. |
| 🎨 [Assets Gallery](https://piotras1.github.io/piotras-cards-pack/smart-button-assets.html) | Backgrounds and images to use with the card. |
| 🔍 [HA Icons](https://piotras1.github.io/piotras-cards-pack/mdi-icon-browser.html) | Searchable gallery of icons to use in the icon fields. |
| 🧰 [My Kiosk](https://github.com/Piotras1/piotras-cards-pack) | View all my cards in one place. |

---

## 📄 License

MIT — free to use, modify, and share.

---

*Created by Piotras. Strictly engineered for reliability.*
