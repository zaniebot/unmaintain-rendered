```yaml
number: 15447
title: New rule for factoring out boolean
type: issue
state: open
author: nickdrozd
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-01-13T00:59:49Z
updated_at: 2025-01-26T10:16:57Z
url: https://github.com/astral-sh/ruff/issues/15447
synced_at: 2026-01-10T11:09:56Z
```

# New rule for factoring out boolean

---

_Issue opened by @nickdrozd on 2025-01-13 00:59_

New rule to distribute `and` across `or` and vice versa.

- `(p and q) or (p and r)` simplifies to `p and (q or r)`
- `(p or q) and (p or r)` simplifies to `p or (q and r)`

This should apply for longer sequences as well, so that

```python
(p and q_1 and ... and q_n) or (p and r_1 and ... and r_n)
```

simplifies to

```python
p and ((q_1 and ... and q_n) or (r_1 and ... and r_n))
```

I guess it would be SIM category? This would be useful in conjunction with the other SIM rules, and also for something like https://github.com/astral-sh/ruff/issues/14636.

I think it is autofixable.

---

_Label `rule` added by @charliermarsh on 2025-01-13 01:52_

---

_Label `needs-decision` added by @charliermarsh on 2025-01-13 01:52_

---

_Renamed from "New rule for boolean distribution" to "New rule for factoring out boolean" by @dylwil3 on 2025-01-15 01:51_

---

_Comment by @VascoSch92 on 2025-01-25 22:26_

Hey,
Is this rule still on the to-do list?
Can I give it a shot? 😊

---

_Comment by @MichaReiser on 2025-01-26 10:16_

Thanks for the interest. We first need to decide on the exact semantics and if the rule fits into ruffs rule set

---
