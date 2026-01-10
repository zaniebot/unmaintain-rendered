---
number: 10806
title: "[`flake8-blind-expect`] Allow `raise ... from ex` in `except Exception as ex` in BLE001"
type: issue
state: closed
author: autinerd
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-04-06T18:33:38Z
updated_at: 2024-04-24T15:56:13Z
url: https://github.com/astral-sh/ruff/issues/10806
synced_at: 2026-01-10T01:22:50Z
---

# [`flake8-blind-expect`] Allow `raise ... from ex` in `except Exception as ex` in BLE001

---

_Issue opened by @autinerd on 2024-04-06 18:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Hi,

currently this is allowed in BLE001:

```python
try:
    ...
except Exception:
    raise
```

But it may be a good idea to allow this format as well (as pylint `broad-exception-caught` does):

```python
try:
    ...
except Exception as ex:
     raise ValueError from ex
```

Thanks in advance!

---

_Label `rule` added by @charliermarsh on 2024-04-07 03:35_

---

_Label `needs-decision` added by @charliermarsh on 2024-04-07 03:35_

---

_Label `needs-decision` removed by @charliermarsh on 2024-04-07 03:35_

---

_Label `bug` added by @charliermarsh on 2024-04-07 03:35_

---

_Comment by @charliermarsh on 2024-04-07 03:35_

Yeah, I would say that should be allowed. (There are other rules in Ruff that would then suggest you remove the redundant alias, but on its own it's within the bounds of `BLE001` to allow this IMO.)

---

_Label `bug` removed by @charliermarsh on 2024-04-07 03:36_

---

_Label `accepted` added by @charliermarsh on 2024-04-07 03:36_

---

_Comment by @autinerd on 2024-04-07 03:51_

Thanks!

The variable is only redundant when it is used in `raise ex` (TRY201) or in `logging.exception()` (TRY401), but in `raise ... from ex` it is needed. 

---

_Referenced in [astral-sh/ruff#11131](../../astral-sh/ruff/pulls/11131.md) on 2024-04-24 15:39_

---

_Closed by @charliermarsh on 2024-04-24 15:56_

---
