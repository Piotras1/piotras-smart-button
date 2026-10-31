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
| 💡 Icon | The icon, its colors, size and shape. For a cover, also its type, for the automatic icon |
| 📝 Text | The font, the shadow, the sizes and the colors of the name and the state badge. The colors are together in the **Colors** section |
| 🔲 Layout | The place of the icon, the name and the state on the card |
| 🎚 Slider & Power | The bar at the bottom: sliders, power, battery, comfort, and the buttons of a player, a fan, a cover and a vacuum. The colors of the bar are together in the **Bar colors** section |
| 🎨 Filters | Brightness, saturation and grayscale of the background, for on and off |
| ⚡ Actions | What happens on tap, double-tap and hold |
| 📄 Service | The countdown after a service call, for scripts and services |

For a vacuum, the **🎚 Slider & Power** tab is called **🧹 Vacuum Buttons**.

The **📄 Service** tab is shown for every entity. It is meant for scripts and services.

The full list of options, with the default values, is in the [Configuration Reference](configuration_3.md). It has one section for each tab.

## The editor changes with the entity

The editor looks at the entity you type and shows only the fields that fit it.

| Entity | What you see | Guide |
|---|---|---|
| `on.clock` | **General** has a **Clock display** section. **Text** has **Clock** and **Greeting** sections. The **Text shadow** switch shows only for the **Digits** style, because the other styles have no text shadow | [Clock](clock_3.md) |
| `on.calendar` | **General** has a **Calendar display** section. **Text** has a **Calendar** section with the size of the letters. The **Slider & Power** tab is hidden. **Icon** shows the calendar colors | [Calendar](calendar_3.md) |
| `weather.*` | **Slider & Power** has the toggle for the bar with humidity and wind | [Mini Weather Card](weather_3.md) |
| `light.*` | **Slider & Power** explains the automatic sliders | [Light & Auto-Dimmer Slider](light_3.md) |
| `fan.*` | **Slider & Power** has **Fan control**: a speed slider, speed buttons, or `−` and `+` buttons. It has also the switch for the spinning icon | [Fan](fan_3.md) |
| `cover.*` | **Slider & Power** has **Cover control**: a position slider, `−` and `+` buttons, or ▲ ■ ▼ buttons that can call your own scripts. **Icon** has **Cover type**, for the automatic icon | [Blinds & Shutters](blinds_3.md), [Gates & Garage Doors](garage_3.md) |
| `vacuum.*` | The tab is called **🧹 Vacuum Buttons**. It has the switch for the buttons, the bar height and the bar colors. It has no power options | [Vacuum](vacuum_3.md) |
| `media_player.*` | **Slider & Power** adds **Show player buttons**. With both toggles on, you can also choose the layout and the height of the player bar | [Media Player](media_3.md) |
| `switch.*`, `input_boolean.*`, `script.*`, `automation.*` | **Slider & Power** has a **Power monitoring** section | [Socket & Power Monitoring](socket_3.md), [Script Button](script_3.md) |
| `climate.*` | **Slider & Power** explains the − and + buttons | [Thermostat](thermostat_3.md) |
| `person.*`, `device_tracker.*` | **Slider & Power** explains the last-change bar | [Person & Device Tracker](person_3.md) |
| A sensor with `device_class: battery` | **Slider & Power** has a **Battery** section. **Icon** does not offer the icon fields, because the icon is automatic | [Battery](battery_3.md) |
| A sensor with `device_class: temperature` or `humidity` | **Slider & Power** has a **Comfort range** section | [Temperature Comfort](temperature_3.md), [Humidity Comfort](humidity_3.md) |

Most of these sections work only after you switch on **Show slider or power bar** in the **🎚 Slider & Power** tab. This is true also for **Fan control**, **Cover control**, the bar height and **Bar colors**. Two switches are there also when the bar is off: **Spin icon when fan is on** and **Show player buttons**.

The editor knows most types from the name of the entity, for example `light.`, `cover.` or `vacuum.`. For battery, temperature and humidity sensors, it reads the `device_class` of the entity, so the entity must exist in Home Assistant. If a section is missing, check the name of the entity.

## Colors

Each color field has three parts: a small square, a color picker and a text field. Tap the square to open the picker. In the text field, you can type a color such as `#f0c040` or `rgba(255,255,255,0.5)`.

For the icon colors and the background colors, you can also type `auto`. The square then shows a rainbow with the letter **A**.

- `auto` in an icon color takes the color from the light.
- `auto` in a background color takes the card color of your Home Assistant theme.

The color fields are in groups, so you find them in one place:

| Tab | Section | The colors in it |
|---|---|---|
| 📝 Text | **Colors** | Text color, Value color and Shadow color. For a clock: Clock color and Greeting color, and Shadow color for the Digits style. For a calendar: Calendar text color |
| 🎚 Slider & Power | **Bar colors** | Label color, Bar color and Bar icons color. A vacuum has no label, so it has only the last two |

The **Shadow color** field shows only while **Text shadow** is on. If you leave a color field empty, the card uses the default look.

## Actions

In the **⚡ Actions** tab, each of tap, double-tap and hold has an **Action**: **Toggle**, **More info**, **Navigate**, **Call service** or **None**.

- **Navigate** adds a **Navigation path** field, for example `/lovelace/0`.
- **Call service** adds a **Service** field, for example `light.turn_on`.

If you do not set an action, tap is **Toggle**, and double-tap and hold are **None**.

The ▲ ■ ▼ buttons of a cover have their own actions. They are in the **🎚 Slider & Power** tab, in the **Cover control** section, when you choose the ▲ ■ ▼ buttons. Each button has **Native cover service** or **Call service**, and a **Service** field for a script such as `script.gate_open`. See [Gates & Garage Doors](garage_3.md).

## What is not in the editor

Some options exist only in YAML. Switch to the code editor to add them:

- `custom_data` — [Custom Data & Templates](custom_data_guide_3.md)
- `custom_tap` — [Custom Tap](custom_tap_3.md)
- `custom_states_labels` — [Custom State Labels](custom_states_labels_3.md)
- `custom_states_on` and `custom_blockade` — [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md)
- `visible_if` — [Conditional Visibility](visible_if_3.md)
- `service_data` of the actions, for tap, double-tap, hold and the cover buttons — [Configuration Reference](configuration_3.md)

The visual editor does not remove them. When you change something in the visual editor, the options you wrote in YAML stay in the card.

> 🔙 Back to the [Main README](../README.md)
