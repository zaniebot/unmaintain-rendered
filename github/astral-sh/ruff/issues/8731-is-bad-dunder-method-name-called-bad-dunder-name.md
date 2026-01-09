---
number: 8731
title: "(🐞) is `bad-dunder-method-name` called `bad-dunder-name`?"
type: issue
state: closed
author: KotlinIsland
labels:
  - documentation
assignees: []
created_at: 2023-11-17T01:07:49Z
updated_at: 2023-11-17T17:26:31Z
url: https://github.com/astral-sh/ruff/issues/8731
synced_at: 2026-01-07T13:12:15-06:00
---

# (🐞) is `bad-dunder-method-name` called `bad-dunder-name`?

---

_Issue opened by @KotlinIsland on 2023-11-17 01:07_

```py
class A:
    def __among_us__(self):  # PLW3201: Bad or misspelled dunder method name `__among_us__`. (bad-dunder-name)
        ...
```
but in the docs it's called `bad-dunder-method-name`
https://docs.astral.sh/ruff/rules/bad-dunder-method-name/


---

_Label `documentation` added by @zanieb on 2023-11-17 14:13_

---

_Comment by @charliermarsh on 2023-11-17 14:34_

We use `bad-dunder-method-name` and I believe it's intentional -- I think Pylint uses `bad-dunder-name`, but we made the name more specific.

---

_Closed by @charliermarsh on 2023-11-17 14:34_

---

_Reopened by @charliermarsh on 2023-11-17 14:34_

---

_Comment by @charliermarsh on 2023-11-17 14:35_

Ohh I didn't realize we were including this in the rule message. Yes, that's a bug, thanks!

---

_Referenced in [astral-sh/ruff#8742](../../astral-sh/ruff/pulls/8742.md) on 2023-11-17 17:21_

---

_Closed by @charliermarsh on 2023-11-17 17:26_

---
