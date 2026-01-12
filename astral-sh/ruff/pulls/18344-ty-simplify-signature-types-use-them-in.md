```yaml
number: 18344
title: "[ty] Simplify signature types, use them in `CallableType`"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/dunder-call
created_at: 2025-05-27T19:50:01Z
updated_at: 2025-05-28T17:11:47Z
url: https://github.com/astral-sh/ruff/pull/18344
synced_at: 2026-01-12T15:56:17Z
```

# [ty] Simplify signature types, use them in `CallableType`

---

_@dcreager_


There were many fields in `Signature` and friends that really had more to do with how a signature was being _used_ — how it was looked up, details about an individual call site, etc. Those fields more properly belong in `Bindings` and friends.

This is a pure refactoring, and should not affect any tests or ecosystem projects.

I started on this journey in support of https://github.com/astral-sh/ty/issues/462. It seemed worth pulling out as a separate PR.

One major concrete benefit of this refactoring is that we can now use `CallableSignature` directly in `CallableType`. (We can't use `CallableSignature` directly in that `Type` variant because signatures are not currently interned.)

---

_Review requested from @carljm by @dcreager on 2025-05-27 19:50_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-27 19:50_

---

_Label `internal` added by @dcreager on 2025-05-27 19:50_

---

_Review requested from @sharkdp by @dcreager on 2025-05-27 19:50_

---

_Label `ty` added by @dcreager on 2025-05-27 19:50_

---

_Comment by @dcreager on 2025-05-27 19:50_

/cc @dhruvmanila re the `CallableType` changes

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1 on 2025-05-27 19:51_

The diff of this file is less chatty if you hide whitespace

---

_Comment by @github-actions[bot] on 2025-05-27 19:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@dcreager reviewed on 2025-05-27 19:54_

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:3516 on 2025-05-28 04:59_

nit: It might be useful to provide a brief description on what "an empty Bindings" mean here as I don't think it's about an empty `Bindings::elements` but more about the fact that this is kind of an "initial" state before matching any parameters to the arguments and doing type checking.

---

_@dhruvmanila approved on 2025-05-28 05:07_

Nice! This looks reasonable to me 👍 

---

_@dcreager reviewed on 2025-05-28 15:53_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:3516 on 2025-05-28 15:53_

Done! By "empty" I meant that you have to do more work to actually analyze a call site using the `Bindings` you get back. I reworded the comment and added some more detail to explain that better.

---

_Merged by @dcreager on 2025-05-28 17:11_

---

_Closed by @dcreager on 2025-05-28 17:11_

---

_Branch deleted on 2025-05-28 17:11_

---
