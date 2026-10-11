# 🔊 Media Player

A card for `media_player` entities. It shows whether the player is playing and adds a volume slider and player buttons.

![Media Player](../img/piotras-smart-button-media-v3.jpg)

## What you get

- The card turns ON when the player is playing. The state badge shows `PLAY` when it is playing and `STOP` in every other state, including paused and idle.
- The icon uses `icon_color` when the card is off and `icon_color_on` when it is on. You can set a different icon for the on state with `icon_on`.
- A volume slider (0–100%) appears automatically if the player reports its volume. It appears when you switch on the slider bar.
- Player buttons appear when you switch on **Show player buttons**: previous, play/pause, next and power.
- When the player is off or unavailable, the player bar shows only the power button. It turns the player on. The volume slider is hidden.
- Three layouts for the player bar. You can also change the height of the bar.
- Tap toggles the player. Double-tap and hold do nothing by default. You can set all three in the **⚡ Actions** tab.
- Everything else works as on any other card: name, icon, colors, size and actions.

## Setup

Open the **🎚 Slider & Power** tab. There are two toggles:

- **Show slider or power bar** (`show_more: true`) — shows the volume slider.
- **Show player buttons** (`show_player: true`) — shows the player buttons.

You can use one toggle or both. When both are on, the **Player bar layout** (`player_mode`) option appears:

| Value | Layout |
|---|---|
| `1` | Volume slider at the bottom, buttons above it |
| `2` | Buttons on the left, volume on the right, in one row |
| `3` | Volume on the left, buttons on the right, in one row |

If only **Show player buttons** is on, the buttons are shown without the volume slider and the layout option is hidden.

Other options in the same tab:

- **Player bar height** (`player_height`, 16–80 px, default 28) — changes the size of the buttons.
- **Bar height** (`slider_height`, 16–60 px) and **Label color** (`slider_label_color`) — the volume slider.

To change the state text, use the **📝 Text** tab: **Custom state (ON)** (`name_on`) and **Custom state (OFF)** (`name_off`). For example, `Playing` and `Silent`. Fill in both. If only one is set, the card ignores them.

## Minimal example

```yaml
type: custom:piotras-smart-button
entity: media_player.living_room
name: Living Room
icon: mdi:speaker
show_more: true
show_player: true
```

<details>
<summary>📋 <code>Full example from screenshot</code></summary>
<br>

```yaml
type: custom:piotras-smart-button
entity: media_player.living_room
name: Living Room
card_width: 200
card_height: 120
icon: mdi:speaker
icon_on: mdi:speaker-wireless
icon_color: '#ff8282'
icon_color_on: '#ffffff'
icon_size: 30
icon_wrap_size: 40
name_on: Playing
name_off: Silent
show_more: true
show_player: true
player_mode: 1
icon_mode: 1
name_mode: 3
value_mode: 3
name_size: 17
state_size: 15
background_color1: '#0080c0'
tap_action:
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

## 🛠️ Ready-to-Use

> 🧩 **[Dynamic Background & Artwork (`mediaBgLogic`)](https://github.com/Piotras1/piotras-smart-button/discussions/13)**
>
> <a href="https://github.com/Piotras1/piotras-smart-button/discussions/13"><img src="https://github.com/user-attachments/assets/cff136f0-d864-40a2-b051-a9c00636b690" width="90" alt="Dynamic Background Preview"></a>

> 🧩 **[Dynamic Track Info & Text Display (`mediaInfoLogic`)](https://github.com/Piotras1/piotras-smart-button/discussions/15)**
>
> <a href="https://github.com/Piotras1/piotras-smart-button/discussions/15"><img src="https://github.com/user-attachments/assets/f4010e30-3a1e-49cc-b65d-6830e6e0fa5e" width="90" alt="Dynamic Track Info Preview"></a>

## More ideas

* 🙌 [Show and Tell](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell) — Dashboards and setups shared by the community — good for ideas.
* 🧩 [Custom Data & Modules](https://github.com/Piotras1/piotras-smart-button/discussions/categories/custom-data-modules) — Ready-made custom_data modules to copy and paste.

---

> 🔙 Back to the [Main README](../README.md)
