## 🚪 Entry Gate & Garage Control

> 🔙 Back to the [Main README](../README.md)

There is no dedicated "garage mode" built into the card — this layout is fully assembled from existing options: `custom_states_on` to control the active icon, `custom_blockade` to self-lock the card while the gate is in motion, `custom_states_labels` for dynamic status text, and separate small action-button cards using the Service Countdown for Open / Stop / Close.

#### 1. Closed — ready to open

![Piotras Smart Button - Entry Gate Closed](../img/piotras-smart-button-garage-2.jpg)

#### 2. Opening — self-blocking while in motion

![Piotras Smart Button - Entry Gate Going Up](../img/piotras-smart-button-garage-3.jpg)

#### 3. Open — ready to close

![Piotras Smart Button - Entry Gate Open](../img/piotras-smart-button-garage-4.jpg)


- **Status card** — main `cover` entity with icon swapping via `custom_states_on`, label text via `custom_states_labels`.
- **Self-locking** — `custom_blockade` disables all taps while the gate is `opening` or `closing`, preventing accidental re-triggering mid-motion.
- **Action buttons** — separate small cards for Open / Stop / Close, each calling its own `cover` service with a 3-second `circle` countdown for immediate tap feedback.

```yaml
# Status card
type: custom:piotras-smart-button
entity: cover.your_gate
name: Garaż
icon: mdi:garage
icon_on: mdi:garage-open
custom_states_on:
  - open
  - opening
custom_states_labels:
  closed: <b>Garage Closed
  open: <b>Garage Open 🔓
  opening: <b>Going up! 🚀
  closing: <b>Going down! 🛑
custom_blockade:
  - opening
  - closing
card_width: auto
card_height: auto
show_name: false
icon_color: "#ff8080"
icon_color_on: "#80ff80"
state_size: 16
background_color1: "#0080c0"
icon_size: 60
icon_style: none
```

```yaml
# Open button
type: custom:piotras-smart-button
entity: cover.your_gate
custom_states_on:
  - closed
custom_blockade:
  - open
  - opening
  - closing
icon_color_on: "#80ff80"
icon_color: "#ff8080"
card_width: 60
card_height: 60
show_service: true
time_service: 5
service_style: circle
show_state: false
icon_on: mdi:arrow-up-bold-circle
icon: mdi:stop-circle-outline
show_shadow: true
background_color1: "#0080c0"
icon_style: none
icon_size: 40
icon_wrap_size: 45
tap_action:
  action: call-service
  service: script.open.your_gate
```

```yaml
# Close button
type: custom:piotras-smart-button
entity: cover.your_gate
custom_states_on:
  - open
custom_blockade:
  - closed
  - opening
  - closing
icon_color_on: "#80ff80"
icon_color: "#ff8080"
card_width: 60
card_height: 60
show_service: true
time_service: 5
service_style: circle
show_state: false
icon_on: mdi:arrow-down-bold-circle
icon: mdi:stop-circle-outline
show_shadow: true
background_color1: "#0080c0"
icon_style: none
icon_size: 40
icon_wrap_size: 45
tap_action:
  action: call-service
  service: script.close.your_gate
```

Looking for more inspiration or advanced configurations for Entry Gate & Garage Control? Check out these community guides:

> 💬 [Community Dashboards & Showcases (Show and Tell)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README.md)
