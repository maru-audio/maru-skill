# Maru

![Maru Audio: instruments, effects and musical tools](./MARU_audio.webp)

**Turn musical ideas into instruments, effects and sounds you can use and share.**

[Maru](https://maru.audio) brings together tools for making audio products,
working with musical ideas and creating sound collections. Build an instrument
around your own way of playing, shape an effect for a particular sound, or turn
a collection of recordings into a playable kit.

You can direct the work through your agent, edit the project yourself, or
combine both. Your work stays in an editable project that you can develop
further and prepare for supported browser and plugin formats.

## Bring Maru to your agent

The Maru skill connects the agent you already use to this ecosystem. It helps
the agent choose the right tools, work with your project and check the result.
Works with Claude Code, Codex, Cursor and other coding agents.

```sh
npx maruaudio skill install
```

Then describe what you want to make:

- “Make a tape effect that gets darker as the repeats fade.”
- “Create a chord tool with voicings I can play from my keyboard.”
- “Turn these percussion recordings into a drum kit.”
- “Help me finish this instrument and prepare a plugin release.”
- “List my plugin on the Maru marketplace.”

### As a plugin

This repository is also a plugin for Claude Code and Codex. One install adds
the skills and Maru's MCP servers.

**Claude Code**

```sh
claude plugin marketplace add maru-audio/maru-skill
claude plugin install maru@maru-audio
```

Then run `/mcp`, choose **maru** and **Authenticate**.

**Codex**

```sh
codex plugin marketplace add maru-audio/maru-skill
codex plugin add maru@maru-audio
```

Then run `codex mcp login maru`. In the ChatGPT desktop app you can also
install it from **Plugins** once the marketplace is added.

| Part | What it does |
| --- | --- |
| `maru-agent` skill | The same skill as above |
| `new-synth` skill | Starts a synth from a template, in your account or in a folder |
| `find-module` skill | Finds a community module, explains its source and forks it with your OK |
| `check-build` skill | Reports your plug-in builds and why the latest one failed |
| `list-on-marketplace` skill | Lists your plug-in on the Maru marketplace, built with any framework: listing, media, offer and release, confirmed with you before it goes to review |
| `maru` server | The hosted connector at `https://maru.audio/mcp`: your Maru account, after you sign in to Maru and approve the connection in your browser |
| `maru-local` server | Runs the published `maruaudio` package from npm on your computer (`npx maruaudio mcp`, pinned to a published release) for the module project in your folder |

Ask in your own words, or pick a skill yourself: type `/maru:new-synth` in
Claude Code, or type `@` in Codex and choose Maru.

The hosted server talks only to maru.audio, as the account you approve. The
local server runs Maru's command-line operations on your folder; the ones
that need your account, such as audio jobs, reach maru.audio after
`maru login`. See the [privacy policy](https://maru.audio/privacy). If you already added Maru
with `npx maruaudio mcp install`, remove those entries so the tools are not
listed twice.

In Claude, ChatGPT and other chat apps, add the hosted connector instead:
[connect your agent](https://maru.audio/docs/mcp-connect).

## One connected ecosystem

**Studio** is Maru's project workspace. The **Engine** provides the foundation
for instruments, effects and musical tools. **Symbolic** works with notes,
chords, scales and rhythms. Maru's sound tools support generation, samples and
sampler banks.

The skill brings these capabilities into your agent's workflow. You bring the
musical direction; Maru provides tools to develop it into something playable.

[Explore Maru](https://maru.audio) ·
[Studio](https://maru.audio/studio-builder) ·
[Engine](https://maru.audio/engine) ·
[Symbolic](https://maru.audio/symbolic)

## Getting started and documentation

Read [Maru skills](https://maru.audio/docs/skills) for setup, how to use the
skill, example workflows and connecting your Maru account. The
[documentation](https://maru.audio/docs) covers the platform and its tools.

[Contact](https://maru.audio/contact) ·
[Report an issue](https://github.com/maru-audio/maru-skill/issues) ·
[MIT license](./LICENSE)
