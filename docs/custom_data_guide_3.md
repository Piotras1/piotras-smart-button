# 🧩 Custom Data & Templates

Use `custom_data` to read entities and calculate things with your own JavaScript. Then use the results in the card: in the name, the icon, the colors, the size and more.

This option is not in the visual editor. Add it in the code editor (YAML), at the top level of the card.

## The main rule

**Read entities and calculate in `custom_data`. Show the result in other options.**

Only the templates in `custom_data` can read entities (`states`) and the user (`user`). Other options, such as `name` or `icon_color`, can only use the results from `custom_data`.

```yaml
type: custom:piotras-smart-button
custom_data:
  count: "p{{{ return Object.keys(states).filter(k => k.startsWith('light.') && states[k].state === 'on').length; }}}q"
name: "p{{{ return customData.count + ' lights on'; }}}q"
show_state: false
icon: mdi:lightbulb-group
```

The card counts the lights that are on and shows, for example, `3 lights on`.

## How it works

`custom_data` is a list of named values. Each value can be:

| Value | Example | Result |
|---|---|---|
| A plain value | `limit: 24` | The value as it is. A number, text, true or false, a list |
| A template | `avg: "p{{{ ... }}}q"` | The result of your JavaScript |
| A reference | `copy: customData.limit` | The value of another entry |

You use the values as `customData.name`:

- As a template: `name: "p{{{ return customData.avg + '°C'; }}}q"`.
- As a reference, when you want the value as it is: `icon_color: customData.color`.

The card calculates `custom_data` again every time Home Assistant sends an update. An entry can use the entries written above it, so you can build the result in steps.

## Templates

A template starts with `p{{{` and ends with `}}}q`. Inside is JavaScript. You must use `return`.

The template must be the whole value. You cannot write text before or after it. To mix text and a result, do it inside the template: `"p{{{ return 'Now: ' + customData.avg; }}}q"`.

In YAML, put the whole template in quotes. For a long template, use a block with `|-`.

What a template can use:

| Name | In `custom_data` | In other options |
|---|---|---|
| `states` | Yes. All entities, for example `states['sensor.t1'].state` | No |
| `user` | Yes. The logged-in user, for example `user.name` | No |
| `customData` | Yes. The entries written above | Yes. All entries |
| `card` | Yes | Yes |
| `customTap` | Yes | Yes. See [Custom Tap](custom_tap_3.md) |

Write `states['entity_id']?.state` with the `?.` sign. When the card loads, Home Assistant has not sent the data yet. With `?.`, the template returns nothing instead of an error.

## Where you can use the results

You can use a template or a reference in these options:

| Group | Options |
|---|---|
| Text | `name`, `name_on`, `name_off` |
| Icon | `icon`, `icon_on`, `icon_off`, `icon_color`, `icon_color_on`, `icon_size`, `icon_wrap_size`, `icon_over_size`, `icon_style` |
| Colors and style | `text_color`, `value_color`, `background_color1`, `background_color2`, `background_color3`, `background_gradient`, `background_gradient_angle`, `border_color`, `border_width`, `border_radius`, `box_shadow`, `font_style` |
| Size | `card_width`, `card_height`, `name_size`, `state_size` |
| Images | `background_image_on`, `background_image_off`, `image_filter_on`, `image_filter_off` |
| Visibility | `show_more`, `show_player`, `show_state`, `show_name`, `show_icon` |
| Bars | `slider_height`, `slider_label_color`, `max_watts`, `con_warning`, `entity_watts`, `comfort_min`, `comfort_max`, `time_service`, `service_style` |
| Clock | `clock_tile_bg`, `clock_12h` |

Templates and references also work in:

- the texts of `custom_states_labels`. See [Custom State Labels](custom_states_labels_3.md).
- the values of `custom_tap`. See [Custom Tap](custom_tap_3.md).

They do not work in `entity`, `tap_action`, `double_tap_action` and `hold_action`. For actions that change with a state, use `custom_tap`.

## Example 1: the average temperature, with a color

```yaml
type: custom:piotras-smart-button
custom_data:
  avg: "p{{{ const v = ['sensor.room_1_temperature', 'sensor.room_2_temperature', 'sensor.room_3_temperature'].map(id => parseFloat(states[id]?.state)).filter(n => !isNaN(n)); return v.length ? Math.round(v.reduce((a, b) => a + b, 0) / v.length * 10) / 10 : null; }}}q"
  color: "p{{{ return customData.avg === null ? '#9e9e9e' : customData.avg > 24 ? '#ff7043' : '#69f0ae'; }}}q"
name: "p{{{ return customData.avg === null ? '--' : customData.avg + '°C'; }}}q"
icon: mdi:thermometer
icon_color: customData.color
show_state: false
```

The first entry calculates the average of three sensors. It skips sensors without a number, for example `unavailable`. The second entry uses the first one to choose a color. The `name` shows the result, and `icon_color` uses the color.

## Example 2: a greeting with the user name

```yaml
type: custom:piotras-smart-button
custom_data:
  hello: "p{{{ return 'Hello, ' + (user?.name ?? ''); }}}q"
name: customData.hello
icon: mdi:hand-wave
show_state: false
```

`name: customData.hello` is a reference. It takes the text as it is.

## Example 3: show or hide parts of the card

```yaml
type: custom:piotras-smart-button
entity: switch.washing_machine_socket
custom_data:
  running: "p{{{ return parseFloat(states['sensor.washing_machine_power']?.state) > 5; }}}q"
name: Washing Machine
icon: mdi:washing-machine
show_more: customData.running
entity_watts: sensor.washing_machine_power
```

The power bar shows only while the washing machine uses more than 5 W.

## When something does not work

The card writes errors in the browser console. The messages start with `[piotras-smart-button]`. In Chrome, press F12 and open the **Console** tab.

| What you see | Reason |
|---|---|
| The text `p{{{ ... }}}q` stays on the card | The template is not the whole value. There is text before or after it |
| The option is empty, for example the name is missing | The template has an error, or it returns nothing. Check that you used `return` |
| A template in `name` does not see an entity | `states` works only in `custom_data`. Read the entity there |
| The first moment after loading looks empty | Home Assistant has not sent the data yet. Use `?.` and handle a missing value |

Good to know:

- The result of `name` is shown as HTML. Do not put text from an unknown source in it.
- A value you do not set stays as the card has it by default.
- Keep the templates short. The card runs them at every update.

## Go further

- 🖱️ [Custom Tap](custom_tap_3.md) — clickable elements and your own panels
- 🏷️ [Custom State Labels](custom_states_labels_3.md) — your own text for each state
- 👁️ [Conditional Visibility](visible_if_3.md) — show or hide the card
- ⚙️ [Configuration Reference](configuration_3.md) — the full list of options

> 🔙 Back to the [Main README](../README_3.md)
