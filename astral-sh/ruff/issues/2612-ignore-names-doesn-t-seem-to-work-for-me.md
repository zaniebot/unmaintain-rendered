```yaml
number: 2612
title: "`ignore-names` doesn't seem to work for me"
type: issue
state: closed
author: tmke8
labels:
  - bug
assignees: []
created_at: 2023-02-06T22:17:57Z
updated_at: 2023-02-07T02:14:56Z
url: https://github.com/astral-sh/ruff/issues/2612
synced_at: 2026-01-10T11:09:45Z
```

# `ignore-names` doesn't seem to work for me

---

_Issue opened by @tmke8 on 2023-02-06 22:17_

I have code similar to this:
```python
def f(C: float) -> None:
    print(f"cost parameter: {C}")
```
And I tried to exclude `C` from the naming check with
```toml
[tool.ruff.pep8-naming]
ignore-names = ["C"]
```
But I still get
```
naming_test.py:1:7: N803 Argument name `C` should be lowercase
Found 1 error.
```

I might be doing something wrong, but I can't figure out what.

---

_Comment by @charliermarsh on 2023-02-07 00:43_

It looks like we're only checking this for `N802`. Might just be an oversight? I can fix it.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-02-07 00:44_

---

_Label `bug` added by @charliermarsh on 2023-02-07 00:44_

---

_Closed by @charliermarsh on 2023-02-07 02:14_

---
