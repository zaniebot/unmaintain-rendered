---
number: 401
title: "`noqa` directives for `pydocstyle` checks should be placed on the annotated line"
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2022-10-11T16:40:17Z
updated_at: 2022-10-11T16:56:41Z
url: https://github.com/astral-sh/ruff/issues/401
synced_at: 2026-01-07T13:12:14-06:00
---

# `noqa` directives for `pydocstyle` checks should be placed on the annotated line

---

_Issue opened by @charliermarsh on 2022-10-11 16:40_

For example, notice that the `noqa` is on the `def` line, not the docstring itself:

```py
def bad_google_string():  # noqa: D400
    """Test a valid something"""
```


---

_Label `bug` added by @charliermarsh on 2022-10-11 16:40_

---

_Comment by @charliermarsh on 2022-10-11 16:56_

Oh, actually, this seems to be a `pydocstyle` thing. Flake8 doesn't have this behavior -- it expects the `noqa` to accompany the docstring.


---

_Closed by @charliermarsh on 2022-10-11 16:56_

---
