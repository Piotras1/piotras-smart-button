## 👤 Person & Device Tracker

When the card detects a `person` or `device_tracker` entity, the Control Zone displays the **last state change time** (HH:MM format) alongside a dynamic icon — home or away. No extra configuration needed beyond enabling `show_more: true`.

Icon colors follow the existing color variables — `icon_color_on` when home, `icon_color` when away — so the bar stays visually consistent with the rest of the card.

![Zrzut ekranu (1292)](https://github.com/user-attachments/assets/51bf40ea-e274-4534-bb04-ab9b0d294421)![Zrzut ekranu (1281)](https://github.com/user-attachments/assets/9805f891-8e9c-409d-941c-173006059b9a)

- `mdi:home` in `icon_color_on` when state is `home`
- `mdi:walk` in `icon_color` when state is `away` / `not_home`
- Time shown as `HH:MM` — language-neutral, no translation needed
- `name_on` / `name_off` and `tap_action` work normally

```yaml
type: custom:piotras-smart-button
entity: person.jan
name: Jan
icon: mdi:account
icon_color: "#aaaaaa"
icon_color_on: "#43d14a"
card_width: 140
card_height: 140
show_image: true
background_image_on: /local/persons/jan.jpg
show_filter: true
show_more: true
slider_height: 26
name_on: In Home
name_off: Outside
tap_action:
  action: more-info
```

Looking for more inspiration or advanced configurations for Person & Device Tracker? Check out these community guides:
 
> 💬 [Community Dashboards & Showcases (Show and Tell)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README.md)