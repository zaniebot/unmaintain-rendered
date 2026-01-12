```yaml
number: 9648
title: F821 false positive in notebook when using magic command as variables
type: issue
state: closed
author: mathause
labels:
  - bug
assignees: []
created_at: 2024-01-26T14:14:50Z
updated_at: 2024-01-29T13:44:34Z
url: https://github.com/astral-sh/ruff/issues/9648
synced_at: 2026-01-12T15:54:49Z
```

# F821 false positive in notebook when using magic command as variables

---

_@mathause_

ruff gives false positives for F821 (Undefined name) in notebooks when using magic command as variables. Ok, this is _very_ specific and potentially related to #9644.

In a jupyter notebook when defining a variable which has the same name as a ipython magic command and using this variable in a following cell and not as the first command triggers F821 (wrongly).

Only classify magic commands as magic if they are preceded with a `%`?

**command**

```bash
ruff check --no-cache --select F821 test.ipynb
```

**version**

```
ruff --version
ruff 0.1.14
```

**MVCE**

(I am not sure how to create a MVCE for a jupyter notebook, but I guess it's easy enough...)


![image](https://github.com/astral-sh/ruff/assets/10194086/b81030dd-fd97-4369-912b-b336b699923b)







---

_Comment by @charliermarsh on 2024-01-26 14:21_

Yeah we can probably improve our heuristics here to specifically detect assignments. The automagics are actually really complicated because it means that (in general) cells may or may not be unsafe depending on the order of their execution (which we can't know). For example: https://github.com/astral-sh/ruff/pull/8398#issuecomment-1790345709.

---

_Comment by @charliermarsh on 2024-01-26 14:23_

I'll try to fix this specific case today :)

---

_Label `bug` added by @charliermarsh on 2024-01-26 14:23_

---

_Comment by @charliermarsh on 2024-01-26 14:24_

But I need to audit the list of automagics which I dread :joy:

---

_Assigned to @charliermarsh by @charliermarsh on 2024-01-26 14:54_

---

_Comment by @charliermarsh on 2024-01-26 16:39_

Fixed in https://github.com/astral-sh/ruff/pull/9653.

---

_Closed by @charliermarsh on 2024-01-29 12:55_

---

_Comment by @mathause on 2024-01-29 13:00_

Thanks!

---

_Comment by @charliermarsh on 2024-01-29 13:44_

Of course! If you see more of these, let us know, since we have to rely on some heuristics for this given the flexibility of notebooks.

---
