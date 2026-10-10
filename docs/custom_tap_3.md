# 🖱️ Custom Tap

Use `custom_tap` to put your own clickable elements inside a card. Each element has its own tap, double-tap and hold. You can build a small panel with several buttons in one card.

There are two helpers:

| Helper | Use it to |
|---|---|
| `customTap.id` | Control entities with Home Assistant actions: toggle, set a value, call a service, open more-info, go to a view |
| `customTap.id_js` | Run your own JavaScript function when the element is tapped |

This option is not in the visual editor. Add it in the code editor (YAML), at the top level of the card.

## How it works

1. In a template, write the HTML of your element and add `${customTap.id(...)}` or `${customTap.id_js(...)}` inside its opening tag.
2. Tell the card what the element does.
3. When you tap that element, the card runs its action. The tap does not reach the card, so the card's own action does not run.

A template starts with `p{{{` and ends with `}}}q`. Inside is JavaScript. You must use `return`. The templates are described in [Custom Data & Templates](custom_data_guide_3.md). Here we use them only to write the HTML of the element.

**The rule for entities:** read anything from an entity in `custom_data`, and only show or use the result in the `name` and in the functions. The template of the `name` cannot read `states` directly. See [Custom Data & Templates](custom_data_guide_3.md).

The HTML goes into the `name`, so it appears where the name would be. You move it with the **Name** grid in the **🔲 Layout** tab. Write the style inside the element, with `style="..."`.

## Control entities with `customTap.id`

You can tell the card what to do in three ways.

**A. By name.** Write `customTap.id('name')` in the element. Describe the action in `custom_tap` under the same name.

```yaml
name: |-
  p{{{ return `<span ${customTap.id('lamp')}>💡 Lamp</span>`; }}}q
custom_tap:
  lamp:
    tap_action:
      action: toggle
      entity: light.living_room
```

**B. Inline.** Write the action right inside `customTap.id({ ... })`. You do not need `custom_tap`.

```yaml
name: |-
  p{{{ return `
    <span ${customTap.id({ action: 'toggle', entity: 'light.living_room' })}>💡 Lamp</span>
  `; }}}q
```

**C. Inline, with tap, double-tap and hold.** Write the three actions inside `customTap.id({ ... })`.

```yaml
name: |-
  p{{{ return `
    <span ${customTap.id({
      tap_action: { action: 'toggle', entity: 'light.living_room' },
      double_tap_action: { action: 'call-service', service: 'light.turn_on',
        service_data: { entity_id: 'light.living_room', brightness_pct: 100 } },
      hold_action: { action: 'more-info', entity: 'light.living_room' }
    })}>💡 Lamp</span>
  `; }}}q
```

The three actions are:

| Option | When it runs |
|---|---|
| `tap_action` | A short tap |
| `double_tap_action` | Two taps one after another |
| `hold_action` | A press of half a second |

In way A, an entry that has `action` directly, without `tap_action`, is a `tap_action`. In way B, the same is true for the object you give to `customTap.id`.

### What an action can do

| `action` | What it does | Fields |
|---|---|---|
| `toggle` | Toggles an entity | `entity` |
| `more-info` | Opens the more-info window of an entity | `entity` |
| `navigate` | Opens a dashboard path | `navigation_path` |
| `call-service` | Calls any Home Assistant service | `service`, `service_data` |

Write `entity` in every `toggle` and `more-info` action. Without it, the action uses the `entity` of the card.

With `call-service`, you can control any entity, not only the one of the card. Write the service as `domain.service`, and the entity in `service_data`:

```yaml
custom_tap:
  dim:
    action: call-service
    service: light.turn_on
    service_data:
      entity_id: light.living_room
      brightness_pct: 30
  movie:
    action: call-service
    service: scene.turn_on
    service_data:
      entity_id: scene.movie
  all_off:
    action: call-service
    service: homeassistant.turn_off
    service_data:
      entity_id:
        - light.living_room
        - light.kitchen
```

`dim` sets a light to 30%. `movie` turns on a scene. `all_off` turns off two lights with one tap.

### Example: a panel with three buttons

```yaml
type: custom:piotras-smart-button
entity: light.living_room
name: |-
  p{{{ return `
    <span ${customTap.id('lamp')} style="padding:4px 8px;">💡 Lamp</span>
    <span ${customTap.id('dim')} style="padding:4px 8px;">🌙 30%</span>
    <span ${customTap.id('movie')} style="padding:4px 8px;">🎬 Movie</span>
  `; }}}q
custom_tap:
  lamp:
    tap_action:
      action: toggle
      entity: light.living_room
    hold_action:
      action: more-info
      entity: light.living_room
  dim:
    action: call-service
    service: light.turn_on
    service_data:
      entity_id: light.living_room
      brightness_pct: 30
  movie:
    action: call-service
    service: scene.turn_on
    service_data:
      entity_id: scene.movie
```

