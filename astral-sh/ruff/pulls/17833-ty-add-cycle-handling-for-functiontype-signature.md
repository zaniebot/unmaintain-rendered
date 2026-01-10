```yaml
number: 17833
title: "[ty] add cycle handling for FunctionType::signature query"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/signaturecycle
created_at: 2025-05-04T15:46:51Z
updated_at: 2025-05-05T19:12:40Z
url: https://github.com/astral-sh/ruff/pull/17833
synced_at: 2026-01-10T18:57:03Z
```

# [ty] add cycle handling for FunctionType::signature query

---

_Pull request opened by @carljm on 2025-05-04 15:46_

This fixes cycle panics in several ecosystem projects (moved to `good.txt` in a following PR https://github.com/astral-sh/ruff/pull/17834 because our mypy-primer job doesn't handle it well if we move projects to `good.txt` in the same PR that fixes `ty` to handle them), as well as in the minimal case in the added mdtest. It also fixes a number of panicking fuzzer seeds. It doesn't appear to cause any regression in any ecosystem project or any fuzzer seed.


---

_Label `ty` added by @carljm on 2025-05-04 15:46_

---

_Renamed from "[red-knot] add cycle handling for FunctionType::signature query" to "[ty] add cycle handling for FunctionType::signature query" by @carljm on 2025-05-04 19:24_

---

_Comment by @github-actions[bot] on 2025-05-04 19:24_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @carljm on 2025-05-04 19:28_

---

_Review requested from @AlexWaygood by @carljm on 2025-05-04 19:28_

---

_Review requested from @sharkdp by @carljm on 2025-05-04 19:28_

---

_Review requested from @dcreager by @carljm on 2025-05-04 19:28_

---

_Converted to draft by @carljm on 2025-05-04 19:35_

---

_Marked ready for review by @carljm on 2025-05-04 19:52_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types/signatures.rs`:295 on 2025-05-04 21:46_

Can you also update the `CallableType::bottom` to create the `Signature` using this method now?

https://github.com/astral-sh/ruff/blob/main/crates/ty_python_semantic/src/types.rs#L7065

We could also delegate the construction to `Signature::new` to avoid providing parameters other than `signature` and `return_ty`



---

_@sharkdp reviewed on 2025-05-05 07:20_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6870 on 2025-05-05 07:20_

Is it correct that basically any initial answer here would solve the self-referential "Function signature" example from `cycle.md` (because after one cycle, we notice that an expression of type function-literal is invalid in this position)? If so, do we anticipate that the bottom signature is the correct/better answer for other cases?

---

_@sharkdp approved on 2025-05-05 07:21_

Thank you

---

_@dhruvmanila reviewed on 2025-05-05 18:48_

---

_@carljm reviewed on 2025-05-05 18:57_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6870 on 2025-05-05 18:57_

> Is it correct that basically any initial answer here would solve the self-referential "Function signature" example from `cycle.md` (because after one cycle, we notice that an expression of type function-literal is invalid in this position)?

Yes

> If so, do we anticipate that the bottom signature is the correct/better answer for other cases?

I haven't been able to come up with (or seen in ecosystem) a test case in which it matters, so I went with the "bottom" signature type simply because it parallels our use of `Never` for fixpoint iterating on types. Do you see advantages to using a different signature here?

---

_@carljm reviewed on 2025-05-05 19:01_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/signatures.rs`:295 on 2025-05-05 19:01_

Good suggestions, thank you! Somehow I missed the existence of `CallableType::bottom`

---

_@sharkdp reviewed on 2025-05-05 19:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6870 on 2025-05-05 19:03_

> Do you see advantages to using a different signature here?

No — just curious. Thanks!

---

_Merged by @carljm on 2025-05-05 19:12_

---

_Closed by @carljm on 2025-05-05 19:12_

---

_Branch deleted on 2025-05-05 19:12_

---
