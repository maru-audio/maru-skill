---
name: check-build
description: Check the state of the user's Maru plug-in builds, or why the latest one failed. Use when someone asks whether their VST3, AU or CLAP build is ready or why it failed.
---

Check the user's Maru plug-in builds, or the one project they named.

1. Call `maru_list_plugin_builds`. If a project was named, find it in the
   result; otherwise list each build project with its formats (VST3, AU,
   CLAP) and their state.
2. For a project whose latest build failed or is still running, call
   `maru_show_plugin_build` with its exact `project` id and report the error
   or progress it gives, in plain words. Give the dashboard link from the
   result for downloads; never write a download URL yourself.
3. If the user is working on the module in this folder, offer to run the
   local checks: `maru_module_doctor` confirms the toolchain is ready and
   `maru_module_check` runs the release checks on the module. Ask before
   starting a local build with `maru_module_build`, which takes minutes.
