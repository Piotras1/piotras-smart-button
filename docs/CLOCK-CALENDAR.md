## 🕒 Clock Display

To activate the clock mode, simply pass on.clock into the entity field. The card transforms into an elegant digital clock, offering a clean, real-time hours and minutes display that fits perfectly into any dashboard layout.

<img width="302" height="132" alt="Zrzut ekranu (1683)" src="https://github.com/user-attachments/assets/2c99ea1e-9839-4a45-97d2-ff9acf7a83a0" />
<img width="303" height="133" alt="Zrzut ekranu (1684)" src="https://github.com/user-attachments/assets/42b63ae5-a377-4518-accf-e94b7175fb7c" />

- Dedicated virtual entity — uses **on.clock** to instantly switch the card into a standalone timekeeper.
- Real-time update — high-precision internal clock ensures the time is always accurate without stressing your HA database.
- Adaptive layout — text scaling and visual elements auto-adjust to maintain crisp legibility regardless of card size.

```yaml
type: custom:piotras-smart-button
entity: on.clock
icon_mode: 1
show_state: false
icon_wrap_size: 28
name_size: 40
icon: mdi:timetable
icon_style: none
show_more: true
```

---

## 📅 Calendar Grid

To activate the calendar grid, simply pass on.calendar into the entity field. It displays a compact monthly calendar overview directly inside the card, automatically handling day names and layout formatting while providing a clear visual indicator for the current date.

<img width="302" height="234" alt="Zrzut ekranu (1685)" src="https://github.com/user-attachments/assets/a5f2bfb7-fd83-40ea-a923-c205709d368e" /><img width="267" height="237" alt="Zrzut ekranu (1689)" src="https://github.com/user-attachments/assets/5118c90d-b23b-4fd2-bcb0-0d88d9fee6d7" />

- Dedicated virtual entity — uses on.calendar to instantly switch the card into a standalone calendar grid.
- Multi-language support — localizes month and day headers automatically based on system settings.
- Current day highlight — applies a distinct visual badge to the current date for instant tracking.
- Grid optimization — clean 7-column layout engineered to fit beautifully inside standard grid sizes.

```yaml
type: custom:piotras-smart-button
entity: on.calendar
card_height: 250
card_width: 260
background_color1: "#804000"
icon_color: "#00ff00"
icon_color_on: "#ff0000"
text_color: "#ffffff"
```

Looking for more inspiration or advanced configurations for Clock Display and Calendar Grid ? Check out these community guides:
 
> 💬 [Community Dashboards & Showcases (Show and Tell)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README.md)
