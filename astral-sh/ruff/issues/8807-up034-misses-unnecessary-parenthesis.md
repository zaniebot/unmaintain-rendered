```yaml
number: 8807
title: UP034 misses unnecessary parenthesis
type: issue
state: closed
author: chbndrhnns
labels: []
assignees: []
created_at: 2023-11-21T15:08:27Z
updated_at: 2023-11-21T21:51:00Z
url: https://github.com/astral-sh/ruff/issues/8807
synced_at: 2026-01-12T15:54:48Z
```

# UP034 misses unnecessary parenthesis

---

_@chbndrhnns_

```py
class C:
    def __init__(self, arg):
        ...

    @classmethod
    def from_env(cls):
        return cls(arg=("str"))
```

Run `ruff check --fix test.py` with such config:

```
[tool.ruff]
select = ["UP"]
```

I expect ruff to remove the parenthesis around "str":

```py
class C:
    def __init__(self, arg):
        ...

    @classmethod
    def from_env(cls):
        return cls(arg=("str"))

```

---

_Comment by @zanieb on 2023-11-21 16:20_

I don't think this is in scope for the upgrade rule as these parentheses were never necessary. Perhaps I misunderstand the intent of the rule though cc @charliermarsh  

---

_Comment by @chbndrhnns on 2023-11-21 16:25_

They were never necessary and PyCharm sometimes adds them as part of an inline refactoring. That’s what made me start looking for an existing rule in ruff.

---

_Comment by @Ezrashaw on 2023-11-21 21:02_

I'm not sure that any lint rule is relevant here. However, the ruff formatter doesn't remove those parentheses, whereas it does for:
```python
def fun():
	return (1 + 1)
```
~~It seems that the formatter doesn't format function call arguments correctly.
I'd like to have a go at this if that's ok.~~
Black also exhibits this behaviour.

---

_Comment by @zanieb on 2023-11-21 21:15_

We could consider a separate rule for removing superfluous parentheses.

---

_Comment by @charliermarsh on 2023-11-21 21:51_

I think that the last time this came up, we said we'd treat it as a request to implement Pylint's C0325: https://github.com/astral-sh/ruff/issues/2389. So let's do the same here.

---

_Closed by @charliermarsh on 2023-11-21 21:51_

---
