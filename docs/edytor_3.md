# 🖥️ Visual Editor

The visual editor lets you set up the card without writing YAML. This guide shows how to open it, what each tab does, and what changes when you pick a different entity.

## Open the editor

1. Add a card to your dashboard.
2. If Piotras Smart Button is not in the list of cards, choose **Manual** and type:

```yaml
type: custom:piotras-smart-button
```

3. Press **Show visual editor**.

Type your entity in the **Entity** field of the **⚙️ General** tab. Every change is saved to the YAML of the card at once. You can switch between the visual editor and the code editor at any time.

## The tabs

| Tab | What you set there |
|---|---|
| ⚙️ General | The entity, the name, and what to show: icon, name, state, press effect. For a clock and a calendar, also their own settings |
| 📐 Size | The size of the card, rounded corners, border and outer shadow |
| 🖼 Background | The color or gradient, or an image, and the inner shadow |
| 💡 Icon | The icon, its colors, size and shape |
| 📝 Text | The text of the name and the state badge: size, color, font |
| 🔲 Layout | The place of the icon, the name and the state on the card |
| 🎚 Slider & Power | The bar at the bottom: sliders, power, battery, comfort, player buttons |
| 🎨 Filters | Brightness, saturation and grayscale of the background, for on and off |
| ⚡ Actions | What happens on tap, double-tap and hold |
| 📄 Service | The countdown after a service call, for scripts and services |

The **📄 Service** tab is shown for every entity. It is meant for scripts and services.

The full list of options, with the default values, is in the [Configuration Reference](configuration_3.md). It has one section for each tab.

## The editor changes with the entity

The editor looks at the entity you type and shows only the fields that fit it.

| Entity | What you see | Guide |
|---|---|---|
| `on.clock` | **General** has a **Clock display** section. **Text** has **Clock** and **Greeting** sections | [Clock](clock_3.md) |
| `on.calendar` | **General** has a **Calendar display** section. The **Slider & Power** tab is hidden. **Icon** shows the calendar colors | [Calendar](calendar_3.md) |
| `weather.*` | **Slider & Power** has the toggle for the bar with humidity and wind | [Mini Weather Card](weather_3.md) |
| `light.*` | **Slider & Power** explains the automatic sliders | [Light & Auto-Dimmer Slider](light_3.md) |
| `fan.*` | The same as a light: a speed slider | |
| `cover.*` | The same as a light: a position slider | [Garage Door & Cover](garage_3.md) |
| `media_player.*` | **Slider & Power** adds **Show player buttons**. With both toggles on, you can also choose the layout and the height of the player bar | [Media Player](media_3.md) |
| `switch.*`, `input_boolean.*`, `script.*`, `automation.*` | **Slider & Power** has a **Power monitoring** section | [Socket & Power Monitoring](socket_3.md), [Script Button](script_3.md) |
| `climate.*` | **Slider & Power** explains the − and + buttons | [Thermostat](thermostat_3.md) |
| `person.*`, `device_tracker.*` | **Slider & Power** explains the last-change bar | [Person & Device Tracker](person_3.md) |
| A sensor with `device_class: battery` | **Slider & Power** has a **Battery** section. **Icon** does not offer the icon fields, because the icon is automatic | [Battery](battery_3.md) |
| A sensor with `device_class: temperature` or `humidity` | **Slider & Power** has a **Comfort range** section | [Temperature Comfort](temperature_3.md), [Humidity Comfort](humidity_3.md) |

Most of these sections work only after you switch on **Show slider or power bar** in the **🎚 Slider & Power** tab.

The editor knows most types from the name of the entity, for example `light.` or `climate.`. For battery, temperature and humidity sensors, it reads the `device_class` of the entity, so the entity must exist in Home Assistant. If a section is missing, check the name of the entity.

## Colors

Each color field has three parts: a small square, a color picker and a text field. Tap the square to open the picker. In the text field, you can type a color such as `#f0c040` or `rgba(255,255,255,0.5)`.

For the icon colors and the background colors, you can also type `auto`. The square then shows a rainbow with the letter **A**.

- `auto` in an icon color takes the color from the light.
- `auto` in a background color takes the card color of your Home Assistant theme.

## Actions

In the **⚡ Actions** tab, each of tap, double-tap and hold has an **Action**: **Toggle**, **More info**, **Navigate**, **Call service** or **None**.

- **Navigate** adds a **Navigation path** field, for example `/lovelace/0`.
- **Call service** adds a **Service** field, for example `light.turn_on`.

If you do not set an action, tap is **Toggle**, and double-tap and hold are **None**.

## What is not in the editor

Some options exist only in YAML. Switch to the code editor to add them:

- `custom_data` — [Custom Data & Templates](custom_data_guide_3.md)
- `custom_tap` — [Custom Tap](custom_tap_3.md)
- `custom_states_labels` — [Custom State Labels](custom_states_labels_3.md)
- `custom_states_on` and `custom_blockade` — [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md)
- `visible_if` — [Conditional Visibility](visible_if_3.md)

The visual editor does not remove them. When you change something in the visual editor, the options you wrote in YAML stay in the card.

> 🔙 Back to the [Main README](../README_3.md)
