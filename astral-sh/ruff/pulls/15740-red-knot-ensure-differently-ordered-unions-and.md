```yaml
number: 15740
title: "[red-knot] Ensure differently ordered unions and intersections are understood as equivalent even inside arbitrarily nested tuples"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/nested-tuples
created_at: 2025-01-25T16:28:29Z
updated_at: 2025-01-25T16:39:09Z
url: https://github.com/astral-sh/ruff/pull/15740
synced_at: 2026-01-12T15:55:52Z
```

# [red-knot] Ensure differently ordered unions and intersections are understood as equivalent even inside arbitrarily nested tuples

---

_@AlexWaygood_

## Summary

On `main`, red-knot:
- Considers `P | Q` equivalent to `Q | P`
- Considered `tuple[P | Q]` equivalent to `tuple[Q | P]`
- Considers `tuple[P | tuple[P | Q]]` equivalent to `tuple[tuple[Q | P] | P]`
- ‼️ Does _not_ consider `tuple[tuple[P | Q]]` equivalent to `tuple[tuple[Q | P]]`

The key difference for the last one of these is that the union appears inside a tuple that is directly nested inside another tuple.

This PR fixes this so that differently ordered unions are considered equivalent even when they appear inside arbitrarily nested tuple types.

## Test Plan

- Added mdtests that fails on `main`
- Checked that all property tests continue to pass with this PR


---

_Label `red-knot` added by @AlexWaygood on 2025-01-25 16:28_

---

_Review requested from @carljm by @AlexWaygood on 2025-01-25 16:28_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-01-25 16:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-01-25 16:28_

---

_Renamed from "Add a failing test" to "[red-knot] Ensure differently ordered unions and intersections are understood as equivalent even inside arbitrarily nested tuples" by @AlexWaygood on 2025-01-25 16:29_

---

_@carljm approved on 2025-01-25 16:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1186 on 2025-01-25 16:38_

we have a [teeny tiny speedup on Codspeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Fnested-tuples), which I suspect is just due to the change on this line

---

_@AlexWaygood reviewed on 2025-01-25 16:38_

---

_Merged by @AlexWaygood on 2025-01-25 16:39_

---

_Closed by @AlexWaygood on 2025-01-25 16:39_

---

_Branch deleted on 2025-01-25 16:39_

---
