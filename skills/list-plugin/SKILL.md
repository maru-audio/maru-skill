---
name: list-plugin
description: Put a plug-in on Maru - a Maru module, a plug-in built with JUCE, iPlug2, DPF, nih-plug or any other framework, or one released on GitHub - using the project's delivery route and the user's publication scope. Use when asked to list, publish, sell, release or share a plug-in or module on Maru or its marketplace.
---

# List a plug-in on Maru

Maru has four routes to a listing. Use the project and the user's intended
delivery to choose the command set. Reuse decisions and authorization already
given in the brief or conversation; ask only about consequential gaps.

## Steps

1. **Verify the intended account.** Recover the explicit CLI profile and issuer
   from setup or the local MCP's unverified context when supplied. Use that
   profile for every account command below; a named profile does not change
   plain CLI defaults. Run `npx maruaudio whoami --json` with that explicit profile and
   compare account, issuer, expiry and required grants with the brief or saved
   checkpoint. Reuse suitable
   authorization; use supported sign-in only when needed and authorized.
   The marketplace route needs
   `npx maruaudio login --scope marketplace:read --scope marketplace:write`.

2. **Choose the route** from the folder and established delivery intent. Ask
   how people should get the plug-in only when that choice is unresolved:

   | The user has | Route | Commands |
   | --- | --- | --- |
   | A Maru module project | Publish the module | `npx --no-install maruaudio publish --dry-run`, then `npx --no-install maruaudio publish`; `npx maruaudio module status` |
   | Built plug-in files from any framework, or their own download page | Marketplace listing | `npx maruaudio marketplace whoami`, `project`, `draft`, `media`, `release`, `offer`, `submit`, `status` |
   | A public GitHub repository with releases | Repository listing | `npx maruaudio listing init`, `check`, `publish`, `status` |
   | A plug-in project from another framework that Maru should build | Hosted build | `npx --no-install maruaudio publish` in the project, then `npx maruaudio build status <id> --wait-seconds 45`, `npx maruaudio release check`, `npx maruaudio release publish` |

   Read the guide for the route before running it: `build-preview-export` for
   a Maru module, `repository-listing` for a GitHub repository,
   `publish-external-plugin` for a hosted build. The marketplace route is the
   `list-on-marketplace` workflow in the Maru agent plugin; follow its
   steps in order.

3. **Who lists it.** For the marketplace, `npx maruaudio marketplace whoami` shows the
   owners the user may list for. If there is more than one (their account and
   organizations), ask which. Never pick an organization. Note an existing
   product so you update it rather than create a second one.

4. **Write the public text with the user.** Draft the title, tagline,
   description and support address from the approved brief and project. Reuse
   the established name and copy. Review changed public text with the user
   when it falls outside their existing instruction.

5. **Apply the route's terms and defaults.** Preserve existing listing settings
   and the user's decisions.

   - **Authored Maru module:** an authorized first browser publication defaults
     to a public, free page with a private source archive. State those defaults
     in the publication plan; missing overrides do not require a price,
     audience or source-visibility interview. A missing licence stays unstated;
     do not invent one. Updates preserve the owner's saved settings. Ask when
     a requested paid offer, public source or third-party redistribution needs
     a licence, rights decision or other choice not already established.
   - **Marketplace, repository or hosted-build route:** follow that route's
     required licence, price and agreement inputs. Reuse explicit decisions;
     ask for consequential missing terms. Never guess or round a price.

   Read any required rights or redistribution declaration from the tool's
   description and pass it only if the user confirmed that declaration. A
   module's ordinary defaults do not accept agreements or grant rights.

6. **Dry run where the route offers one.** `npx --no-install maruaudio publish --dry-run` for a
   module, `npx maruaudio listing check` for a repository, `"dryRun": true` for
   `npx maruaudio marketplace draft`.

7. **Publish within the approved scope.** Show the name, applied price, actual
   licence (or that none is stated), page audience and source visibility. Use
   existing explicit authorization for this exact
   publication; ask only when it is missing or the scope changed. Then run
   the step that publishes or submits for review.

8. **Read the outcome back** (see **Verify**) and report.

Over MCP, the local `maru-local` server has every marketplace step as a tool,
such as `maru_whoami_marketplace` and `maru_draft_marketplace`. The hosted
connector covers repository listings (`maru_listing_publish`,
`maru_listing_status`), hosted builds (`maru_build_create`,
`maru_build_status`, `maru_build_release_check`, `maru_build_release_publish`)
and listing review (`maru_inspect_marketplace`, `maru_validate_marketplace`,
`maru_preview_marketplace`, `maru_submit_marketplace`). Read an operation's
exact input with `npx maruaudio tools describe --json`.

## Rules

- Preserve the user's chosen owner, public text, licence, price, audience and
  source visibility. Use the module defaults above only when no override is
  established; ask about consequential gaps, not defaults the route supplies.
  Never silently widen the user's scope.
- A request to build or package does not authorize publishing or
  submitting. Publication needs the user's explicit yes, given in this
  conversation or earlier for this exact action.
- Never print or paste a token. In CI, `MARU_TOKEN` holds a token with only
  `listings:publish`, created by the user at https://maru.audio/account/tokens.
- Give only links the tools returned. Never write a download or page address
  yourself.
- Retain the intended account and publication's project, revision and Release
  identities in the development checkpoint. Before a retry or update, verify
  the explicit profile with `whoami` again and continue the same project;
  bootstrap context and a prior success are not fresh account verification.
- If a step is refused, report its message and the command it names; do not
  work around it. A publisher agreement is accepted by the user on Maru, not
  by the agent.

## Verify

Confirm the listing from Maru's side, not from the submitting command's
reply:

- Marketplace: `npx maruaudio marketplace status <productId>` shows what is live, what
  waits for Maru's scan and review, and anything refused with its reason.
- Repository: `npx maruaudio listing status` shows the listings and their last read.
- Module: `npx maruaudio module status` shows the live release, whether its source and
  a screenshot are attached, and any interrupted publish. For an unlisted
  module, retain the returned full page URL and use it unchanged for resolve
  and install. A shorthand `@handle/slug[@version]` refusal does not establish
  that the publication is missing. Check the installed `npx maruaudio resolve --help`
  and `npx maruaudio install --help` for usage.
- Hosted build: `npx maruaudio build status <id>` shows each bundle's inspection and
  health checks; a check that did not run is not a pass.

Health checks and reviews measure whether a plug-in loads and renders; they
do not judge its sound. Before listing a Maru module, run `verify-module` and
attach a screenshot from `npx maruaudio module screenshot`.

## Report

End with: the route, the owner, the name, the applied price and actual licence
(or that none is stated); what is live; what waits for review; anything refused,
with Maru's reason and next action in its words; and the page address from the
status result.

## Maru feedback

If part of this workflow was harder than Maru should make it (a route that
was hard to choose, a step that needed information Maru could have read, a
refusal without a next step), end your reply with the line
`MARU FEEDBACK: <version>`, where `<version>` is what `npx maruaudio --version` prints
(with only the hosted connector, write `connector` and the server version
your MCP client shows). Follow it with a short prompt that could be used to
improve Maru: what you were doing, what was awkward, and the command, tool or
API you wished for. Offer to file it at
https://github.com/maru-audio/maru-skill/issues or send it through
https://maru.audio/contact; file nothing without the user's yes.
