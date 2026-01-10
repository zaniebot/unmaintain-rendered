```yaml
number: 14092
title: "Avoid cloning `Name` when looking up function and class types"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: micha/lookup-name
created_at: 2024-11-04T14:44:17Z
updated_at: 2024-11-04T20:18:13Z
url: https://github.com/astral-sh/ruff/pull/14092
synced_at: 2026-01-10T20:50:57Z
```

# Avoid cloning `Name` when looking up function and class types

---

_Pull request opened by @MichaReiser on 2024-11-04 14:44_

## Summary

Uses Salsa's new `Lookup` functionality for `FunctionType` and `ClassType` names. 
The `Lookup` trait allows looking-up interned values by a borrowed value. In this case
we can lookup the interned type using the borrowed `str` and only allocate the owned `Name`` when 
the type doesn't already exist.

This was part of https://github.com/astral-sh/ruff/pull/14087 but I extracted it into its own PR to see if it is the reason for the significant incremental perf improvement.

## Test Plan

`cargo test`


---

_Review requested from @carljm by @MichaReiser on 2024-11-04 14:44_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-04 14:44_

---

_Review requested from @sharkdp by @MichaReiser on 2024-11-04 14:44_

---

_Label `internal` added by @MichaReiser on 2024-11-04 14:44_

---

_Label `red-knot` added by @MichaReiser on 2024-11-04 14:44_

---

_@AlexWaygood approved on 2024-11-04 14:46_

---

_Merged by @MichaReiser on 2024-11-04 14:52_

---

_Closed by @MichaReiser on 2024-11-04 14:53_

---

_Branch deleted on 2024-11-04 14:53_

---

_Comment by @github-actions[bot] on 2024-11-04 15:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @carljm on 2024-11-04 20:13_

> This was part of #14087 but I extracted it into its own PR to see if it is the reason for the significant incremental perf improvement.

Was it? I don't see any CodSpeed results reported here.

---

_Comment by @MichaReiser on 2024-11-04 20:18_

Nope, this wasn't it :)

---
