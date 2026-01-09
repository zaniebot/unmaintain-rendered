---
number: 3656
title: "feat(complete): Skeleton for Rust-driven completions"
type: pull_request
state: merged
author: epage
labels: []
assignees: []
merged: true
base: master
head: complete
created_at: 2022-04-22T21:52:37Z
updated_at: 2025-06-05T08:51:23Z
url: https://github.com/clap-rs/clap/pull/3656
synced_at: 2026-01-07T13:12:20-06:00
---

# feat(complete): Skeleton for Rust-driven completions

---

_Pull request opened by @epage on 2022-04-22 21:52_

This implements the rudiments of completions within Rust.

What this can do for Bash
- Nest into subcommands
- Provide single-value positional value completions
  - `PossibleValues`
  - Some of `ValueHint`
- Provide short and long flag completions
- Provide subcommand completions

What this does not do
- Flag values
- Multiple values per argument
- Removing options based on conflicts
- Handle all of the random App and Arg settings
- Correctly append spaces when needed
- Shells other than Bash

Part of #3166

---
