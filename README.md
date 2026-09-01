![HACS](https://img.shields.io/badge/HACS-Default-orange?style=flat-square)
![HACS Downloads](https://img.shields.io/github/downloads/Piotras1/piotras-smart-button/piotras-smart-button-loader.js?logo=homeassistant&color=41BDF5&displayAssetName=false)
![GitHub Stars](https://img.shields.io/github/stars/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=stars&color=brightgreen)
![GitHub Issues](https://img.shields.io/github/issues/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=issues)
[![GitHub Release](https://img.shields.io/github/v/release/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=released)](https://github.com/Piotras1/piotras-smart-button/releases)
![GitHub Release Date](https://img.shields.io/github/release-date/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=released)

[![Guides](https://img.shields.io/badge/Guides-Show%20%26%20Tell-olive?style=flat-square&logo=github)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)
[![Guides](https://img.shields.io/badge/Guides-Custom%20Data%20Modules-teal?style=flat-square&logo=github)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/custom-data-modules)
[![GitHub Discussions](https://img.shields.io/badge/Discussions-Join%20the%20community-blueviolet?style=flat-square&logo=github)](https://github.com/Piotras1/piotras-smart-button/discussions)
![GitHub Discussions](https://img.shields.io/github/discussions/Piotras1/piotras-smart-button?style=flat-square&logo=github&label=discussions&color=blueviolet)

[![Kiosk](https://img.shields.io/badge/My%20Kiosk-View%20All%20Cards-orange?style=flat-square&logo=github)](https://github.com/Piotras1/piotras-cards-pack)
[![Assets Gallery](https://img.shields.io/badge/Assets%20Gallery-Smart%20Button-00b4d8?style=flat-square&logo=materialdesign)](https://piotras1.github.io/piotras-cards-pack/smart-button-assets.html)
[![MDI Icons](https://img.shields.io/badge/HA%20Icons-Search%20Gallery-00b4d8?style=flat-square&logo=materialdesign)](https://piotras1.github.io/piotras-cards-pack/mdi-icon-browser.html)
## Piotras Smart Button

### A powerful, all-in-one Lovelace card for Home Assistant — combining control, monitoring, visualization and custom logic in one highly configurable card.

<img width="1200" height="600" alt="piotras-smart-button" src="https://github.com/user-attachments/assets/2e6c1c86-8ee6-4e69-b357-ce285ea1fbc5" />

### 📚 Why another button card?

**Piotras Smart Button is not designed for just one type of entity.
It combines controls, status displays, visualizations and custom logic into a single highly configurable Lovelace card.
Use it as a light controller, thermostat, media controller, weather display, clock, power monitor, gate controller, alarm indicator — or build your own use case with Custom Data.**

### 🖥 Visual Editor

**Configure the card directly from the Home Assistant UI.
Adjust layout, size, icon, text, background, sliders, actions and services without manually editing YAML.**

### 🧩 Build Your Own Card Logic
**Add your own JavaScript logic to read Home Assistant states, calculate values, create conditions, generate text and colors, and reuse the results throughout the card.**

---

## ✨ Features

<details>
<summary><b>🔍 Click here to view the full text-based features list (SEO & Indexing)</b></summary>

### 🚀 Complete Features Specifications

*   **🎨 Visual Editor (No YAML Needed)** — Full GUI configuration with native tabs: General, Size, Background, Icon, Text, Layout, Slider & Power, Filters, Actions, and Service.
*   **📐 9-Grid Layout System** — Position your Icon, Name, and State badge completely independently across a 3×3 matrix.
*   **🎛️ Adaptive Auto-Sliders** — Smart auto-detection for Brightness, Color Temperature, Volume, Cover Position, and Fan Speed.
*   **🖼️ Dynamic Backgrounds & Smart Filters** — Solid colors, 2/3-color gradients, or full-res images with automated CSS filters (brightness, saturation, grayscale) based on state.
*   **⏱️ Service Countdown System** — Animated SVG circle or progress bar after a call-service, with an optional card blockade.
*   **⚡ Power Monitoring & Pulsing Alerts** — Real-time consumption bar with a configurable pulsing warning threshold for power spikes.
*   **🚧 Gate & Garage Mode** — Specialized cover layout with dynamic directional arrows tracking opening/closing states.
*   **🕹️ Interactive Action Buttons** — Clickable sub-button grids embedded directly into the Control Zone for rooms, scenes, or media.
*   **👥 Person & Device Tracking** — Displays the precise time since the last state change inside the Control Zone with home/away icon colors.
*   **🔋 Battery Monitoring** — Dynamic icon auto-adjusted to charge level, color bars, and optional charging state integration.
*   **🌡️ Thermostat & Climate Control** — Dedicated temperature adjustment buttons in the Control Zone and real-time room temp as a state badge.
*   **🟢 Comfort Zone Engine** — Card shifts state and changes icon colors based on custom comfort ranges: blue (too cold/dry), green (comfortable), red (too hot/humid).
*   **🕒 Integrated Clock & Calendar (`on.clock`)** — Standalone digital timekeeper with a clean, multi-language monthly calendar grid.
*   **🌦️ Mini Weather Dashboard** — Compact layout showing temperature, conditions, theme-colored icons, humidity, and wind speed.
*   **🧹 Smart Vacuum Status** — Dedicated support for vacuums featuring a dynamic spinning icon during sweeping and hardware-brand mapping.
*   **🚨 Advanced Alarm Status** — Security monitoring for disarmed, arming, armed, and vacation states with dynamic pulsing animations.
*   **🧩 Custom Data & Templates Engine** — Write your own JavaScript logic (`custom_data`) to read sensors, compute conditions, colors, or text, and reuse the result across almost any field on the card, including Custom State Labels.
*   **👁️ Conditional Visibility (`visible_if`)** — Hide or show the entire card dynamically based on a boolean, `custom_data` reference, or template.

</details>

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
/local/piotras-smart-button/piotras-smart-button-loader.js?v=2.0.0
```
- Resource type: **JavaScript Module**
6. Hard reload your browser (`Ctrl+Shift+R`).

</details>

---

## 👤 Person & Device Tracker

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-person-2.jpg" alt="Piotras Smart Button - Person & Device Tracker Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Person & Device Tracker Documentation](docs/PERSON.md)**

---

## 🧭 Navigation Mode (Neumorphic Style)

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-navigation-2.jpg" alt="Piotras Smart Button - Neumorphic Navigation Menu Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Navigation Mode Documentation](docs/NAVIGATION.md)**

---

## 🎛️ Socket & Power Monitoring

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-socket-2.jpg" alt="Piotras Smart Button - Socket & Power Monitoring Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Socket & Power Monitoring Documentation](docs/SOCKET.md)**

---

## 💡 Light & Auto-Dimmer Slider

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-lighting-4.jpg" alt="Piotras Smart Button - Light & Auto-Dimmer Slider Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Light & Auto-Dimmer Documentation](docs/LIGHT.md)**

---

## 🕒📅 Clock & Calendar Display

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-clock.jpg" alt="Piotras Smart Button - Clock & Calendar Grid Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Clock & Calendar Documentation](docs/CLOCK-CALENDAR.md)**

---

## 🌡️ Thermostat

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-thermostat.jpg" alt="Piotras Smart Button - Thermostat Control Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Thermostat Documentation](docs/THERMOSTAT.md)**

---

## 🔊 Media Player

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-media-2.jpg" alt="Piotras Smart Button - Media Player Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Media Player Documentation](docs/MEDIA.md)**

---  

## 📜 Script Button

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-script-2.jpg" alt="Piotras Smart Button - Script Button Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Script Button Documentation](docs/SCRIPT.md)**

---

## 🚨 Advanced Alarm Status

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-alarm-2.jpg" alt="Piotras Smart Button - Advanced Alarm Status Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Advanced Alarm Status Documentation](docs/ALARM.md)**

---

## 🌤️ Mini Weather Card

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-weather-2.jpg" alt="Piotras Smart Button - Mini Weather Card Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Mini Weather Card Documentation](docs/WEATHER.md)**

---

## 🔋 Battery

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-battery-2.jpg" alt="Piotras Smart Button - Battery Card Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Battery Card Documentation](docs/BATTERY.md)**

---

## 🌡️ Temperature & Humidity Comfort

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-temperature.jpg" alt="Piotras Smart Button - Temperature & Humidity Comfort Card Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Temperature & Humidity Comfort Card Documentation](docs/TEMPERATURE.md)**

---

## 🧹 Smart Vacuum Status

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-vacuum-1.jpg" alt="Piotras Smart Button - Smart Vacuum Status Card Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Smart Vacuum Status Card Documentation](docs/VACUUM.md)**

---

## 🚪 Entry Gate & Garage Control

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-garage-1.jpg" alt="Piotras Smart Button - Entry Gate & Garage Control Preview">

For full features, layout settings, and ready-to-use YAML examples, check out the dedicated guide:
> 🔗 **[Detailed Entry Gate & Garage Control Documentation](docs/GARAGE.md)**

---

## 🖥 Visual Editor

**For the full list of parameters, check the:**
> 👉 [Edytor Guide](docs/EDYTOR.md)

---

## ⚙️ Configuration Reference
**For the full list of parameters, check the:**
> 👉 [Configuration Reference Guide](docs/CONFIGURATION.md)

---

## 🧩 Custom Data & Templates

Write your own JavaScript logic — read sensors, build conditions, colors, or text — and reuse the result across any field on the card, including inside Custom State Labels.

For the full guide (syntax, referencing rules, ready-made modules, and common mistakes), check out the dedicated guide:
> 🔗 **[Detailed Custom Data Documentation](docs/custom_data_guide.md)**

---

## 🏷️ Custom State Labels

<img src="https://raw.githubusercontent.com/Piotras1/piotras-smart-button/refs/heads/main/img/piotras-smart-button-custom-labels.jpg" alt="Piotras Smart Button - Custom State Labels Preview">

For full documentation on matching rules, HTML labels, climate/weather value2, and clock greetings, check out the dedicated guide:
> 🔗 **[Detailed Custom State Labels Documentation](docs/custom_states_labels.md)**

---

## 🎛️ Custom States On & Blockade

For full documentation on forcing the active visual state and locking card interaction, check out the dedicated guide:
> 🔗 **[Detailed Custom States On & Blockade Documentation](docs/custom_states_on_and_custom_blockade.md)**

---

## 👁️ Conditional Visibility

Show or hide the entire card dynamically — based on a boolean, a `custom_data` reference, or a live template — without deleting it from the dashboard.

For the full guide (accepted value types, the `states` access rule, and common mistakes), check out the dedicated guide:
> 🔗 **[Detailed Conditional Visibility Documentation](docs/visible_if.md)**

---

## 📄 License

MIT — free to use, modify, and share.

---

*Created by Piotras. Strictly engineered for reliability.*
