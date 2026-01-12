```yaml
number: 16138
title: "[red-knot] Remove a parameter from the `symbol_by_id()` query"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/dunder-slots-arg
created_at: 2025-02-13T13:22:21Z
updated_at: 2025-02-13T21:17:37Z
url: https://github.com/astral-sh/ruff/pull/16138
synced_at: 2026-01-12T15:55:53Z
```

# [red-knot] Remove a parameter from the `symbol_by_id()` query

---

_@AlexWaygood_

Salsa queries are more performant when they have fewer parameters, so this might lead to a performance improvement

---

_Label `performance` added by @AlexWaygood on 2025-02-13 13:22_

---

_Label `red-knot` added by @AlexWaygood on 2025-02-13 13:22_

---

_@MichaReiser approved on 2025-02-13 13:27_

I like this better even if it doesn't improve perf because it moves the computation closer to where it's needed and makes it more lazy. It should also remove the `symbol_table` dependency for the query calling `symbol` which is nice. 

---

_Marked ready for review by @AlexWaygood on 2025-02-13 13:28_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-13 13:28_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-13 13:28_

---

_Comment by @AlexWaygood on 2025-02-13 13:30_

no impact on perf :-( https://codspeed.io/astral-sh/ruff/branches/alex%2Fdunder-slots-arg

but yeah, as you say, a nice small change anyway :-)

---

_Label `performance` removed by @AlexWaygood on 2025-02-13 13:32_

---

_Merged by @AlexWaygood on 2025-02-13 13:33_

---

_Closed by @AlexWaygood on 2025-02-13 13:33_

---

_Branch deleted on 2025-02-13 13:33_

---

_Comment by @carljm on 2025-02-13 21:07_

> I like this better even if it doesn't improve perf because it moves the computation closer to where it's needed

Yes, I like this too.

> and makes it more lazy. It should also remove the `symbol_table` dependency for the query calling `symbol` which is nice.

But I don't think this is accurate. The outer query still needs the symbol table to get from name to symbol ID, and now we also need the symbol table again in the inner query to go from symbol ID back to name. (You can see in the diff that it doesn't remove any `symbol_table` query usages). So this introduces strictly more direct dependencies on the symbol table, and more symbol table lookups (though the added one is just an IndexVec indexing, not a hashmap lookup, which is good.)

---

_Comment by @carljm on 2025-02-13 21:17_

> no impact on perf

FWIW, I wouldn't expect removing an argument here to improve perf. The things that I'd expect to improve perf are either 1) getting it back down to just one argument (other than the db) so as to avoid the need for an extra interning of the arguments, which I don't think is possible for this query since we don't have a symbol ingredient, or 2) reducing the overall number of possible cached results from the query. Removing an argument _could_ have the latter effect in some cases, but in this case it doesn't because a given symbol always either is or is not named `__slots__`, so both before and after this diff we only cached exactly one possible result for a given scope and symbol. (That is, the value of `is_dunder_slots` was inherently locked to the value of the `symbol` argument, which is both the reason we _could_ move this calculation inside, and also the reason moving it inside doesn't actually reduce our total count of cached values.)

---
