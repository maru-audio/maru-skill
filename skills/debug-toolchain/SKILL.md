---
name: debug-toolchain
description: Find out why a Maru command, MCP tool, sign-in, build or workbench is not working - the CLI, sign-in, the hosted and local MCP servers, the native toolkit, the Processor compiler, browser checks, or a module that is silent - and fix the cause instead of working around it. Use when a npx --no-install maruaudio command fails or is missing, Maru tools do not appear, a build or check reports something unavailable, or a module makes no sound.
---

# Debug the Maru toolchain

Most failures are a missing piece of setup, and Maru names the fix itself.
Read what it prints, run the command it names (with the user's OK where the
rules below ask for it), and run the failing step again.

## Steps

1. **Version and machine.** `npx --no-install maruaudio --version`, then `npx --no-install maruaudio doctor`. Doctor
   lists Node, sign-in, installed components, the Processor compiler, build
   tools and the agent skill, each row with `fix:` when something is missing.
   `npx --no-install maruaudio doctor --paths` shows where Maru reads and writes. If `maru` itself
   is missing, use `npx --yes maruaudio@latest` within authorized setup; a global install is optional.
   Inside a module project, use the project's own CLI (`npx --no-install maruaudio …`, or its
   `package.json` scripts) so the version matches its dependencies.

2. **Read the failure exactly.** Rerun the failing command with `--json`
   (errors arrive as JSON on stderr) or `--verbose`. The exit code says what
   kind of failure it is:

   | Code | Meaning                                            | What to do                                                            |
   | ---- | -------------------------------------------------- | --------------------------------------------------------------------- |
   | 1    | The operation ran and failed                       | Read its report; fix the module or input                              |
   | 2    | Usage or validation error; nothing ran             | Check `npx --no-install maruaudio <group> <command> --help` or `npx --no-install maruaudio tools describe --json` |
   | 3    | Not signed in, or credentials rejected             | Use authorized sign-in or ask, then `npx --no-install maruaudio login`                      |
   | 4    | Missing prerequisite                               | Run the `fix:` command it printed                                     |
   | 5    | Refused by the item's licence                      | Report the licence; do not copy around it                             |
   | 6    | A paid item this account does not own              | Tell the user                                                         |
   | 7    | The author does not share this module's DSP source | Offer to play or install it from the page `maru` printed instead      |

   Over MCP, inspect `isError`, the operation's status and every relevant check
   row as well as transport success. An error returned as tool content is still
   a failed operation; an `ok` envelope does not erase failed or unavailable rows.
   Preserve the original result before repair and distinguish a client, network
   or sandbox refusal from a Maru operation that ran and failed.

3. **Sign-in.** `npx --no-install maruaudio whoami` with the intended profile and API origin. Reuse
   suitable existing sign-in; check the intended operation's grants as well as
   identity. Hosted connector access does not prove the CLI can publish, and
   successful identity lookup does not prove publication authority. Use existing
   sign-in authorization or ask before `npx --no-install maruaudio login`; without a browser,
   `npx --no-install maruaudio login --print-url` prints a link for the user to open. Local JS
   development does not require sign-in. Use supported credential interfaces;
   do not extract connector credentials or assume automatic CLI token import.

4. **MCP tools missing or listed twice.** `npx --no-install maruaudio mcp status` shows which agents
   have Maru's servers. The hosted `maru` server (https://maru.audio/mcp) acts
   on the user's account after they sign in and approve it in the browser; the
   local `maru-local` server runs `npx --no-install maruaudio mcp` on the open folder and needs Node
   and `npx`. If the agent has both the Maru agent plugin and entries from
   `npx --no-install maruaudio mcp install`, the tools appear twice: offer `npx --no-install maruaudio mcp remove`. Change
   the agent's configuration only within existing setup authorization or after asking.
   Over MCP, a tool name is the operation name with `maru_` in front, such as
   `maru_module_doctor`.
   After any required client reload, verify actual skill discovery and a local
   guide or project operation from the intended folder. Inspect the selected
   root when a project is missing; do not recreate it in the wrong root.
   Configuration and standalone transport checks alone do not prove attachment.

