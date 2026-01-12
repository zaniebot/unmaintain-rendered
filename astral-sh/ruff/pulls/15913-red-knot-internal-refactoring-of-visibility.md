```yaml
number: 15913
title: "[red-knot] Internal refactoring of visibility constraints API"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/vc-api
created_at: 2025-02-03T15:24:31Z
updated_at: 2025-02-03T20:13:12Z
url: https://github.com/astral-sh/ruff/pull/15913
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Internal refactoring of visibility constraints API

---

_@dcreager_

This extracts some pure refactoring noise from https://github.com/astral-sh/ruff/pull/15861.  This changes the API for creating and evaluating visibility constraints, but does not change how they are respresented internally.  There should be no behavioral or performance changes in this PR.

Changes:

- Hide the internal representation isn't changed, so that we can make changes to it in #15861.
- Add a separate builder type for visibility constraints. (With TDDs, we will have some additional builder state that we can throw away once we're done constructing.)
- Remove a layer of helper methods from `UseDefMapBuilder`, making `SemanticIndexBuilder` responsible for constructing whatever visibility constraints it needs.

---

_Review requested from @carljm by @dcreager on 2025-02-03 15:24_

---

_Review requested from @MichaReiser by @dcreager on 2025-02-03 15:24_

---

_Review requested from @AlexWaygood by @dcreager on 2025-02-03 15:24_

---

_Review requested from @sharkdp by @dcreager on 2025-02-03 15:24_

---

_Renamed from "Dcreager/vc api" to "[red-knot] Internal refactoring of visibility constraints API" by @dcreager on 2025-02-03 15:25_

---

_Label `internal` added by @dcreager on 2025-02-03 15:25_

---

_Label `red-knot` added by @dcreager on 2025-02-03 15:25_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:189 on 2025-02-03 15:27_

Adding this as a separate type made it easier to verify that no callers are constructing this manually, and always use one of the builder's constructor methods

---

_@MichaReiser reviewed on 2025-02-03 15:28_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:273 on 2025-02-03 15:28_

Is the motivation for this to improve perf?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:201 on 2025-02-03 15:28_

Moved into this module to be near the other visibility constraint stuff

---

_@dcreager reviewed on 2025-02-03 15:29_

---

_@dcreager reviewed on 2025-02-03 15:58_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/visibility_constraints.rs`:273 on 2025-02-03 15:58_

This is to make sure that terminals are always normalized. Before we didn't have separate a variant for `AlwaysFalse`, we represented that as `VisibleIfNot(AlwaysTrue)`.  I added the additional variant to make sure all terminals had first-class support in the enum.  The AND and OR constructors were already handling cases where we statically know the result (e.g. `A ∧ false == false` for all `A`) so I wanted to be consistent here.

---

_@carljm approved on 2025-02-03 20:00_

---

_Merged by @dcreager on 2025-02-03 20:13_

---

_Closed by @dcreager on 2025-02-03 20:13_

---

_Branch deleted on 2025-02-03 20:13_

---
