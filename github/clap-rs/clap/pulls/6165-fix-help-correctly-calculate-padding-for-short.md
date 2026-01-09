---
number: 6165
title: "fix(help): Correctly calculate padding for short-only args "
type: pull_request
state: merged
author: epage
labels: []
assignees: []
merged: true
base: master
head: shirt
created_at: 2025-10-29T14:13:41Z
updated_at: 2025-10-29T14:19:57Z
url: https://github.com/clap-rs/clap/pull/6165
synced_at: 2026-01-07T13:12:20-06:00
---

# fix(help): Correctly calculate padding for short-only args 

---

_Pull request opened by @epage on 2025-10-29 14:13_

This manifests in two ways
- If a value is taken, we double the padding
- If `ArgAction::Count` is used, we don't account for `...` and crash

Fixes https://github.com/clap-rs/clap/issues/6164

---
