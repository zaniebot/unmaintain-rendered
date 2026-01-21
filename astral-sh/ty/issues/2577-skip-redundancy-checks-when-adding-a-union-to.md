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
updated_at: 2026-01-21T13:44:59Z
url: https://github.com/astral-sh/ty/issues/2577
synced_at: 2026-01-21T14:07:04Z
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
