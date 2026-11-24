---
name: add-parameter
description: Add one parameter (a knob, switch or mode) to an existing Maru module, through the schema, the Processor, the face and presets, and prove from rendered audio and a face screenshot that it works. Use when asked to add, expose or wire a control, parameter or automation target to a Maru instrument or effect.
---

# Add a parameter to a Maru module

A Maru parameter is defined once, in the module's schema. The DSP, plug-in
hosts, MIDI CC, presets and the face all read it by its path. Adding one is a
small, countable set of edits; if it needs more, that is a gap to report, not
a pattern to copy.

## Steps

1. **Read the contract first.** `maru guide read parameters-and-schema`, plus
   `processor-authoring` for a Processor and `ui-kit-faces` if the control will
   be visible. Read the module's current parameters from its schema file
   (usually `src/contract.ts`) rather than from the face. For an official
   module, `maru module params <slug>` prints its table.

2. **Agree on the parameter.** Confirm with the user its label, range,
   default, units and whether it is shown on the face. Pick a `path` that
   describes what it shapes; the path becomes a stable automation ID.

3. **Make the edits in this order.** In the starter layout:
   1. **Schema** (`src/contract.ts`): append one descriptor with `path`,
      `label`, `type`, `min`, `max`, `default`, and `units`, `step`, `taper`
      or `options` where they apply. Append it at the end; never reorder,
      rename or reuse an existing path.
   2. **Processor** (`src/dsp/processor.ts`): add the typed state field, set
      it in `createState` and `reset`, add the `setParameter` case for the
      path, and use the value in `process`, through an existing
      `maruaudio/dsp` leaf where one fits. A Graph module maps it with
      `expose()` instead.
   3. **Face** (`src/ui/index.tsx`), only if it is visible: bind a
      `maruaudio/ui` control to the path, such as `SchemaFace` or
      `FaceParamKnob`. Take the range and default from the descriptor; do not
      repeat them as literals. `maru guide props <Component>` lists a
      component's props.
   4. **Presets and tapes**: factory presets and `src/qualification.ts`
      derive their defaults from the schema. Change a preset only when it
      should sound different with the new control, and check that every
      preset path still exists.

4. **Check while editing.** Run the project's typecheck and
   `maru module check . --fast` after each step. A typecheck catches a
   missing `setParameter` case in the starter projects.

5. **Verify from outside the edit** with the checks below, then report.

## Rules

- One parameter is one descriptor and one `setParameter` case (or
  `expose()`), plus a face binding if it is visible. Do not add a second
  parameter registry, a reflective dispatch or a face-local value map.
- Parameter order is the host's automation ABI. Append; never reorder or
  remove a parameter in a released module.
- Fix a control's feel with `taper` in the schema, never with a curve in the
  face or a remap in the DSP.
- `process` allocates nothing, locks nothing and performs no I/O. Read
  `maru guide read restricted-typescript` before writing DSP.
- Never change the module `id`. Never hand-edit `build/`, `dist/` or
  `*.generated.ts`.
- If adding the parameter forces you to copy its range or default into
  several files, report that as Maru feedback instead of normalising the
  copies.

## Verify

Prove the parameter changes the output, from tools that did not write the
edit. Report each result, and report any check that could not run as a gap.

1. **Structure.** The full `maru module check .`. Its `parameter-layout` row
   scores the schema's sections and labels; act on its findings.
2. **Rendered effect of the new parameter.**
   - An effect: `maru module eval --suite levels --sweep <path> --no-presets --max-seconds 2`
     sweeps the new parameter from the defaults and reports output minus input
     loudness, true peak and the transfer slope per step. Add
     `--tolerance <LU>` if the parameter must not change the level.
   - A Graph module: `maru module eval` before and after reports peak and RMS.
   - Any module with a native build: render the standalone driver at two
     values and compare the files, for example
     `maru app run . --render low.wav --param <path>=<min> --seconds 2` and the
     same with `<max>`; for an instrument add `--notes notes.json` and
     `--expect-audible`.
   A parameter whose renders do not differ is not working, whatever the code
   says.
3. **Face.** `maru module screenshot .` at the defaults, and with
   `--preset <id>` for a preset that sets the new parameter, to show the face
   draws the value it was given rather than one it keeps itself. Show the user
   the images.
4. **Formats.** `maru module verify .` for every declared native format when
   `maru module doctor` reports the native toolchain ready. Name each format
   that was not built.

Over MCP, the local `maru-local` server runs these as `maru_module_check`,
`maru_module_eval`, `maru_module_screenshot` and `maru_module_verify`. The
hosted connector reads project source but does not edit it: a Studio project
is changed in Studio.

Measurements show that the parameter acts; they do not approve how it sounds.
Ask the user to play it in the workbench (`npm run dev`); agent sessions stay
silent (`npm run dev -- --silent`).

## Report

End with: the parameter's path, range, default and where it is shown; each
file changed; the measured numbers from **Verify** (values compared, loudness
or peak per step); the screenshots; checks that did not run and why; and any
preset you changed.

## Maru feedback

If part of this workflow was harder than Maru should make it (a missing
command or tool, an awkward step, values you had to copy by hand), end your
reply with the line `MARU FEEDBACK: <version>`, where `<version>` is what
`maru --version` prints (with only the hosted connector, write `connector`
and the server version your MCP client shows). Follow it with a short prompt
that could be used to improve Maru: what you were doing, what was awkward, and
the command, tool or API you wished for. Offer to file it at
https://github.com/maru-audio/maru-skill/issues or send it through
https://maru.audio/contact; file nothing without the user's yes.
