---
name: new-synth
description: Start a new synth from a Maru template, in the user's Maru account or in a folder on their computer. Use when someone asks to start, create or sketch a new synth or instrument with Maru.
---

Start a new Maru synth for the idea the user described.

1. Ask where the user wants to work if it is not clear: in their Maru account
   (Studio, nothing installed) or in a folder on this computer. Without a
   terminal on the user's computer (for example in a chat app), work in their
   account.
2. In their account, call `maru_list_studio_templates` with
   `kind: "instrument"` and `present: true`, so the user sees the templates.
   Suggest the one that fits the idea in a sentence and let them choose; do
   not pick for them. Then call
   `maru_create_studio_project` with the chosen template's exact
   `templateKey`, a short title and the idea as the `brief`. Give the user the
   Studio link from the result.
3. In a folder, ask which formats the synth should ship in (browser, and any
   of standalone, VST3, CLAP, AU), say that you will create a module project
   there and wait for the user to agree. Then run
   `npx maruaudio init <folder> --kind processor-instrument --local` and follow
   the next steps it prints. The `create-module` skill has the full workflow.
   Before implementing the face, read `maru guide read face-from-reference
   ui-kit-faces`: choose a PluginUI assembly/theme, sketch the musical groups
   and map them to shared controls. Use the same design workflow in Studio
   through `maru_read_guide`; a synth without a reference image still needs a
   visual plan and a rendered review.
4. If the idea is close to an official instrument, mention it: `maru_list_modules`
   and `maru_read_module_spec` show what each official synth already does.

Never invent a template key, project or link: use only what the tools return.
