---
name: create-module
description: Start a new Maru module (an instrument, effect, MIDI tool, sampler or sequencer) with a joint sound-and-interface brief, using the user's context and clarifying consequential gaps. Use when asked to create, start, scaffold or bootstrap a Maru instrument, effect, synth, plug-in or module project.
---

# Create a Maru module

Start a module from a Maru starter with one joint sound-and-face brief, then
prove the starter builds, renders and shows its face. When the request includes
DSP and UI, carry both through implementation and verification with `maru-agent`;
scaffolding or a state-only face does not finish that request.

## Steps

1. **Check the CLI.** Use the project's installed CLI (`npm exec -- maru …`) when present; run
   `maru --version`, then `maru doctor`. If missing, use
   `npx --yes maruaudio@latest` for authorized setup; a global install is optional. Match prerequisites
   to the requested operation. With only the hosted connector and no shell, skip to
   step 3 and work in the user's account.

2. **Plan sound and face together.** `maru guide read project-planning
face-from-reference ui-kit-faces` (MCP: `maru_read_guide` with those three
   names). Reuse a supplied brief and existing decisions; ask only about missing
   choices that change the result. For an open-ended idea, invite a short sound
   and design choice before committing to either: the musical character and
   response, then the visual direction and most important player gesture. Use
   plain language and concrete alternatives, recommend a direction, and offer
   **"Choose for me"**. For example: "Warm and subtle or crunchy and obvious?
   A studio console or a playful graphic face? I can choose a coherent pair."
   Ask only the relevant parts; a detailed brief, a small fix or "use your
   judgment" needs no new interview. If the user delegates, choose, state the
   assumptions briefly and proceed. While optional preferences are unanswered,
   continue the reversible starter/preview work; do not make them a setup gate.
   Do not require algorithm or architecture choices to get started; respect a
   technical direction the user supplies.
   Record real DSP/control behavior, player
   tasks, art direction and shared components in one brief; save it in
   `DEVELOPMENT.md` once the project exists. Keep planning brief enough to open
   the starter before extended design.
   Default to control contract -> working PluginUI React face -> DSP integration
   -> end-to-end proof. A concrete sound feasibility risk may need a small early
   probe. This sequence requires no extra approval within the authorized task.

3. **Resolve the destination from context.** Use the requested account, folder
   or current module workspace. If none is established, ask whether the user
   wants Studio in their Maru account or a folder on this computer.

4. **Choose a supported starter for the requested behavior.** Use the stated
   audio/MIDI inputs and outputs to choose the kind. Ask when the musical role
   or ports are ambiguous; explain the consequence in plain language. Do not
   ask the user to choose Graph versus Processor as an implementation detail.

   In a folder, `maru init --kind` offers these starters (`maru init --help`
   lists them for the installed version):

   | Kind                          | What it starts                                                                                   |
   | ----------------------------- | ------------------------------------------------------------------------------------------------ |
   | `processor-effect`            | Audio in, audio out, with its own DSP, a custom face and factory presets                         |
   | `processor-instrument`        | MIDI in, audio out: a polyphonic synth with its own DSP                                          |
   | `effect`                      | Audio in, audio out, wired from existing Maru DSP nodes (a Graph)                                |
   | `instrument`                  | MIDI in, audio out, wired from existing Maru DSP nodes (a Graph)                                 |
   | `--face-first --params a,b,c` | A passthrough effect with these parameters bound to a starter face, for designing the face first |
   | `--from <reference>`          | A copy of an official or remixable community module, such as `@maru/osc-mini`                    |
   | `firmware --device <id>`      | A source-only firmware listing for a device in Maru's hardware catalog                           |

   A MIDI tool (MIDI in, MIDI out, no audio) and a generator (audio out with
   no input) have no starter of their own. Say so, then offer to start from
   `processor-instrument` or `processor-effect` and change its facade as
   `maru guide read midi-and-event-modules` describes.

   In the user's account, call `maru_list_studio_templates` with
   `present: true` (optionally with `kind`: `effect`, `instrument`, `sampler`
   or `sequencer`). Reuse a selected template; ask if several materially
   different choices fit the brief.

5. **Record the intended formats.** Reuse formats already specified. Ask about
   missing distribution needs while continuing independent design work; a
   local prototype can proceed with browser verification and native formats
   recorded as undecided. Every audio module runs in the
   browser. Native formats are `standalone`, `vst3`, `clap`, `au` (macOS only)
   and `aax` (opt-in; the user supplies the AAX SDK). The default native set is
   VST3, CLAP, AU and standalone. Do not promise all of them without a stated
   need and qualification. For a local project, record a choice other than the default
   after the project exists, in `.maru/config.json` as
   `{"schemaVersion": 1, "build": {"nativeFormats": ["vst3", "clap"]}}` with
   the user's formats, and check it with `maru module paths .`. Local native builds need Maru's native toolkit (`maru doctor` shows
   whether it is installed). For hosted output, discover actual export and
   publication capabilities with `build-preview-export`; browser publication
   does not establish native builds, signing or delivery.

6. **Create within the authorized scope.** State the destination and kind,
   then proceed when the request already authorizes creation. Ask only for
   unresolved consequential choices. Creation does not authorize publishing.
   - In a folder: `maru init <dir> --kind <kind> --local` (or the
     `--face-first`, `--from` or firmware flags from the table, with
     `--local`). A request for a working local project includes installing
     its dependencies with `--install`. In a README-only clone, save the
     original brief before using `--force` with the same local init command
     targeting `.`; alternatively scaffold in a staging directory and merge
     without losing existing work. Follow
     the printed next steps and read the generated `AGENTS.md` and `README.md`.
     Keep one package manager and lockfile, and record the installed SDK version.
   - In the account: `maru_create_studio_project` with the exact `templateKey`
     the user chose, a short title and the idea as the `brief`. Give the user
     the Studio link from the result.

