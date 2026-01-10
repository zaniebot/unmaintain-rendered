```yaml
number: 20442
title: "[`playground`] Enable inline noqa for multiline strings in playground"
type: pull_request
state: merged
author: TaKO8Ki
labels:
  - playground
assignees: []
merged: true
base: main
head: playground-noqa-multiline
created_at: 2025-09-16T20:12:36Z
updated_at: 2025-09-17T07:30:10Z
url: https://github.com/astral-sh/ruff/pull/20442
synced_at: 2026-01-10T17:40:28Z
```

# [`playground`] Enable inline noqa for multiline strings in playground

---

_Pull request opened by @TaKO8Ki on 2025-09-16 20:12_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fixes #19015

Switch ruff_wasm to pass `Flags::from_settings`, matching CLI/server behavior. This enables NOQA flag.

https://github.com/astral-sh/ruff/blob/e363c4e48e8982f40b228366b8e0aec9d4525bad/crates/ruff_linter/src/directives.rs#L20

## Test Plan

<!-- How was it tested? -->

I've confirmed that it works in my local environemt. If it's necessary to add other test, please let me know.

<img width="1458" height="982" alt="スクリーンショット 2025-09-17 045233" src="https://github.com/user-attachments/assets/051bf16a-54df-4fdc-8b42-512dab5cc292" />

---

_Comment by @github-actions[bot] on 2025-09-16 20:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-09-17 07:29_

Nice! Thank you

---

_Label `playground` added by @MichaReiser on 2025-09-17 07:29_

---

_Merged by @MichaReiser on 2025-09-17 07:29_

---

_Closed by @MichaReiser on 2025-09-17 07:29_

---

_Branch deleted on 2025-09-17 07:30_

---
