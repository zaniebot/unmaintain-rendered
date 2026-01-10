---
number: 1164
title: "Should `top_materialization` use `default_specialization` for type vars?"
type: issue
state: open
author: sharkdp
labels:
  - type properties
assignees: []
created_at: 2025-09-10T14:58:55Z
updated_at: 2026-01-07T00:13:49Z
url: https://github.com/astral-sh/ty/issues/1164
synced_at: 2026-01-10T01:51:14Z
---

# Should `top_materialization` use `default_specialization` for type vars?

---

_Issue opened by @sharkdp on 2025-09-10 14:58_

@AlexWaygood:

> I wondered if `class.top_materialization()` would work here, but it looks like under the hood that uses `default_specialization()`, so it suffers from the same issue as our current logic on `main`. (It probably _shouldn't_ use `default_specialization()`?)

@dcreager

> I think you're right — it should translate typevars into the bound/constraints in covariant position, and to `Never` (the implicit lower bound of every typevar) in contravariant position. (With the caveat that we can't really express the constraints of a constrained typevar as a single type — we'd need a "one-of" connective instead of union, and an "instance of this type but not any subtypes".)

_Originally posted by @AlexWaygood in https://github.com/astral-sh/ruff/pull/20325#discussion_r2336862128_
            

---

_Label `type properties` added by @sharkdp on 2025-09-10 15:00_

---

_Comment by @AlexWaygood on 2026-01-06 23:19_

This came up in https://github.com/astral-sh/ty/issues/2356, so I'm bumping the priority 

---

_Added to milestone `Pre-stable 1` by @AlexWaygood on 2026-01-06 23:19_

---

_Comment by @carljm on 2026-01-07 00:13_

I think this belongs in stable, and it seems not-very-hard, so I'd have no problem doing it soon -- but in general I don't think one bug report necessarily warrants bumping priority into pre-stable 1. Most of the issues there are things we've gotten many, many bug reports about. (Or else they are e.g. fatals.)

---

_Removed from milestone `Pre-stable 1` by @carljm on 2026-01-07 00:13_

---

_Added to milestone `Stable` by @carljm on 2026-01-07 00:13_

---
