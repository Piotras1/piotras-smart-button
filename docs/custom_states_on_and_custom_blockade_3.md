# 🎛️ Custom States On & Blockade

Two options for the states of an entity:

- `custom_states_on` — you decide which states count as "on".
- `custom_blockade` — you decide in which states the card is locked.

These options are not in the visual editor. Add them in the code editor (YAML), at the top level of the card.

## Custom States On

By default, each card decides on its own when it is "on". For example, a light is on in the state `on`, and a media player is on in the state `playing`. With `custom_states_on`, you write your own list.

```yaml
type: custom:piotras-smart-button
entity: sensor.dishwasher
name: Dishwasher
icon: mdi:dishwasher
custom_states_on:
  - running
  - drying
```

The card is on when the state of the entity is on the list. In every other state, it is off. When the card is on, it uses `icon_color_on`, `icon_on` and the "on" background, as usual.

If `custom_states_on` is set, it replaces the built-in rule for every type of entity, including battery, thermostat, alarm and vacuum.

### What you can write in the list

- **A state**, for example `running` or `home`. Capital letters do not matter.
- **A number rule**, if the state is a number, for example a power sensor:

| Rule | Matches |
|---|---|
| `">=50"` | 50 and more |
| `">50"` | more than 50 |
| `"<=20"` | 20 and less |
| `"<20"` | less than 20 |
| `"21-60"` | from 21 to 60, both included |

Put the number rules in quotes. You can mix states and rules in one list.

```yaml
type: custom:piotras-smart-button
entity: sensor.washing_machine_power
name: Washing Machine
icon: mdi:washing-machine
custom_states_on:
  - ">=5"
```

The card is on when the power is 5 W or more.

### What the badge shows

When you set `custom_states_on`, the state badge shows the state of the entity in capital letters, for example `RUNNING` or `FINISHED`, instead of `ON` and `OFF`. This does not apply to cards that have their own texts, such as a light, a media player, a thermostat, a fan or a cover.

To show your own text, use one of these:

- `name_on` and `name_off` in the **📝 Text** tab. Fill in both.
- `custom_states_labels` for a text per state. See [Custom State Labels](custom_states_labels_3.md).

## Custom Blockade

`custom_blockade` locks the card when the entity is in one of the listed states. While the card is locked, tap, double-tap and hold do nothing. The card does not shrink when you press it, and the mouse pointer does not change.

```yaml
type: custom:piotras-smart-button
entity: cover.garage_door
name: Garage
icon: mdi:garage
custom_blockade:
  - opening
  - closing
```

The garage card does not react while the door is moving.

### What you can write in the list

Only states. Capital letters do not matter. Number rules do not work here.

A good use is `unavailable` and `unknown`. The card is then locked when Home Assistant cannot reach the device:

```yaml
custom_blockade:
  - unavailable
  - unknown
```

### What is not locked

The lock stops only the tap, double-tap and hold of the card. It does not stop sliders, player buttons and your own clickable elements from [Custom Tap](custom_tap_3.md).

The lock is not the same as **Block the card during notification** (`blockade_card`) in the **📄 Service** tab. That one locks the card for the time of a countdown.

## Example: both options together

```yaml
type: custom:piotras-smart-button
entity: cover.garage_door
name: Garage
icon: mdi:garage
icon_on: mdi:garage-open
custom_states_on:
  - open
  - opening
  - closing
custom_blockade:
  - opening
  - closing
```

The card looks "on" when the door is open or moving. While the door is moving, the card is locked.

## Good to know

- Both options need `entity`. Without it, they do nothing.
- The card checks the lists when Home Assistant sends an update.
- The older name `vacuum_states_on` is still read, but only for entities that are not a vacuum. For a vacuum, use `custom_states_on`.

## Go further

- 🏷️ [Custom State Labels](custom_states_labels_3.md) — your own text for each state
- 👁️ [Conditional Visibility](visible_if_3.md) — show or hide the card
- ⚙️ [Configuration Reference](configuration_3.md) — the full list of options

> 🔙 Back to the [Main README](../README_3.md)
