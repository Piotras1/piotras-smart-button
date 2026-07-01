# 🖥 Visual Editor

> 🔙 Back to the [Main README](../README.md)

The Visual Editor allows configuring `custom:piotras-smart-button` directly from the Home Assistant interface — no manual YAML editing required for the most common settings. The editor groups options into clearly labeled sections and automatically adapts the available fields depending on the selected entity and its domain.

---

## 1. General

![Visual Editor - General](../img/piotras-smart-button-edytor-1.jpg)

Configure primary settings and element visibility for the card.

**Entity and Name:**
- **Entity:** Selects the Home Assistant entity displayed or controlled by the card.
- **Display name:** Sets a custom label shown on the card (e.g., `All Home`).

**Visibility Options:**
- **Show icon:** Toggles icon visibility on the card.
- **Show name:** Toggles display name visibility.
- **Show entity state:** Toggles the state badge visibility.

---

## 2. Size

![Visual Editor - Size](../img/piotras-smart-button-edytor-2.jpg)

Adjust the card dimensions and outline styles to match your dashboard theme.

**Size Card:**
- **Card width:** Sets the card width in pixels, or `auto` to fill available space.
- **Card height (px or auto):** Sets the card height in pixels, or `auto`.

**Style Card:**
- **Border radius (px):** Controls how rounded the corners are. Set to `50` for a perfect circle (Navigation Mode).
- **Border width (px):** Sets the border thickness. Set to `0` to hide the border entirely.
- **Border color:** HEX color for the card outline.
- **Box shadow (outer):** CSS `box-shadow` value for an outer glow or drop shadow effect (e.g., `10px 10px 15px 0px rgba(0,0,0,0.50)`).

---

## 3. Background

![Visual Editor - Background](../img/piotras-smart-button-edytor-3.jpg)

Manage card background options: solid color, multi-color gradient, or background image.

**General Settings:**
- **Enable background image:** Toggles the use of a background photo or texture. Image paths are set via YAML (`background_image_on` / `background_image_off`).
- **Enable image filters:** Activates CSS filters on the card background — applies to colors, gradients, and images alike. Configure filter values in the **Filters** tab.
- **Card inner shadow:** Toggles the inner dark overlay gradient that improves text readability on image backgrounds.

**Gradient:**
- **Color 1:** Sets the primary background color. Used alone = solid background.
- **Color 2 / Color 3:** Adding Color 2 automatically switches to **Linear Gradient** mode. Adding Color 3 creates a 3-stop gradient. To return to solid, clear Color 2 and Color 3.
- **Angle (°):** Controls the gradient direction (e.g., `135` = diagonal, `180` = top to bottom).

---

## 4. Icon

![Visual Editor - Icon](../img/piotras-smart-button-edytor-4.jpg)

Customize the icon appearance and behavior for both ON and OFF states.

**Style Icon:**
- **Icon (mdi:...):** Default icon from the Material Design Icons library (e.g., `mdi:lightbulb`). Leave `Icon when ON` empty to use the same icon for both states.
- **Icon when ON (mdi:...):** Alternative icon shown when the entity is active (e.g., `mdi:lightbulb-on`).
- **Icon color (OFF):** HEX color for the icon in the OFF/inactive state.
- **Icon color (ON):** HEX color for the icon in the ON/active state.

**Size Icon:**
- **Icon size (px):** Size of the icon itself.
- **Wrap size (px):** Size of the icon's container (the colored circle/square around the icon).
- **Icon style:** Pre-defined shape for the icon container: `circle_color`, `circle`, `square_color`, `square`, `none`.
- **Icon inside card:** When enabled (default), the icon stays within the card boundaries. When disabled, the icon can overflow into the corners or outside the card edges — useful for oversized decorative icons.

---

## 5. Text

![Visual Editor - Text](../img/piotras-smart-button-edytor-5.jpg)

Manage typography, colors, and custom state labels.

**Name and Badge:**
- **Font style (1-4):** Global text formatting: `1` normal · `2` small-caps · `3` monospace · `4` uppercase + letter-spacing. Applies to both the name and state badge.

**Name:**
- **Name font size (px):** Size of the main display name.
- **Text color:** HEX color for the name label.

**State Badge:**
- **State font size (px):** Size of the state badge text.
- **Value color:** HEX color for the state badge (independent of name color).
- **Custom state (ON) / (OFF):** Override the auto-detected state label with custom text. Leave both empty to use auto-detected labels (e.g., `DIM`, `PLAY`, `HEAT`) based on the entity domain.

⚠️ **Both fields must be filled together** — setting only one of them has no effect. For per-state label control across multiple states, use `custom_states_labels` in YAML instead.

---

## 6. Layout

![Visual Editor - Layout](../img/piotras-smart-button-edytor-6.jpg)

Position individual card elements using a 9-grid directional interface.

**Placement Grids:**
- **Icon / Name / State:** Each element has its own 9-position grid (top-left to bottom-right). Click any position to anchor the element there.

**Layout Logic:**
- **Auto-Stacking:** Elements sharing the same grid position stack vertically in this order: `icon` → `name` → `state`.
- **Control Zone behavior:** The Control Zone (Slider/Power Bar) always occupies the bottom row. When enabled, all other elements automatically shift upward to avoid overlap — bottom row positions (7/8/9) are remapped to the middle row (4/5/6).

