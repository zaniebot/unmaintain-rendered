```yaml
number: 16823
title: "[red-knot] Improve property test performance by cloning db instead of holding `MutexGuard`"
type: pull_request
state: merged
author: cake-monotone
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: cake/make-property-tests-faster
created_at: 2025-03-18T07:46:55Z
updated_at: 2025-03-18T08:19:31Z
url: https://github.com/astral-sh/ruff/pull/16823
synced_at: 2026-01-12T15:55:58Z
```

# [red-knot] Improve property test performance by cloning db instead of holding `MutexGuard`

---

_@cake-monotone_

## Summary

This PR brings an optimization.

- `get_cached_db` no longer returns a `MutexGuard`; instead, it returns a cloned database.

### `get_cached_db`

Previously, the `MutexGuard` was held inside the property test function (defined in the macro), which prevented multiple property tests from running in parallel. More specifically, the program could only test one random test case at a time, which likely caused a significant bottleneck.

On my local machine, running:

```
QUICKCHECK_TESTS=100000 cargo test --release -p red_knot_python_semantic -- --ignored stable
```

showed about **a 75% speedup** (from \~60s to \~15s).


---

_Review requested from @carljm by @cake-monotone on 2025-03-18 07:46_

---

_Review requested from @AlexWaygood by @cake-monotone on 2025-03-18 07:46_

---

_Review requested from @sharkdp by @cake-monotone on 2025-03-18 07:46_

---

_Review requested from @dcreager by @cake-monotone on 2025-03-18 07:46_

---

_Renamed from "[red-knot] Make proprety tests faster" to "[red-knot] Improve property test performance by cloning db instead of holding `MutexGuard`" by @cake-monotone on 2025-03-18 07:51_

---

_Comment by @github-actions[bot] on 2025-03-18 07:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-03-18 08:09_

Nice find. 

---

_Label `testing` added by @MichaReiser on 2025-03-18 08:09_

---

_Label `red-knot` added by @MichaReiser on 2025-03-18 08:09_

---

_Merged by @MichaReiser on 2025-03-18 08:09_

---

_Closed by @MichaReiser on 2025-03-18 08:09_

---

_Branch deleted on 2025-03-18 08:19_

---
