```yaml
number: 17192
title: "[red-knot] add cycle handling to FunctionType::signature query"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
draft: true
base: main
head: cjm/pylintcycle
created_at: 2025-04-04T00:14:29Z
updated_at: 2025-05-04T21:41:32Z
url: https://github.com/astral-sh/ruff/pull/17192
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] add cycle handling to FunctionType::signature query

---

_Pull request opened by @carljm on 2025-04-04 00:14_

This fixes cycle panics in several ecosystem projects (moved to `good.txt` here), as well as in the minimal case in the added mdtest. It also fixes a number of panicking fuzzer seeds. It doesn't appear to cause any regression in any ecosystem project or any fuzzer seed.


---

_Review requested from @AlexWaygood by @carljm on 2025-04-04 00:14_

---

_Review requested from @sharkdp by @carljm on 2025-04-04 00:14_

---

_Review requested from @dcreager by @carljm on 2025-04-04 00:14_

---

_Comment by @github-actions[bot] on 2025-04-04 00:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@MichaReiser approved on 2025-04-04 05:03_

---

_Comment by @sharkdp on 2025-04-04 07:17_

Given the abundance of different query cycles that we seem to discover 'organically', would it make sense to add regression tests for them, whenever we encounter one? It seems a bit fragile to rely on ecosystem checks to avoid accidental reintroduction of a panic?

---

_Label `red-knot` added by @AlexWaygood on 2025-04-04 07:20_

---

_@sharkdp reviewed on 2025-04-04 07:22_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/signatures.rs`:240 on 2025-04-04 07:22_

I'm curious how you came up with the name? I understand that `Never` is the bottom type, but `(…) -> Never` is a gradual type which does not participate in subtyping. Did you call it "bottom" because every arrow type is assignable to it?

---

_@carljm reviewed on 2025-04-04 13:51_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:240 on 2025-04-04 13:51_

Good catch, I actually meant for this to be a `(*args: object, **kwargs: object) -> Never` signature, which is the fully-static "bottom" Callable type (subtype of all other callable types), but wrongly used the gradual form instead. Will fix.

---

_Comment by @carljm on 2025-04-04 13:52_

> Given the abundance of different query cycles that we seem to discover 'organically', would it make sense to add regression tests for them, whenever we encounter one? It seems a bit fragile to rely on ecosystem checks to avoid accidental reintroduction of a panic?

Yes, I thought of this last night too and I agree. It requires more investigation time to narrow down the precise code construct as a minimal repro, but this is valuable to do anyway, as it can help catch cases where the cycle is not actually necessary and could be handled without fixpoint iteration, e.g. by using a more fine-grained query.

---

_Comment by @carljm on 2025-04-04 15:26_

(All of the prior cycle handling that was added was necessary in order to avoid panics on existing code samples in the corpus tests. Some of those I intend to investigate more closely, but we do at least have regression protection already for all of those.)

---

_Comment by @MichaReiser on 2025-04-28 06:59_

What's the plan here. Should we land or close this PR?

---

_Comment by @carljm on 2025-04-28 22:24_

> Should we land or close this PR?

Neither yet, still actively evaluating its ecosystem effects in light of other changes.

---

_Converted to draft by @carljm on 2025-04-28 22:24_

---

_Closed by @carljm on 2025-05-04 15:24_

---

_Reopened by @carljm on 2025-05-04 15:24_

---

_Closed by @carljm on 2025-05-04 15:32_

---

_Reopened by @carljm on 2025-05-04 15:32_

---

_Comment by @carljm on 2025-05-04 21:41_

replaced by https://github.com/astral-sh/ruff/pull/17833

---

_Closed by @carljm on 2025-05-04 21:41_

---
