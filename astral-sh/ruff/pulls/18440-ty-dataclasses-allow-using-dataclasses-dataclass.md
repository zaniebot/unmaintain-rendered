```yaml
number: 18440
title: "[ty] dataclasses: Allow using dataclasses.dataclass as a function."
type: pull_request
state: merged
author: abhijeetbodas2001
labels:
  - ty
assignees: []
merged: true
base: main
head: dataclass
created_at: 2025-06-03T13:03:59Z
updated_at: 2025-06-03T18:37:11Z
url: https://github.com/astral-sh/ruff/pull/18440
synced_at: 2026-01-12T15:56:19Z
```

# [ty] dataclasses: Allow using dataclasses.dataclass as a function.

---

_@abhijeetbodas2001_

## Summary

Part of https://github.com/astral-sh/ty/issues/111

Using `dataclass` as a function, instead of as a decorator did not work as expected prior to this.
Fix that by modifying the dataclass overload's return type.

## Test Plan

New mdtests, fixing the existing TODO.


---

_Review requested from @carljm by @abhijeetbodas2001 on 2025-06-03 13:04_

---

_Review requested from @AlexWaygood by @abhijeetbodas2001 on 2025-06-03 13:04_

---

_Review requested from @sharkdp by @abhijeetbodas2001 on 2025-06-03 13:04_

---

_Review requested from @dcreager by @abhijeetbodas2001 on 2025-06-03 13:04_

---

_Label `ty` added by @ntBre on 2025-06-03 13:05_

---

_Comment by @github-actions[bot] on 2025-06-03 13:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-06-03 16:35_

---

_Renamed from "dataclasses: Allow using dataclasses.dataclass as a function." to "[ty] dataclasses: Allow using dataclasses.dataclass as a function." by @MichaReiser on 2025-06-03 16:39_

---

_@carljm approved on 2025-06-03 16:50_

Nice, thank you!

---

_Merged by @carljm on 2025-06-03 16:50_

---

_Closed by @carljm on 2025-06-03 16:50_

---

_Branch deleted on 2025-06-03 18:37_

---
