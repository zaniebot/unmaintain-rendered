```yaml
number: 18078
title: "[ty] Add cycle handling for unpacking targets"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: dhruv/unpack-cycle-handling
created_at: 2025-05-13T20:26:17Z
updated_at: 2025-05-13T21:28:08Z
url: https://github.com/astral-sh/ruff/pull/18078
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Add cycle handling for unpacking targets

---

_@dhruvmanila_

## Summary

This PR adds cycle handling for `infer_unpack_types` based on the analysis in astral-sh/ty#364.

Fixes: astral-sh/ty#364

## Test Plan

Add a cycle handling test for unpacking in `cycle.md`


---

_Review requested from @carljm by @dhruvmanila on 2025-05-13 20:26_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-05-13 20:26_

---

_Label `bug` added by @dhruvmanila on 2025-05-13 20:26_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-05-13 20:26_

---

_Review requested from @dcreager by @dhruvmanila on 2025-05-13 20:26_

---

_Label `ty` added by @dhruvmanila on 2025-05-13 20:26_

---

_Comment by @github-actions[bot] on 2025-05-13 20:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ error[type-assertion-failure] tests/annotations/declarations.py:951:5: Argument does not have asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 649 diagnostics
+ Found 650 diagnostics

```
</details>


---

_@carljm approved on 2025-05-13 21:04_

Looks good!

---

_Closed by @dhruvmanila on 2025-05-13 21:24_

---

_Reopened by @dhruvmanila on 2025-05-13 21:24_

---

_Merged by @dhruvmanila on 2025-05-13 21:27_

---

_Closed by @dhruvmanila on 2025-05-13 21:27_

---

_Branch deleted on 2025-05-13 21:27_

---
