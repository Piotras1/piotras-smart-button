# 🚪 Gates & Garage Doors

A card for `cover` entities that work like a gate or a garage door. It shows the state, picks the icon for you, and gives you open, stop and close buttons. The buttons can call the service of the cover, or your own scripts.

For blinds and shutters with a position, see [Blinds & Shutters](blinds_3.md).

## What you get

- Tap toggles the cover. Double-tap does nothing by default, and a long press works like a normal tap until you set a hold action. You can set all three in the **⚡ Actions** tab. See the warning about the long press below.
- The state badge shows `CLOSED`, `OPEN`, `OPENING` or `CLOSING`. You can write your own texts, see [Your own text for each state](#your-own-text-for-each-state).
- The card is lit while the gate is open, and also while it is opening or closing.
- The icon is chosen for you by the type and by the state: one icon for closed, one for open, and an arrow while it moves. An icon that you set always wins.
- Three buttons on the bar: ▲ open, ■ stop and ▼ close. The card shows only the buttons that the cover supports.
- While the gate moves, the button of that direction is highlighted. The ▲ button is dimmed when the gate is fully open, and the ▼ button when it is closed. A dimmed button still works.
- Each button can call your own script instead of the service of the cover.
- These options work as usual: `name`, `name_on`, `name_off`, `icon`, `icon_style`, `tap_action`, `double_tap_action`, `hold_action`.

## Setup

- Open the **🎚 Slider & Power** tab and switch on Show slider or power bar (`show_more: true`). Without it, the buttons are not shown.
- In the same tab, in the section **Cover control**, set the Control type (`cover_control`) to **▲ ■ ▼ buttons (open / stop / close)**.
- To use your own scripts, set an action for each button. In the same section you find three fields: **▲ Open button**, **■ Stop button** and **▼ Close button** (`cover_up_action`, `cover_stop_action`, `cover_down_action`). For each of them:
   - **Native cover service** — the button calls `cover.open_cover`, `cover.stop_cover` or `cover.close_cover`. This is the default.
   - **Call service (e.g. script)** — the button calls the service that you write in the field **Service**, for example `script.gate_open`.
- A button with your own script is shown even when the cover does not support that function.
- To pass data to a service, use YAML (`service_data`). The editor has no field for it.
- To choose the icon, open the **💡 Icon** tab and set Cover type (automatic icon) (`cover_type`) to **Gate** or **Garage door**. If you leave it on `auto`, the card takes the type from the `device_class` of the cover. Leave the Icon field empty to use the automatic icon.
- The size of the buttons follows the Bar height (`slider_height`, 16–60 px). For buttons, 24 or more is a good choice.
- To change the colors, use Bar color (`bar_color`) and Bar icons color (`bar_icon_color`) in the same tab. If you leave Bar icons color empty, the icons on the buttons use `icon_color` and `icon_color_on`. Both fields have an **Auto** button: `bar_color: auto` is a soft tint of the text color of your Home Assistant theme, and `bar_icon_color: auto` is the text color of your theme.

| Cover type | Closed | Open | Opening / closing |
|---|---|---|---|
| Gate | `mdi:gate` | `mdi:gate-open` | `mdi:arrow-up-box` / `mdi:arrow-down-box` |
| Garage door | `mdi:garage` | `mdi:garage-open` | `mdi:arrow-up-box` / `mdi:arrow-down-box` |
| Door | `mdi:door-closed` | `mdi:door-open` | `mdi:arrow-up-box` / `mdi:arrow-down-box` |

> 💡 **A button is missing?** The card reads the functions of the cover from its `supported_features`. A cover without a stop function has no ■ button. If you control the cover with scripts, add an action for that button, and it will show.

> 💡 **The gate does not light up while it moves?** If you use `custom_states_on`, add `opening` and `closing` to the list.

> ⚠️ **A long press can open the gate.** A long press works like a tap. If the tap is **Toggle** and you did not set a hold action, a long press also toggles the gate. To be safe, set the tap or the hold to **More info**, as in the examples below.

## Your own text for each state

With `custom_states_labels` (YAML), you can write your own text for the state badge, for example in your own language. The card checks it first, so it has priority over `name_on` and `name_off`.

```yaml
custom_states_labels:
  open: Gate is open
  closed: Gate is closed
  opening: Opening...
  closing: Closing...
  unavailable: No connection
```

Two things to remember:

- A cover that is only partly open shows its position in percent. If you write a text for `open`, the text is shown instead of the percentage.
- If you do not write `unavailable`, a gate that is unavailable shows `CLOSED`. A text for `unavailable` shows the real situation.

## 📋 Full example

A garage door that you control with its own service:

```yaml
type: custom:piotras-smart-button
entity: cover.garage_door
name: Garage
icon_color: '#9e9e9e'
icon_color_on: '#ff9800'
icon_size: 32
show_more: true
slider_height: 30
cover_control: buttons
cover_type: garage
tap_action:
  action: more-info
```

A gate that you control with scripts:

```yaml
type: custom:piotras-smart-button
entity: cover.driveway_gate
name: Gate
show_more: true
slider_height: 30
cover_control: buttons
cover_type: gate
bar_icon_color: '#ffb74d'
cover_up_action:
  action: call-service
  service: script.gate_open
cover_stop_action:
  action: call-service
  service: script.gate_stop
cover_down_action:
  action: call-service
  service: script.gate_close
tap_action:
  action: more-info
```

For a gate, `more-info` is a safe action for a tap. The toggle of some gates opens and closes in turns, so you may not want it on a short tap.

## Go further

Everything above works from the visual editor, except `custom_states_labels`. For special options, see the guides:

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
