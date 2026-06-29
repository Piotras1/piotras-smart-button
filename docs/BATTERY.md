## 🔋 Battery

When the card detects a `sensor` entity with `device_class: battery`, the Control Zone displays a **color-coded charge bar** (red → yellow → green) and the current charge percentage. The main card icon is automatically replaced with a dynamic battery icon matching the charge level.

For full charging state support, add an optional `entity_battery_state` pointing to a sensor that reports `charging`, `discharging`, `full`, or `not_charging`.

![Zrzut ekranu (1293)](https://github.com/user-attachments/assets/03390d5f-f975-499e-b2b9-e2fdd25e1045)![Zrzut ekranu (1294)](https://github.com/user-attachments/assets/a9a00304-aacd-475c-9cf8-df29503ec3b9)

- Icon auto-adjusts: `mdi:battery-10` through `mdi:battery`, `mdi:battery-charging` when charging
- Card turns **ON** (active color) when `entity_battery_state` reports `charging`
- Card turns **OFF** when `discharging`, `not_charging`, or `full`
- `⚡` suffix appears next to percentage when charging
- `icon_color` and `icon_color_on` still control the icon ring color
- `name_on` / `name_off` work normally — e.g. `name_on: Charging` / `name_off: Discharging`
- `entity_battery_state` is **optional** — the card detects `device_class: battery` automatically and shows the dynamic icon even without it

```yaml
type: custom:piotras-smart-button
entity: sensor.lenovo_battery
entity_battery_state: sensor.lenovo_battery_state
name: Lenovo Battery
icon_color: "#aaaaaa"
icon_color_on: "#43d14a"
card_width: 140
card_height: 120
show_more: true
slider_height: 26
slider_label_color: "#ffffff"
name_on: Charging
name_off: Discharging
tap_action:
  action: more-info
```

Looking for more inspiration or advanced configurations for Battery? Check out these community guides:
 
> 💬 [Community Dashboards & Showcases (Show and Tell)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README.md)