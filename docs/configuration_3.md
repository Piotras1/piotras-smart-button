# ⚙️ Configuration Reference

The full list of options of the card. The options are in the same groups as the tabs of the visual editor. At the end are the options that exist only in YAML.

Every card starts with:

```yaml
type: custom:piotras-smart-button
```

If you do not write an option, the card uses the default value from the tables.

## ⚙️ General

| Option | Default | What it does |
|---|---|---|
| `entity` | none | The entity of the card. Use `on.clock` for a clock and `on.calendar` for a calendar. A card can also work without an entity |
| `name` | none | The name shown on the card |
| `show_icon` | `true` | Shows the icon |
| `show_name` | `true` | Shows the name |
| `show_state` | `true` | Shows the state badge |
| `show_press_effect` | `true` | The card shrinks a little when you press it. Set `false` to turn it off |

## 📐 Size

| Option | Default | What it does |
|---|---|---|
| `card_width` | `140` | Width. A number in px, a percent such as `100%`, or `auto` |
| `card_height` | `120` | Height. A number in px, or `auto` |
| `border_radius` | `12` | Rounded corners in px. Range in the editor: 0–80 |
| `border_width` | `0` | Border width in px. Range in the editor: 0–20 |
| `border_color` | `rgba(255,255,255,0.2)` | Border color |
| `box_shadow` | none | Outer shadow, as a CSS value. For example `0 0 12px rgba(0,0,0,0.8)` |

## 🖼 Background

| Option | Default | What it does |
|---|---|---|
| `background_color1` | `#1a1a2e` | The background color. Use `auto` for the card color of your Home Assistant theme |
| `background_color2` | empty | A second color. With it, the background becomes a gradient |
| `background_color3` | empty | A third color for the gradient |
| `background_gradient_angle` | `135` | The direction of the gradient. Range in the editor: 0–360 |
| `show_image` | `false` | Uses an image as the background instead of the colors |
| `background_image_on` | none | The image when the card is on. For example `/local/images/on.jpg` |
| `background_image_off` | none | The image when the card is off. If empty, the card uses the image for on |
| `show_filter` | `false` | Turns on the filters of the **🎨 Filters** tab |
| `show_shadow` | `true` | The inner shadow of the card |

With only `background_color1`, the background is a solid color. With two or three colors, it is a gradient.

## 🎨 Filters

Filters change the whole background: colors, gradients and images. They work when `show_filter` is `true`.

| Option | Default | What it does |
|---|---|---|
| `image_filter_on` | `brightness(1) saturate(1.1)` | The filter when the card is on |
| `image_filter_off` | `brightness(0.45) saturate(0.2) grayscale(0.5)` | The filter when the card is off |

In the editor, you set each filter with three sliders: **Brightness** (0–2), **Saturate** (0–2) and **Grayscale** (0–1). The editor writes the result as text in the form above.

## 💡 Icon

| Option | Default | What it does |
|---|---|---|
| `icon` | none | The icon, for example `mdi:lightbulb`. If empty, the card shows `mdi:lightning-bolt`. Battery, temperature, humidity and weather cards choose their own icon |
| `icon_on` | none | The icon when the card is on. If empty, the card uses `icon` |
| `icon_color` | `#f0c040` | The icon color when the card is off. Use `auto` to take the color from the light |
| `icon_color_on` | `#ffffff` | The icon color when the card is on. Use `auto` to take the color from the light |
| `icon_size` | `28` | Icon size in px. Range in the editor: 10–100 |
| `icon_wrap_size` | `48` | The size of the circle or square behind the icon, in px. Range in the editor: 20–120 |
| `icon_style` | `circle_color` | The shape behind the icon: `circle_color`, `circle`, `square_color`, `square` or `none` |
| `show_icon_full` | `true` | Keeps the icon inside the card. Set `false` to let it go over the edge |
| `icon_over_size` | `4` | How much the icon goes over the edge, when `show_icon_full` is `false`. A higher value gives a smaller overlap. Range in the editor: 2–10 |

With `auto`, the color comes from the light: its `rgb_color` or `hs_color`. If the entity has no color, the card uses the default.

On a calendar card, `icon_color` and `icon_color_on` are the colors of the past days and of the today badge. See [Calendar](calendar_3.md).

## 📝 Text

| Option | Default | What it does |
|---|---|---|
| `name_size` | `14` | Name font size in px. Range in the editor: 6–40 |
| `state_size` | `12` | State badge font size in px. Range in the editor: 6–40 |
| `text_color` | `#ffffff` | The color of the name |
| `value_color` | the value of `text_color` | The color of the state badge |
| `font_style` | `1` | `1` normal, `2` small caps, `3` monospace, `4` uppercase. Range in the editor: 1–4 |
| `name_on` | none | The text of the state badge when the card is on |
| `name_off` | none | The text of the state badge when the card is off |

`name_on` and `name_off` work only when both are set.

On a clock card, `name_size` and `text_color` are the size and color of the clock, and `state_size` and `value_color` are the size and color of the greeting. See [Clock](clock_3.md).

## 🔲 Layout

The card has nine places, numbered like this:

| | | |
|---|---|---|
| ↖ `1` | ↑ `2` | ↗ `3` |
| ← `4` | · `5` | → `6` |
| ↙ `7` | ↓ `8` | ↘ `9` |

| Option | Default | What it does |
|---|---|---|
| `icon_mode` | `5` | The place of the icon |
| `name_mode` | `5` | The place of the name |
| `value_mode` | `5` | The place of the state badge |

Elements in the same place stack: icon, then name, then state.

