```yaml
number: 18397
title: "[ty] Update docs for Python version inference"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: alex/pyversion-docs
created_at: 2025-05-30T21:42:09Z
updated_at: 2025-05-30T21:45:30Z
url: https://github.com/astral-sh/ruff/pull/18397
synced_at: 2026-01-10T18:45:04Z
```

# [ty] Update docs for Python version inference

---

_Pull request opened by @AlexWaygood on 2025-05-30 21:42_

## Summary

Following https://github.com/astral-sh/ruff/pull/18057, we now infer the Python version from the user's virtual environment if it's possible to do so and the user has not configured a Python version explicitly. I forgot to update the docs.

## Test Plan

`cargo dev generate-all`


---

_Label `documentation` added by @AlexWaygood on 2025-05-30 21:42_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-30 21:42_

---

_Label `ty` added by @AlexWaygood on 2025-05-30 21:42_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-05-30 21:42_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-30 21:42_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-30 21:42_

---

_Comment by @github-actions[bot] on 2025-05-30 21:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-05-30 21:44_

---

_Merged by @AlexWaygood on 2025-05-30 21:45_

---

_Closed by @AlexWaygood on 2025-05-30 21:45_

---

_Branch deleted on 2025-05-30 21:45_

---
