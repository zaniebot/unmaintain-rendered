---
number: 755
title: "U007 Use `X | Y` for type annotations, does not respect target-version"
type: issue
state: closed
author: lovetox
labels:
  - bug
assignees: []
created_at: 2022-11-15T16:24:28Z
updated_at: 2022-11-15T16:52:13Z
url: https://github.com/astral-sh/ruff/issues/755
synced_at: 2026-01-07T13:12:14-06:00
---

# U007 Use `X | Y` for type annotations, does not respect target-version

---

_Issue opened by @lovetox on 2022-11-15 16:24_

Hi,

The error 

`U007 Use `X | Y` for type annotations`

is emitted even though 

`target-version = "py39"` is set in `pyproject.toml`

This python feature is only available in 3.10

https://peps.python.org/pep-0604/

---

_Label `bug` added by @charliermarsh on 2022-11-15 16:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-11-15 16:46_

---

_Referenced in [astral-sh/ruff#757](../../astral-sh/ruff/pulls/757.md) on 2022-11-15 16:51_

---

_Comment by @charliermarsh on 2022-11-15 16:51_

Thank you!

---

_Comment by @charliermarsh on 2022-11-15 16:51_

(We just had the wrong version encoded.)

---

_Closed by @charliermarsh on 2022-11-15 16:52_

---
