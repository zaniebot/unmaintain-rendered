---
number: 6056
title: PERF401 (use-list-comprehension) with walrus operator
type: issue
state: closed
author: fabien-michel
labels: []
assignees: []
created_at: 2023-07-25T07:56:52Z
updated_at: 2023-07-25T10:59:09Z
url: https://github.com/astral-sh/ruff/issues/6056
synced_at: 2026-01-07T13:12:15-06:00
---

# PERF401 (use-list-comprehension) with walrus operator

---

_Issue opened by @fabien-michel on 2023-07-25 07:56_

I'm not sure the PERF401 should trigger when a walrus operator is implied in the condition.

The warning appear for this code.
```python
def odd_only(a):
    if a % 2:
        return a
    return False

filtered = []
for a in range(1, 5):
    if b := odd_only(a):
        filtered.append(b)
```
Which should be changed to
```python
filtered = [b for a in range(1, 5) if (b := odd_only(a))]
```
I'm found this kind of code not clear, because of the walrus operator.

The workaround is to not use the walrus operator:
```python
filtered = []
for a in range(1, 5):
    b = odd_only(a)
    if b:
        filtered.append(b)
```
So, as there is an equivalence, and the warn is not triggered, it should also not warn when a walrus operator is used.

The same behavior occur with perflint, I submitted the same bug: https://github.com/tonybaloney/perflint/issues/43

---

_Comment by @tjkuson on 2023-07-25 10:35_

I am not sure `use-list-comprehension` should ignore the walrus operator; my understanding of [PEP 572](https://peps.python.org/pep-0572/) is that use in comprehensions is an explicit motivation for the assignment operator. 

---

_Comment by @fabien-michel on 2023-07-25 10:47_

You are true, sorry for the noise. Personally, I find this type of writing difficult to read.

---

_Closed by @fabien-michel on 2023-07-25 10:47_

---

_Comment by @tjkuson on 2023-07-25 10:56_

No need to apologise! Many Python developers don't like the walrus operator, so I think the issue is entirely reasonable. Personally, I like them. However, there's probably a compelling reason to implement something like [flake8-walrus](https://github.com/asottile/flake8-walrus) in a hypothetical Ruff category that prevents the use of language and library features (similar to `clippy::restriction`).

---

_Referenced in [astral-sh/ruff#7418](../../astral-sh/ruff/issues/7418.md) on 2024-10-03 23:06_

---

_Referenced in [astral-sh/ruff#13621](../../astral-sh/ruff/issues/13621.md) on 2024-10-03 23:13_

---
