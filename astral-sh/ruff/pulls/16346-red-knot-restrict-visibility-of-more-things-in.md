```yaml
number: 16346
title: "[red-knot] Restrict visibility of more things in `class.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/class-private
created_at: 2025-02-24T13:16:28Z
updated_at: 2025-02-24T14:30:57Z
url: https://github.com/astral-sh/ruff/pull/16346
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] Restrict visibility of more things in `class.rs`

---

_Pull request opened by @AlexWaygood on 2025-02-24 13:16_

## Summary

A followup to https://github.com/astral-sh/ruff/pull/16337. Restricting visibility has the advantage that we get unused-code warnings if a method, function or type becomes unused. And it also means that it's harder for external modules to accidentally use methods that are meant to be implementation details, and aren't meant to be for external use.

## Test Plan

<!-- How was it tested? -->


---

_Label `red-knot` added by @AlexWaygood on 2025-02-24 13:16_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-24 13:16_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-24 13:16_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-24 13:16_

---

_@MichaReiser approved on 2025-02-24 14:29_

---

_Merged by @AlexWaygood on 2025-02-24 14:30_

---

_Closed by @AlexWaygood on 2025-02-24 14:30_

---

_Branch deleted on 2025-02-24 14:30_

---
