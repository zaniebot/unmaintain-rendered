```yaml
number: 13703
title: new rule - ban nested comprehensions
type: issue
state: open
author: DetachHead
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2024-10-10T13:16:57Z
updated_at: 2024-10-17T21:51:57Z
url: https://github.com/astral-sh/ruff/issues/13703
synced_at: 2026-01-10T11:09:55Z
```

# new rule - ban nested comprehensions

---

_Issue opened by @DetachHead on 2024-10-10 13:16_

IMO nested comprehensions are very difficult to read:

specifically, i always read them in the wrong order, i guess because when converting a regular nested for loop to a comprehension the inner-most nested part gets moved to the top, my brain subconsciously assumes the control flow is inverted and that the expression should be read bottom-up:

```py
# normal for loops:
value = []
for a in range(0, 5):        # 1
    for b in range(5, 10):   # 2
        value.append([a, b]) # 3
```
```py
# in my head i always misread them in this order:
value = [
    [a, b]                 # 3
    for a in range(0, 5)   # 2
    for b in range(5, 10)  # 1
]

# instead of the correct order which is:
value = [
    [a, b]                 # 3
    for a in range(0, 5)   # 1
    for b in range(5, 10)  # 2
]
```

i guess if a rule to ban them was introduced, it would conflict with [`PERF401`](https://docs.astral.sh/ruff/rules/manual-list-comprehension/). is it possible to have that rule change its behavior to not report on cases that would result in a nested comprehension only if this new rule is enabled?

related issues:
- #13564 
- #11936

---

_Comment by @KotlinIsland on 2024-10-10 13:18_

I think what you are looking for here is `itertools.product`, you can thank me later 😎

---

_Label `rule` added by @AlexWaygood on 2024-10-10 14:27_

---

_Label `needs-decision` added by @AlexWaygood on 2024-10-10 14:27_

---

_Comment by @lengau on 2024-10-17 16:06_

Agreed with both. If the rule could suggest replacing nested comprehensions with comprehensions using `itertools.product` that would be ideal!

---

_Comment by @DetachHead on 2024-10-17 21:51_

this example was just the first thing that came to mind for an example of a nested for loop, `itertools.product` would work in this case but there are other cases where a nested for loop is used for other purposes

---
