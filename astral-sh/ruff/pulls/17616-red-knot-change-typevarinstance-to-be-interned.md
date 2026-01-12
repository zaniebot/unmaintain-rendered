```yaml
number: 17616
title: "[red-knot] change TypeVarInstance to be interned, not tracked"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/interntv
created_at: 2025-04-24T21:13:59Z
updated_at: 2025-04-24T21:52:28Z
url: https://github.com/astral-sh/ruff/pull/17616
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] change TypeVarInstance to be interned, not tracked

---

_@carljm_

## Summary

Tracked structs have some issues with fixpoint iteration in Salsa, and there's not actually any need for this to be tracked, it should be interned like most of our type structs.

The removed comment was probably never correct (in that we could have disambiguated sufficiently), and is definitely not relevant now that `TypeVarInstance` also holds its `Definition`.

## Test Plan

Existing tests.


---

_Label `red-knot` added by @carljm on 2025-04-24 21:13_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-24 21:14_

---

_Review requested from @sharkdp by @carljm on 2025-04-24 21:14_

---

_Review requested from @dcreager by @carljm on 2025-04-24 21:14_

---

_@dcreager approved on 2025-04-24 21:37_

Hallelujah

---

_Comment by @dcreager on 2025-04-24 21:38_

Failed test looks like it just timed out — is that spurious?

---

_Comment by @carljm on 2025-04-24 21:40_

> Failed test looks like it just timed out — is that spurious?

Yeah, we've seen this before -- need to track it down. I don't think it's related to this PR, but I'll trigger a CI re-run to check.

---

_Closed by @carljm on 2025-04-24 21:40_

---

_Reopened by @carljm on 2025-04-24 21:40_

---

_Comment by @github-actions[bot] on 2025-04-24 21:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Merged by @carljm on 2025-04-24 21:52_

---

_Closed by @carljm on 2025-04-24 21:52_

---

_Branch deleted on 2025-04-24 21:52_

---
