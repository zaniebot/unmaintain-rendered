```yaml
number: 19244
title: "[ty] Do not run `mypy_primer.yaml` when all changed files are Markdown files"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ci
  - ty
assignees: []
merged: true
base: main
head: mypy-primer
created_at: 2025-07-09T18:33:21Z
updated_at: 2025-07-09T18:40:48Z
url: https://github.com/astral-sh/ruff/pull/19244
synced_at: 2026-01-12T15:56:34Z
```

# [ty] Do not run `mypy_primer.yaml` when all changed files are Markdown files

---

_@InSyncWithFoo_

## Summary

Resolves [#781](https://github.com/astral-sh/ty/issues/781).

After this change, a PR that changes only `.md` files anywhere in the codebase will not cause the `mypy_primer.yaml` workflow to be run.

## Test Plan

None.


---

_Comment by @github-actions[bot] on 2025-07-09 18:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@AlexWaygood approved on 2025-07-09 18:40_

Thanks!

---

_Label `ci` added by @AlexWaygood on 2025-07-09 18:40_

---

_Label `ty` added by @AlexWaygood on 2025-07-09 18:40_

---

_Merged by @AlexWaygood on 2025-07-09 18:40_

---

_Closed by @AlexWaygood on 2025-07-09 18:40_

---

_Branch deleted on 2025-07-09 18:40_

---
