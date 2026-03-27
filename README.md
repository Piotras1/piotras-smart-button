##  Piotras Smart Button
### Release v1.0.0

A high-performance Home Assistant button card designed for ultimate control and deep customization.  
Features a unique 9-grid layout system, dynamic background effects, and auto-detecting control sliders.

## ✨ Features
- **9-Grid Element Placement** – independently position Icon, Name, and State.
- **Adaptive Auto-Sliders** – smart detection of Brightness, Temp, Volume, and more.
- **Dynamic Backgrounds** – support for 2/3-color gradients and filtered images.
- **Advanced Action Engine** – optimized Tap, Double Tap, and Hold for mobile/desktop.
- **Power Monitoring** – integrated bar with configurable safety warnings.

---
![Zrzut ekranu (1115)a](https://github.com/user-attachments/assets/8913b4f7-18ce-41cb-9460-546c6f49b9fd)![Zrzut ekranu (1116)a](https://github.com/user-attachments/assets/39951fd9-a3b5-47df-97e9-a55047fe8183)![Zrzut ekranu (1115)b](https://github.com/user-attachments/assets/10f1c332-7c81-493d-b757-ba618669feec)![Zrzut ekranu (1116)b](https://github.com/user-attachments/assets/d1a0fa72-6b9b-4cd9-9083-78dc696b0ea0)

---

## 🧩 Layout: The 9-Grid System

### Description
The card uses a unique grid system (1-9) to position elements, giving you complete freedom over the UI structure. The grid is a 3x3 matrix:
- **1, 2, 3** – Top Row (Left, Center, Right)
- **4, 5, 6** – Middle Row (Left, Center, Right)
- **7, 8, 9** – Bottom Row (Left, Center, Right)

By assigning `icon_mode`, `name_mode`, and `value_mode` to these numbers, you can overlap elements or separate them into distinct corners.

---

![Zrzut ekranu (1117)a](https://github.com/user-attachments/assets/fbd34755-2e10-412e-9e66-4777cffbddb9)![Zrzut ekranu (1118)a](https://github.com/user-attachments/assets/30afccae-2acc-429d-9375-f72dcb3fe128)![Zrzut ekranu (1117)b](https://github.com/user-attachments/assets/4c9e7e84-35d0-4459-bdcc-73dd32a5b047)![Zrzut ekranu (1118)b](https://github.com/user-attachments/assets/c6297306-ba6b-44f4-a438-d053dcf8c3c4)

---

### Visual Hierarchy & Style

The card is built on a layered architecture to ensure maximum readability even with complex backgrounds:

* **Dynamic Background Layer**: Supports solid colors, 2/3-color gradients, or high-resolution images.
* **Smart Filter Engine**: When `show_filter` is enabled, the card automatically applies CSS filters (brightness, saturation, grayscale) to the background image based on the entity's state (ON/OFF).
* **Glassmorphic Overlays**: Elements like the **Icon Wrap** and **Control Zone** use backdrop-blur and semi-transparent backgrounds to pop against the main image.
* **Glow & Shadow Effects**: Icons and text feature multi-layered drop shadows and glows (tinted by `icon_color`) to maintain high contrast regardless of the background complexity.
* **Interactive Controls**: Sliders and Power Bars are docked at the bottom in a dedicated translucent "Control Zone" that appears only when `show_more` is active, keeping the main UI clean.

### Configuration Example

```yaml
type: custom:smart-button-card
entity: light.living_room
name: "Salon"
icon: mdi:lightbulb
card_width: 140
card_height: 140
show_more: true
icon_mode: 5
name_mode: 8
value_mode: 9
```

---

## 🧩 Layout: Power & Safety Monitor
### Description

- **Designed for smart plugs and sockets, this mode provides immediate visual feedback on current power consumption.**
- **Dynamic Fill**– The bar fills based on max_watts ratio.
- **Visual Alert** – If consumption exceeds con_warning (%), the bar triggers a pulse animation to warn the user.

```yaml
type: custom:smart-button-card
entity: switch.coffee_maker
name: "Coffee Machine"
entity_watts: sensor.coffee_machine_power
max_watts: 2500
con_warning: 85
show_more: true
icon_color_on: "#ffffff"
icon_color: "#f0c040"
```


## ⚙️ Configuration Reference

### Card-level options
| Option | Type | Default | Description |
|---|---|---|---|
| `entity` | string | — | Main entity ID |
| `name` | string | — | Display name |
| `icon` | string | `mdi:lightning-bolt` | MDI icon |
| `card_width` | number | `140` | Width in px |
| `card_height` | number/string| `120` | Height in px or `"auto"` |
| `border_radius` | number | `12` | Corner radius (px) |
| `border_width` | number | `0` | Border width (px) |
| `border_color` | string | `"rgba(255,255,255,0.2)"` | Border color |
| `show_icon` | boolean | `true` | Show or hide icon |
| `show_name` | boolean | `true` | Show or hide name |
| `show_state` | boolean | `true` | Show or hide state label |
| `show_more` | boolean | `false` | Enable Sliders or Power Bar zone |

### Visual Appearance & Colors
| Option | Type | Default | Description |
|---|---|---|---|
| `background_color1` | string | `#1a1a2e` | Primary background color |
| `background_gradient` | list | `null` | List of colors for gradient |
| `background_gradient_angle`| number| `135` | Angle of the gradient (deg) |
| `icon_color` | string | `#f0c040` | Icon color when OFF |
| `icon_color_on` | string | `#ffffff` | Icon color when ON |
| `icon_size` | number | `28` | Icon size (px) |
| `icon_wrap_size` | number | `48` | Glow ring size (px) |
| `text_color` | string | `#ffffff` | Color for name and state |
| `name_on` | string | `null` | Custom state label when ON |
| `name_off` | string | `null` | Custom state label when OFF |

### Background Images & Filters
| Option | Type | Default | Description |
|---|---|---|---|
| `show_image` | boolean | `false` | Enable background images |
| `background_image_on` | string | `null` | Image path for ON state |
| `background_image_off` | string | `null` | Image path for OFF state |
| `show_filter` | boolean | `false` | Enable CSS filters on images |
| `image_filter_on` | string | `"brightness(1) saturate(1.1)"` | Filter for ON state |
| `image_filter_off` | string | `"brightness(0.45)..."` | Filter for OFF state |

### Power Bar Monitoring
| Option | Type | Default | Description |
|---|---|---|---|
| `entity_watts` | string | `null` | Sensor for power monitoring |
| `max_watts` | number | `2000` | Max scale for power bar |
| `con_warning` | number | `80` | Threshold (%) for pulse warning |

### Actions
| Option | Type | Default | Description |
|---|---|---|---|
| `tap_action` | object | `toggle` | Single tap action |
| `double_tap_action`| object | `more-info`| Double tap action |
| `hold_action` | object | `more-info`| Long press action |

---
*Created by Piotras. Strictly engineered for reliability.*
