# 🔹 Person & Device Tracker

A card for `person` and `device_tracker` entities. It shows whether someone is home and how long ago the state changed.

![Person & Device Tracker 1](../img/piotras-smart-button-person-1.jpg)
![Person & Device Tracker 2](../img/piotras-smart-button-person-2.jpg)

## What you get

- The card turns ON when the state is `home`. In every other state it is OFF.
- The icon uses `icon_color_on` when the person is home and `icon_color` when away. You can set a different icon for the on state with `icon_on`.
- The state badge shows `ON` or `OFF`. To show `Home` and `Away` instead, use `name_on` and `name_off` (see Setup).
- A bar at the bottom of the card shows when the state last changed. It appears when you switch on the slider bar.
- The bar has its own icon: a house when the person is home, a walking figure when away. It uses the same colors as the main icon.
- Tap toggles the entity. Double-tap and hold do nothing by default. For a tracker, you will usually want to change tap to **More info** in the **⚡ Actions** tab.
- These options work as usual: `name`, `icon`, `icon_style`, `tap_action`, `double_tap_action`, `hold_action`.

## Setup

Open the **🎚 Slider & Power** tab and switch on **Show slider or power bar** (`show_more: true`). Without it, the last-change bar is not shown.

The bar shows:

- `now` or minutes ago (for example `5 min. ago`) when the change was less than an hour ago.
- The time as `HH:MM` when the change was less than 24 hours ago.
- Days or weeks ago (for example `2 days ago`) for older changes.

For a `device_tracker`, the card uses the `last_seen` attribute when the tracker has one. Otherwise it uses the time of the last state change. For a `person`, it always uses the time of the last state change.

The relative text follows the language of Home Assistant. If the language is not supported, the bar shows short English text such as `now`, `5min`, `2d`.

In the same tab you can set the **Bar height** (`slider_height`, 16–60 px) and the **Label color** (`slider_label_color`).

To change the state text, use the **📝 Text** tab: **Custom state (ON)** (`name_on`) and **Custom state (OFF)** (`name_off`). Fill in both. If only one is set, the card ignores them.

## Minimal example

```yaml
type: custom:piotras-smart-button
entity: person.jan
name: Jan
show_more: true
```

<details>
<summary>Full example</summary>

```yaml
type: custom:piotras-smart-button
entity: person.jan
name: Jan
icon: mdi:account
icon_on: mdi:account-check
icon_color: "#f0c040"
icon_color_on: "#69f0ae"
icon_style: circle_color
icon_size: 28
name_on: Home
name_off: Away
show_more: true
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
