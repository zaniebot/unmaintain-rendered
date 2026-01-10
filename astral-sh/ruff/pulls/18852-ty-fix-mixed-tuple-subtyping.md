```yaml
number: 18852
title: "[ty] Fix mixed tuple subtyping"
type: pull_request
state: merged
author: carljm
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: cjm/tuplefix
created_at: 2025-06-21T18:03:24Z
updated_at: 2025-06-23T12:47:47Z
url: https://github.com/astral-sh/ruff/pull/18852
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Fix mixed tuple subtyping

---

_Pull request opened by @carljm on 2025-06-21 18:03_

## Summary

The code in the `Variable` branch of `VariableLengthTupleSpec::has_relation_to` made the incorrect assumption that if you zip two possibly-different-length iterators together and iterate over the resulting zip iterator, the original two iterators will only have their common elements consumed. But in fact, the zip iterator detects that it is done when it receives a `None` from one iterator and `Some()` element from the other iterator, which means that it consumes one additional element from the longer iterator. This meant that we failed to detect mismatched types on this extra consumed element, because we never compared it to the variable type of the other tuple.

Use `zip_longest` from itertools as an alternative, which allows us to combine all the handling into just two `zip_longest`, one for prefixes and one for suffixes.

Marking this PR internal since it fixes a bug in a commit that wasn't released yet.

## Test Plan

Added mdtests that failed before this fix and pass after it.


---

_Label `internal` added by @carljm on 2025-06-21 18:03_

---

_Review requested from @AlexWaygood by @carljm on 2025-06-21 18:03_

---

_Label `ty` added by @carljm on 2025-06-21 18:03_

---

_Review requested from @sharkdp by @carljm on 2025-06-21 18:03_

---

_Review requested from @dcreager by @carljm on 2025-06-21 18:03_

---

_Comment by @github-actions[bot] on 2025-06-21 18:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser reviewed on 2025-06-21 18:14_

Nit: An alternative would be https://docs.rs/itertools/latest/itertools/trait.Itertools.html#method.zip_longest

---

_Comment by @carljm on 2025-06-21 18:42_

@MichaReiser recommending itertools? What is happening? 😆 

...ok, that is pretty nice. Updated.

---

_@MichaReiser approved on 2025-06-21 19:27_

---

_Merged by @carljm on 2025-06-21 20:09_

---

_Closed by @carljm on 2025-06-21 20:09_

---

_Branch deleted on 2025-06-21 20:09_

---

_@dcreager reviewed on 2025-06-23 12:47_

Thanks for catching this!

---
