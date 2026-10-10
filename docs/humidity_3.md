# 🔹 Humidity Comfort

A card for humidity sensors. It works with any entity that has `device_class: humidity`. You set a comfort range, and the card shows whether the current value is inside it.

![Humidity Comfort 1](../img/piotras-smart-button-humidity-1.jpg)
![Humidity Comfort 2](../img/piotras-smart-button-humidity-2.jpg)

## What you get

- The card turns ON when the humidity is inside your comfort range (`comfort_min` to `comfort_max`, both values included). It is OFF when the value is outside the range.
- Without a comfort range, the card is always OFF. Set both values.
- The icon is `mdi:water-percent` when the value is in range and `mdi:water-alert` when it is not. You can replace it with your own `icon` and `icon_on`.
- The icon uses `icon_color` when the card is off and `icon_color_on` when it is on.
- A bar at the bottom of the card shows the current value in percent, for example `45%`. It appears when you switch on the slider bar.
- The icon in the bar changes color with the range: blue when the value is below the range, green when it is inside, red when it is above. Without a comfort range, the bar icon uses the label color.
- The state badge shows `ON` when the value is in range and `OFF` when it is not.
- Tap toggles the entity. Double-tap and hold do nothing by default. For a sensor, you will usually want to change tap to **More info** in the **⚡ Actions** tab.
- These options work as usual: `name`, `name_on`, `name_off`, `icon`, `icon_on`, `icon_style`, `tap_action`, `double_tap_action`, `hold_action`.

## Setup

Open the **🎚 Slider & Power** tab and switch on **Show slider or power bar** (`show_more: true`). In the **Comfort range** section, fill in:

- **Comfort min** (`comfort_min`) — the lowest comfortable value in percent.
- **Comfort max** (`comfort_max`) — the highest comfortable value in percent.

The editor accepts values from -50 to 100 in steps of 0.5.

In the same tab you can set the **Bar height** (`slider_height`, 16–60 px) and the **Label color** (`slider_label_color`).

To change the state text, use the **📝 Text** tab: **Custom state (ON)** (`name_on`) and **Custom state (OFF)** (`name_off`). For example, `Comfort` and `Check air`. Fill in both. If only one is set, the card ignores them.

## Minimal example

```yaml
type: custom:piotras-smart-button
entity: sensor.humidity
name: Humidity
show_more: true
comfort_min: 40
comfort_max: 60
```

<details>
<summary>Full example</summary>

```yaml
type: custom:piotras-smart-button
entity: sensor.humidity
name: Humidity
icon_color: "#f0c040"
icon_color_on: "#69f0ae"
icon_style: circle_color
icon_size: 28
name_on: Comfort
name_off: Check air
show_more: true
comfort_min: 40
comfort_max: 60
slider_height: 30
slider_label_color: "rgba(255,255,255,0.85)"
card_width: 140
card_height: 120
tap_action:
  action: more-info
double_tap_action:
  action: none
hold_action:
  action: more-info
```

</details>

## Go further

Everything above works from the visual editor. For special options, see the guides:

- 🏷️ [Custom State Labels](custom_states_labels_3.md) — your own text for each state
- 🎛️ [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md) — decide what counts as "on" and lock the card
- 👁️ [Conditional Visibility](visible_if_3.md) — show or hide the card
- 🧩 [Custom Data & Templates](custom_data_guide_3.md) — your own logic in JavaScript
- 🖱️ [Custom Tap](custom_tap_3.md) — clickable elements and your own panels
- ⚙️ [Configuration Reference](configuration_3.md) — the full list of options

## More ideas

> 💬 [Show and Tell](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README_3.md)
