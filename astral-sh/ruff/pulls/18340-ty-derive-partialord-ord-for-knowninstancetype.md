```yaml
number: 18340
title: "[ty] Derive `PartialOrd, Ord` for `KnownInstanceType`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/knowninstance-ord
created_at: 2025-05-27T18:17:01Z
updated_at: 2025-05-27T18:37:02Z
url: https://github.com/astral-sh/ruff/pull/18340
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Derive `PartialOrd, Ord` for `KnownInstanceType`

---

_@AlexWaygood_

## Summary

Unlike a series of integers or strings, there's no self-evident way in which a series of `KnownInstanceType`s should always be ordered if you're sorting them. This has always been the motivation for not deriving `PartialOrd` and `Ord` on this enum, and instead tediously writing this huge `match` statement out by hand.

Following https://github.com/astral-sh/ruff/pull/18212, however, we must explicitly derive `PartialOrd` and `Ord` for any Salsa-tracked structs that we wish to sort into an order. This is a good change -- it sharply brings into focus the fact that the orderings Salsa was previously implicitly deriving for us were also arbitrary orderings that had no grounding in the underlying type being represented by the Salsa ID. We already knew this, but now it's explicit.

The ordering of `KnownInstanceType`s isn't inherently any more arbitrary than the orderings we're deriving elsewhere; so let's just derive `PartialOrd, Ord` here too. It makes the code a fair bit simpler :-)

## Test Plan

`cargo test -p ty_python_semantic`


---

_Review requested from @carljm by @AlexWaygood on 2025-05-27 18:17_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-27 18:17_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-27 18:17_

---

_Label `internal` added by @AlexWaygood on 2025-05-27 18:17_

---

_Label `ty` added by @AlexWaygood on 2025-05-27 18:17_

---

_Comment by @github-actions[bot] on 2025-05-27 18:19_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/known_instance.rs`:27 on 2025-05-27 18:33_

```suggestion
/// The id may change between runs, or when the known instance was garbage collected and recreated.
```

---

_@carljm approved on 2025-05-27 18:34_

---

_@carljm reviewed on 2025-05-27 18:35_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/known_instance.rs`:27 on 2025-05-27 18:35_

Oh never mind, I see now that this was probably intentional -- the wrapped data are typevars.

---

_Merged by @AlexWaygood on 2025-05-27 18:37_

---

_Closed by @AlexWaygood on 2025-05-27 18:37_

---

_Branch deleted on 2025-05-27 18:37_

---
