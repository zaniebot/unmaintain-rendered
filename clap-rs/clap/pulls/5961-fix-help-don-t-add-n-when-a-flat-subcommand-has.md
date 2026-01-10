---
number: 5961
title: "fix(help): Don't add \\n when a flat subcommand has no args"
type: pull_request
state: merged
author: BrandonXLF
labels: []
assignees: []
merged: true
base: master
head: flat-subcommands-no-args
created_at: 2025-03-27T00:16:42Z
updated_at: 2025-03-27T01:40:20Z
url: https://github.com/clap-rs/clap/pull/5961
synced_at: 2026-01-10T01:28:25Z
---

# fix(help): Don't add \n when a flat subcommand has no args

---

_Pull request opened by @BrandonXLF on 2025-03-27 00:16_

Check to see if a subcommand has arguments and only print the preceding newline if it does.

Fixes #5960.

---