---

## 7. Slider & Power

![Visual Editor - Slider & Power](../img/piotras-smart-button-edytor-7.jpg)

Enable the Control Zone — an interactive strip at the bottom of the card.

**Bar / Slider:**
- **Show slider or power bar:** Enables the Control Zone. Its **content is auto-detected** based on the entity domain — you don't choose a type manually:
  - `light` → brightness + color temperature sliders
  - `fan` → speed percentage + preset mode slider
  - `cover` → position slider
  - `media_player` → volume slider
  - `climate` → `−`/`+` temperature stepper buttons
  - `sensor` (battery) → charge bar with percentage
  - `sensor` (temperature/humidity) → value with comfort color indicator
  - `person` / `device_tracker` → time since last state change
  - `weather` → humidity + wind speed
  - `on.clock` → current date
  - fallback → Power Bar (if `entity_watts` is set)
- **Bar height (px):** Height of the Control Zone strip. Content scales proportionally.
- **Label color:** HEX color for all labels and icons inside the Control Zone.

**Power Monitoring:**
- **Power entity:** Sensor tracking real-time energy consumption. Shown as a color-coded bar (green → yellow → red) when no other Control Zone content is detected.
- **Max watts:** Upper scale limit for the power bar.
- **Enable power warning:** Enables a pulsing animation when consumption exceeds the threshold. Note: set to `false` in YAML to disable — setting `0` does **not** disable it.

---

## 8. Filters

![Visual Editor - Filters](../img/piotras-smart-button-edytor-8.jpg)

Apply CSS image filters to the card background separately for ON and OFF states.

**General Switch:**
- **Enable image filters:** Toggles the entire filter module. Filters apply to the whole card background — including solid colors, gradients, and images.

**Filter — entity ON:**
- **Brightness:** Background brightness when active (e.g., `2.00` = double brightness).
- **Saturate:** Color saturation level (e.g., `0.50` = half saturation).
- **Grayscale:** Gray conversion amount (e.g., `0.00` = full color, `1.00` = full grayscale).
- *Generated string (read-only preview):* e.g., `brightness(2.00) saturate(0.50)`

**Filter — entity OFF:**
- **Brightness:** Dims the background when the entity is inactive (e.g., `0.45`).
- **Saturate:** Reduces color intensity for the inactive state (e.g., `0.20`).
- **Grayscale:** Adds gray for the inactive state (e.g., `0.50`).
- *Generated string (read-only preview):* e.g., `brightness(0.45) saturate(0.20) grayscale(0.50)`

---

## 9. Actions

![Visual Editor - Actions](../img/piotras-smart-button-edytor-9.jpg)

Define what happens when the user interacts with the card.

**Tap (Single Click):**
- **Action:** Behavior on a standard single tap. Default: `Toggle` (switches the entity on/off using `homeassistant.toggle`).

**Double-Tap:**
- **Action:** Behavior on a quick double tap. Default: `none`.

**Hold (Long Press):**
- **Action:** Behavior when pressing and holding the card for ~500ms. Default: `none`.
- **Service:** The Home Assistant service or script to call (e.g., `script.no_op`). Used when Action is set to `Call service`.

Available actions: `Toggle`, `More info`, `Navigate`, `Call service`, `None`.

⚠️ `Call service` does **not** automatically include the card's entity — if the service needs a target entity, add `entity_id` manually in `service_data` via YAML.

---

## 10. Service

![Visual Editor - Service](../img/piotras-smart-button-edytor-10.jpg)

Configure animated countdown feedback and interaction locking after triggering a service call.

**Notification Settings:**
- **Enable script notification support:** Enables the countdown module. Activates when any action is set to `Call service`.
- **Block the card during notification:** Prevents re-triggering the service while the countdown is active.
- **Notification duration:** Countdown length in seconds (e.g., `10`). Configurable freely — the editor suggests 5–60s as a practical range.
- **Display style:**
  - `Circle` — Animated SVG ring centered on the card, sized to the icon wrap. Only available when the Control Zone (`show_more`) is **off**.
  - `Bar` — Progress bar docked at the bottom of the card. Works with any entity and with the Control Zone enabled.

**Interaction Lock Logic:**
- **With Entity:** Blocks only the service call — prevents accidental double-triggering while the countdown runs. Other actions (toggle, navigate, more-info) remain active.
- **No Entity:** Blocks the entire card from any input until the countdown reaches 0s.

---

# Tips

- Use **Layout** for precise element positioning — combine with `show_more` to avoid the Control Zone overlap.
- Use **Background** to build themed dashboard tiles — solid colors, gradients, or full background images with filter transitions.
- Use **Icon styles** for visual consistency across your dashboard (`circle_color`, `square`, `none`, etc.).
- Use **Slider & Power** for any entity with a controllable value or sensor data worth monitoring at a glance.
- Use **Actions** for automations and scripts — combine Tap, Double-Tap, and Hold for up to 3 different behaviors on one card.
- **Advanced options** (`custom_states_labels`, `custom_states_on`, `custom_blockade`) are not exposed in the editor — they require YAML and are covered in the [Configuration Reference](CONFIGURATION.md).

> 🔙 Back to the [Main README](../README.md)
