```yaml
number: 11758
title: "[red-knot] rename Definition::None to Definition::Unbound"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/no-def-none
created_at: 2024-06-05T17:14:02Z
updated_at: 2024-06-05T17:32:27Z
url: https://github.com/astral-sh/ruff/pull/11758
synced_at: 2026-01-12T15:55:38Z
```

# [red-knot] rename Definition::None to Definition::Unbound

---

_@carljm_

## Summary

After looking at this a bit, I think it does make sense to have `Unbound` as part of the `Definition` enum; if we are modeling `Unbound` as a type (which currently we are), then every symbol implicitly starts each scope with a "definition" as unbound, and the cleanest way to model that is as a real `Definition`. We should be able to handle a definition of "unbound" anywhere we handle definitions.

But the name `None` wasn't clear enough; changing the name to `Unbound` and adding a doc comment.

Also change `[first].into_iter()` to `std::iter::once(first)`, from post-land code review on a prior PR.

## Test Plan

Existing tests.


---

_Label `red-knot` added by @carljm on 2024-06-05 17:14_

---

_Review requested from @AlexWaygood by @carljm on 2024-06-05 17:14_

---

_Review requested from @MichaReiser by @carljm on 2024-06-05 17:14_

---

_@AlexWaygood approved on 2024-06-05 17:27_

---

_Comment by @github-actions[bot] on 2024-06-05 17:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/mlflow/mlflow">mlflow/mlflow</a> (error)</summary>
<p>

```
Failed to clone mlflow/mlflow: error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.
error: 4377 bytes of body are still expected
fetch-pack: unexpected disconnect while reading sideband packet
fatal: early EOF
fatal: fetch-pack: invalid index-pack output
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-06-05 17:32_

---

_Closed by @carljm on 2024-06-05 17:32_

---

_Branch deleted on 2024-06-05 17:32_

---
