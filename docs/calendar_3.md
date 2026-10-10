# 🔹 Calendar

A month calendar card. It shows the current month, highlights today and marks days with events. It does not use a Home Assistant entity. You turn it on with a special entity name: `on.calendar`.

![Calendar 1](../img/piotras-smart-button-calendar-1.jpg)
![Calendar 2](../img/piotras-smart-button-calendar-2.jpg)

## What you get

- The card switches to calendar mode when `entity` is `on.calendar`. The entity does not need to exist in Home Assistant.
- A grid with the current month. The month name and the weekday names follow the language of Home Assistant. The week starts on Monday.
- Today is highlighted with a badge. Past days of the current month are dimmed.
- With an events sensor (see below), days with upcoming events are underlined. Tap such a day to see the time, title and description of its events. The list closes after 10 seconds, or when you tap it.
- If events reach into the next month, a small arrow appears next to the month name. Tap it to preview the next month. The card goes back to the current month after 30 seconds.
- The card has no ON and OFF state. Tap on the card itself does nothing by default. You can set your own action for tap, double-tap and hold in the **⚡ Actions** tab.
- These options work as usual: `card_width`, `card_height`, `border_radius`, background and filters.

The calendar uses `card_width` and `card_height` to scale the grid. If you do not set them, it assumes 200 × 180 px. Give the card enough space for the grid.

## Setup

Enter `on.calendar` in **Entity** in the **⚙️ General** tab. The **Calendar display** section appears below, with two options:

- **Extended events entity** (`calendar_extended`) — a sensor with the events. Without it, the card shows the month grid only, with no marked days.
- **Days ahead (event window)** (`calendar_days_ahead`) — how many days from today are checked for events. Range in the editor: 5–30. Default: 7.

Colors:

- **Past days color** (`icon_color`) in the **💡 Icon** tab — the color of the past days and of the event title in the day view.
- **Today badge color** (`icon_color_on`) in the **💡 Icon** tab — the color of the today badge, the underline of marked days and the month arrow.
- **Calendar text color** (`text_color`) in the **📝 Text** tab — the color of the month name, weekday names and day numbers.

The **State font size** (`state_size`) changes the size of the text in the grid.

### Extra entities

| Option | What it is for |
|---|---|
| `calendar_extended` | A sensor with an `events` attribute in the Home Assistant calendar format (`start`, `end`, `summary`, `description`). The card marks the days that have events |

## Minimal example

```yaml
type: custom:piotras-smart-button
entity: on.calendar
calendar_extended: sensor.calendar_extended
card_width: 200
card_height: 180
```

<details>
<summary>Full example</summary>

```yaml
type: custom:piotras-smart-button
entity: on.calendar
calendar_extended: sensor.calendar_extended
calendar_days_ahead: 14
icon_color: "#f0c040"
icon_color_on: "#ffffff"
text_color: "#ffffff"
state_size: 12
card_width: 220
card_height: 200
border_radius: 20
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

## More ideas

> 💬 [Show and Tell](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README_3.md)
