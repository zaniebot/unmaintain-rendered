---
number: 12254
title: "Native server does not respect `.gitignore`"
type: issue
state: open
author: dhruvmanila
labels:
  - bug
  - server
assignees: []
created_at: 2024-07-09T07:39:16Z
updated_at: 2025-04-03T13:48:10Z
url: https://github.com/astral-sh/ruff/issues/12254
synced_at: 2026-01-07T13:12:15-06:00
---

# Native server does not respect `.gitignore`

---

_Issue opened by @dhruvmanila on 2024-07-09 07:39_

Following up on https://github.com/astral-sh/ruff/issues/12242, the native server does not respect `.gitignore`, specifically it does not consider the `respect_gitignore` file resolver setting.

---

_Label `bug` added by @dhruvmanila on 2024-07-09 07:39_

---

_Label `server` added by @dhruvmanila on 2024-07-09 07:39_

---

_Added to milestone `Ruff Server: Stable` by @dhruvmanila on 2024-07-10 03:00_

---

_Comment by @dhruvmanila on 2024-07-17 11:52_

Currently, this is a bit difficult to figure out i.e., whether a single path is being gitignored or not. `ruff-lsp` didn't do that either because it uses stdin and `ruff` would always provide diagnostics if checked via stdin

https://github.com/astral-sh/ruff/blob/9163ad8cc4d411eac9d2cbfe840f3076e34a6c71/crates/ruff_workspace/src/resolver.rs#L649-L651

The `ignore` crate has a [`Gitignore`](https://docs.rs/ignore/latest/ignore/gitignore/struct.Gitignore.html) struct which could possibly be used but we'd still need to collect all the ignore files to consider in it.

---

_Comment by @MichaReiser on 2024-07-17 12:21_

I think I'm fine not supporting this for now. But yeah, we face a similar problem in red-knot. We may have to vendor the `ignore` crate longterm and check for every new file if it is excluded.

---

_Removed from milestone `Ruff Server: Stable` by @dhruvmanila on 2024-07-17 12:23_

---

_Added to milestone `Ruff Server: Post-Stable` by @dhruvmanila on 2024-07-17 12:23_

---

_Removed from milestone `Ruff Server: Post-Stable` by @dhruvmanila on 2025-04-03 13:48_

---
