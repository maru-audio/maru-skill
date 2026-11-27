---
name: plugin-accessibility
description: Review and improve the accessibility of an audio plug-in's interface and parameters - names, groups, units and descriptions that screen readers, hosts and hardware controllers announce, keyboard reach, focus, contrast, scaling and value entry. Use when asked to make a plug-in, module face or instrument UI accessible, to audit it for blind or low-vision musicians, or before releasing a plug-in interface. Works for Maru modules and for plug-ins built with other frameworks.
---

Accessibility in a plug-in has two layers. Fix the first before the second:
hosts, generic editors, hardware controllers and screen readers all read the
parameters, while only some people ever see the custom interface.

## 1. Parameters

Every host presents the parameter list: automation lanes, generic editors,
controller pages and screen-reader announcements. Check each parameter for:

- a short, specific label that is unique within its group and does not repeat
  the group name ("Cutoff" in Filter);
- a group named after what it shapes (Filter, Envelope, Output), ordered along
  the signal path;
- units on every time, frequency and level, and percent for 0 to 1 amounts;
- a one-sentence description where the label alone does not say what happens;
- value text a person can read aloud ("200 ms", "Saw", "-6 dB"), including
  for switches and choices, and value parsing so typed entry works in hosts
  that offer it;
- a short label of up to 8 characters for long names, for hardware and narrow
  host displays;
- a sensible default and a stable order: hosts identify parameters in saved
  automation by their order or ID, so never reorder or renumber to regroup.

For a Maru module these are schema fields (`sections`, `group`, `units`,
`description`, `shortLabel`, `tier`), explained at
https://maru.audio/docs/accessible-controls. Run `maru module check` (or
`maru_module_check` on the local server) and act on its `parameter-layout`
findings. In other frameworks, find the equivalent: parameter groups, value
to text and text to value functions, and the host's parameter info.

## 2. The interface

Check the custom face with the interface itself, not by reading code alone:

- **Keyboard**: every control can be reached with Tab in a sensible order,
  shows a visible focus ring, and moves with arrow keys, with a fine step on a
  modifier and Home or End for the range ends. Do not capture keys the host
  uses for transport and editing (space, numbers, letters) unless the plug-in
  window clearly has focus and the user is in a text field.
- **Screen readers**: each control exposes a role (slider, switch, button,
  combo box), its name and its current value with units, and announces value
  changes. Custom-drawn knobs need these added explicitly; a canvas or bitmap
  face exposes nothing by default. Test with VoiceOver on macOS and NVDA on
  Windows where possible, and note that what reaches the reader depends on the
  host and the plug-in format.
- **Seeing it**: text and essential graphics meet WCAG 2.1 AA contrast (4.5:1
  for text, 3:1 for control outlines and indicators). No state is shown by
  colour alone: pair it with position, shape, text or a pattern. Meters and
  modulation displays stay readable for common colour-vision differences.
- **Size**: the window scales (at least 100% to 200%) without clipping, and
  controls stay at least 24 by 24 points at the default size.
- **Precision**: a control's value can be typed, reset to default (for
  example double-click) and adjusted finely, so nothing depends on a precise
  drag.
- **Motion**: animation is decorative only and respects the system's
  reduced-motion setting.

For a Maru module, `maru_module_screenshot` captures the face, and
`maru_read_ui_props` shows the props of the UI kit controls it uses; the UI
kit's knobs, sliders and menus already provide roles, names, values and
keyboard behaviour, so prefer them to custom drawing.

## Report

List findings by layer, most serious first: what is wrong, who it affects,
the fix and where to make it. Say which checks were run against the real
interface and which were only read from code, and do not call the plug-in
accessible from a code reading alone.
