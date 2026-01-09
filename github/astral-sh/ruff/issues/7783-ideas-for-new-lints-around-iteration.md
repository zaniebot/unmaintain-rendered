---
number: 7783
title: Ideas for new lints around iteration
type: issue
state: open
author: alexprengere
labels:
  - rule
  - accepted
assignees: []
created_at: 2023-10-03T15:48:53Z
updated_at: 2023-10-12T19:39:49Z
url: https://github.com/astral-sh/ruff/issues/7783
synced_at: 2026-01-07T13:12:15-06:00
---

# Ideas for new lints around iteration

---

_Issue opened by @alexprengere on 2023-10-03 15:48_

I apologize if you not looking for new lints to implement in Ruff.

I do a fair share of code reviews, and I quite often stumble on these issues which are not caught by any linter AFAICT (flake8, pylint, refurb, ruff). I am not sure how hard those would be to implement, as for the first one you would need some type inference.

```python
a = [1, 2]
b = [3, 4]

# Not good
for x in a + b:
    print(x)

# Better but not really good either
for x in *a, *b:
    print(x)

# Best
for x in itertools.chain(a, b):
    print(x)
```

This happens really often when people want to iterate on multiple lists, tuples, etc.
Another one in a similar spirit:

```python
a = [1, 2]
b = [3, 4]
c = [a, b]

# Not good
for x in itertools.chain(*c):
    print(x)

# Better
for x in itertools.chain.from_iterable(c):
    print(x)
```

---

_Comment by @charliermarsh on 2023-10-03 15:52_

Thanks! One question -- given:

```python
# Not good
for x in a + b:
    print(x)
```

When would it be _invalid_ to rewrite as `itertools.chain(a, b)`?


---

_Label `waiting-on-author` added by @charliermarsh on 2023-10-03 15:52_

---

_Comment by @alexprengere on 2023-10-03 16:24_

If you are sure `a` and `b` are lists or tuples, I think it is never invalid to rewrite.
It can be invalid on classes that would be iterable but also implement some type of addition that is not concatenation, like a numpy matrix:

```python
import numpy as np

a = b = np.matrix([[1,2], [3,4]])

for x in a + b:
    print(x)
# [[2 4]]
# [[6 8]]

for x in itertools.chain(a, b):
    print(x)
# [[1 2]]
# [[3 4]]
# [[1 2]]
# [[3 4]]
```

---

_Comment by @charliermarsh on 2023-10-03 16:26_

Thanks, that makes sense. We do have the ability to do limited within-file inference for lists, tuples, sets, and dictionaries, with the downside that the inference can lead to false-negatives (but is a reasonable fit for rules like this).

---

_Label `waiting-on-author` removed by @charliermarsh on 2023-10-03 18:49_

---

_Label `rule` added by @charliermarsh on 2023-10-03 18:49_

---

_Label `accepted` added by @charliermarsh on 2023-10-03 18:49_

---

_Comment by @Skylion007 on 2023-10-08 19:47_

flake8-simplify or flake8-comprehensions may be interested in this rule as well.

---

_Comment by @Avasam on 2023-10-12 19:39_

Idk how complete it is, but refurb added `itertool.chain` related checks lately: https://github.com/dosisod/refurb/pull/293

---
