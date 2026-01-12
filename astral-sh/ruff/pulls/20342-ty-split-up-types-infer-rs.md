```yaml
number: 20342
title: "[ty] split up `types/infer.rs`"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/infer-split
created_at: 2025-09-10T23:08:24Z
updated_at: 2025-09-11T02:25:09Z
url: https://github.com/astral-sh/ruff/pull/20342
synced_at: 2026-01-12T15:56:59Z
```

# [ty] split up `types/infer.rs`

---

_@carljm_

## Summary

This module had grown so large that GitHub sometimes wouldn't syntax highlight it in reviews. Split it up, by separating `TypeInferenceBuilder` from the public inference queries, and splitting out type and annotation expressions from the main builder.

Apologies in advance about the merge conflicts -- there won't be any good time to do this, but it needs to happen sometime.

After this PR, `types/infer/builder.rs` is still ~9k LOC, down from over 12k. That makes `types.rs` our largest module again, at ~11k. It is also overdue for having more things split out of it.

## Test Plan

Pure refactor, existing tests.


---

_Label `internal` added by @carljm on 2025-09-10 23:08_

---

_Label `ty` added by @carljm on 2025-09-10 23:08_

---

_Comment by @github-actions[bot] on 2025-09-10 23:10_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-10 23:13_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-09-10 23:40_

---

_Review requested from @AlexWaygood by @carljm on 2025-09-10 23:40_

---

_Review requested from @sharkdp by @carljm on 2025-09-10 23:40_

---

_Review requested from @dcreager by @carljm on 2025-09-10 23:40_

---

_@dcreager approved on 2025-09-11 02:02_

Sorely needed!

I used `diff.colorMoved=zebra` and `diff.colorMovedWS=ignore-all-space` git configs to verify that the bulk of the diff is a move.

---

_Merged by @carljm on 2025-09-11 02:25_

---

_Closed by @carljm on 2025-09-11 02:25_

---

_Branch deleted on 2025-09-11 02:25_

---
