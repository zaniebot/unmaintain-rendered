```yaml
number: 19615
title: "[ty] Discard `Definition`s when normalizing `Signature`s"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/discard-definition
created_at: 2025-07-29T10:34:30Z
updated_at: 2025-07-29T13:37:49Z
url: https://github.com/astral-sh/ruff/pull/19615
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Discard `Definition`s when normalizing `Signature`s

---

_Pull request opened by @AlexWaygood on 2025-07-29 10:34_

## Summary

I noticed while looking at some debug prints that we appear to be holding onto the original `Definition` associated with a `Signature` when we normalize the `Signature`. This defeats the point of normalization of types: it means that two equivalent `Signature`s (and therefore two equivalent `CallableType`s) might not share the same Salsa ID even after being normalized if they originated from different `StmtFunctionDef`s in the source code.

I'm not sure whether or not this would lead to a user-facing bug right now, but it feels like a latent bug waiting to pounce, so this PR fixes it. Fixing the latent bug also has the advantage that it makes the debug representation of normalized `CallableType`s more concise, and probably reduces memory a little bit (because we need to intern fewer `Signature`s when normalizing `CallableType`s). I think there are no downsides to this change: normalized types should usually only be created transitively, so this shouldn't have any impact on goto-definition or go-to type definition.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Label `ty` added by @AlexWaygood on 2025-07-29 10:34_

---

_Comment by @github-actions[bot] on 2025-07-29 10:36_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-29 10:38_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-07-29 10:38_

---

_Review requested from @carljm by @AlexWaygood on 2025-07-29 10:39_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-29 10:39_

---

_Review requested from @dcreager by @AlexWaygood on 2025-07-29 10:39_

---

_Label `internal` added by @AlexWaygood on 2025-07-29 11:10_

---

_@dcreager approved on 2025-07-29 13:37_

Makes sense to me!

---

_Merged by @AlexWaygood on 2025-07-29 13:37_

---

_Closed by @AlexWaygood on 2025-07-29 13:37_

---

_Branch deleted on 2025-07-29 13:37_

---
