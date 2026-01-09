---
number: 3586
title: "[Autofix] Autofix fails with docstring"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-03-17T21:05:26Z
updated_at: 2023-03-18T18:48:54Z
url: https://github.com/astral-sh/ruff/issues/3586
synced_at: 2026-01-07T13:12:14-06:00
---

# [Autofix] Autofix fails with docstring

---

_Issue opened by @qarmin on 2023-03-17 21:05_

Ruff 1dd3cbd047beda01a7833e1b20e10e745757fe3e + PR #3584
No config
```
ruff . --fix
```
on
```
class MeasurementSchemaUpdateRequest(object):
    def columns(self, columns):
        """Set the columns of this MeasurementSchemaUpdateRequest.

        An ordered collection of column definitions
        """  # noqa: E򉷸01
```
cause error
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/Desktop/RunEveryCommand/Ruff/Broken/50790measurement_schema_update_request0.py`, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!
```


---

_Label `bug` added by @charliermarsh on 2023-03-17 22:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-03-17 22:15_

---

_Referenced in [astral-sh/ruff#3589](../../astral-sh/ruff/pulls/3589.md) on 2023-03-17 22:29_

---

_Closed by @charliermarsh on 2023-03-18 18:48_

---
