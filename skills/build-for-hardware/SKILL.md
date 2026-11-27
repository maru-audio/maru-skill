---
name: build-for-hardware
description: Build or adapt a Maru module, community firmware or MIDI-controller app for an exact music device. Use for device targets, firmware source forks, physical control mappings, hardware mocks and device-specific testing or delivery.
---

# Build for music hardware

Carry the user's project through the authoring and verification paths that the
selected device actually supports. Reuse the stated project, device and source;
ask only for missing choices that affect what runs or where it is written.

## Choose the route

Distinguish three products before choosing a starter or tool:

| Product | Starting point | Execution boundary |
| --- | --- | --- |
| Existing native firmware | Its canonical source, pinned revision, licence and build instructions | Keep its native toolchain, exact-board package and recovery procedure |
| A portable Maru instrument, effect or MIDI Processor | The existing module or a supported module starter | Keep musical behavior in the module; select an exact target/profile and its adapters |
| A controller app | A MIDI/event module and a documented controller profile | State whether it runs on the computer or inside the device; MIDI support alone establishes no on-device runtime |

The device's Build page is the starting point for its sourced projects and
interfaces. A firmware example or a registered compiler target does not qualify
an image for a physical unit. Preserve upstream credit and source/asset rights.

## Discover the installed workflow

With a shell, inspect `npx maruaudio --version`, `npx maruaudio hardware --help`, `npx maruaudio guide list`
and `npx maruaudio tools list`. Read the installed guides whose triggers match. When
listed, `firmware-and-device-authoring` covers these routes; Processor work also
uses `processor-authoring`, `restricted-typescript` and `midi-and-event-modules`.
Inspect schemas with `npx maruaudio tools describe <operation>` instead of guessing flags.

With MCP, inspect the tools the selected server advertises and read the matching
guides through its guide operation. Copy exact advertised tool names. Local
hardware tools need a host that binds the selected folder and target tools;
the hosted account connector does not gain USB access or local file access.
A CLI lifecycle command is not automatically an MCP tool.

An older install may lack a guide, operation, starter or target producer. Report
the exact version and named missing capability. Continue supported work that
does not depend on it; do not invent a command, compiler or simulated success.

## Develop and rehearse

- **Native source:** build the unchanged pinned example first. Make the
  requested fork/edit with its provenance retained, then run that project's
  supported host tests or mock. Use Maru's existing firmware listing workflow
  to describe the product; a source-only listing is not a firmware build.
- **Processor:** use the existing board/profile inspection and project-creation
  plan before creating the hardware project. Read its constraints and generated
  instructions, then validate, check prerequisites and run the supported build
  and emulation/test stages. Confirm that the project can use the installed
  tools and documented compiler payload. Report missing prerequisites by name.
- **Controller app:** use the actual starter/profile capabilities the installed
  tools return. When a MIDI-only starter is absent, say so and follow the
  MIDI/event guide to adapt a supported starter's declaration and ports. Keep
  key, encoder, modifier, chord and mode behavior in one bounded module state
  machine; physical bindings and feedback adapters consume those actions.

Keep target-specific code below the module boundary. Do not change synthesis,
numeric semantics, sample rate or required controls to hide a target failure.
An alternative mapping or feature set needs an explicit authored choice.
Test input ordering, held modifiers, simultaneous presses, release order,
reset/disconnect, feedback limits and stuck-note recovery through the same
module in the mock and supported device adapter.

Retain exact source, target/profile, compiler, package and report identities.
Use the target owner's existing reports and job/history paths. Report progress,
cancellation, failures and unavailable prerequisites as the owner returns them.

## Verify and deliver

Keep source checks, host renders, emulator results and physical-device evidence
separate. For a native-versus-generated comparison, match algorithm, tables,
numeric policy, events, voices and control cadence before comparing cost.
Measure actual device memory, timing/deadlines, audio/MIDI and controls before
claiming on-device support. Agent sessions stay silent; listening is a separate
human result.

Review the exact target package, identity, backup and recovery requirements
before a physical write. Use the device's existing installer and its confirmation
gates. Authorization to build or publish a listing does not authorize flashing.

Module browser and native releases, firmware listing publication and device
installation have separate qualification and receipts. Use the existing
publication operation's dry-run where available, preserving its authentication,
version and rights checks. Publish or install only within the user's authorized
scope; no new confirmation is needed when that exact action is already approved.

Report the selected route/device, source and target identities, what was built,
which checks ran and where, remaining gaps and the concrete next step. A mocked
pass or a successful listing must never be reported as a physical install.

## Maru feedback

When a missing operation, diagnostic or supported path made this harder than it
should be, end with `MARU FEEDBACK: <version>` and a short description of the
goal, obstacle and tool the agent needed. Use `connector` plus its reported
version when only MCP is available. Offer the report for the user to share;
send or file nothing without their authorization.