## 🎚 Slider & Power

| Option | Default | What it does |
|---|---|---|
| `show_more` | `false` | Shows the bar at the bottom of the card. What it shows depends on the entity: a slider, the power, the battery level, the comfort value, the thermostat buttons, the last change, humidity and wind, or the date on a clock |
| `slider_height` | `26` | Bar height in px. Range in the editor: 16–60 |
| `slider_label_color` | `rgba(255,255,255,0.85)` | The color of the text on the bar |
| `show_player` | `false` | Shows the player buttons of a media player |
| `player_mode` | `1` | The layout of the player bar: `1` volume at the bottom and buttons above, `2` buttons left and volume right, `3` volume left and buttons right |
| `player_height` | `28` | The size of the player buttons in px. Range in the editor: 16–80 |
| `entity_battery_state` | none | The sensor with the charging state of a battery |
| `comfort_min` | none | The lowest comfortable value of a temperature or humidity sensor. Range in the editor: -50 to 100 |
| `comfort_max` | none | The highest comfortable value of a temperature or humidity sensor. Range in the editor: -50 to 100 |
| `entity_watts` | none | The power sensor of a socket |
| `max_watts` | `2000` | The power in watts that fills the whole power bar. Range in the editor: 100–10000 |
| `con_warning` | `80` | The percent of the power bar above which it pulses. Use `false` to turn the warning off. Range in the editor: 1–100 |

## 📄 Service

| Option | Default | What it does |
|---|---|---|
| `show_service` | `false` | Shows a countdown after a **Call service** action |
| `blockade_card` | `false` | Locks the card during the countdown |
| `time_service` | `10` | The length of the countdown in seconds. Range in the editor: 5–60 |
| `service_style` | `circle` | `circle` or `bar`. With `show_more` on, the card always uses the bar |

## ⚡ Actions

| Option | Default | What it does |
|---|---|---|
| `tap_action` | `toggle` | A short tap |
| `double_tap_action` | `none` | Two taps one after another |
| `hold_action` | `none` | A press of half a second |

Each action has these fields in the editor:

| Field | What it does |
|---|---|
| `action` | `toggle`, `more-info`, `navigate`, `call-service` or `none` |
| `navigation_path` | The path to open, for `navigate`. For example `/lovelace/0` |
| `service` | The service to call, for `call-service`. For example `light.turn_on` |

## 🕒 Clock

| Option | Default | What it does |
|---|---|---|
| `clock_mode` | `1` | `1` digits, `2` flip tiles, `3` analog, `4` LED |
| `clock_tile_bg` | none | The background of the tiles, the analog face or the LED panel |
| `clock_12h` | `false` | 12-hour format with AM/PM |

## 📅 Calendar

| Option | Default | What it does |
|---|---|---|
| `calendar_extended` | none | The sensor with the events |
| `calendar_days_ahead` | `7` | How many days from today are checked for events. Range in the editor: 5–30 |

## Options only in YAML

These options are not in the visual editor.

| Option | What it does | Guide |
|---|---|---|
| `custom_data` | Reads entities and calculates values with JavaScript | [Custom Data & Templates](custom_data_guide_3.md) |
| `custom_tap` | Your own clickable elements inside the card | [Custom Tap](custom_tap_3.md) |
| `custom_states_labels` | Your own text for each state, or for each range of numbers | [Custom State Labels](custom_states_labels_3.md) |
| `custom_states_on` | Which states count as "on" | [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md) |
| `custom_blockade` | In which states the card is locked | [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md) |
| `visible_if` | Shows or hides the card | [Conditional Visibility](visible_if_3.md) |

More values that you can write only in YAML:

| Option | What you can write |
|---|---|
| `tap_action`, `double_tap_action`, `hold_action` | `service_data` for `call-service`, and `entity` for `toggle` and `more-info`. Without `entity`, the action uses the entity of the card |
| `max_watts` | The name of a sensor instead of a number, for example `sensor.socket_max_power`. The card takes the maximum from the state of that sensor |
| `time_service` | Two numbers, for example `"5/10"`. The countdown starts at 5 seconds of 10 |

Older names that still work: `vacuum_states_labels` (the same as `custom_states_labels`) and `vacuum_states_on` (works only for entities that are not a vacuum).

## Templates

You can use a template or a reference to `custom_data` in most of the options above. See [Custom Data & Templates](custom_data_guide_3.md) for the list.

## Guides by type of entity

| Entity | Guide |
|---|---|
| Light | [Light & Auto-Dimmer Slider](light_3.md) |
| Socket and switch | [Socket & Power Monitoring](socket_3.md) |
| Thermostat | [Thermostat](thermostat_3.md) |
| Person and device tracker | [Person & Device Tracker](person_3.md) |
| Battery | [Battery](battery_3.md) |
| Temperature sensor | [Temperature Comfort](temperature_3.md) |
| Humidity sensor | [Humidity Comfort](humidity_3.md) |
| Media player | [Media Player](media_3.md) |
| Script | [Script Button](script_3.md) |
| Alarm | [Advanced Alarm Status](alarm_3.md) |
| Weather | [Mini Weather Card](weather_3.md) |
| Vacuum | [Vacuum](vacuum_3.md) |
| Garage door and cover | [Garage Door & Cover](garage_3.md) |
| Clock | [Clock](clock_3.md) |
| Calendar | [Calendar](calendar_3.md) |
| Small buttons | [Chip Buttons](chip_3.md) |
| Menu of views | [Navigation Mode](navigation_3.md) |

> 🔙 Back to the [Main README](../README_3.md)
