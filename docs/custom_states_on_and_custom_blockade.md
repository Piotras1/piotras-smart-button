# `custom_states_on` and `custom_blockade` — how they work

These two options are independent from `custom_states_labels` (covered in a separate document) — they don't change any label's text, only the card's visual "active" state (`custom_states_on`) and whether interaction is blocked (`custom_blockade`). They can be freely combined with `custom_states_labels`, but work fully on their own.

## `custom_states_on` — forcing the "active" visual state for any entity

`custom_states_on` is a **separate** option from `custom_states_labels` — it has nothing to do with the label's text, only with whether the card considers the entity "on/active" visually. It affects:
- which icon is shown: `icon_on` instead of `icon`,
- the icon color: `icon_color_on` instead of `icon_color`,
- the card's background highlight/gradient (the `active` CSS class),
- the background image filter, if you use `background_image_on`/`background_image_off`.

```yaml
type: custom:piotras-smart-button
entity: input_select.garage_door_status
name: Garage
icon: mdi:garage
icon_on: mdi:garage-open
custom_states_on:
  - open
  - opening
```

By default (without `custom_states_on`), the card guesses whether a state is "active" based on the entity's domain, e.g.:
- `battery` → `charging`,
- `climate` → `hvac_action` in `heating`/`cooling`,
- `alarm_control_panel` → `armed_*`/`triggered`-type states,
- `vacuum` → `cleaning`/`returning`,
- any other domain → state in the list `["on", "open", "true", "home", "playing", "unlocked"]`.

