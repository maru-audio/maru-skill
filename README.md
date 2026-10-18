# Maru agent skill

![Maru Audio — instruments, effects and musical tools](./MARU_audio.webp)

A skill that teaches coding agents (Claude Code, Codex, Cursor and others) to
build audio plugins, instruments, effects, MIDI tools, sample kits and banks,
and to generate audio or transcribe it to MIDI, using the
[Maru](https://maruaudio.com) CLI.

The skill is deliberately small. It tells the agent to ask the installed `maru`
CLI for commands, schemas and guides, so the agent always sees the features of
the version you have installed.

## Install

```sh
npx maruaudio skill install
```

Or directly with the Skills CLI:

```sh
npx skills add maru-audio/maru-skill --skill maru-agent
```

Add `--global` to install for your user instead of the current project, or
`--agent claude-code` to choose an agent. No sign-in is needed.

## Create your first project

Install the CLI globally, or use `npx maruaudio` in place of `maru`:

```sh
npm i -g maruaudio
maru doctor
maru init my-effect --kind processor-effect
cd my-effect
npm install
npm run dev
```

Choose `processor-instrument` for a MIDI instrument. `maru init --help`
lists the available templates. Each project includes source, a face, agent
instructions and scripts for development and builds.

Ask your agent to refine the project, then check and build the target you need:

```sh
npm run typecheck
npm run module:check
npm run check:portability
npm run build:vst3
npm run verify:formats
```

The live workbench lets you audition changes. Native builds need the platform
toolchain and validators reported by the CLI; check the build report before
calling a format verified. Maru is in alpha, and target availability depends
on the module and installed tools.

## Discover the CLI

```sh
maru --help
maru guide list
maru guide read cli-and-operations build-preview-export
maru tools list
maru tools describe validate_module
```

The installed CLI supplies operation schemas and task-specific guides for
DSP, faces, MIDI, presets, sample kits, banks, generation and transcription.
`maru mcp` exposes the same operations to agents that support MCP.
The [skill instructions](./skills/maru-agent/SKILL.md) explain how agents
use this discovery workflow.

## Manage

```sh
npx maruaudio skill status   # installed? where, and for which agents
npx maruaudio skill update   # fetch the latest version
npx maruaudio skill remove   # uninstall (asks first; --yes to skip)
```

## Requirements

- Node.js 22.12 or later and the Maru CLI: `npm i -g maruaudio`, or let the
  agent use `npx maruaudio`.
- Some features need more, such as `maru login` for hosted generation. Run
  `maru doctor` to see what this machine can do and the command that fixes
  each missing prerequisite.

## Try it

Ask your agent, for example:

- "Make a tape-saturation effect with drive and tone controls."
- "Build a 16-pad drum kit from this loop."
- "Turn this melody recording into MIDI."
- "Add a preset bank to my synth."

## Links

- [Documentation](https://maruaudio.com/docs)
- [Maru CLI and SDK](https://www.npmjs.com/package/maruaudio)
- [Maru on GitHub](https://github.com/maru-audio/MARU)
- [Issues](https://github.com/maru-audio/maru-skill/issues)
- [Contact](https://maruaudio.com/contact)

## License

[MIT](./LICENSE).