5. **Module toolchain.** In the project, `npx --no-install maruaudio module doctor` reports whether
   the engine, worklet runtime and required tools are complete enough to build
   and export. `npx --no-install maruaudio module paths .` and `npx --no-install maruaudio module outputs .` show the
   project's settings, completed outputs and missing release steps. For local
   JS development, the npm SDK supplies the workbench/player and CLI assets:
   use the generated `dev` script instead of building a replacement host or
   obtaining a Maru source checkout. A missing native toolkit blocks native
   commands, not this preview. Keep a requested preview process alive, verify
   its ready result and share its URL before continuing other checks.

6. **Native builds.** A command marked `Requires: toolchain:native` in its
   help needs the native toolkit; local toolkit access is for approved developers.
   For hosted outputs, read `build-preview-export` and discover current
   capabilities and their refusal reasons. Publishing a browser release does
   not prove that native builds, signing or delivery are available.
   `npx --no-install maruaudio setup --list` shows available and installed components;
   follow the installed command's supported setup route when authorized. For one built
   bundle, `npx --no-install maruaudio plugin inspect <bundle>` reports what loading it found.

7. **Browser checks and screenshots.** `npx --no-install maruaudio module screenshot` and the
   browser gates of `npx --no-install maruaudio module check` need Chromium. When they report a
   missing browser, pass `--chrome <path>` or set `CHROME_PATH`.

8. **Silent module.** Before blaming the DSP, check in the workbench
   (`npm run dev -- --silent`) the selected audio or MIDI input, the
   transport, the monitor level, the host power control and its diagnostics.
   Then render offline (`npx --no-install maruaudio module preview --check`, or
   `npx --no-install maruaudio module eval --suite levels` for an effect) to see whether the module
   itself is silent.

9. **Rerun the step that failed** and report what changed.

## Rules

- Fix the cause Maru names. Do not work around a missing component with a
  different runtime, a hand-written substitute or a mocked result.
- Reuse existing authorization for setup and reversible local fixes. Ask for
  additional account/configuration changes, charged work, publication or
  destructive actions; judge the operation rather than the presence of `--yes`.
- Never read or expose secret files or ask the user to paste a token. Use
  Maru's sign-in and credential interfaces.
- Quote `maru`'s own fix command exactly; do not paraphrase it into a
  different command. Check it against the installed command's help before
  retrying. If the printed fix is unsupported, retain that contradiction and
  use a supported repair rather than repeating it or inventing a flag.
- Read `npx --no-install maruaudio guide read cli-and-operations` before explaining why a command
  is unavailable.

## Verify

The fix worked when the step that failed now succeeds: the original command
exits 0 with the expected outcome, or the MCP operation's content confirms
success without `isError`, or `npx --no-install maruaudio doctor` shows the relevant row as `ok`.
For a workbench, confirm the live server and URL. Run the failing step again
and quote its result. If it still fails, report the new
message, not the old one.

## Report

End with: the failing command and its original message, the cause, what you
ran to fix it (and what the user approved), the installed version and selected
project/profile where relevant, the result of the rerun, and any
remaining blocker with the command `maru` names for it.

## Maru feedback

If part of this workflow was harder than Maru should make it (an error that
did not name its fix, a missing diagnostic, a setup step you had to guess),
end your reply with the line `MARU FEEDBACK: <version>`, where `<version>` is
what `npx --no-install maruaudio --version` prints (with only the hosted connector, write
`connector` and the server version your MCP client shows). Follow it with a
short prompt that could be used to improve Maru: what you were doing, what was
awkward, and the command, tool or API you wished for. Offer to file it at
https://github.com/maru-audio/maru-skill/issues or send it through
https://maru.audio/contact; file nothing without the user's yes.
