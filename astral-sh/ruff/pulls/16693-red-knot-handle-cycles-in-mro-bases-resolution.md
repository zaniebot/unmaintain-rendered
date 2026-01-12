```yaml
number: 16693
title: "[red-knot] handle cycles in MRO/bases resolution"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/mrocycle
created_at: 2025-03-13T01:03:56Z
updated_at: 2025-03-13T15:16:05Z
url: https://github.com/astral-sh/ruff/pull/16693
synced_at: 2026-01-12T15:55:56Z
```

# [red-knot] handle cycles in MRO/bases resolution

---

_@carljm_

There can be semi-cyclic inheritance patterns (e.g. recursive generics) that are not technically inheritance cycles, but that can cause us to hit Salsa query cycles in evaluating a type's MRO. Add fixed-point handling to these MRO-related queries so we don't panic on these cycles.

The details of what queries we hit in what order in this case will change as we implement support for generics, but ultimately we will probably need cycle handling for all queries that can re-enter type inference, otherwise we are susceptible to small changes in query execution order causing panics.

Fixes astral-sh/ruff#14333
Further reduces the panicking set of seeds in astral-sh/ty#228


---

_Label `red-knot` added by @carljm on 2025-03-13 01:03_

---

_Review requested from @MichaReiser by @carljm on 2025-03-13 01:03_

---

_Review requested from @AlexWaygood by @carljm on 2025-03-13 01:03_

---

_Review requested from @sharkdp by @carljm on 2025-03-13 01:03_

---

_Comment by @github-actions[bot] on 2025-03-13 01:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Comment by @carljm on 2025-03-13 01:22_

Actually, after looking at this a bit more, I'm not yet convinced this is the right fix. Investigating a bit more.

---

_Converted to draft by @carljm on 2025-03-13 01:22_

---

_Marked ready for review by @carljm on 2025-03-13 02:38_

---

_Comment by @carljm on 2025-03-13 02:38_

Ok, I do think this is right, re-submitting for review.

---

_@MichaReiser approved on 2025-03-13 08:48_

I don't mind all the added tracing calls but I'm curious how verbose it becomes. But we can always remove them in the future if needed. I can definetely see how they're useful and using `KNOT_LOG` can help to narrow down the scope of logs

---

_Comment by @carljm on 2025-03-13 15:15_

The particular reason these tracing calls are important is that Salsa's behavior with queries that are methods is currently pretty unfriendly: the queries are all named `inner_fn_name_` and are very difficult to distinguish from each other in Salsa traces. This is probably something that can be improved in Salsa, and then these traces in red-knot would be less necessary.

---

_Merged by @carljm on 2025-03-13 15:16_

---

_Closed by @carljm on 2025-03-13 15:16_

---

_Branch deleted on 2025-03-13 15:16_

---
