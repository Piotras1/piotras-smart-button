# 🪟 Blinds & Shutters

A card for `cover` entities that have a position: blinds, roller shutters, curtains and awnings. It shows the state, picks the icon for you, and lets you set the position with a slider, with `−` and `+` buttons or with open, stop and close buttons.

For a gate or a garage door, see [Gates & Garage Doors](garage_3.md).

## What you get

- Tap toggles the cover: it opens when closed and closes when open. Double-tap and hold do nothing by default. You can set all three in the **⚡ Actions** tab.
- The state badge shows `CLOSED`, `OPEN`, `OPENING` or `CLOSING`. If the cover is only partly open, it shows the position, for example `40%`.
- The card is lit while the cover is open, and also while it is opening or closing.
- The icon is chosen for you by the type of the cover and by its state: one icon for closed, one for open, and an arrow while it moves. A cover that is only partly open uses the open icon. An icon that you set always wins.
- Three ways to control the cover: a **position slider**, **− / + buttons** or **▲ ■ ▼ buttons**. You choose one in the editor.
- These options work as usual: `name`, `name_on`, `name_off`, `icon`, `icon_style`, `tap_action`, `double_tap_action`, `hold_action`.

## Setup

- Open the **🎚 Slider & Power** tab and switch on Show slider or power bar (`show_more: true`). Without it, the bar is not shown.
- In the same tab, in the section **Cover control**, choose the Control type (`cover_control`):
   - **Slider** — a slider for the position, from 0% (closed) to 100% (open). This is the default. It needs a cover that reports `current_position`.
   - **− / + buttons (position)** — a `−` button, the position in percent and a `+` button. A tap on the position opens or closes the cover, or stops it while it moves. It needs a cover that reports and can set the position. For other covers, the card shows the ▲ ■ ▼ buttons.
   - **▲ ■ ▼ buttons (open / stop / close)** — three buttons that open, stop and close the cover. The card shows only the buttons that the cover supports. While the cover moves, the button of that direction is highlighted.
- For the `−` and `+` buttons, set the step in `− / + step (%)` (`cover_step`, 1–50, default 10).
- The size of the buttons follows the Bar height (`slider_height`, 16–60 px). For buttons, 24 or more is a good choice.
- To change the colors, use Bar color (`bar_color`) and Bar icons color (`bar_icon_color`) in the same tab. If you leave Bar icons color empty, the icons on the buttons use `icon_color` and `icon_color_on`.
- To choose the icon type, open the **💡 Icon** tab and set Cover type (automatic icon) (`cover_type`). With `auto`, the card takes the type from the `device_class` of the cover. Leave the Icon field empty to use the automatic icon.

| Cover type | Closed | Open |
|---|---|---|
| Shutter / roller (also the default) | `mdi:window-shutter` | `mdi:window-shutter-open` |
| Blind | `mdi:blinds` | `mdi:blinds-open` |
| Curtain | `mdi:curtains-closed` | `mdi:curtains` |
| Window | `mdi:window-closed` | `mdi:window-open` |

## Your own text

You can show your own text for each state and for each range of the position:

```yaml
custom_states_labels:
  closed: Closed
  opening: Going up
  closing: Going down
  ">=100": Open
  "1-99": Partly open
```

- Do not add an `open` key if you want the ranges to work. It would win over them.
- The card checks the keys from top to bottom and uses the first one that fits.

If you use `custom_states_on`, add `opening` and `closing` to the list. Then the card stays lit while the cover moves.

## 📋 Full example

```yaml
type: custom:piotras-smart-button
entity: cover.bedroom_blind
name: Bedroom
icon_color: '#9e9e9e'
icon_color_on: '#ffd54f'
icon_size: 32
show_more: true
slider_height: 30
cover_control: plusminus
cover_step: 25
cover_type: blind
bar_icon_color: '#ffd54f'
tap_action:
  action: toggle
hold_action:
  action: more-info
```

The same cover with open, stop and close buttons:

```yaml
type: custom:piotras-smart-button
entity: cover.bedroom_blind
show_more: true
cover_control: buttons
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
