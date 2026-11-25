---
name: maru-agent
description: "Make instruments, effects, music and MIDI tools, sample kits and sampler banks with Maru. Use for Maru projects, DSP, plugin interfaces, device builds, firmware source forks, controller apps, musical-data code with Symbolic, audio generation and audio-to-MIDI, and for listing a plugin built with JUCE, iPlug2 or any other framework on the Maru marketplace: its page, media, offer and releases (uploaded, linked to the maker's download page, or from GitHub releases)."
---

# Maru

Help people turn musical ideas into instruments, effects, musical tools and
sound collections they can use and share. Start with the intended musical
result and the user's existing work.

Maru connects several capabilities: Studio for project work, the Engine for
instruments and effects, Symbolic for musical structures, and sound tools for
generation, samples and banks. Choose the parts the task needs. A Symbolic
script or a sample collection does not need an instrument project.

When introducing Maru, explain what the user can make and how the agent helps
them do it. Keep setup commands, runtime requirements and skill mechanics out
of that introduction; explain them when needed for the task. Point people to
[Maru skills](https://maru.audio/docs/skills) for setup and usage, and the
[platform documentation](https://maru.audio/docs) for detail.

The CLI, `maru`, supplies commands, options, schemas and guides for the
installed version. Discover those interfaces rather than relying on memory
or treating this file as an API reference.

## Workflow skills

This skill is the entry point. For these jobs, use the focused skill, which
has its own steps, rules, checks and report:

| Job                                                                    | Skill                                                                                                    |
| ---------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| Start a new instrument, effect, MIDI tool, sampler or sequencer        | `create-module`: uses the brief's kind, formats and destination; clarifies missing consequential choices |
| Add a knob, switch or mode to an existing module                       | `add-parameter`                                                                                          |
| Test or verify a module, or decide whether work is finished            | `verify-module`                                                                                          |
| A `maru` command, MCP tool, sign-in, build or workbench is not working | `debug-toolchain`                                                                                        |
| Build for a device, fork firmware, or make a hardware/controller app   | `build-for-hardware`: uses exact device evidence and the installed target workflow                       |
| Put a plug-in or module on Maru or its marketplace                     | `list-plugin`                                                                                            |

They ship with the Maru agent plugin. If one is not installed, read
`skills/<name>/SKILL.md` from https://github.com/maru-audio/maru-skill and
follow it.

## 1. Check the CLI

```sh
maru --version
```

Inside a project, use its installed CLI (`npm exec -- maru …`) and scripts so
versions agree. For an authorized setup without a CLI, use
`npx --yes maruaudio@latest` to start; a global install is optional.
Record the version actually used; source documentation or a plugin update does
not establish which SDK the project or local server runs. Preserve the existing
package manager and lockfile.

Then check what this machine can do:

```sh
maru doctor
```

It reports sign-in state, installed components and missing prerequisites, each
with the command that fixes it. Match missing prerequisites to the operation
requested: a missing native toolkit does not block a local JS preview.

## 2. Discover before acting

```sh
maru --help                  # command groups
maru <group> --help          # commands and options in one group
maru guide list              # guides, each with the situation it applies to
maru guide read <name...>    # read the guides whose trigger matches the task
maru tools describe <operation> --json # exact schema for the operation needed
```

Read the matching guides before writing code or running a build. Add `--json`
to any command when you need structured output. Read focused guides in small
batches within the installed interface's limit; do not load every guide or tool
schema before starting a known starter.

The same guides are published as Markdown for reading without the CLI: the
[guide index](https://maru.audio/guides/llms.txt) lists each with when to
read it, `https://maru.audio/guides/<name>.md` serves one, and
[all guides](https://maru.audio/guides/llms-full.txt) come in one file.
Prefer `maru guide read` when the CLI is installed: it matches its version.

When choosing or implementing DSP, look up the relevant existing effects, nodes
or modules and prefer reuse where it meets the brief:

```sh
maru dsp effects             # named effects you can chain in MultiFX, by family
maru module list             # modules in the catalog
maru dsp nodes               # the engine's DSP nodes and their parameters
```

If the host supports MCP, `maru mcp` serves the same operations as tools.
`maru mcp install` registers Maru's hosted connector with the agent, for the
user's Maru account (projects, builds, community modules, generation; sign-in
on first use); `--local` registers `maru mcp` for this checkout and `--both`
registers both. It changes the agent's configuration; use existing setup
authorization or ask before changing it.
Preserve a working connection. Verify that the running client can discover the
needed skills and perform a local guide or project operation against the intended
folder after any required reload; written configuration or a standalone server
test alone does not prove client attachment. Check account access when the task
first needs it, rather than delaying local JS work for sign-in.

## 3. Scaffold, preview and build

For a new local module, use the brief's musical role, formats and destination,
as `create-module` describes. For an open brief, invite consequential sound and
design choices with a recommendation and a "choose for me" option; reuse a
clear brief or delegated judgment without another interview. Inspect
`maru init --help` and use the bundled scaffolder:

```sh
maru init my-effect --kind processor-effect --local --install
# For a MIDI instrument: --kind processor-instrument
```

The scaffold prints the dependency-install command and next steps. Follow the
generated project's `AGENTS.md` and README, inspect its package scripts, and
install dependencies with the project's package manager. A request for a
working local project includes its dependency setup. Preserve a README-only
repository's brief before scaffolding into it; `--force` may overwrite files.
Reuse its musical choices and save the brief and resume checkpoint in
`DEVELOPMENT.md`; do not repeat the product interview after a restart.

Run the generated `dev` script for the bundled workbench and hot reload. The
npm SDK supplies that runner; do not build a separate host page or require a
Maru source checkout for the first JS preview. Keep the process running for
the user's requested preview, check its actual ready result and share the URL
as soon as it is ready, before extended design or native discovery. Label the
starter's sound honestly. Then continue implementation and verification there.

The project's README becomes the module's public About page when it is
published. Keep it for players and put development notes in `DEVELOPMENT.md`.

Choose guidance by the user's goal; read only what the task needs using
`maru guide read <name...>`:

| Goal                                                                                             | Guidance                                                                                                                                                                                                                                                                                                                                                                                     |
| ------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Design any module face, including MIDI tools, with or without an image                           | `face-from-reference`, `ui-kit-faces`: control inventory, PluginUI assembly/theme, wireframe, shared components and rendered review; order DSP proofs by the risks in `project-planning`                                                                                                                                                                                                     |
| Build or change an instrument or effect                                                          | `processor-authoring`, `parameters-and-schema`; `ui-kit-faces` for controls, `build-preview-export` for delivery                                                                                                                                                                                                                                                                             |
| Set up CI builds or publish from GitHub                                                          | `build-preview-export`; [CI workflow example](https://maru.audio/docs/build-and-publish-in-ci), after checking installed command and component availability                                                                                                                                                                                                                                  |
| Build a MIDI processor or sequencer                                                              | `midi-and-event-modules`, plus the module guides for the parts being changed                                                                                                                                                                                                                                                                                                                 |
| Work with notes, chords, scales or rhythms in code                                               | [Symbolic library guide](https://maru.audio/docs/symbolic-library); check the installed package's public exports                                                                                                                                                                                                                                                                             |
| Make sounds, kits or sampler banks                                                               | `sample-kits-and-banks`, `generation-and-transcription`; `assets-and-resources` when loading samples into a module                                                                                                                                                                                                                                                                           |
| Generate audio or transcribe a recording                                                         | `generation-and-transcription`                                                                                                                                                                                                                                                                                                                                                               |
| Build firmware or a MIDI-controller app for a music device                                       | `build-for-hardware`; discover matching installed guides and exact device/target capabilities                                                                                                                                                                                                                                                                                                |
| List firmware you wrote for a music device                                                       | `build-for-hardware` and the [Community firmware guide](https://maru.audio/docs/community-firmware)                                                                                                                                                                                                                                                                                          |
| List my plugin on the Maru marketplace (built with JUCE, iPlug2, DPF, nih-plug or anything else) | The `list-on-marketplace` task: `maru marketplace whoami`, `project`, `draft`, `media`, `release`, `offer`, `submit`, `status`. Confirm the name, price and licence with the user before submitting; no Maru build                                                                                                                                                                           |
| The same plugin from a public GitHub repository and its releases                                 | `repository-listing`: `maru listing init`, `check`, `publish`, `status`. In CI, `MARU_TOKEN` holds a token with only `listings:publish` from https://maru.audio/account/tokens                                                                                                                                                                                                               |
| List a plugin already built with another framework (JUCE, iPlug2, binaries on GitHub releases)   | `repository-listing`: `maru listing init`, `check`, `publish`, `status`; no Maru build. In CI, `MARU_TOKEN` holds a token with only `listings:publish` from https://maru.audio/account/tokens                                                                                                                                                                                                |
| Publish this plugin on Maru (a NIH-plug, DPF, JUCE or other project that isn't a Maru module)    | `publish-external-plugin`: run `maru publish` in the project; it detects the framework and offers the three routes. `--route project` uploads, detects and builds after the maker confirms recipe and platform; then `maru build status <id> --wait-seconds 45`, `maru release check`, `maru release publish`. Each failure ends with a code in brackets; the guide says what to do for each |

For interface icons, browse [Maru Icons](https://pluginui.com/icons) and use
the separately published `maru-icons/react` components. They work with Maru
controls and other React interfaces; reuse the collection before drawing new
glyphs. Read its [usage and license notices](https://github.com/maru-audio/maru-icons)
when distributing icons or using Maru brand marks. Detailed face guidance stays
in `ui-kit-faces`.

Music theory explanations can be answered directly. Musical-data scripts use
Symbolic without scaffolding an audio module or starting its workbench. Read
`cli-and-operations` for command conventions and `maru guide list` for other
available guides, including presets and saved state.

- Keep the declaration, parameter contract, Processor, face and presets in
  the generated project. Use its public `maruaudio/*` imports and shared
  controls; the face controls the module rather than implementing its audio.
- Run the project's typecheck and `maru module check . --fast` while editing.
  Before calling the work done, run the full `maru module check .`: `--fast`
  skips the publish and browser gates, so it is not a pass. If a gate fails or
  is unavailable, say so and fix it rather than reporting success. After
  changing the face, run `maru module screenshot .`: it renders the built face
  at its declared size and refuses one that is blank, erroring or overflowing,
  and the image shows the user what changed. Inspect
  `maru module check --help` and `maru plugin build --help` before choosing a
  build target.
- Distinguish a local package from an online publication. Read
  `build-preview-export` and inspect the installed CLI's help for the commands
  that perform each. A request to build locally does not authorize publication;
  existing explicit authorization to publish counts.
- For authorized sharing, reuse the selected account, API origin and CLI profile.
  When local MCP supplies unverified CLI context, use its explicit `--profile`
  for subsequent CLI commands; a named profile does not change plain CLI defaults.
  Check `maru whoami` and the intended operation's required grants; hosted MCP
  sign-in does not establish CLI publication authority. Use supported sign-in
  interfaces only when needed. Never extract connector credentials or promise
  automatic credential transfer between the connector and CLI.
- Before publishing, resolve first-publication identity through the installed
  guidance, confirm audience, source visibility and licence from the brief, and
  inspect the dry-run. Retain the publisher's receipts and resume state. Verify
  the returned page and exact account, module/project, revision and Release
  identities it supplies. A dry-run, local install or preview is not sharing;
  retries and updates must continue the same project rather than create a copy.
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
- For an effect, check its output level against its input after changing a
  gain law, drive, mix or preset: `maru module eval --suite levels` prints
  output minus input loudness per preset at several input levels. Narrow it
  while iterating (`--sweep <param>`, `--no-presets`, `--input-lufs -18`,
  `--max-seconds 2`) and add `--tolerance <LU>` to make it fail. It runs only
  when asked; report the measured numbers, and remember that equal loudness
  readings do not replace listening.
- Claim only targets you actually built and checked. Never present a mock or
  placeholder render as real output.
- Read MCP results for `isError` and operation failures; a successful tool
  transport alone does not establish that a build or server started.
- When something is missing (sign-in, a component, an extension), report the
  exact command `maru` printed, such as `maru login`.
  Do not work around it with a different runtime or a hand-written substitute.
- Use existing authorization for setup and reversible local work. Obtain
  authorization for additional account/configuration changes, paid or hosted
  work, publication or destructive actions. Judge a command's effect; `--yes`
  is not itself a new action or permission.
- Never approve sound by reading code, and do not play audio aloud: start the
  workbench with `--silent` and leave listening to the user.
- Finish with: files you changed or created, output files, the settings used
  (including seeds), the checks you ran and their results, and anything that
  was unavailable.

## Maru feedback

If part of the work was harder than Maru should make it (a missing command or
tool, an awkward step, a check you could not run), end your reply with the
line `MARU FEEDBACK: <version>`, where `<version>` is what `maru --version`
prints (with only the hosted connector, write `connector` and the server
version your MCP client shows). Follow it with a short prompt that could be
used to improve Maru: what you were doing, what was awkward, and the command,
tool or API you wished for. Offer to file it at
https://github.com/maru-audio/maru-skill/issues or send it through
https://maru.audio/contact; file nothing without the user's yes.

Documentation for people: https://maru.audio/docs/skills
