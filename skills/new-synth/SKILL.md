---
name: new-synth
description: Start a new synth from a Maru template, in the user's Maru account or in a folder on their computer. Use when someone asks to start, create or sketch a new synth or instrument with Maru.
---

Start a new Maru synth for the idea the user described.

Use `create-module` for the joint sound-and-face brief, early preview,
implementation and checks. Reuse the user's choices; for an open idea, offer
recommended sound/design directions and "choose for me" as that skill describes.

1. Ask where the user wants to work only if context does not establish it:
   in their Maru account
   (Studio, nothing installed) or in a folder on this computer. Without a
   terminal on the user's computer (for example in a chat app), work in their
   account.
2. In their account, call `maru_list_studio_templates` with
   `kind: "instrument"` and `present: true`, so the user sees the templates.
   Recommend the one that fits the brief in a sentence. Ask when materially
   different choices remain; when the user delegates, choose the best fit.
   Then call `maru_create_studio_project` with the selected template's exact
   `templateKey`, a short title and the idea as the `brief`. Give the user the
   Studio link from the result.
3. In a folder, reuse the stated formats. If distribution is undecided,
   continue with a browser preview and ask about native needs alongside the
   work. State the destination, then proceed within the authorized creation
   request. Use the installed CLI, or `npx --yes maruaudio@latest` for authorized
   setup, to run `maru init <folder> --kind processor-instrument --local --install`
   and follow its next steps. The `create-module` skill has the full workflow.
   Before implementing the face, read `maru guide read face-from-reference
   ui-kit-faces`: choose a PluginUI assembly/theme, sketch the musical groups
   and map them to shared controls. Use the same design workflow in Studio
   through `maru_read_guide`; a synth without a reference image still needs a
   visual plan and a rendered review.
4. If the idea is close to an official instrument, mention it: `maru_list_modules`
   and `maru_read_module_spec` show what each official synth already does.

Never invent a template key, project or link: use only what the tools return.
