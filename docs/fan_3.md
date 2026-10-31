# 🌀 Fan Speed Control

A card for `fan` entities. It shows the fan state, turns the icon while the fan is on, and gives you three ways to set the speed: a slider, one button for each speed, or `−` and `+` buttons.

## What you get

- Tap toggles the fan. Double-tap and hold do nothing by default. You can set all three in the **⚡ Actions** tab.
- The state badge shows `ON` or `OFF`. You can show your own text for each speed with `custom_states_labels` (see below).
- The icon turns while the fan is on. The higher the speed, the faster it turns. If the fan has `direction: reverse`, the icon turns the other way.
- Three ways to control the speed: a **slider**, **speed buttons** or **− / + buttons**. You choose one in the editor.
- If the fan has preset modes, a second slider chooses the preset. It stays under the buttons.
- These options work as usual: `name`, `name_on`, `name_off`, `icon`, `icon_style`, `tap_action`, `double_tap_action`, `hold_action`.

## Setup

- Open the **🎚 Slider & Power** tab and switch on Show slider or power bar (`show_more: true`). Without it, the bar is not shown.
- In the same tab, in the section **Fan control**, choose the Control type (`fan_control`):
   - **Slider** — a slider for the speed in percent. This is the default.
   - **Speed buttons** — a power button and one button for each speed. The button of the current speed is highlighted. The card reads the number of speeds from `percentage_step`, and it needs 2 to 6 speeds. For other fans, the card shows `−` and `+` instead.
   - **− / + buttons** — a `−` button, the current percentage and a `+` button. A tap on the percentage turns the fan on or off. When the speed gets to 0%, the fan turns off.
- For the `−` and `+` buttons, set the step in `− / + step (%)` (`fan_step`, 1–50, default 10). If the fan has its own steps of 5% or more, the card uses them and ignores this value.
- The size of the buttons follows the Bar height (`slider_height`, 16–60 px). For buttons, 24 or more is a good choice.
- To change the colors, use Bar color (`bar_color`) and Bar icons color (`bar_icon_color`) in the same tab. If you leave Bar icons color empty, the icons on the buttons use `icon_color` and `icon_color_on`.
- To turn the spinning icon off, switch off Spin icon when fan is on (`fan_spin: false`).

> 💡 **How many speeds does my fan have?** Open Developer tools → States and look at the `percentage_step` of your fan. `33.33` means 3 speeds, `25` means 4 speeds, `1` means a smooth fan with no steps. A smooth fan works best with the slider or with `−` and `+`.

## Your own text for each speed

The state badge can show a word for each range of the speed. Write the ranges in `custom_states_labels`:

```yaml
custom_states_labels:
  "off": OFF
  "1-40": LOW
  "41-80": MEDIUM
  ">80": HIGH
```

- Write `off` in quotes, so that YAML does not read it as `false`.
- Do not add an `on` key. It would win over the ranges.
- Start the first range at `1`, so that the off fan does not show `LOW`.
- The card checks the keys from top to bottom and uses the first one that fits.

## 📋 Full example

```yaml
type: custom:piotras-smart-button
entity: fan.living_room
name: Fan
icon_color: '#9e9e9e'
icon_color_on: '#4fc3f7'
icon_size: 32
show_more: true
slider_height: 30
fan_control: buttons
fan_spin: true
bar_icon_color: '#4fc3f7'
custom_states_labels:
  "off": OFF
  "1-40": LOW
  "41-80": MEDIUM
  ">80": HIGH
tap_action:
  action: toggle
hold_action:
  action: more-info
```

A fan with a smooth speed, set with `−` and `+`:

```yaml
type: custom:piotras-smart-button
entity: fan.bedroom
show_more: true
fan_control: plusminus
fan_step: 20
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
