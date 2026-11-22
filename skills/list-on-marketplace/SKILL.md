---
name: list-on-marketplace
description: List a plug-in on the Maru marketplace, built with any framework (JUCE, iPlug2 or other), from a folder on the user's computer. Use when someone wants to sell, share or publish an existing plug-in on Maru.
---

List the user's plug-in on the Maru marketplace, from the folder they named or this one.

These steps run the `maru` command line on the user's computer. If you have no terminal there (for example in a chat app), say so and suggest doing this from a coding agent such as Claude Code or Codex.

Work through these steps in order. Run each as `maru marketplace <step>` with
`--json` (or the same tool on the local `maru-local` server, such as
`maru_whoami_marketplace`). Every result has `next`: the step to take and a
sentence to tell the user. Read `maru marketplace <step> --help` or
`maru tools describe --json` for the exact input of each.

1. **Sign in.** `maru whoami`. If not signed in, ask before running
   `maru login --scope marketplace:read --scope marketplace:write`.
2. **Who lists it.** `maru marketplace whoami`. If the user can list for more
   than one owner (their account and organizations), ask which one. Never
   pick an organization yourself. Note any product they already have, so you
   update it instead of creating a second one.
3. **Read the project.** `maru marketplace project <folder>`: framework, name,
   version, formats, built files and media. Nothing is uploaded. If no built
   file is there, offer to build and package it (zip a `.vst3`, `.component`
   or `.clap` bundle), or use a download page instead.
4. **Write the listing with the user.** Draft the title, tagline, description,
   support address and plug-in facts from what you read, then **show the user
   the text that will be public and confirm the name**. Create it with
   `maru marketplace draft` (`slug`, `organizationId` when an organization
   owns it, `listing`); run it with `"dryRun": true` first if the user wants
   to see what it would do. Keep the returned `productId`.
5. **Media.** Offer screenshots (you may capture the plug-in's window if the
   user wants), audio demos with a dry take, video files or a YouTube or Vimeo
   address. Before uploading files, **read the user Maru's rights
   declaration** from the tool's description and pass it as `rights` only if
   they confirm it. Add them with `maru marketplace media`.
6. **Release.** Ask how people get it, then use one of three ways:
   - Upload the built files: `maru marketplace release` with
     `"action": "upload"`. Ask the user for the licence (its name or SPDX
     expression and the text people agree to) and read them the
     redistribution declaration; pass both only as they state them. For a
     zipped bundle, include its exact plug-in identities. Running it again
     resumes an interrupted upload.
   - Their own download page: `"action": "vendor-url"`. Maru links to it and
     never hosts or scans those files.
   - A public GitHub repository with releases: `maru listing init`, `check`
     and `publish` (`maru guide read repository-listing`).
7. **Offer.** Ask the user what it costs. Free, free with an optional tip, or
   **Buy at their own shop** with the price and currency they state and their
   checkout address. **Never guess or round a price.** Set it with
   `maru marketplace offer`, with `install` too if they want to limit who can
   install it through Maru.
8. **Confirm, then submit.** Show the user the name, the price, the licence
   and what will be public, and ask before submitting. Then
   `maru marketplace submit` with `{"stage": "release", "input": {"productId",
   "releaseId"}}`. Nothing goes to review without this step.
9. **Report.** `maru marketplace status <productId>`: say what is live,
   what waits for Maru's scan and review, and anything refused with Maru's
   reason, then the next action in its words and the page address
   (`https://maru.audio` followed by `pagePath`). If it says the publisher
   agreement needs accepting, the user does that on the product's page on
   Maru; you cannot accept it for them.

Never print or paste a token. If a step is refused, report its message and
the command it names rather than working around it.