7. **Open the starter and check its baseline.** In a folder, start the generated
   `dev` script silently and keep it running for the user's preview. The npm
   SDK includes the workbench/player and hot reload; use them before considering
   a custom runner. Confirm readiness and give the URL promptly. A native
   toolkit or Maru source checkout is not required for the first JS preview.
   Verify the actual built face and module identity at that URL before extended
   design, account setup or native discovery. Describe starter or passthrough
   sound as such. If the port is occupied, use a free port accepted by the
   installed command; do not stop another server or assume a source-only flag
   is released.
   Run `maru module check . --fast` and inspect its starting face; retain the
   result to distinguish setup failures. Run full **Verify** on the finished
   module. Save the joint brief from step 2 in the project's Markdown notes,
   then follow the UI checkpoint and integration below with
   `maru-agent` or `add-parameter`. A starter check proves only the starter.

8. **Build the UI checkpoint.** Follow the face guides for the wireframe and
   shared component map. Use the provisional schema and host-controlled preview
   values in the existing workbench; `--face-first` suits a local UI study. Inspect
   a target image before React: supplied reference, existing PluginUI capture
   or generated concept. For a new open brief without a concrete visual target,
   establish that concept first. Save the image/reference and its defining
   composition, typography and material. Inspect the real TSX/CSS parts that
   express it; palette swaps alone are not distinct directions. Keep the
   inventory honest and move explanations into help/docs. Build and capture the actual React face from PluginUI
   primitives, exercise edits, states, presets and popups, and inspect all declared
   sizes before deep DSP. Keep audio code out of the face. Record temporary sound
   and state-only parameters in project notes, without explanatory panel clutter.

9. **Integrate DSP through the same contract.** Implement the requested sound,
   verify units/ranges/defaults/tapers, signal/control flow, choices/actions/state,
   presets and available host capabilities. Name real observable display data.
   Reconcile any necessary contract changes with the UI; preserve its chosen
   composition and shared behavior. Rerun UX checks after wiring, then measure
   actual sound. State-only controls and starter sound do not finish a combined
   request. Observe a face edit and a DSP edit activating through the retained
   workbench; a save or rebuild alone does not prove hot reload. Record the
   working preview and resume checkpoint in `DEVELOPMENT.md`. Judge visual
   quality separately from DSP evidence and listening.

## Rules

- Reuse the user's answers and established workspace context. Ask only for
  missing decisions that change the sound, player experience, musical role,
  destination or distribution. Save chosen defaults with the brief so a resumed
  agent does not ask again. Aesthetic defaults do not grant publication or
  generation permission.
- Use only kinds, template keys and references that `maru init --help` or
  `maru_list_studio_templates` returned. Never invent a template key, a
  project or a link.
- Scaffold with `maru init`; do not assemble a module project by hand.
- Pass `--local`: in a terminal, `maru init` otherwise offers to publish the
  new module to the user's profile. Publishing needs the user's explicit
  request.
- Preserve established module identity. Resolve a provisional local identity
  once through the installed publication guidance before first sharing; keep
  that identity for retries and updates. Never hand-edit `build/`, `dist/` or
  `*.generated.ts`.
- Start the workbench silently: `npm run dev -- --silent` (or
  `maru dev --silent`). Agent sessions do not play audio aloud; the user
  listens.

## Verify

Run these on the new project and read each result. A check that could not run
is a gap, not a pass.

1. `maru module check . --fast` while setting up, then the full
   `maru module check .` before you call the project ready.
2. Offline render: for an effect, `maru module eval --suite levels --max-seconds 2`;
   for a Graph module, `maru module eval`; for any module,
   `maru module preview --check` renders its demo plans and checks the audio.
   Report the measured numbers.
3. `maru module screenshot .` renders the built face at its declared size and
   refuses a blank, erroring or clipped face. Show the user the image.
   Review hierarchy, typography, grouping and interaction semantics against
   the saved brief; check minimum and maximum sizes too. Exercise parameter
   edits and preset recall through the real workbench. Report mechanical
   results separately from visual judgment and listening.
4. For each native format the user chose, `maru module verify .` (or
   `maru plugin build . --format <format>`) when the native toolchain is
   present. If `maru module doctor` reports it missing, say which formats were
   not built.

The same checks run over MCP on the local `maru-local` server as
`maru_module_check`, `maru_module_eval`, `maru_module_screenshot` and
`maru_module_verify`. The hosted connector cannot run them on a Studio
project; say that its preview in Studio is where the user plays it.

The user's listening is a separate step. Automated checks establish only the
behavior and targets they exercised; they do not approve its sound.

## Report

End with: where the project is (folder path or Studio link), the kind and
formats the user chose, files created, each check from **Verify** with its
result or the reason it did not run, the retained live preview URL and its
observed state, and the next step. When sharing is also requested, follow
`maru-agent`'s publication guidance and report the returned page and exact
publication identities separately from local verification.

## Maru feedback

If part of this workflow was harder than Maru should make it (a missing
command or tool, an awkward step, a starter you had to work around), end your
reply with the line `MARU FEEDBACK: <version>`, where `<version>` is what
`maru --version` prints (with only the hosted connector, write `connector`
and the server version your MCP client shows). Follow it with a short prompt
that could be used to improve Maru: what you were doing, what was awkward, and
the command, tool or API you wished for. Offer to file it at
https://github.com/maru-audio/maru-skill/issues or send it through
https://maru.audio/contact; file nothing without the user's yes.