Tap `Lamp` to toggle the light. Hold it for half a second to open more-info. Tap `30%` to dim the light. Tap `Movie` to turn on the scene.

## Run JavaScript with `customTap.id_js`

Use `customTap.id_js` when a ready-made action is not enough. For example, when the result depends on the current state, or when you want to do several things with one tap.

You give it functions instead of actions. The names are `action` (a short tap), `double_tap_action` and `hold_action`. You can write only the ones you need.

Read the entity in `custom_data`, and use the result in the function:

```yaml
type: custom:piotras-smart-button
entity: light.living_room
custom_data:
  brightness: "p{{{ const s = states['light.living_room']; return Math.round((s?.attributes?.brightness ?? 0) / 255 * 100); }}}q"
name: |-
  p{{{ return `
    <span ${customTap.id_js({
      action() {
        card._hass.callService('light', 'turn_on', {
          entity_id: 'light.living_room',
          brightness_pct: Math.min(100, customData.brightness + 10)
        });
      },
      hold_action() {
        card._hass.callService('light', 'turn_off', { entity_id: 'light.living_room' });
      }
    })}>💡 ${customData.brightness}%</span>
  `; }}}q
```

The element shows the brightness, for example `💡 30%`. A tap raises it by 10%. A hold turns the light off. After each update from Home Assistant, the function uses the new value.

Inside the functions, you can use:

| Name | What it is |
|---|---|
| `customData` | The values from `custom_data`. Use it to read entities |
| `card._hass.callService(domain, service, data)` | Calls a Home Assistant service. Use it to control entities |
| Any variable from your template | For example a value you set before `return` |

Good to know:

- A short tap is the function named `action`. Do not call it `tap_action`.
- A function you do not write does nothing. For example, without `double_tap_action`, a double tap does nothing.
- If a function fails, the card writes the error in the browser console and keeps working. It does not show an error on the card.
- After the function runs, the card refreshes.
- You do not need `custom_tap` for `customTap.id_js`.

## Example: show a live state on the element

The template of the `name` can read the values from `custom_data`, but it cannot read `states` directly. Read the state in `custom_data`, and show it in the `name`.

```yaml
type: custom:piotras-smart-button
entity: light.living_room
custom_data:
  lamp_state: "p{{{ return states['light.living_room']?.state === 'on' ? 'On' : 'Off'; }}}q"
name: |-
  p{{{ return `
    <span ${customTap.id('lamp')}>💡 ${customData.lamp_state}</span>
  `; }}}q
custom_tap:
  lamp:
    action: toggle
    entity: light.living_room
```

The text on the element changes between `On` and `Off`. Write `states[...]?.state` with the `?.` sign. It protects the card while Home Assistant is loading.

## More options

- **The card's own tap as well.** By default, a tap on your element stops there. To run the card's own action too, write `stopPropagation: false`. For `customTap.id`, it is the second value: `customTap.id('lamp', { stopPropagation: false })`. For `customTap.id_js`, it goes inside the object: `customTap.id_js({ action() { ... }, stopPropagation: false })`.
- **Press effect.** The whole card shrinks a little when you press it. To turn this off, switch off **Show press effect** (`show_press_effect: false`) in the **⚙️ General** tab, in the **Visibility** section. Your elements keep their own press effect.
- **Templates in `custom_tap`.** The values in `custom_tap` can be templates. There you can use `states`, for example to choose the entity by the state of another entity.
- **Double-tap delay.** If an element has a `double_tap_action`, the card waits 0.3 seconds after a tap to see if a second one follows.

## Good to know

- `custom_tap` elements work even when the card is locked with `custom_blockade`. See [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md).
- In way A, if the name in `customTap.id('name')` is not in `custom_tap`, nothing happens when you tap the element.
- You can mix all the ways in one card.

## Go further

- 🧩 [Custom Data & Templates](custom_data_guide_3.md) — your own logic in JavaScript
- 🎛️ [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md) — decide what counts as "on" and lock the card
- ⚙️ [Configuration Reference](configuration_3.md) — the full list of options

> 🔙 Back to the [Main README](../README_3.md)
