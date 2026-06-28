## 🔊 Media Player

When the card detects a `media_player` entity, it automatically renders a volume slider in the Control Zone. Tap toggles playback (PLAY/STOP). The slider adjusts volume level directly by dragging.

![Zrzut ekranu (1186)](https://github.com/user-attachments/assets/5f67ec1f-95eb-4514-9d24-671c1b00d6b7)![Zrzut ekranu (1188)](https://github.com/user-attachments/assets/13220e11-c6db-43c1-acb2-8e1f64f86b44)


- **Auto-detected** — `volume_level` slider requires no extra configuration.
- **Different backgrounds per state** — `background_image_off` and `background_image_on` show a different image when stopped vs playing.
- **Full toggle** — tap starts or stops playback; hold opens more-info for full media controls.

```yaml
type: custom:piotras-smart-button
entity: media_player.your_media_player
name: Speaker Bedroom
icon: mdi:radio
icon_color_on: "#ff8000"
card_width: 180
card_height: 120
border_width: 1
icon_size: 40
icon_wrap_size: 50
icon_over_size: 5
icon_color: "#c0c0c0"
font_style: 2
name_size: 20
state_size: 20
icon_mode: 1
name_mode: 5
value_mode: 3
show_image: true
show_filter: true
show_more: true
show_icon_full: false
background_image_off: /local/your_image_off.png
background_image_on: /local/your_image_on.png
tap_action:
  action: toggle
hold_action:
  action: more-info
```

Looking for more inspiration or advanced configurations for Media Player? Check out these community guides:
 
> 💬 [Community Dashboards & Showcases (Show and Tell)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README.md)
