```yaml
number: 19880
title: "[ty] Remove unsafe `salsa::Update` implementations in `tuple.rs`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/tuple-derives
created_at: 2025-08-12T14:49:09Z
updated_at: 2025-08-12T14:53:36Z
url: https://github.com/astral-sh/ruff/pull/19880
synced_at: 2026-01-12T15:56:49Z
```

# [ty] Remove unsafe `salsa::Update` implementations in `tuple.rs`

---

_@AlexWaygood_

## Summary

No tests seem to fail if I remove these, and they're pretty complicated :-)

## Test Plan

`cargo test -p ty_python_semantic`


---

_Label `internal` added by @AlexWaygood on 2025-08-12 14:49_

---

_Label `ty` added by @AlexWaygood on 2025-08-12 14:49_

---

_@MichaReiser approved on 2025-08-12 14:52_

---

_Marked ready for review by @AlexWaygood on 2025-08-12 14:52_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-12 14:52_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-12 14:52_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-12 14:52_

---

_Merged by @AlexWaygood on 2025-08-12 14:53_

---

_Closed by @AlexWaygood on 2025-08-12 14:53_

---

_Branch deleted on 2025-08-12 14:53_

---
