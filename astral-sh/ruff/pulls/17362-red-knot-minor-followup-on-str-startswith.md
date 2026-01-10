```yaml
number: 17362
title: "[red-knot] Minor followup on str.startswith"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/startswith-followup
created_at: 2025-04-11T18:31:45Z
updated_at: 2025-04-11T18:40:09Z
url: https://github.com/astral-sh/ruff/pull/17362
synced_at: 2026-01-10T19:40:37Z
```

# [red-knot] Minor followup on str.startswith

---

_Pull request opened by @sharkdp on 2025-04-11 18:31_

## Summary

Minor follow-up to https://github.com/astral-sh/ruff/pull/17351, thank you @AlexWaygood.

---

_Label `red-knot` added by @sharkdp on 2025-04-11 18:31_

---

_Review requested from @carljm by @sharkdp on 2025-04-11 18:31_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-11 18:31_

---

_Review requested from @dcreager by @sharkdp on 2025-04-11 18:31_

---

_Comment by @github-actions[bot] on 2025-04-11 18:33_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:6197 on 2025-04-11 18:34_

```suggestion
    /// literal return types if the instance and the prefix are both string literals,
    /// and this allows us to infer statically known branches for common tests
    /// such as `if sys.platform.startswith("linux")`
```

---

_@AlexWaygood approved on 2025-04-11 18:34_

thank you!

---

_Merged by @sharkdp on 2025-04-11 18:40_

---

_Closed by @sharkdp on 2025-04-11 18:40_

---

_Branch deleted on 2025-04-11 18:40_

---
