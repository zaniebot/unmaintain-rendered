```yaml
number: 1809
title: SIM401 should not apply to non-concrete default values
type: issue
state: closed
author: thomasdesr
labels:
  - bug
assignees: []
created_at: 2023-01-12T07:16:25Z
updated_at: 2023-01-18T16:43:42Z
url: https://github.com/astral-sh/ruff/issues/1809
synced_at: 2026-01-10T11:09:44Z
```

# SIM401 should not apply to non-concrete default values

---

_Issue opened by @thomasdesr on 2023-01-12 07:16_

Thanks again for Ruff ❤️ 

As of `ruff==0.0.219` `SIM401` seems to be incorrectly applied when the default value is not a constant value.

```python
import asyncio


async def some_async_func():
    await asyncio.sleep(10)


async def foo():
    d = {"key": "value"}
    key = "key"

    if key in d:
        value = d[key]
    else:
        value = await some_async_func()

    return value
```

Believes it can be simplified (& maybe more dangerously will be auto-fixed) to:
```python
import asyncio


async def some_async_func():
    await asyncio.sleep(10)


async def foo():
    d = {"key": "value"}
    key = "key"

    value = d.get(key, await some_async_func())

    return value

```

However these two are not interchangeable as `some_async_func()` may be arbitrarily expensive as demonstrated here by the `asyncio.sleep(10)`


Note: This is also a bug in upstream flake8-simplify https://github.com/MartinThoma/flake8-simplify/issues/166

---

_Comment by @charliermarsh on 2023-01-12 12:12_

Ahh, good catch! We can fix this.

---

_Label `bug` added by @charliermarsh on 2023-01-12 12:12_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-01-12 15:42_

---

_Closed by @charliermarsh on 2023-01-12 17:51_

---

_Comment by @thomasdesr on 2023-01-12 19:37_

Thanks!

---

_Comment by @charliermarsh on 2023-01-12 19:38_

I went off some heuristics (ignore any expressions containing function calls, awaits, etc.) — let me know if it misses anything going forward.

---

_Comment by @thomasdesr on 2023-01-13 08:06_

They look like sufficient for my code base! Thanks so much :D

---

_Comment by @andersk on 2023-01-18 16:36_

We should also ignore subscripting, since it might throw `KeyError`. For example, this fix is wrong:

```diff
-             if "title" in fileinfo: 
-                 file_name = fileinfo["title"] 
-             else: 
-                 file_name = fileinfo["name"] 
+             file_name = fileinfo.get("title", fileinfo["name"])
```

---

_Comment by @charliermarsh on 2023-01-18 16:43_

Makes sense, I can fix that.

---
