```yaml
number: 16001
title: "[red-knot] Partial revert of relative import handling for files in the root of a search path"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/revert-relative-import-handling
created_at: 2025-02-06T18:36:13Z
updated_at: 2025-02-07T10:04:12Z
url: https://github.com/astral-sh/ruff/pull/16001
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Partial revert of relative import handling for files in the root of a search path

---

_Pull request opened by @MichaReiser on 2025-02-06 18:36_

## Summary

This PR reverts the behavior changes from https://github.com/astral-sh/ruff/pull/15990

But it isn't just a revert, it also:

* Adds a test covering this specific behavior
* Preserves the improvement to use `saturating_sub` in the package case to avoid overflows in the case of invalid syntax
* Use `ancestors` instead of a `for` loop

## Test Plan

Added test


---

_Renamed from "micha/revert relative import handling" to "Partial revert of relative import handling for files in the root of a search path" by @MichaReiser on 2025-02-06 18:36_

---

_Renamed from "Partial revert of relative import handling for files in the root of a search path" to "[red-knot] Partial revert of relative import handling for files in the root of a search path" by @MichaReiser on 2025-02-06 18:37_

---

_Label `red-knot` added by @MichaReiser on 2025-02-06 18:37_

---

_Marked ready for review by @MichaReiser on 2025-02-06 18:40_

---

_Review requested from @carljm by @MichaReiser on 2025-02-06 18:40_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-02-06 18:40_

---

_Review requested from @sharkdp by @MichaReiser on 2025-02-06 18:40_

---

_@AlexWaygood approved on 2025-02-06 18:41_

Thank you! And sorry for not catching this in review of your earlier PR

---

_Comment by @MichaReiser on 2025-02-06 18:44_

> Thank you! And sorry for not catching this in review of your earlier PR

No worries. I should have verified it. I'll wait to merge until the Ruff release is done.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:225 on 2025-02-06 18:47_

It also sneaks in a prose usage of "Red Knot", in the eternal "Red Knot" vs "red-knot" wars ;)

---

_@carljm approved on 2025-02-06 18:47_

---

_@AlexWaygood reviewed on 2025-02-06 18:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/relative.md`:225 on 2025-02-06 18:48_

cc. @BurntSushi

---

_Merged by @MichaReiser on 2025-02-07 10:04_

---

_Closed by @MichaReiser on 2025-02-07 10:04_

---

_Branch deleted on 2025-02-07 10:04_

---