For `input_select` (and similar entities without a built-in on/off concept), the state `open` would **already** be treated as active by default (it's in the list above), but `opening` would not. That's why the example above sets `custom_states_on: [open, opening]` — so **both** values turn on `icon_on` and the highlight, overriding the default list.

⚠️ `custom_states_on` (as a non-empty list) **fully and consistently replaces** the default "active" logic for domains **without their own built-in logic** (e.g. `input_select`, `switch`, `lock`, `cover`, `media_player`, `light`, `fan`, `person`, `binary_sensor` with custom states). For five **special** domains — `climate`, `battery` (a sensor with `device_class: battery`), comfort sensors (`temperature`/`humidity` with `comfort_min`/`comfort_max`), `alarm_control_panel`, and `vacuum` — things are more complicated, see the warning below.

### 🐛 Known card bug: on `climate`/`battery`/`comfort`/`alarm`/`vacuum`, the background and icon can disagree

This is a genuine inconsistency in the card's own code (not a documentation error), so it's described here as a warning. On every Home Assistant state update, the card computes "is active" **twice, with two different functions, in a different order**:

1. The function driving the **card's background and the `active` CSS class** checks `custom_states_on` **first** — if it's set, it always wins, regardless of domain.
2. The function driving **which icon is actually shown** (`icon_on` vs `icon`) checks domains in a different order: `battery` → `climate` → comfort sensor → `alarm_control_panel` → `vacuum` → **and only at the very end** `custom_states_on`. For these five domains, your `custom_states_on` list is therefore effectively **ignored when choosing the icon** — even though the background already reacts correctly to your list.

**Practical consequence:** if you set `custom_states_on` on a `climate`/`battery`/`alarm_control_panel`/`vacuum` entity, or a temperature/humidity sensor with `comfort_min`/`comfort_max` — the card's background (glow, icon-ring border color) will react to your list, but **`icon_on` may not appear**, because the icon choice is still computed the old way (`hvac_action`, `charging`, alarm state, etc.), ignoring your `custom_states_on`.

➡️ For these five domains, `custom_states_on` therefore works only **halfway** (background yes, icon no) — it isn't fully reliable. For every other domain, both functions compute the same thing, and `custom_states_on` works 100% consistently, as shown in this document's examples (e.g. the `input_select` garage example).


ℹ️ Matching in `custom_states_on`, just like in `custom_states_labels`, is **case-insensitive**.

`custom_states_on` and `custom_states_labels` are two independent options — you can use one without the other (e.g. only change the label text without changing the icon, or vice versa), but in practice, for entities like `input_select`, both are usually set together, as in the example above.

## `custom_blockade` — blocking the card for selected states

`custom_blockade` is a list of entity states (exact match, **case-insensitive** — same as `custom_states_on`) that, when the entity's current state is on the list, **completely disable interaction with the card**: `tap_action`, `hold_action`, and `double_tap_action` stop working for as long as the entity is in one of the listed states.

```yaml
type: custom:piotras-smart-button
entity: cover.garage_door
name: Garage
icon: mdi:garage
icon_on: mdi:garage-open
custom_states_on:
  - open
  - opening
custom_blockade:
  - opening
  - closing
```

In this example, while the garage door is in motion (`opening`/`closing`), tapping/holding the card **triggers no action at all** — until the door finishes moving and reaches `open` or `closed`. This guards against, e.g., sending the command multiple times mid-animation, or interrupting an alarm cycle while it's `arming`.

Another typical example — blocking interaction while an alarm is `triggered` (so an accidental tap on the card doesn't try to change anything while the alarm is actually going off):

```yaml
type: custom:piotras-smart-button
entity: alarm_control_panel.home
name: Alarm
custom_blockade:
  - triggered
  - arming
```

Important technical notes on how this works:
- The card signals the block **visually** with a `default` cursor (instead of the normal "tap" scale-down effect) — this is a subtle hint, not a clear message or a gray overlay.
- The block applies **only** to interaction (taps), it has no effect on the displayed label (`custom_states_labels`) or on the "active" visual state (`custom_states_on`) — these three options are completely independent and can be freely combined.
- `custom_blockade` is a **different option** from `blockade_card` (a separate mechanism — a temporary lock after calling `call-service`, for the duration of a countdown, unrelated to the entity's state). Don't confuse the two.
- If the entity's state isn't on the `custom_blockade` list — the card works normally, with no changes.

## Tricks: unusual uses of `custom_states_on`

### 1. Forcing the card to display an entity's raw content (`input_text`, `input_number`)

By default, with no `custom_*` options, a domain the card doesn't recognize (e.g. `input_text`, `input_number`) only displays a generic `ON`/`OFF` label — because the card has nothing else to show. However, if you add `custom_states_on` (or its older alias `vacuum_states_on`) as a **non-empty array**, even with a single "throwaway" value that can never realistically match — the card switches to a different internal code path, whose fallback prints the **literal entity state** (in UPPERCASE):

```yaml
type: custom:piotras-smart-button
entity: input_text.test_text
vacuum_states_on:
  - null
```

Here, `- null` will never match any real `input_text` state — but simply providing the array is enough to make the card start showing the text field's actual content as the main label, instead of a fixed `ON`/`OFF`. This is handy for entities with arbitrary, changing content (notes, counters, text statuses), where listing every possible value in `custom_states_labels` would be impractical.

⚠️ **Gotcha:** this code path first checks whether the entity's state matches (case-insensitively) one of a handful of fixed keywords from the vacuum dictionary (`cleaning`, `docked`, `returning`, `paused`, `idle`, `error`, `charging`). If your `input_text` happens to contain exactly one of those words (e.g. you literally type "idle" or "error"), the card will show the translated vacuum-dictionary word instead of your actual content. In practice this rarely matters (low collision chance for arbitrary content), but it's worth knowing where an occasional "weird" result might come from.

### 2. The "trigger button" pattern — `custom_states_on` as a readiness indicator, not a literal on/off

`custom_states_on` doesn't have to correspond to an entity's literal "on" state — you can use it semantically, as an indicator that "this button is active/ready to be pressed right now." Example: a dedicated **open-only** button for a garage door, which lights up (ready to use) when the door is closed, and becomes completely unresponsive while the door is moving or already open (so you don't try to open something that's already open or mid-motion):

```yaml
type: custom:piotras-smart-button
entity: cover.test_garage_door
custom_states_on:
  - closed
custom_blockade:
  - open
  - opening
  - closing
```

Here `custom_states_on: [closed]` doesn't mean "the door is on when closed" in a physical sense — it's simply a visual signal that "this button is waiting for your tap" (lit-up `icon_on`), while `custom_blockade` completely disables the tap response once the door is already open/in motion. Together they create a self-locking, single-action trigger button.

### 3. Highlighting a button based on an exact `input_number` value

`custom_states_on` can also match the exact numeric value stored as an `input_number` entity's state — useful, for example, for building a set of "vacuum room #X" buttons, where each button only lights up when the currently selected room number matches its own number:

```yaml
type: custom:piotras-smart-button
entity: input_number.room_number
custom_states_on:
  - "3.0"
```

⚠️ An `input_number` entity's state is a **string**, not a number — and it often includes a trailing zero after the decimal point (e.g. `"3.0"`, not `"3"`), depending on the `step` configured for that `input_number`. The match is exact (though case-insensitive, which doesn't matter for numbers) — `"3.0"` will not match a state of `"3"` or vice versa. Check the exact format in Home Assistant's Developer Tools → States before typing the value into the card's config.

