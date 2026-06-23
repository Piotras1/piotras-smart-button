## 🌡️ Thermostat

When the card detects a `climate` entity, the Control Zone displays **temperature control buttons** — `−` on the left, target temperature in the center, `+` on the right. Each press adjusts the target temperature by **0.5°**. The state badge on the main card automatically shows the **current room temperature** instead of ON/OFF.

The card turns **ON** (active color) when `hvac_action` is `heating` or `cooling`, and **OFF** when `idle` or `off`. When the thermostat is fully off, the Control Zone shows a red **OFF** label instead of the temperature buttons.

![Piotras Smart Button - Thermostat Control States](../img/piotras-smart-button-thermostat.jpg)

- Target temperature displayed `2px` larger than other bar labels for quick readability
- `−` / `+` buttons styled as small circles, spread to card edges
- Room temperature shown as state badge (e.g. `21.5°`) — replaces ON/OFF labels
- `icon` and icon colors set freely — the card does not override them for climate
- `slider_height` and `slider_label_color` apply normally

```yaml
type: custom:piotras-smart-button
entity: climate.salon
name: Salon
icon: mdi:sofa
icon_color: "#aaaaaa"
icon_color_on: "#ff6b35"
card_width: 160
card_height: 140
show_image: true
background_image_on: /local/rooms/salon.jpg
show_filter: true
show_more: true
slider_height: 32
slider_label_color: "#ffffff"
tap_action:
  action: more-info
```

Looking for more inspiration or advanced configurations for Thermostat ? Check out these community guides:
 
> 💬 [Community Dashboards & Showcases (Show and Tell)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README.md)
