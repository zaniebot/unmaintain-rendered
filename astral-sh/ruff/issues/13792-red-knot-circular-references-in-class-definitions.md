```yaml
number: 13792
title: "[red-knot] circular references in class definitions panic"
type: issue
state: closed
author: rtpg
labels:
  - bug
  - ty
assignees: []
created_at: 2024-10-17T11:15:50Z
updated_at: 2025-03-12T12:41:42Z
url: https://github.com/astral-sh/ruff/issues/13792
synced_at: 2026-01-10T11:09:55Z
```

# [red-knot] circular references in class definitions panic

---

_Issue opened by @rtpg on 2024-10-17 11:15_

```
class C(list[C]): ...
```
the above code causes `red_knot` to panic, due to something a bit subtle:

- We query for the type of `C` via `infer_definition_types`
- during this process we infer the type of `list[C]`
- during that process we (correctly!) annotate the type of `list` and stuff it in the inference expressions
- We then try to get the type of `C`... and call `infer_definition_types`

This leads to `C` being infered of type `Unknown` thanks to `infer_definition_types_cycle_recover`, which is fine and great... but due to something I don't quite understand, the result of `infer_definition_types_cycle_recover` ends up being the _final_ definition of `infer_definition_types(C)`.

As a result we lose the inference on `list`! So later linting code that traverses the AST tries to look up the type of the `list` expression and blows up.

My understanding here is that the queries are like:

```
infer_definition_types(C) -> infer_definition_types(C)
--------------------------------------------------------
infer_definition_types(C) -> [C: Unknown]
--------------------------------------------------------
[C: ClassType (base: Unknown), expressions: {list} ]
```

_but_ that  future queries to the DB for `infer_defintion_types(C)` ends up returning `[C: Unknown]`. I have been reading the docs on salsa cycling a bit on this point to try and decipher what is going on here but maybe somebody else would have a better understanding of what can be done here.


---

_Comment by @MichaReiser on 2024-10-17 11:20_

@carljm's working on fixpoint iteration that will allow cyclic dependencies

---

_Label `red-knot` added by @MichaReiser on 2024-10-17 11:20_

---

_Comment by @rtpg on 2024-10-17 12:19_

Alright, good to know! I had this intuition that I didn't need general fixed point iteration (just not caching the cycle recovery result for definitions), but trying to `db.report_untracked_read` my way out of this got me nowhere.

Will studiously wait on this one for the adults to show up with the principled solution when the time is right.

---

_Comment by @carljm on 2024-10-17 14:35_

Salsa's current cycle handling intentionally sets the "cycle fallback" result as the result for _every_ query in the cycle, not just the single query that finally triggered the cycle. This is done for determinism reasons (so that results don't change depending on where you happen to enter a cycle.) I think this behavior explains the result you're seeing.

For the code you posted, I would expect it to work in a stub file (where class bases are deferred) but not in a regular Python file, where they are eagerly evaluated.

Salsa's current cycle recovery is not very useful for us, which is why I'm working on the fixpoint iteration feature, which we definitely need, for example, for evaluating types in loopy control flow graphs.

---

_Added to milestone `Red Knot 2024` by @carljm on 2024-11-07 15:23_

---

_Label `bug` added by @AlexWaygood on 2024-11-09 13:01_

---

_Removed from milestone `Red Knot 2024` by @carljm on 2025-01-09 17:52_

---

_Added to milestone `Red Knot Q1 2025` by @carljm on 2025-01-09 17:52_

---

_Assigned to @carljm by @carljm on 2025-03-01 17:26_

---

_Closed by @carljm on 2025-03-12 12:41_

---

_Closed by @carljm on 2025-03-12 12:41_

---
