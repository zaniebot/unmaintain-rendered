```yaml
number: 12393
title: "[red-knot] use a simpler builtin in the benchmark"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/simplify-benchmark
created_at: 2024-07-18T20:23:45Z
updated_at: 2024-07-18T21:04:35Z
url: https://github.com/astral-sh/ruff/pull/12393
synced_at: 2026-01-10T21:47:02Z
```

# [red-knot] use a simpler builtin in the benchmark

---

_Pull request opened by @carljm on 2024-07-18 20:23_

In preparation for supporting resolving builtins, simplify the benchmark so it doesn't look up `str`, which is actually a complex builtin to deal with because it inherits `Sequence[str]`.


---

_Label `red-knot` added by @carljm on 2024-07-18 20:24_

---

_Review requested from @MichaReiser by @carljm on 2024-07-18 20:24_

---

_Review requested from @AlexWaygood by @carljm on 2024-07-18 20:24_

---

_Comment by @AlexWaygood on 2024-07-18 20:25_

I beat you by four minutes! :-) https://github.com/astral-sh/ruff/pull/12392

---

_Comment by @carljm on 2024-07-18 20:26_

Yeah but can we go with this one anyway, since I already rebased the next PR on top of it? :) I thought you said you were gonna quit for the day!

---

_Comment by @AlexWaygood on 2024-07-18 20:28_

> Yeah but can we go with this one anyway, since I already rebased the next PR on top of it? :) I thought you said you were gonna quit for the day!

Micha started reviewing! I couldn't go just when all the fun was starting 😆

---

_@AlexWaygood approved on 2024-07-18 20:28_

---

_Comment by @github-actions[bot] on 2024-07-18 20:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @carljm on 2024-07-18 21:04_

---

_Closed by @carljm on 2024-07-18 21:04_

---

_Branch deleted on 2024-07-18 21:04_

---
