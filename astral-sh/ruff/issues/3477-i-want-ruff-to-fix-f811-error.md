```yaml
number: 3477
title: "I want ruff to fix `F811` error."
type: issue
state: closed
author: yuji38kwmt
labels:
  - fixes
assignees: []
created_at: 2023-03-13T04:17:03Z
updated_at: 2024-01-09T00:29:22Z
url: https://github.com/astral-sh/ruff/issues/3477
synced_at: 2026-01-10T11:09:46Z
```

# I want ruff to fix `F811` error.

---

_Issue opened by @yuji38kwmt on 2023-03-13 04:17_



# Environments

```
$ uname -a
Linux example 5.15.0-60-generic #66-Ubuntu SMP Fri Jan 20 14:29:49 UTC 2023 x86_64 x86_64 x86_64 GNU/Linux
$ python --version
Python 3.11.2
$ ruff --version
ruff 0.0.254
```

# What's happened?
I want to fix the following code.

```
import pandas
import pandas
df = pandas.Series(["a", "b", "c"])
```

But ruff did not fix line 1.

```
$ ruff sample.py  --fix --isolated
sample.py:2:8: F811 Redefinition of unused `pandas` from line 1
Found 1 error.
```

# What do I expect?
I expect that ruff fix `F811` error.


---

_Comment by @charliermarsh on 2023-03-13 18:46_

We used to fix this, but it's not always safe and can be hard to get right. Not in this trivial case, but, e.g., see #2044.

---

_Label `autofix` added by @charliermarsh on 2023-03-13 18:46_

---

_Comment by @yuji38kwmt on 2023-03-14 01:03_

Thanks! I Got it.

---

_Comment by @charliermarsh on 2023-03-18 04:08_

I think we should consider fixing these in the event that _both_ are imports.

---

_Comment by @zanieb on 2023-05-04 15:46_

I definitely agree these should be fixed if both are imports — in my move from `isort` to `ruff` this is one of the biggest changes to my workflow. For example, when resolving import merge conflicts (which are common) I usually just accept both changes then rely on the linter to resolve duplicate and unused imports.

Does this seem like a straight-forward feature to implement? Happy to give it a go with some guidance.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-07 01:03_

---

_Closed by @charliermarsh on 2024-01-09 00:29_

---
