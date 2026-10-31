# 🧹 Vacuum

A card for `vacuum` entities. It shows what the vacuum is doing, moves the icon while it cleans, and gives you start, pause and base buttons.

## What you get

- Tap toggles the vacuum. Double-tap and hold do nothing by default. You can set all three in the **⚡ Actions** tab.
- The state badge shows the state of the vacuum in the language of your Home Assistant: for example `CLEANING`, `DOCKED`, `RETURNING`, `PAUSED`, `IDLE`, `ERROR` or `CHARGING`. The card knows English, Polish, German, French, Spanish, Italian and Dutch. For a state it does not know, it shows the state in capital letters.
- The card is lit while the vacuum is `cleaning` or `returning`. In the other states, it is off.
- The icon sweeps from side to side while the vacuum is `cleaning`.
- Three buttons on the bar: ▶ start, ⏸ pause and 🏠 base. They call `vacuum.start`, `vacuum.pause` and `vacuum.return_to_base`.
- These options work as usual: `name`, `name_on`, `name_off`, `icon`, `icon_style`, `tap_action`, `double_tap_action`, `hold_action`.

## Setup

- Open the tab **🧹 Vacuum Buttons** and switch on Show control buttons (start / pause / dock) (`show_more: true`). Without it, the buttons are not shown.
- In the same tab, set the Bar height (`slider_height`, 16–60 px). It sets the size of the bar, of the buttons and of their icons. For buttons, 24 or more is a good choice.
- To change the colors, use Bar color (`bar_color`) and Bar icons color (`bar_icon_color`) in the same tab. If you leave Bar icons color empty, the icons on the buttons use `icon_color` and `icon_color_on`.
- A vacuum card has no power options, so this tab shows only the switch, the bar height and the colors.
- To change the state text, write your own text for each state in `custom_states_labels` (YAML). See below.
- To change which states light the card, set `custom_states_on` (YAML). For example, add `paused` to keep the card lit when the vacuum is paused.
- To configure icon colors, use the following options:
   - icon_color: '#9e9e9e' — sets the icon color when the card is off.
   - icon_color_on: '#4caf50' — sets the icon color when the card is lit.

## Your own text for each state

```yaml
custom_states_labels:
  cleaning: Cleaning now
  returning: Going home
  docked: At home
  paused: Waiting
  error: Needs help
```

The card checks your text first. For a state that you did not write, it uses its own text. The older name `vacuum_states_labels` works too.

## 📋 Full example

```yaml
type: custom:piotras-smart-button
entity: vacuum.robot
name: Robot
icon: mdi:robot-vacuum
icon_color: '#9e9e9e'
icon_color_on: '#4caf50'
icon_size: 32
show_more: true
slider_height: 30
bar_color: 'rgba(0, 0, 0, 0.5)'
bar_icon_color: '#ffffff'
custom_states_labels:
  cleaning: Cleaning now
  returning: Going home
  docked: At home
custom_states_on:
  - cleaning
  - returning
  - paused
tap_action:
  action: more-info
```

## Go further

Everything above works from the visual editor. For special options, see the guides:

- 🏷️ [Custom State Labels](custom_states_labels_3.md) — your own text for each state
- 🎛️ [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md) — decide what counts as "on" and lock the card
- 👁️ [Conditional Visibility](visible_if_3.md) — show or hide the card
- 🧩 [Custom Data & Templates](custom_data_guide_3.md) — your own logic in JavaScript
- 🖱️ [Custom Tap](custom_tap_3.md) — clickable elements and your own panels
- ⚙️ [Configuration Reference](configuration_3.md) — the full list of options

## More ideas

* 🙌 [Show and Tell](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell) — Dashboards and setups shared by the community — good for ideas.
* 🧩 [Custom Data & Modules](https://github.com/Piotras1/piotras-smart-button/discussions/categories/custom-data-modules) — Ready-made custom_data modules to copy and paste.

---

> 🔙 Back to the [Main README](../README.md)
