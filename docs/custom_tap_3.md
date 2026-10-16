# 🖱️ Custom Tap

Use `custom_tap` to put your own clickable elements inside a card. Each element has its own tap, double-tap and hold. You can build a small panel with several buttons in one card.

There are two helpers:

| Helper | Use it to |
|---|---|
| `customTap.id` | Control entities with Home Assistant actions: toggle, set a value, call a service, open more-info, go to a view |
| `customTap.id_js` | Run your own JavaScript when the element is tapped, to change what the card shows: a selection, a page, a tooltip. It does not control entities |

This option is not in the visual editor. Add it in the code editor (YAML), at the top level of the card.

## How it works

1. In `custom_data`, write a template with the HTML of your element. Add `${customTap.id(...)}` or `${customTap.id_js(...)}` inside its opening tag.
2. In `name`, write `customData.` and the name of the entry. The HTML appears where the name would be.
3. Tell the card what the element does.
4. When you tap that element, the card runs its action. The card's own action does not run.

A template starts with `p{{{` and ends with `}}}q`. Inside is JavaScript. You must use `return`. Read entities only in `custom_data`. See [Custom Data & Templates](custom_data_guide_3.md).

You move the elements with the **Name** grid in the **🔲 Layout** tab. Write the style inside the element, with `style="..."`.

## Control entities with `customTap.id`

**A. By name.** Write `customTap.id('name')` in the element. Describe the action in `custom_tap` under the same name.

```yaml
custom_data:
  panel: |-
    p{{{ return `<span ${customTap.id('lamp')}>💡 Lamp</span>`; }}}q
name: customData.panel
custom_tap:
  lamp:
    tap_action:
      action: toggle
      entity: light.living_room
```

**B. Inline.** Write the action right inside `customTap.id({ ... })`. You do not need `custom_tap`.

```yaml
custom_data:
  panel: |-
    p{{{ return `
      <span ${customTap.id({ action: 'toggle', entity: 'light.living_room' })}>💡 Lamp</span>
    `; }}}q
name: customData.panel
```

Each element can have three actions:

| Option | When it runs |
|---|---|
| `tap_action` | A short tap |
| `double_tap_action` | Two taps one after another |
| `hold_action` | A press of half a second |

An entry that has `action` directly, without `tap_action`, is a `tap_action`. In way B, you can also write the three actions inside the object.

### What an action can do

| `action` | What it does | Fields |
|---|---|---|
| `toggle` | Toggles an entity | `entity` |
| `more-info` | Opens the more-info window of an entity | `entity` |
| `navigate` | Opens a dashboard path | `navigation_path` |
| `call-service` | Calls any Home Assistant service | `service`, `service_data` |

Write `entity` in every `toggle` and `more-info` action. Without it, the action uses the `entity` of the card.

With `call-service`, you can control any entity. Write the service as `domain.service`, and the entity in `service_data`:

```yaml
custom_tap:
  dim:
    action: call-service
    service: light.turn_on
    service_data:
      entity_id: light.living_room
      brightness_pct: 30
  all_off:
    action: call-service
    service: homeassistant.turn_off
    service_data:
      entity_id:
        - light.living_room
        - light.kitchen
```

### Example: a panel with a live state

```yaml
type: custom:piotras-smart-button
entity: light.living_room
custom_data:
  lamp_state: |-
    p{{{ return states['light.living_room']?.state === 'on' ? 'On' : 'Off'; }}}q
  panel: |-
    p{{{ return `
      <span ${customTap.id('lamp')} style="padding:4px 8px;">💡 ${customData.lamp_state}</span>
      <span ${customTap.id('dim')} style="padding:4px 8px;">🌙 30%</span>
      <span ${customTap.id('movie')} style="padding:4px 8px;">🎬 Movie</span>
    `; }}}q
name: customData.panel
custom_tap:
  lamp:
    tap_action:
      action: toggle
      entity: light.living_room
    double_tap_action:
      action: call-service
      service: light.turn_on
      service_data:
        entity_id: light.living_room
        brightness_pct: 100
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

The first element shows `On` or `Off`. A tap toggles the light, a double tap sets it to 100%, and a hold opens more-info. `30%` dims the light. `Movie` turns on a scene.

If an element has a `double_tap_action`, the card waits 0.3 seconds after a tap to see if a second one follows.

## Run JavaScript with `customTap.id_js`

Use `customTap.id_js` when a tap should change only what the card shows. For example: select an item, open a tooltip, go to the next page. This state exists only in the card. It has no Home Assistant action. To control an entity, use `customTap.id`.

You give it a function instead of an action:

```yaml
type: custom:piotras-smart-button
entity: light.living_room
custom_data:
  slots: |-
    p{{{
      const items = ['Morning', 'Afternoon', 'Evening'];
      const selected = (typeof card._selectedSlot === 'number') ? card._selectedSlot : 0;

      let html = '';
      items.forEach((label, i) => {
        const tapAttr = customTap.id_js({
          action: () => {
            card._selectedSlot = i;
            card._updateState();
          }
        });
        const active = i === selected;
        html += `<span ${tapAttr} style="padding:6px 12px;border-radius:8px;cursor:pointer;background:${active ? '#3388ff' : 'rgba(255,255,255,0.08)'};">${label}</span>`;
      });

      return `<div style="display:flex;gap:8px;">${html}</div>`;
    }}}q
name: customData.slots
```

Tap `Afternoon`. It turns blue, and the other two stay gray.

How it works:

1. The function `action` runs at the moment of the tap.
2. `card` is the card itself. It is the same object on every redraw. Store your own state in it, in a property that starts with `_`, for example `card._selectedSlot`.
3. Changing `card` does not redraw the card. End the function with `card._updateState()`. The card then calculates `custom_data` again, and `custom_data` reads `card._selectedSlot`.

Good to know:

- A short tap is `action`. You can also write `hold_action` and `double_tap_action`, each as its own function in the same object. A function you do not write does nothing.
- `id_js` is always written next to the element. It has no entry in `custom_tap`.
- If a function fails, the card writes the error in the browser console and keeps working.

## More options

- **The card's own tap as well.** By default, a tap on your element stops there. To run the card's own action too, write `stopPropagation: false`. For `customTap.id`, it is the second value: `customTap.id('lamp', { stopPropagation: false })`. For `customTap.id_js`, it goes inside the object.
- **Press effect.** The whole card shrinks a little when you press it. To turn this off, switch off **Show press effect** (`show_press_effect: false`) in the **⚙️ General** tab, in the **Visibility** section. Your elements keep their own press effect.
- **References in `custom_tap`.** The values in `custom_tap` can be references to `custom_data`, for example `entity: customData.target`.
- `custom_tap` elements work even when the card is locked with `custom_blockade`.
- If the name in `customTap.id('name')` is not in `custom_tap`, nothing happens when you tap the element.

## Go further

- 🧩 [Custom Data & Templates](custom_data_guide_3.md) — your own logic in JavaScript
- 🎛️ [Custom States On & Blockade](custom_states_on_and_custom_blockade_3.md) — decide what counts as "on" and lock the card
- ⚙️ [Configuration Reference](configuration_3.md) — the full list of options

> 🔙 Back to the [Main README](../README.md)
