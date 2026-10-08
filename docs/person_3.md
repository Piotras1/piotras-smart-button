# 👤 Person & Device Tracker

Shows when a person or device last changed state, with a home/away icon. Works with `person` and `device_tracker`.

![Person & Device Tracker 1](../img/piotras-smart-button-person-1.jpg)
![Person & Device Tracker 2](../img/piotras-smart-button-person-2.jpg)

## What you get
- Last state change time in the Control Zone, as `HH:MM` (no translation needed)
- Home: `mdi:home` in `icon_color_on` · Away: `mdi:walk` in `icon_color`
- `name_on`, `name_off` and `tap_action` work as usual

## Setup
Turn on the Control Zone in the visual editor (`show_more: true` in YAML). Nothing else is required.

## Minimal example
```yaml
type: custom:piotras-smart-button
entity: person.jan
show_more: true
```

<details>
<summary>Full example with photo background</summary>

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

</details>

## More ideas
> 💬 [Show and Tell](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)
> 🔙 Back to the [Main README](../README_3.md)
