## 🧭 Navigation Mode (Neumorphic Style)
> 🔙 Back to the [Main README](../README.md)

The card works equally well as a navigation controller — icon-only, circular, with a tactile press-and-hold feel.

Key features:

- **Active State Glow** — the current page highlights with a colored glow, immediately visible where you are.
- **Press & Hold feedback** — button stays visually pressed ~500ms before redirecting.
- **Minimalist look** — icon-only with `border_radius: 50` for a perfect circle, no clutter.
- **Still functional** — navigation buttons can still show background statuses (`show_service`, `entity_watts`) in real-time.

![Zrzut ekranu (1174)a](https://github.com/user-attachments/assets/d4a332d1-9811-47a7-8f61-7adacb4270b9)

![Zrzut ekranu (1175)](https://github.com/user-attachments/assets/7856887a-f315-4faf-9a49-18947315f8dc)

```yaml
type: custom:piotras-smart-button
icon: mdi:folder-home
icon_color: "#5badff"
show_filter: false
show_state: false
show_name: false
show_image: false
background_color1: "#5badff"
card_width: 70
card_height: 70
border_radius: 50
icon_size: 44
icon_wrap_size: 48
tap_action:
  action: navigate
  navigation_path: /dashboard-home/0
```