# custom:piotras-smart-button -- Visual Editor Guide

The Visual Editor allows configuring `custom:piotras-smart-button`
directly from the Home Assistant interface.

No manual YAML editing is required for the most common settings. The
editor groups options into clear sections and adapts available settings
depending on the selected entity.

------------------------------------------------------------------------

## Sections

1.  General
2.  Size
3.  Background
4.  Icon
5.  Text
6.  Layout
7.  Slider & Power
8.  Filters
9.  Actions
10. Service

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 1. General

Basic card configuration.

Options:

-   Entity
-   Name
-   Icon
-   State Labels

The Entity field selects the Home Assistant object displayed or
controlled by the card.

Examples:

``` yaml
sensor.temperature
switch.garage
light.living_room
climate.home
```

State Labels allow replacing default states with custom text.

Example:

``` yaml
on: ACTIVE
off: INACTIVE
```

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-1.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 2. Size

Controls card dimensions and appearance.

Available options:

-   Card width
-   Card height
-   Border radius
-   Border width
-   Shadow

Used to match the card with the dashboard theme.

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-2.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 3. Background

Controls the card background.

Features:

-   background colors
-   gradients
-   ON/OFF images
-   image filters

Allows creating dynamic dashboard styles.

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-3.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 4. Icon

Controls icon appearance.

Available styles:

-   circle_color
-   circle
-   square_color
-   square
-   none

Additional settings:

-   icon size
-   icon color
-   ON state color
-   wrapper size

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-4.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 5. Text

Controls displayed text.

Options:

-   name size
-   state size
-   text colors
-   value colors
-   alignment

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-5.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 6. Layout

The Layout tab uses a 3x3 positioning system.

    1 2 3
    4 5 6
    7 8 9

Elements:

-   Icon
-   Name
-   State

Each element can be positioned independently.

When multiple elements use the same position they are automatically
stacked.

Example:

``` yaml
icon_mode: 1
name_mode: 8
value_mode: 8
```

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-6.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 7. Slider & Power

The Control Zone provides additional information and controls.

Used for:

-   batteries
-   lights
-   climate
-   devices with adjustable values

Battery support:

-   charging
-   discharging
-   full
-   not_charging

Example:

``` yaml
entity_battery_state: sensor.device_battery_state
```

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-7.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 8. Filters

Controls visual effects.

Examples:

-   state based appearance
-   image adjustments
-   additional styling

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-8.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 9. Actions

Defines card interactions.

Supported actions:

-   tap
-   hold
-   double tap

Examples:

-   toggle entity
-   open more-info
-   navigate
-   call service

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-9.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

------------------------------------------------------------------------

```{=html}
<table>
```
```{=html}
<tr>
```
```{=html}
<td width="50%">
```
# 10. Service

Service countdown configuration.

Used for temporary actions.

Options:

-   countdown time
-   circle style
-   progress bar style

Example:

``` yaml
time_service: 30
```

```{=html}
</td>
```
```{=html}
<td width="50%">
```
`<img src="../img/piotras-smart-button-edytor-10.jpg">`{=html}

```{=html}
</td>
```
```{=html}
</tr>
```
```{=html}
</table>
```

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
