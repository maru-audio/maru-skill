# Maru

![Maru Audio — instruments, effects and musical tools](./MARU_audio.webp)

Make instruments, effects and musical tools with your coding agent.
Works with Claude Code, Codex, Cursor and other agents.

## Install

```sh
npx maruaudio skill install
```

Then describe what you want to make. Your agent handles setup and finds the
right Maru tools for the job.

## Try it

Ask your agent, for example:

- "Make a tape-saturation effect with drive and tone controls."
- "Build a 16-pad drum kit from this loop."
- "Turn this melody recording into MIDI."
- "Build a chord progression tool with scales and rhythm patterns."
- "Add a preset bank to my synth."

## What you can make

| Area                    | Examples                                                     |
| ----------------------- | ------------------------------------------------------------ |
| Instruments and effects | Synths, audio effects, controls and presets                  |
| Music and MIDI          | Chords, scales, rhythms, sequencers and note transformations |
| Sounds and samples      | Generated sounds, drum kits, sampler banks and audio-to-MIDI |

One skill covers these workflows and reads the relevant guides as needed.
Music and MIDI code uses [Symbolic](https://maruaudio.com/symbolic), included
in Maru.

## Use the CLI directly

```sh
npx maruaudio init my-effect --kind processor-effect --install
cd my-effect
npm run dev
```

Edit your project and hear the changes in the live workbench. The generated
project includes instructions and scripts for checking and building it:

```sh
npm test
npm run build:vst3
```

Ask your agent to build and verify the format you need. Run
`npx maruaudio doctor` if something needs setup.

## Update

```sh
npx maruaudio skill update
```

For a user-wide install, add `--global`. To choose an agent, add
`--agent claude-code` or `--agent codex` to the install command.

You can also install directly with `npx skills add maru-audio/maru-skill`.
The installer currently identifies the skill as `maru-agent`.

## Links

- [Documentation](https://maruaudio.com/docs)
- [Maru CLI and SDK](https://www.npmjs.com/package/maruaudio)
- [Maru on GitHub](https://github.com/maru-audio/MARU)
- [Issues](https://github.com/maru-audio/maru-skill/issues)
- [Contact](https://maruaudio.com/contact)

Maru is in alpha. Some workflows need sign-in or additional build tools.

## License

[MIT](./LICENSE).
