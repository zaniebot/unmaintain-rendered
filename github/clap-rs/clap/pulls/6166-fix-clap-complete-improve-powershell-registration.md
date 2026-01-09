---
number: 6166
title: "fix(clap_complete): improve powershell registration script"
type: pull_request
state: merged
author: fabalchemy
labels: []
assignees: []
merged: true
base: master
head: fix-dynamic-powershell-completion
created_at: 2025-10-29T22:28:54Z
updated_at: 2025-10-29T22:35:38Z
url: https://github.com/clap-rs/clap/pull/6166
synced_at: 2026-01-07T13:12:20-06:00
---

# fix(clap_complete): improve powershell registration script

---

_Pull request opened by @fabalchemy on 2025-10-29 22:28_

<!--
Thanks for helping out!

Please link the appropriate issue from your PR.

If you don't have an issue, we'd recommend starting with one first so the PR can focus on the
implementation (unless its an obvious bug or documentation fix that will have
little conversation).
-->

This PR modifies the PowerShell registration script for dynamic completions to handle the situation where the cursor is not on an partial argument. Previously, `clap_complete` would not generate any completion when hitting TAB for the first argument, and would generate a duplicate of the previous argument when present.

```console
> dynamic [TAB]
# tries to complete with a path
dynamic ./

> dynamic --format [TAB]
# duplicates the previous argument
dynamic --format --format
```

The proposed solution is to trim the argument list at the cursor position (matching the behavior of the Fish implementation), and to add an empty argument if the word to complete is empty.

With these changes, both the use-cases shown above work as expected:

```console
> dynamic [TAB]
dynamic --

> dynamic --[TAB]
--input   --format  --help

> dynamic --format [TAB]
json  yaml  toml
```

Fixes https://github.com/clap-rs/clap/issues/6010

---

_@epage approved on 2025-10-29 22:33_

---
