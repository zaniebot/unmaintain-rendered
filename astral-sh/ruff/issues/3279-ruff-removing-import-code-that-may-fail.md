---
number: 3279
title: Ruff removing import code, that may fail
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-02-28T19:38:39Z
updated_at: 2023-03-01T23:08:38Z
url: https://github.com/astral-sh/ruff/issues/3279
synced_at: 2026-01-10T01:22:41Z
---

# Ruff removing import code, that may fail

---

_Issue opened by @qarmin on 2023-02-28 19:38_

ruff 0.0.253

Unused import inside try and except block which catch ModuleNotFoundError error, should not be removed(`ruff check . --fix` do this, without any config file)
```
     @staticmethod
     def check_orjson():
         try:
-            import orjson
             return True
         except ModuleNotFoundError:
             return False
```


---

_Comment by @charliermarsh on 2023-02-28 19:40_

Interesting, this is arguably correct but I can see why we'd want to avoid this -- the `ModuleNotFoundError` is arguably a "usage" of the import.

---

_Label `bug` added by @charliermarsh on 2023-02-28 19:40_

---

_Comment by @JonathanPlasse on 2023-02-28 20:45_

You should maybe use [`importlib.util.find_spec`](https://docs.python.org/3/library/importlib.html#importlib.util.find_spec) instead and check if the return is not `None`.

---

_Referenced in [astral-sh/ruff#3288](../../astral-sh/ruff/pulls/3288.md) on 2023-03-01 04:25_

---

_Comment by @qarmin on 2023-03-01 05:39_

I found this when trying to add basic ruff support for Bottles app, in my project I only once ever used such code.

When looking at https://github.com/search?l=Python&p=1&q=ModuleNotFoundError&type=Code, I see that this is used a lot

---

_Closed by @charliermarsh on 2023-03-01 23:08_

---

_Referenced in [astral-sh/ruff#3378](../../astral-sh/ruff/issues/3378.md) on 2023-03-07 03:50_

---
