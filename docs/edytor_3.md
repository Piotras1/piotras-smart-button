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
| 📐 Size | The size of the card (the width and the height have an **Auto** button), rounded corners, border and outer shadow |
| 🖼 Background | The color or gradient, or an image, and the inner shadow |
| 💡 Icon | The icon, its colors, size and shape. For a cover, also its type, for the automatic icon |
| 📝 Text | The font, the shadow, the sizes and the colors of the name and the state badge. The colors are together in the **Colors** section |
| 🔲 Layout | The place of the icon, the name and the state on the card |
| 🎚 Slider & Power | The bar at the bottom: sliders, power, battery, comfort, and the buttons of a player, a fan, a cover and a vacuum. The colors of the bar are together in the **Bar colors** section. The tab is locked when the card has no bar, see [When the bar is not available](#when-the-bar-is-not-available) |
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
| `on.calendar` | **General** has a **Calendar display** section. **Text** has a **Calendar** section with two sizes: **Calendar text size** and **Event details size**. The **Slider & Power** tab is hidden. **Icon** shows the calendar colors | [Calendar](calendar_3.md) |
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
| No entity, or an entity without a bar (for example `lock.*`, `binary_sensor.*`, a sensor with another `device_class`) | **Slider & Power** is locked and shows 🔒 | [When the bar is not available](#when-the-bar-is-not-available) |

Most of these sections work only after you switch on **Show slider or power bar** in the **🎚 Slider & Power** tab. This is true also for **Fan control**, **Cover control**, the bar height and **Bar colors**. Two switches are there also when the bar is off: **Spin icon when fan is on** and **Show player buttons**.

The editor knows most types from the name of the entity, for example `light.`, `cover.` or `vacuum.`. For sensors, it reads the `device_class` of the entity as soon as Home Assistant gives it the states, so the entity must exist in Home Assistant. If a section is missing, check the name of the entity.

## When the bar is not available

The bar at the bottom needs an entity that has one. The **🎚 Slider & Power** tab is locked, and shows 🔒 in its name, when:

- the card has no entity, or
- the entity is of a type that has no bar.

These entities have a bar:

| Entity | What the bar shows |
|---|---|
| `light.*`, `fan.*`, `cover.*`, `media_player.*` | Sliders and buttons |
| `climate.*` | The `−` and `+` buttons |
| `vacuum.*` | The buttons |
| `person.*`, `device_tracker.*` | The last change |
| `weather.*` | Humidity and wind |
| `switch.*`, `input_boolean.*`, `script.*`, `automation.*` | The power bar (it needs a power sensor) |
| A sensor with `device_class` `battery`, `temperature` or `humidity` | The battery level or the comfort range |
| `on.clock` | The date |

A locked tab shows a short message and this list instead of the options. If the card still has `show_more: true` in YAML, the tab also shows a warning with a **Turn off** button.

When you change the entity to one without a bar, the editor turns off `show_more` and `show_player` for you.

The card itself ignores `show_more` for an entity without a bar. The icon, the name and the state stay where you put them. See `show_more` in the [Configuration Reference](configuration_3.md).

## Colors

Each color field has three parts: a small square, a color picker and a text field. Tap the square to open the picker. In the text field, you can type a color such as `#f0c040` or `rgba(255,255,255,0.5)`.

Some color fields have an **Auto** button too. Press it, or type `auto` in the text field. The square then shows a rainbow with the letter **A**. To go back, type a color.

| Field | Tab | What `auto` does |
|---|---|---|
| Text color, Value color | 📝 Text | The text color of your Home Assistant theme. It follows light and dark mode |
| Icon color, Icon color (ON) | 💡 Icon | The color of the entity, for example the color of a light. If the entity has no color, the default color |
| Color 1, Color 2, Color 3 | 🖼 Background | The card color of your theme |
| Border color | 📐 Size | The border color of your theme |
| Bar color | 🎚 Slider & Power | A soft tint of the text color of your theme |
| Bar icons color, Label color | 🎚 Slider & Power | The text color of your theme |
| Tile / face background | ⚙️ General (clock) | A soft tint of the text color |

The **Card width** and **Card height** fields in the **📐 Size** tab have an **Auto** button too. With auto, the width fills the free space, and the height fits the content of the card.

The colors that follow your theme change at once when you switch the theme.

The color fields are in groups, so you find them in one place:

| Tab | Section | The colors in it |
|---|---|---|
| 📝 Text | **Colors** | Text color, Value color and Shadow color. For a clock: Clock color and Greeting color, and Shadow color for the Digits style. For a calendar: Calendar text color |
| 🎚 Slider & Power | **Bar colors** | Label color, Bar color and Bar icons color. A vacuum has no label, so it has only the last two |

**Bar icons color** shows only for a bar that has buttons or icons: a vacuum, a thermostat, a cover with the `−` and `+` or the ▲ ■ ▼ buttons, a fan with speed buttons or `−` and `+`, and a media player with **Show player buttons** on. For the other bars, such as a slider, a power bar or a battery bar, the field is hidden, because it would change nothing.

The **Shadow color** field shows only while **Text shadow** is on. If you leave a color field empty, the card uses the default look.

## Actions

In the **⚡ Actions** tab, each of tap, double-tap and hold has an **Action**: **Toggle**, **More info**, **Navigate**, **Call service** or **None**.

- **Navigate** adds a **Navigation path** field, for example `/lovelace/0`.
- **Call service** adds a **Service** field, for example `light.turn_on`.

If you do not set an action, tap is **Toggle**, and double-tap and hold are **None**. When hold is **None**, a long press works like a normal tap.

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

## If the editor shows an error message

If the card has a value that the editor cannot show, for example text where a number is expected, the editor shows a short message instead of the tabs. Switch to the code editor, fix the value, and the tabs come back. The reason is written in the browser console (F12), in lines that start with `[PSB editor]`.

> 🔙 Back to the [Main README](../README.md)
