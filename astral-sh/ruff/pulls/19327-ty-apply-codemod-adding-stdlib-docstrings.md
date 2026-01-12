```yaml
number: 19327
title: "[ty] Apply codemod adding stdlib docstrings"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - do-not-merge
  - ty
assignees: []
draft: true
base: main
head: alex/add-docstrings
created_at: 2025-07-14T12:41:06Z
updated_at: 2025-07-14T16:03:00Z
url: https://github.com/astral-sh/ruff/pull/19327
synced_at: 2026-01-12T15:56:36Z
```

# [ty] Apply codemod adding stdlib docstrings

---

_@AlexWaygood_

## Summary

PR that adds the codemod being setup in https://github.com/astral-sh/ruff/pull/19311, to check that it doesn't have a deleterious impact on performance. It's just for testing purposes, since the idea is that this codemod will be autoapplied during every typeshed sync.

## Test Plan

CI


---

_Label `do-not-merge` added by @AlexWaygood on 2025-07-14 12:41_

---

_Label `ty` added by @AlexWaygood on 2025-07-14 12:41_

---

_Renamed from "add the docstrings" to "[ty] Apply codemod adding stdlib docstrings" by @AlexWaygood on 2025-07-14 12:41_

---

_Comment by @github-actions[bot] on 2025-07-14 12:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~63MB
+     memo fields = ~66MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~152MB
+     memo fields = ~159MB

```
</details>


---

_Comment by @AlexWaygood on 2025-07-14 12:57_

(the failing tests are just some snapshots that would need to be updated)

---

_Closed by @AlexWaygood on 2025-07-14 16:02_

---

_Branch deleted on 2025-07-14 16:03_

---
