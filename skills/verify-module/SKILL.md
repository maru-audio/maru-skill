---
name: verify-module
description: Check a Maru module from the outside before calling it done - release checks, offline render metrics, a face screenshot and a build in every declared format - and report what passed, what failed and what could not run. Use when asked to test, verify, check, qualify or validate a Maru module, or before saying a module, a change or a release is finished.
---

# Verify a Maru module

Prove a module works with evidence that does not come from the edit itself:
Maru's own checks, rendered audio, the built face and native builds. Your
reading of the code is not evidence, and neither is a tool's "done".

## Steps

1. **Know what is declared.** Read the module's declaration (`src/index.ts`)
   for its kind and ports. Reuse the brief and checkpoint in `DEVELOPMENT.md`;
   record the installed CLI/SDK version and the requested verification scope.
   Run `npx --no-install maruaudio module paths .` for the project settings,
   including the declared native formats (`build.nativeFormats` in
   `.maru/config.json`; VST3, CLAP, AU and standalone when unset).
   Keep local JS review, native qualification and online sharing as separate
   outcomes; default native settings do not require toolkit setup before a JS
   preview can be reviewed.

2. **Check the toolchain.** `npx --no-install maruaudio module doctor` (MCP: `maru_module_doctor`).
   Note what it reports missing; every check that needs it becomes a gap.
   Missing native tools do not prevent running the independent browser checks.

3. **Run the release checks.** `npx --no-install maruaudio module check .` (MCP:
   `maru_module_check`). `--fast` skips the browser and Processor-compiler
   gates, so a fast pass is not a pass. Add `--tests` to run the project's own
   test script too.

4. **Render offline and read the numbers.** Choose by what the module is:
   - An effect: `npx --no-install maruaudio module eval --suite levels` (MCP: `maru_module_eval` with
     `suite: "levels"`). It renders the defaults and every factory preset at
     several input levels and reports output minus input loudness, true peak
     and the transfer slope. `--tolerance <LU>` and `--true-peak-ceiling <dBTP>`
     make it fail outside those limits.
   - A Graph module: `npx --no-install maruaudio module eval` reports peak and RMS of a fixed
     render, or the output event count for a control Graph.
   - Any module: `npx --no-install maruaudio module preview --check` renders its demo plans and
     checks the audio; it fails when nothing sounds.
   - After a native build: `npx --no-install maruaudio app run . --render out.wav --seconds 3`,
     with `--notes notes.json --expect-audible` for an instrument, renders the
     standalone app's own host offline. `--param <path>=<value>` sets a
     parameter first.

5. **Capture the face.** `npx --no-install maruaudio module screenshot .` (MCP:
   `maru_module_screenshot`) builds the module, opens its own built face at
   its declared size and refuses a blank, unready, clipped or erroring face.
   `--preset <id>` captures a factory preset. Inspect the installed command's
   help and guides for available host-follow and interaction checks. A static
   capture alone does not prove controls, preset recall or state restoration;
   exercise those through the actual face/host bridge and report each result.
   When live iteration is in scope, observe separate face and DSP edits reaching
   the active preview; report a last-working preview separately from a failed
   requested edit. A rebuilt artifact alone does not prove reload activation.
   Show the user the images. `npx --no-install maruaudio module verify-browser .` drives the face
   in a headless browser and writes a report; read what it actually checked.

6. **Build every declared format.** `npx --no-install maruaudio module verify .` (MCP:
   `maru_module_verify`) builds and qualifies each declared native format and
   writes per-format evidence. For one format, `npx --no-install maruaudio plugin build . --format <format>`;
   read its build report, because a bundle on disk does not prove its
   validator passed. `npx --no-install maruaudio plugin inspect <bundle>` loads one built bundle in
   a diagnostic probe. Without the native toolkit these builds cannot run
   locally; record every affected native format as not built. For hosted
   output, discover current capabilities through `build-preview-export` and
   report any refusal. Browser publication does not establish native build,
   signing, installation or delivery evidence.

7. **Report**, as below, then hand the listening step to the user.

## Rules

- Never approve sound by reading code. Measurements show the module renders,
  stays in level and responds to its controls; they do not show it sounds
  good. Listening is the user's step.
- Agent sessions stay silent. Use offline renders; start the workbench with
  `npm run dev -- --silent` (or `npx --no-install maruaudio dev --silent`).
- An unavailable, skipped or `--fast` check is not a pass. Say which check,
  why it did not run, and the command that would make it available, in
  `maru`'s own words.
- Inspect MCP `isError`, operation status and individual check rows; successful
  transport or an `ok` envelope alone is not a passing check. Preserve failed
  results before repair and distinguish environment/client refusals from module
  failures. Keep a requested live preview running and report its actual URL.
- Claim only the formats you built and checked. A browser preview does not
  prove a native format, and a native build does not prove a DAW or device.
- Do not weaken a check, raise a tolerance or edit a reference to make it
  pass. Report the failure and fix the module.
- Never present a placeholder or mock render as real output.

## Verify

This skill is the verification. Before you report, confirm that each row of
the report below has a result you saw in this session, a measured number
where one applies, or the reason it did not run.

## Report

End with a table, one row per check:

| Check              | Command                                                                            | Result                                                        | Evidence                                  |
| ------------------ | ---------------------------------------------------------------------------------- | ------------------------------------------------------------- | ----------------------------------------- |
| Toolchain          | `npx --no-install maruaudio module doctor`                                                               | ready / missing items                                         | what it listed                            |
| Release checks     | `npx --no-install maruaudio module check .`                                                              | pass / fail / unavailable rows                                | failing row names                         |
| Offline render     | the eval, preview or app-run command used                                          | numbers                                                       | loudness, peak, RMS or note checks        |
| Face               | `npx --no-install maruaudio module screenshot .`, plus the installed host's supported interaction checks | captured / refused; interaction passed / failed / unavailable | image paths and observed control behavior |
| Each native format | `npx --no-install maruaudio module verify .` or `npx --no-install maruaudio plugin build`                                      | built and qualified / failed / not built                      | report path                               |
| Listening          | the user                                                                           | not done by the agent                                         | what to listen for                        |

Then list the files you changed, if any, and the next action for each gap.
If sharing was requested, also verify the returned page and bind it to the
publisher's exact account, module/project, revision and Release identities.
Retain its receipt and resume state for updates to the same project; a local
package, dry-run or preview URL does not establish online publication. Report
missing publication evidence as a gap rather than inferring it from local checks.

## Maru feedback

If part of this workflow was harder than Maru should make it (a check you
could not run, evidence you had to assemble by hand, a report that was hard
to read), end your reply with the line `MARU FEEDBACK: <version>`, where
`<version>` is what `npx --no-install maruaudio --version` prints (with only the hosted connector,
write `connector` and the server version your MCP client shows). Follow it
with a short prompt that could be used to improve Maru: what you were doing,
what was awkward, and the command, tool or API you wished for. Offer to file
it at https://github.com/maru-audio/maru-skill/issues or send it through
https://maru.audio/contact; file nothing without the user's yes.
