```yaml
number: 18889
title: "[ty] Minor change to builtins.md test"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/builtin-shadowing-tests
created_at: 2025-06-23T10:23:42Z
updated_at: 2025-06-23T10:32:52Z
url: https://github.com/astral-sh/ruff/pull/18889
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Minor change to builtins.md test

---

_Pull request opened by @sharkdp on 2025-06-23 10:23_

## Summary

As far as I can tell, the two existing tests did the exact same thing. Remove the redundant test, and add tests for all combinations of declared/not-declared and local/"public" use of the name.

Proposing this as a separate PR before the behavior might change via https://github.com/astral-sh/ruff/pull/18750

---

_Label `ty` added by @sharkdp on 2025-06-23 10:23_

---

_Review requested from @carljm by @sharkdp on 2025-06-23 10:23_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-06-23 10:23_

---

_Review requested from @dcreager by @sharkdp on 2025-06-23 10:23_

---

_Comment by @github-actions[bot] on 2025-06-23 10:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `testing` added by @AlexWaygood on 2025-06-23 10:29_

---

_@AlexWaygood approved on 2025-06-23 10:30_

---

_Merged by @sharkdp on 2025-06-23 10:32_

---

_Closed by @sharkdp on 2025-06-23 10:32_

---

_Branch deleted on 2025-06-23 10:32_

---
