```yaml
number: 17544
title: "[red-knot] mypy_primer: extend ecosystem checks"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/mypy_primer-extend-ecosystem-checks
created_at: 2025-04-22T11:10:43Z
updated_at: 2025-04-22T11:39:44Z
url: https://github.com/astral-sh/ruff/pull/17544
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] mypy_primer: extend ecosystem checks

---

_@sharkdp_

## Summary

Takes the `good.txt` changes from #17474, and removes the following projects:
- arrow (not part of mypy_primer upstream)
- freqtrade, hydpy, ibis, pandera, xarray (saw panics locally, all related to try_metaclass cycles)

Increases the mypy_primer CI run time to ~4 min.

## Test Plan

Three successful CI runs.


---

_Label `testing` added by @sharkdp on 2025-04-22 11:10_

---

_Label `red-knot` added by @sharkdp on 2025-04-22 11:10_

---

_Comment by @github-actions[bot] on 2025-04-22 11:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @sharkdp on 2025-04-22 11:33_

---

_Review requested from @carljm by @sharkdp on 2025-04-22 11:33_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-22 11:33_

---

_Review requested from @dcreager by @sharkdp on 2025-04-22 11:33_

---

_@AlexWaygood approved on 2025-04-22 11:34_

---

_Merged by @sharkdp on 2025-04-22 11:39_

---

_Closed by @sharkdp on 2025-04-22 11:39_

---

_Branch deleted on 2025-04-22 11:39_

---
