---
name: maru-agent
description: Make instruments, effects, music and MIDI tools, sample kits and sampler banks with Maru. Use for Maru projects, DSP, plugin interfaces, musical-data code with Symbolic, audio generation and audio-to-MIDI.
---

# Maru

Maru is an audio creation platform. Its CLI, `maru`, is the source of truth for
what you can do: commands, options, schemas and guides come from the installed
version, so always ask the CLI rather than relying on memory or on this file.

## 1. Check the CLI

```sh
maru --version
```

If `maru` is not installed, use `npx maruaudio@latest` in its place for every
command below, and suggest that the user install it with `npm i -g maruaudio`.

Then check what this machine can do:

```sh
maru doctor
```

It reports sign-in state, installed components and missing prerequisites, each
with the command that fixes it.

## 2. Discover before acting

```sh
maru --help                  # command groups
maru <group> --help          # commands and options in one group
maru guide list              # guides, each with the situation it applies to
maru guide read <name...>    # read the guides whose trigger matches the task
maru tools describe --json   # exact input schemas for every operation
```

Read the matching guides before writing code or running a build. Add `--json`
to any command when you need structured output.

If the host supports MCP, `maru mcp` serves the same operations as tools.

## 3. Scaffold, preview and build

For a new local module, inspect `maru init --help` and use the bundled
scaffolder instead of assembling a project by hand:

```sh
maru init my-effect --kind processor-effect
# For a MIDI instrument: --kind processor-instrument
```

The scaffold prints the dependency-install command and next steps. Follow the
generated project's `AGENTS.md` and README, inspect its package scripts, and
install dependencies with the project's package manager. Scaffolding alone
does not install them; `--install` opts in.

Choose guidance by the user's goal; read only what the task needs using
`maru guide read <name...>`:

| Goal                                               | Guidance                                                                                                            |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------- |
| Build or change an instrument or effect            | `processor-authoring`, `parameters-and-schema`; `ui-kit-faces` for controls, `build-preview-export` for delivery    |
| Build a MIDI processor or sequencer                | `midi-and-event-modules`, plus the module guides for the parts being changed                                        |
| Work with notes, chords, scales or rhythms in code | [Symbolic library guide](https://maruaudio.com/docs/symbolic-library); check the installed package's public exports |
| Make sounds, kits or sampler banks                 | `sample-kits-and-banks`, `generation-and-transcription`; `assets-and-resources` when loading samples into a module  |
| Generate audio or transcribe a recording           | `generation-and-transcription`                                                                                      |

Music theory explanations can be answered directly. Musical-data scripts use
Symbolic without scaffolding an audio module or starting its workbench. Read
`cli-and-operations` for command conventions and `maru guide list` for other
available guides, including presets and saved state.

- Keep the declaration, parameter contract, Processor, face and presets in
  the generated project. Use its public `maruaudio/*` imports and shared
  controls; the face controls the module rather than implementing its audio.
- Run the generated `dev` script for the live workbench. Edit and audition
  there, using the user's listening feedback for musical acceptance.
- Run the project's typecheck and fast module checks, then its portability
  checks for compiled targets. Inspect `maru module check --help` and
  `maru plugin build --help` before choosing a build target.
- Use the generated build script for the requested format, such as
  `build:vst3`, and its verification script. Read the build report: a binary
  alone does not prove validation passed. Report missing validators or
  toolchains and qualify only the formats actually checked.

For an existing project, use its current layout and scripts. Hosted Studio
projects use the `project` operations described by the CLI; do not confuse
those with local `maru init` scaffolding. Web packaging, native export and
remote publication are distinct actions; inspect the operation schema before
running one.

## 4. Work rules

- Start from the smallest real project that meets the musical goal. Read a
  project's source before changing it.
- Check results with the CLI: validate, preview, evaluate and inspect actual
  audio and MIDI output. A passing build or a silent render is not a finished
  instrument; say what you verified and how.
- Claim only targets you actually built and checked. Never present a mock or
  placeholder render as real output.
- When something is missing (sign-in, a component, an extension), report the
  exact command `maru` printed, such as `maru login`.
  Do not work around it with a different runtime or a hand-written substitute.
- Obtain the user's authorization before signing in, downloading components,
  running paid or hosted generation, publishing, or passing `--yes` to a
  destructive command. Existing explicit authorization for that action counts.
- Finish with: files you changed or created, output files, the settings used
  (including seeds), the checks you ran and their results, and anything that
  was unavailable.

Documentation for people: https://maruaudio.com/docs
