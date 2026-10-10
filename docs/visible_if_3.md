# 👁️ Conditional Visibility

Use `visible_if` to show or hide a card. The card is shown when the condition is true and hidden when it is false.

This option is not in the visual editor. Add it in the code editor (YAML), at the top level of the card.

## How it works

```yaml
type: custom:piotras-smart-button
entity: switch.socket_1
visible_if: false
```

If `visible_if` is not set, the card is always shown. The card checks the condition every time Home Assistant sends an update, so the card appears and disappears on its own when the state changes.

## What you can put in `visible_if`

| Value | Result |
|---|---|
| `true` | The card is shown |
| `false` | The card is hidden |
| A template: `p{{{ ... }}}q` | The card is shown when the template returns a true value |

Any other value is treated as true or false by its content. A text such as `"off"` counts as true, so do not use plain text.

## Templates

A template starts with `p{{{` and ends with `}}}q`. Inside is JavaScript. You must use `return`.

In a `visible_if` template, read Home Assistant data through `card._hass`:

| Name | What it is |
|---|---|
| `card._hass.states` | All entities. Use `card._hass.states['binary_sensor.someone_home'].state` |
| `card._hass.user` | The logged-in Home Assistant user. For example `card._hass.user.is_admin` |

Write `card._hass.states['entity_id']?.state` with the `?.` sign. If the entity does not exist, the card is hidden instead of showing an error.

In YAML, write the template as a block that starts with `|-`, as in the examples below. The template goes on the next line, indented.

## Example 1: show the card only when someone is home

```yaml
type: custom:piotras-smart-button
entity: switch.socket_1
name: Washing Machine
icon: mdi:washing-machine
visible_if: |-
  p{{{ return card._hass.states['binary_sensor.someone_home']?.state === 'on'; }}}q
```

When `binary_sensor.someone_home` is `on`, the card is shown. In every other state, it is hidden.

## Example 2: show the card only to administrators

```yaml
type: custom:piotras-smart-button
entity: switch.server_rack
name: Server Rack
icon: mdi:server
visible_if: |-
  p{{{ return card._hass.user.is_admin === true; }}}q
```

The card is shown to users with administrator rights. Other users do not see it.

## Good to know

- You can use more than one entity in a template. For a longer template, use a YAML block with `|`.
- The card is checked when Home Assistant sends an update. A condition that depends only on the clock, for example the hour, changes only at the next update.
- A hidden card does not react to taps. It is not displayed.
- `visible_if` hides the card on the dashboard. It does not stop the entity or the actions from working in Home Assistant.
- `visible_if` hides the card for the user who looks at the dashboard. It is not a security feature. Use Home Assistant permissions to protect devices.

## Go further

- 🧩 [Custom Data & Templates](custom_data_guide_3.md) — your own logic in JavaScript
- 🎛️ [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md) — decide what counts as "on" and lock the card
- ⚙️ [Configuration Reference](configuration_3.md) — the full list of options

> 🔙 Back to the [Main README](../README_3.md)
