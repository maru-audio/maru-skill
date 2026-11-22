---
name: find-module
description: Find a published community module to start from, read how it works and fork it into the user's Maru account. Use when someone wants an existing Maru module, effect or instrument to learn from or remix.
---

Find a community module the user could start from, for what they described.

1. Call `maru_search_community_modules` with a short `query` for the idea and,
   when it is clear, a `kind` (instrument, effect, generator, midi or control).
   Show up to five results with their title, creator and one line each.
2. When the user picks one, call `maru_list_community_module_files` with its
   exact `owner` and `slug`, then `maru_read_community_module_source` for the
   file that holds the part they care about. Quote the lines you explain and
   credit the creator.
3. To fork it, first call `maru_library_resolve` with the module's reference
   and show its licence and whether this account may remix it. Ask before
   forking; then call `maru_remix` with the same reference and give the user
   the Studio link from the result.

Remixing creates a project in the user's account. Do not call `maru_remix`
until the user has said yes.
