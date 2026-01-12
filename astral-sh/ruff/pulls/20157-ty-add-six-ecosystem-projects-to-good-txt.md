```yaml
number: 20157
title: "[ty] add six ecosystem projects to good.txt"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/ecosystem
created_at: 2025-08-29T17:36:20Z
updated_at: 2025-08-29T18:37:31Z
url: https://github.com/astral-sh/ruff/pull/20157
synced_at: 2026-01-12T15:56:55Z
```

# [ty] add six ecosystem projects to good.txt

---

_@carljm_

## Summary

These projects all check successfully now.

(Pandas still takes 9s, as the comment in `bad.txt` said, but I don't think this is slow enough to exclude it; mypy-primer overall still runs in 4 minutes, faster than e.g. the test suite on Windows.)

## Test Plan

mypy-primer CI.


---

_Label `ty` added by @carljm on 2025-08-29 17:36_

---

_Comment by @github-actions[bot] on 2025-08-29 17:38_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-29 17:41_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-08-29 18:27_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-29 18:27_

---

_Review requested from @sharkdp by @carljm on 2025-08-29 18:27_

---

_Review requested from @dcreager by @carljm on 2025-08-29 18:27_

---

_Comment by @carljm on 2025-08-29 18:37_

Going to go ahead and land this without review; seems uncontroversial :)

---

_Merged by @carljm on 2025-08-29 18:37_

---

_Closed by @carljm on 2025-08-29 18:37_

---

_Branch deleted on 2025-08-29 18:37_

---
