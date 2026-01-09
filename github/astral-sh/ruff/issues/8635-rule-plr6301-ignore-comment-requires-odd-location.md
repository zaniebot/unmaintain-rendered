---
number: 8635
title: "Rule `PLR6301` ignore comment requires odd location"
type: issue
state: closed
author: ofek
labels:
  - bug
assignees: []
created_at: 2023-11-12T20:56:59Z
updated_at: 2023-11-12T21:39:40Z
url: https://github.com/astral-sh/ruff/issues/8635
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule `PLR6301` ignore comment requires odd location

---

_Issue opened by @ofek on 2023-11-12 20:56_

```
Method `construct_build_command` could be a function, class method, or static method
```

The following is the proper way to ignore `PLR6301` when the function/method spans multiple lines:

```python
def construct_build_command(
    self,  # noqa: PLR6301
    *,
    directory=None,
    targets=(),
    hooks_only=False,
    no_hooks=False,
    clean=False,
    clean_hooks_after=False,
    clean_only=False,
):
```

Should it not be the case that ignore comments be on the method?

---

_Comment by @charliermarsh on 2023-11-12 21:01_

Yeah this seems off. I think it's flagging the `self`, but we should probably move it to the method name.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-12 21:01_

---

_Label `bug` added by @charliermarsh on 2023-11-12 21:04_

---

_Referenced in [astral-sh/ruff#8637](../../astral-sh/ruff/pulls/8637.md) on 2023-11-12 21:07_

---

_Closed by @charliermarsh on 2023-11-12 21:37_

---

_Comment by @ofek on 2023-11-12 21:39_

Thanks!

---
