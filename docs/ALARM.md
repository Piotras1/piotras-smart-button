## 🚨 Advanced Alarm Status

Provide your standard Home Assistant alarm control panel entity (e.g., alarm_control_panel.home_alarm) to monitor your security system. This view focuses purely on rendering highly precise state feedback and visual security cues, while triggering actions open the native Home Assistant more-info dialog for secure PIN entry.

<img width="521" height="85" alt="Zrzut ekranu (1672)" src="https://github.com/user-attachments/assets/a6ab3ce0-aab5-4e6f-934a-33eaba7d8070" />
<img width="524" height="79" alt="Zrzut ekranu (1671)" src="https://github.com/user-attachments/assets/5789c5dd-64af-4045-a23e-780b9fb1f4fc" />
<img width="522" height="80" alt="Zrzut ekranu (1670)" src="https://github.com/user-attachments/assets/2e41a182-5cdd-4f73-a9e1-9ab4b518c8d6" />

- Multi-state tracking — seamlessly fetches and displays native statuses including disarmed, arming, armed_home, armed_away, and triggered.
- Dynamic pulsing animations — critical or transitional states trigger smooth background pulsing and high-visibility icon shifts to capture immediate attention.
- Native HA translations — automatically inherits localized security terminology straight from Home Assistant's core backend, ensuring seamless multi-language compatibility.

```yaml
type: custom:piotras-smart-button
entity: alarm_control_panel.test_alarm
card_width: auto
icon_mode: 4
value_mode: 6
name: Alarm
name_size: 25
state_size: 13
icon_size: 40
icon_wrap_size: 42
card_height: 80
icon: mdi:alarm-light-off
icon_on: mdi:alarm-light
icon_color: "#d4d4d4"
icon_color_on: "#ff8000"
show_filter: true
tap_action:
  action: more-info
```

Looking for more inspiration or advanced configurations for Advanced Alarm Status? Check out these community guides:
 
> 💬 [Community Dashboards & Showcases (Show and Tell)](https://github.com/Piotras1/piotras-smart-button/discussions/categories/show-and-tell)

> 🔙 Back to the [Main README](../README.md)