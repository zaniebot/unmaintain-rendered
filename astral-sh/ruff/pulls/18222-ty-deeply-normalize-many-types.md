```yaml
number: 18222
title: "[ty] Deeply normalize many types"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/deeply-normalize
created_at: 2025-05-20T15:24:48Z
updated_at: 2025-05-20T15:41:27Z
url: https://github.com/astral-sh/ruff/pull/18222
synced_at: 2026-01-12T15:56:15Z
```

# [ty] Deeply normalize many types

---

_@AlexWaygood_

## Summary

This PR applies deep normalization to many type variants that wrap other types. This should mean that we have more robust handling for equivalence and gradual equivalence even when encountering differently ordered unions.

I went down this road because I was hoping it would solve https://github.com/astral-sh/ty/issues/459. It doesn't look like it does (at least not on its own), but it seems like something that should make us more robust in the long term anyway.

## Test Plan

`cargo test -p ty_python_semantic`


---

_Label `ty` added by @AlexWaygood on 2025-05-20 15:24_

---

_Comment by @github-actions[bot] on 2025-05-20 15:27_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @AlexWaygood on 2025-05-20 15:32_

1% speedup on the many_string_assignments benchmark! No idea why, but I'll take it 😄 https://codspeed.io/astral-sh/ruff/branches/alex%2Fdeeply-normalize

---

_Marked ready for review by @AlexWaygood on 2025-05-20 15:32_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-20 15:32_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-20 15:32_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-20 15:32_

---

_Label `internal` added by @AlexWaygood on 2025-05-20 15:35_

---

_@carljm approved on 2025-05-20 15:38_

Thank you!

---

_Merged by @AlexWaygood on 2025-05-20 15:41_

---

_Closed by @AlexWaygood on 2025-05-20 15:41_

---

_Branch deleted on 2025-05-20 15:41_

---
