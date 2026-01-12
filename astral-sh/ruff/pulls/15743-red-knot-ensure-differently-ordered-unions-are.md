```yaml
number: 15743
title: "[red-knot] Ensure differently ordered unions are considered equivalent when they appear inside tuples inside top-level intersections"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/tuples-in-intersections
created_at: 2025-01-25T18:12:53Z
updated_at: 2025-01-25T18:19:30Z
url: https://github.com/astral-sh/ruff/pull/15743
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Ensure differently ordered unions are considered equivalent when they appear inside tuples inside top-level intersections

---

_@AlexWaygood_

## Summary

Another oversight in type equivalence, that I spotted by re-reading some of the code in `types.rs`. In this round of type-equivalence whackamole, we were not considering `tuple[P | Q] & R` to be equivalent to `tuple[Q | P] & R`. This bug only manifested if a union was nested directly inside a tuple nested directly inside a top-level intersection.

It sure would be easier to ensure we got all these cases right if unions only ever appeared in a canonical order 😉 I'm not _yet_ pushing for us to reconsider the direction we went in https://github.com/astral-sh/ruff/pull/15516, but there _is_ definitely a maintenance burden here.

## Test Plan

- New mdtest added that fails on `main`
- Checked that all stable property tests continue to pass


---

_Label `red-knot` added by @AlexWaygood on 2025-01-25 18:12_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-25 18:12_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-25 18:12_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-25 18:12_

---

_Comment by @AlexWaygood on 2025-01-25 18:19_

[Neutral on codspeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Ftuples-in-intersections)

---

_Merged by @AlexWaygood on 2025-01-25 18:19_

---

_Closed by @AlexWaygood on 2025-01-25 18:19_

---

_Branch deleted on 2025-01-25 18:19_

---
