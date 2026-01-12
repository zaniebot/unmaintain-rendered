```yaml
number: 16441
title: "[red-knot] Reject HTML comments in mdtest unless they are `snapshot-diagnostics` or are explicitly allowlisted"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/html-comment-allowlist
created_at: 2025-02-28T15:29:47Z
updated_at: 2025-02-28T16:27:30Z
url: https://github.com/astral-sh/ruff/pull/16441
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Reject HTML comments in mdtest unless they are `snapshot-diagnostics` or are explicitly allowlisted

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/16352. (An alternative, simpler approach to https://github.com/astral-sh/ruff/pull/16426.) All HTML comments are now rejected unless they are either `snapshot-diagnostics` or are explicitly included in an allowlist. This prevents `snpshot-diagnosticss` typos, and similar, which lead to tests silently never running at all.

## Test Plan

Added unit tests to `red_knot_test` and did manual testing.


---

_Label `testing` added by @AlexWaygood on 2025-02-28 15:29_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-28 15:29_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-28 15:29_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-28 15:29_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-28 15:29_

---

_Review comment by @carljm on `crates/red_knot_test/src/parser.rs`:1817 on 2025-02-28 16:25_

Fair warning, I'm going to ask you to pronounce this at our next standup.

---

_@carljm approved on 2025-02-28 16:26_

This looks good to me! Thank you

---

_@AlexWaygood reviewed on 2025-02-28 16:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_test/src/parser.rs`:1817 on 2025-02-28 16:27_

hahaha, I had to think up an _outrageous_ typo or the `typos` pre-commmit check kept on complaining that there was a typo in the test here

---

_Merged by @AlexWaygood on 2025-02-28 16:27_

---

_Closed by @AlexWaygood on 2025-02-28 16:27_

---

_Branch deleted on 2025-02-28 16:27_

---
