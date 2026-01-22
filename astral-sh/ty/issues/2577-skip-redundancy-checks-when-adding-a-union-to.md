```yaml
number: 2577
title: Skip redundancy checks when adding a union to another union
type: issue
state: open
author: MichaReiser
labels:
  - help wanted
  - performance
assignees: []
created_at: 2026-01-21T13:44:59Z
updated_at: 2026-01-22T00:22:00Z
url: https://github.com/astral-sh/ty/issues/2577
synced_at: 2026-01-22T01:09:06Z
```

# Skip redundancy checks when adding a union to another union

---

_@MichaReiser_

ty normalizes its unions. It should, therefore, not be necessary to test for redundancy of elements in a union that's being added to another union. At least if both unions were built using the same context. 

I tried implementing this optimisation in https://github.com/astral-sh/ruff/pull/22071 and the result looked promising but there were some ecosystem changes that I didn't fully understand and I didn't had the time to dive deep on figuring out what's going on. 

I think we should either give this optimization another try or build an understanding of why it isn't correct.

---

_Label `help wanted` added by @MichaReiser on 2026-01-21 13:45_

---

_Label `performance` added by @MichaReiser on 2026-01-21 13:45_

---

_Comment by @carljm on 2026-01-22 00:22_

To clarify terminology: in ty, we use "normalized" to mean something more extreme, where a union has a fixed ordering and all types within it are also normalized. Unions in ty are not necessarily "normalized" by this definition -- most are not.

What is true of most unions in ty is that they are simplified -- redundant elements are removed. But this is not true of all unions: cycle normalization creates non-simplified unions (because simplification requires `has_relation_to` checks, which can then cause cycles in cycle handling...). So given that non-simplified unions can exist, I don't think this optimization is currently safe. We would have to track an extra bit in every union marking whether it had already been simplified or not.

---
