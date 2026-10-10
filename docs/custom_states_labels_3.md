# 🏷️ Custom State Labels

Use `custom_states_labels` to write your own text for the state badge of a card. For example, show `Bright` instead of `DIM`, or `Hot` instead of a number.

This option is not in the visual editor. Add it in the code editor (YAML), at the top level of the card.

## How it works

You write a list of pairs. On the left is the state to match. On the right is the text to show.

```yaml
type: custom:piotras-smart-button
entity: light.living_room
name: Living Room
custom_states_labels:
  "off": Dark
  "0-30": Dim
  "31-70": Medium
  ">70": Bright
```

Always put the left side in quotes. Without quotes, YAML can read `on` and `off` as true and false, and a key that starts with `>` is not valid.

If no pair matches, the card shows its usual text. This is `name_on` and `name_off` if you set them, or the built-in text for the entity type.

## What the left side can be

The card checks the pairs in this order:

1. **The exact state of the entity**, for example `"playing"`, `"home"` or `"armed_away"`. Capital letters do not matter.
2. **The level of a light, fan or cover.** If the state did not match, the card tries the number: brightness in percent for a light, speed in percent for a fan, and position in percent for a cover.
3. **A number rule**, if the state is a number, for example a temperature, humidity or battery sensor.

The number rules:

| Rule | Matches |
|---|---|
| `">=80"` | 80 and more |
| `">80"` | more than 80 |
| `"<=20"` | 20 and less |
| `"<20"` | less than 20 |
| `"21-60"` | from 21 to 60, both included |

The card checks the number rules in the order you write them, and the first match wins. Put the most narrow rule first.

A range such as `"0-20"` followed by `"21-60"` leaves a gap for numbers like 20.5. To avoid gaps, use the signs:

```yaml
custom_states_labels:
  "<=20": Low
  "<=60": Medium
  ">60": High
```

## Where the text appears

- On most cards, the text replaces the state badge.
- Your text wins over `name_on`, `name_off` and the built-in texts, but only for the states you list. Other states keep their usual text.
- The text changes only the badge. It does not change what counts as "on". For that, see [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md).

Three cards work a little differently:

| Card | What happens |
|---|---|
| Thermostat | The badge still shows the room temperature. Your text appears as a second badge. The card matches it against the heating state (`heating`, `cooling`), then the mode (`heat`, `off`), then the preset. The first match is shown |
| Weather | The badge still shows the weather. Your text appears as a second badge. The card matches it against the temperature, so use number rules |
| Clock | Your text appears under the clock as a greeting. The card matches it against the hour, from 0 to 23 |

## Example 1: words for a temperature sensor

```yaml
type: custom:piotras-smart-button
entity: sensor.living_room_temperature
name: Living Room
icon: mdi:thermometer
custom_states_labels:
  "<18": Cold
  "<=24": Comfy
  ">24": Hot
```

The badge shows `Cold`, `Comfy` or `Hot`, depending on the temperature.

## Example 2: a greeting on the clock

```yaml
type: custom:piotras-smart-button
entity: on.clock
custom_states_labels:
  "5-11": Good morning
  "12-17": Good afternoon
  "18-21": Good evening
  "22-4": Good night
```

A range where the first number is bigger than the second goes over midnight. `"22-4"` means from 22:00 to 04:59.

The greeting needs **Show entity state** to be on (it is on by default). You can change its size and color in the **📝 Text** tab, in the **Greeting** section: **Greeting font size** and **Greeting color**.

## Good to know

- The text on the right can also come from a template or from `custom_data`. See [Custom Data & Templates](custom_data_guide_3.md).
- The older name `vacuum_states_labels` works in the same way.
- The card checks the labels when Home Assistant sends an update. The greeting on the clock is checked every second, together with the clock.

## Go further

- 🎛️ [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md) — decide what counts as "on" and lock the card
- 🧩 [Custom Data & Templates](custom_data_guide_3.md) — your own logic in JavaScript
- ⚙️ [Configuration Reference](configuration_3.md) — the full list of options

> 🔙 Back to the [Main README](../README_3.md)
