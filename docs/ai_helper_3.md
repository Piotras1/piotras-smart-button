# 🤖 Write card code with AI

AI assistants (ChatGPT, Gemini, Claude and others) often do not know the syntax of this card. They invent code from other cards, and the result does not work.

Copy the block below and paste it at the start of your chat. Then describe what you want. The assistant will use the right syntax.

## Paste this first

```text
You are helping me write YAML for the Home Assistant card `custom:piotras-smart-button` (version 3.0).
Use ONLY the syntax below. If you are not sure an option exists, say so. Do not guess.
Answer with YAML only, and explain in one or two short sentences.

CUSTOM DATA
1. `custom_data` is a list of named values at the top level of the card. A value is a plain value, a template, or a reference.
2. A template starts with `p{{{` and ends with `}}}q`. Inside is JavaScript, and it MUST use `return`.
3. The template must be the WHOLE value. No text before or after it. Put it in quotes, or in a `|-` block for long code.
4. Read entities ONLY inside `custom_data`: `states['sensor.x']?.state`. Always use `?.`.
   Also available there: `user`, `customData` (entries written ABOVE), `card`.
5. Other options (`name`, `icon`, `icon_color`, `show_more`, `background_color1`, ...) only take a reference: `name: customData.label`.
   Templates in those options cannot read `states`. References do not work in `entity`, `tap_action`, `double_tap_action`, `hold_action`.
6. An entry can use only the entries written above it.
7. The result of `name` is shown as HTML.
8. This is NOT button-card. Never use `[[[ ... ]]]`, `variables:`, or `states['x'].state` without `?.`.

CORRECT EXAMPLE
type: custom:piotras-smart-button
custom_data:
  temp: p{{{ const t = parseFloat(states['sensor.room_temperature']?.state); return isNaN(t) ? null : t; }}}q
  label: p{{{ return customData.temp === null ? '--' : customData.temp + '°C'; }}}q
  color: p{{{ return customData.temp === null ? '#9e9e9e' : customData.temp > 24 ? '#ff7043' : '#69f0ae'; }}}q
name: customData.label
icon: mdi:thermometer
icon_color: customData.color
show_state: false

CLICKABLE ELEMENTS (custom_tap)
- Build the element as HTML in `custom_data`. Put `${customTap.id(...)}` or `${customTap.id_js(...)}` inside its opening tag. Show it with `name: customData.<key>`.
- Control entities: `customTap.id('lamp')` plus an entry `lamp:` under `custom_tap:`, or inline: `customTap.id({ action: 'toggle', entity: 'light.living_room' })`.
- Actions: `toggle` (entity), `more-info` (entity), `navigate` (navigation_path), `call-service` (service: 'domain.service', service_data).
  Each element can have `tap_action`, `double_tap_action`, `hold_action`.
- Your own JavaScript, without controlling entities: `customTap.id_js({ action: () => { card._selected = 1; card._updateState(); } })`.
  Keep your own state in `card._something`. ALWAYS end the function with `card._updateState()`.
- Put `pointer-events: none` on an `<ha-icon>` inside a clickable element.
```

## Check the answer

- The card shows the text `p{{{ ... }}}q`? The template is not the whole value. Remove any text around it.
- A value is empty? The template has an error or no `return`.
- Nothing changes after an update? Hard reload the browser (`Ctrl+Shift+R`).
- The card writes errors in the browser console (F12). The messages start with `[piotras-smart-button]`.

## Learn more

- 🧩 [Custom Data & Templates](custom_data_guide_3.md)
- 🖱️ [Custom Tap](custom_tap_3.md)
- 💬 [Custom Data Modules](https://github.com/Piotras1/piotras-smart-button/discussions/categories/custom-data-modules) — ready-made modules to copy

> 🔙 Back to the [Main README](../README.md)
