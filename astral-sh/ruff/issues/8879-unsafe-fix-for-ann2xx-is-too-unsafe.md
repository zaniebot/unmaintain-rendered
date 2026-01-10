```yaml
number: 8879
title: "Unsafe fix for `ANN2XX` is too unsafe"
type: issue
state: closed
author: tylerlaprade
labels:
  - bug
assignees: []
created_at: 2023-11-28T20:01:59Z
updated_at: 2023-11-28T21:10:44Z
url: https://github.com/astral-sh/ruff/issues/8879
synced_at: 2026-01-10T11:09:51Z
```

# Unsafe fix for `ANN2XX` is too unsafe

---

_Issue opened by @tylerlaprade on 2023-11-28 20:01_

```
def do_thing(thing: Thing | None):
    if not thing:
        return None
    return { 'foo': 1 }
```
The (unsafe) autofix adds ` -> None` as the return type. I know this is under "unsafe" fixes, but this should be easy enough to avoid. https://github.com/JelleZijlstra/autotyping handles this case, so maybe that logic can be reused.

---

_Comment by @zanieb on 2023-11-28 20:05_

```python
Thing = dict


def do_thing(thing: Thing | None):
    if not thing:
        return None
    return {"foo": 1}
```
```
❯ ruff check example.py --select ANN
example.py:4:5: ANN201 Missing return type annotation for public function `do_thing`
Found 1 error.
```
```
❯ ruff version
ruff 0.1.6 (f460f9c5c 2023-11-17)
```

I don't get a suggested fix for this example. Could you provide more reproduction details?

---

_Comment by @charliermarsh on 2023-11-28 20:18_

I can reproduce this with preview enabled -- I think it's just a bug.

---

_Label `bug` added by @charliermarsh on 2023-11-28 20:18_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-28 20:18_

---

_Comment by @charliermarsh on 2023-11-28 20:37_

Huh, I can reproduce this in the playground but not locally.

---

_Comment by @charliermarsh on 2023-11-28 20:38_

Oh, you need to have the target Python version set to >= 3.10. Okay. I'm on it.

---

_Closed by @charliermarsh on 2023-11-28 21:10_

---
