---
name: make-presets
description: "Make and save presets for Maru instruments and effects by describing a sound, without writing code. Use when someone wants a preset, a sound for a module, a variation of a factory preset (\"darker\", \"longer tail\"), or to save and share sounds they made with a Maru module."
---

Help the user get the sound they describe out of an existing Maru module. This
is playing and sound design, not module development: do not scaffold or edit
module source.

## Find a starting point

1. When the user names no module, call `maru_search_module_presets` with their
   words ("warm pad", "plucky bass"). Show up to five hits with the module,
   the preset name and the measured words the result gives. Quote those words;
   never describe a sound beyond what they say.
2. When the user names a module, call `maru_read_module_spec` for its factory
   presets and `maru_list_module_params` for its controls, ranges and units.
   `maru_list_modules` lists the official modules.

## Shape the sound

3. For a change relative to a preset ("darker", "shorter attack", "wider"),
   call `maru_search_module_presets` with `moduleSlug` and `fromPresetId`. When
   no preset moves that way, its `parameters` list names the controls whose
   measured sweeps do, and in which direction.
4. Call `maru_preview_module_preset` with the module, the starting preset and
   the parameter values by path. It saves nothing. Give the user the `url` so
   they can play it, and say which controls changed and which values were
   clamped.
5. Iterate from what they hear. Change a few controls at a time and say what
   each change is meant to do.

## Save it

6. Saving needs the user's Maru account and a project. In a project attached
   to the session, call `maru_list_project_presets`, then
   `maru_insert_project_preset` with a name, the parameter values and, if they
   want it, a one-line description. Presets are private unless the user asks
   for another visibility. Ask before saving or changing visibility.
7. To change a saved preset, call `maru_edit_project_preset` with its `id`.
8. Without a project, `maru_list_studio_templates` and
   `maru_create_studio_project` start one, or the user can open the preview
   link and save it in Studio.

## Racks

A rack connects several modules into one chain (for example a synth into a
delay and a reverb). Racks are arranged and saved in Studio on maru.audio;
describe the chain and the settings for each module, give a preview link for
each module's sound, and point the user to Studio to connect them.
