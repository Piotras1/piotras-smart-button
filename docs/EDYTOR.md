# 🖥 Visual Editor

The Visual Editor allows configuring `custom:piotras-smart-button`
directly from the Home Assistant interface.

No manual YAML editing is required for the most common settings. The
editor groups options into clear sections and adapts available settings
depending on the selected entity.

------------------------------------------------------------------------

## 1. General

| General Configuration & Description | Preview |
| :--- | :--- |
| Configure primary settings and element visibility for the card.<br><br>**Entity and Name:**<br>• **Entity:** Selects the Home Assistant object displayed or controlled by the card.<br>• **Display name:** Sets a custom label for the card (e.g., `All Home`).<br><br>**Visibility Options:**<br>• **Show icon:** Toggles icon visibility.<br>• **Show name:** Toggles name visibility.<br>• **Show entity state:** Toggles the state label visibility. | <img src="../img/piotras-smart-button-edytor-1.jpg" width="350"> |

------------------------------------------------------------------------

## 2. Size

| Size Configuration & Description | Preview |
| :--- | :--- |
| Adjust the card dimensions and outline styles to match your dashboard theme.<br><br>**Size Card:**<br>• **Card width:** Sets the width of the card (e.g., `auto`).<br>• **Card height (px or auto):** Sets the card height in pixels (e.g., `140`).<br><br>**Style Card:**<br>• **Border radius (px):** Defines how rounded the corners are (e.g., `12`).<br>• **Border width (px):** Sets the thickness of the border line (e.g., `1`).<br>• **Border color:** Standard HEX color picker for the card outline (e.g., `#804040`).<br>• **Box shadow (outer):** Advanced outer shadow effect using CSS styling. | <img src="../img/piotras-smart-button-edytor-2.jpg" width="350"> |

------------------------------------------------------------------------

## 3. Background

| Background Configuration & Description | Preview |
| :--- | :--- |
| Manage card background options, including solid colors, multi-color linear gradients, and image behavior.<br><br>**General Settings:**<br>• **Enable background image:** Toggles the use of a background photo or texture.<br>• **Enable image filters:** Activates custom visual filters (ideal for unique ON/OFF state styles).<br>• **Card inner shadow:** Toggles the inner glow/shadow effect of the card background.<br><br>**Gradient Settings:**<br>• **color 1 / 2 / 3:** Setting just `color 1` creates a solid color background. Adding `color 2` or `color 3` automatically triggers a Linear Gradient mode.<br>• **Angle (°):** Controls the direction of the color transitions (e.g., `133`). | <img src="../img/piotras-smart-button-edytor-3.jpg" width="350"> |

------------------------------------------------------------------------

## 4. Icon

| Icon Configuration & Description | Preview |
| :--- | :--- |
| Customize the behavior, appearance, and placement of the icon for different device states.<br><br>**Style Icon:**<br>• **Icon (mdi:...):** Sets the default icon name from the Material Design Icons library (e.g., `mdi:lightbulb`).<br>• **Icon when ON (mdi:...):** Defines an alternative icon for the `ON` state (e.g., `mdi:lightbulb-on`). Leave empty to use the same icon for both states.<br>• **Icon color (OFF) / (ON):** Assigns separate HEX colors for each state.<br><br>**Size Icon:**<br>• **Icon size (px):** Sets the size of the icon itself (e.g., `44`).<br>• **Wrap size (px):** Controls the dimensions of the icon's container bounding box.<br>• **Icon style:** Applies pre-defined styles or custom shapes to the icon container.<br>• **Icon inside card:** Toggles containment. Disabling this allows the icon to overflow outside the card boundaries or into the corners. | <img src="../img/piotras-smart-button-edytor-4.jpg" width="350"> |

------------------------------------------------------------------------

## 5. Text

| Text Configuration & Description | Preview |
| :--- | :--- |
| Manage typography, colors, and custom text labels for both the card name and state badges.<br><br>**Name and Badge:**<br>• **Font style (1-4):** Selects the global text formatting style (`1.normal`, `2.small-caps`, `3.monospace`, `4.uppercase`).<br><br>**Name:**<br>• **Name font size (px):** Sets the size of the main display name (e.g., `22`).<br>• **Text color:** HEX color picker for the primary card title text.<br><br>**State Badge:**<br>• **State font size (px):** Sets the text size for the device state label (e.g., `18`).<br>• **Value color:** HEX color picker for the state text value.<br>• **Custom state (ON) / (OFF):** Overrides default Home Assistant states with custom labels. Leave empty to auto-detect labels (like DIM, PLAY, HEAT) dynamically. | <img src="../img/piotras-smart-button-edytor-5.jpg" width="350"> |

------------------------------------------------------------------------

## 6. Layout


------------------------------------------------------------------------

## 7. Slider & Power


------------------------------------------------------------------------

## 8. Filters


------------------------------------------------------------------------

## 9. Actions


------------------------------------------------------------------------

## 10. Service


------------------------------------------------------------------------

# Tips

For best results:

-   use Layout for precise positioning
-   use Background for dashboard themes
-   use Icon styles for consistency
-   use Slider & Power for sensors and devices
-   use Actions for automations

The Visual Editor exposes the most common options while advanced YAML
configuration remains available for advanced users.
