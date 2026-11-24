---
name: list-plugin
description: Put a plug-in on Maru - a Maru module, a plug-in built with JUCE, iPlug2, DPF, nih-plug or any other framework, or one released on GitHub - by choosing the right route with the user and confirming the name, price and licence before anything goes to review. Use when asked to list, publish, sell, release or share a plug-in or module on Maru or its marketplace.
---

# List a plug-in on Maru

Maru has four routes to a listing. Each is a different command set, and none
of them should be chosen for the user without their answer.

## Steps

1. **Sign in.** `maru whoami`. If not signed in, ask before `maru login`.
   The marketplace route needs
   `maru login --scope marketplace:read --scope marketplace:write`.

2. **Ask which route fits**, after looking at the folder and asking the user
   how people should get the plug-in:

   | The user has | Route | Commands |
   | --- | --- | --- |
   | A Maru module project | Publish the module | `maru publish --dry-run`, then `maru publish`; `maru module status` |
   | Built plug-in files from any framework, or their own download page | Marketplace listing | `maru marketplace whoami`, `project`, `draft`, `media`, `release`, `offer`, `submit`, `status` |
   | A public GitHub repository with releases | Repository listing | `maru listing init`, `check`, `publish`, `status` |
   | A plug-in project from another framework that Maru should build | Hosted build | `maru publish` in the project, then `maru build status <id> --wait-seconds 45`, `maru release check`, `maru release publish` |

   Read the guide for the route before running it: `build-preview-export` for
   a Maru module, `repository-listing` for a GitHub repository,
   `publish-external-plugin` for a hosted build. The marketplace route is the
   `list-on-marketplace` workflow in the Maru agent plugin; follow its
   steps in order.

3. **Who lists it.** For the marketplace, `maru marketplace whoami` shows the
   owners the user may list for. If there is more than one (their account and
   organizations), ask which. Never pick an organization. Note an existing
   product so you update it rather than create a second one.

4. **Write the public text with the user.** Draft the title, tagline,
   description and support address from what you read in the project. Show
   the user the exact text that will be public and confirm the name before
   saving it.

5. **Licence, rights and price come from the user.** Ask for the licence
   (its name or SPDX expression, and the text people agree to). Read the user
   the rights or redistribution declaration from the tool's description and
   pass it only if they confirm it. Ask what it costs; never guess or round a
   price.

6. **Dry run where the route offers one.** `maru publish --dry-run` for a
   module, `maru listing check` for a repository, `"dryRun": true` for
   `maru marketplace draft`.

7. **Confirm, then submit.** Show the name, the price, the licence and what
   will be public, and ask. Only then run the step that publishes or submits
   for review.

8. **Read the outcome back** (see **Verify**) and report.

Over MCP, the local `maru-local` server has every marketplace step as a tool,
such as `maru_whoami_marketplace` and `maru_draft_marketplace`. The hosted
connector covers repository listings (`maru_listing_publish`,
`maru_listing_status`), hosted builds (`maru_build_create`,
`maru_build_status`, `maru_build_release_check`, `maru_build_release_publish`)
and listing review (`maru_inspect_marketplace`, `maru_validate_marketplace`,
`maru_preview_marketplace`, `maru_submit_marketplace`). Read an operation's
exact input with `maru tools describe --json`.

## Rules

- Ask the user which route, which owner, the public text, the licence and
  the price. Do not decide any of them.
- A request to build or package does not authorize publishing or
  submitting. Publication needs the user's explicit yes, given in this
  conversation or earlier for this exact action.
- Never print or paste a token. In CI, `MARU_TOKEN` holds a token with only
  `listings:publish`, created by the user at https://maru.audio/account/tokens.
- Give only links the tools returned. Never write a download or page address
  yourself.
- If a step is refused, report its message and the command it names; do not
  work around it. A publisher agreement is accepted by the user on Maru, not
  by the agent.

## Verify

Confirm the listing from Maru's side, not from the submitting command's
reply:

- Marketplace: `maru marketplace status <productId>` shows what is live, what
  waits for Maru's scan and review, and anything refused with its reason.
- Repository: `maru listing status` shows the listings and their last read.
- Module: `maru module status` shows the live release, whether its source and
  a screenshot are attached, and any interrupted publish.
- Hosted build: `maru build status <id>` shows each bundle's inspection and
  health checks; a check that did not run is not a pass.

Health checks and reviews measure whether a plug-in loads and renders; they
do not judge its sound. Before listing a Maru module, run `verify-module` and
attach a screenshot from `maru module screenshot`.

## Report

End with: the route, the owner, the name, the price and the licence the user
confirmed; what is live; what waits for review; anything refused, with
Maru's reason and next action in its words; and the page address from the
status result.

## Maru feedback

If part of this workflow was harder than Maru should make it (a route that
was hard to choose, a step that needed information Maru could have read, a
refusal without a next step), end your reply with the line
`MARU FEEDBACK: <version>`, where `<version>` is what `maru --version` prints
(with only the hosted connector, write `connector` and the server version
your MCP client shows). Follow it with a short prompt that could be used to
improve Maru: what you were doing, what was awkward, and the command, tool or
API you wished for. Offer to file it at
https://github.com/maru-audio/maru-skill/issues or send it through
https://maru.audio/contact; file nothing without the user's yes.
