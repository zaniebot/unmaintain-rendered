```yaml
number: 18466
title: "[ty] Only consider a type `T` a subtype of a protocol `P` if all of `P`'s members are fully bound on `T`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: alex/proto-subtyping-boundness
created_at: 2025-06-04T17:41:58Z
updated_at: 2025-06-04T19:39:21Z
url: https://github.com/astral-sh/ruff/pull/18466
synced_at: 2026-01-12T15:56:19Z
```

# [ty] Only consider a type `T` a subtype of a protocol `P` if all of `P`'s members are fully bound on `T`

---

_@AlexWaygood_

## Summary

Fixes https://github.com/astral-sh/ty/issues/578

## Test Plan

mdtests


---

_Label `bug` added by @AlexWaygood on 2025-06-04 17:42_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-04 17:42_

---

_Label `ty` added by @AlexWaygood on 2025-06-04 17:42_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-04 17:42_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-04 17:42_

---

_Comment by @github-actions[bot] on 2025-06-04 17:45_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pytest (https://github.com/pytest-dev/pytest)
- warning[possibly-unbound-attribute] src/_pytest/config/__init__.py:1212:48: Attribute `default` on type `Argument` is possibly unbound
- Found 777 diagnostics
+ Found 776 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- warning[possibly-unbound-attribute] sklearn/cluster/tests/test_hierarchical.py:175:16: Attribute `distances_` on type `AgglomerativeClustering` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/model_selection/tests/test_search.py:1567:27: Attribute `refit_time_` on type `GridSearchCV | RandomizedSearchCV` is possibly unbound
- warning[possibly-unbound-attribute] sklearn/model_selection/tests/test_search.py:1568:16: Attribute `refit_time_` on type `GridSearchCV | RandomizedSearchCV` is possibly unbound
- Found 2562 diagnostics
+ Found 2559 diagnostics

```
</details>


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/instance.rs`:48 on 2025-06-04 19:31_

Doesn't it already do this part? So it seems weird to have it as part of a TODO comment
```suggestion
```

---

_@carljm approved on 2025-06-04 19:31_

Nice!

---

_Merged by @AlexWaygood on 2025-06-04 19:39_

---

_Closed by @AlexWaygood on 2025-06-04 19:39_

---

_Branch deleted on 2025-06-04 19:39_

---
