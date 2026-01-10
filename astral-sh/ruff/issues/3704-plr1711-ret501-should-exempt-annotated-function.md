---
number: 3704
title: "PLR1711, RET501 should exempt annotated function where return type isn’t `-> None`"
type: issue
state: closed
author: andersk
labels:
  - bug
assignees: []
created_at: 2023-03-24T00:10:38Z
updated_at: 2023-03-24T03:12:00Z
url: https://github.com/astral-sh/ruff/issues/3704
synced_at: 2026-01-10T01:22:42Z
---

# PLR1711, RET501 should exempt annotated function where return type isn’t `-> None`

---

_Issue opened by @andersk on 2023-03-24 00:10_

Ruff flags PLR1711 useless-return and RET501 unnecessary-return-none in this code:

```python
class BaseCache:
    def get(self, key: str) -> str | None:
        print(f"{key} not found")
        return None
```

```
test.py:4:9: RET501 [*] Do not explicitly `return None` in function if it is the only possible return value
test.py:4:9: PLR1711 [*] Useless `return` statement at end of function
```

But mypy complains if `return None` is removed:

```
test.py:2: error: Missing return statement  [return]
```

or if `return None` is changed to `return`:

```
test.py:4: error: Return value expected  [return-value]
```

mypy can be appeased by changing the return type annotation to `-> None`, and that’s okay in many cases (e.g. `Callable[..., None]` covariantly coerces to `Callable[..., str | None`), but it’s not without problems—for example, it prevents a derived class from overriding the method to return a `str`.

So I think RET501 and PLR1711 should exempt annotated functions with a return type other than `-> None`.

---

_Comment by @charliermarsh on 2023-03-24 00:14_

I agree.

---

_Label `bug` added by @charliermarsh on 2023-03-24 00:14_

---

_Comment by @JonathanPlasse on 2023-03-24 00:52_

I would like to work on it.

---

_Referenced in [astral-sh/ruff#3705](../../astral-sh/ruff/pulls/3705.md) on 2023-03-24 01:34_

---

_Closed by @charliermarsh on 2023-03-24 03:12_

---

_Referenced in [astral-sh/ruff#8212](../../astral-sh/ruff/pulls/8212.md) on 2023-10-25 12:33_

---

_Referenced in [astral-sh/ruff#12198](../../astral-sh/ruff/issues/12198.md) on 2024-07-05 07:39_

---

_Referenced in [astral-sh/ruff#14377](../../astral-sh/ruff/issues/14377.md) on 2024-11-16 16:21_

---
