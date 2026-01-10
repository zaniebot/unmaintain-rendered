```yaml
number: 12189
title: "SIM108 suggested for code block that could have been simplified to an `or`"
type: issue
state: closed
author: brettcannon
labels:
  - rule
  - accepted
assignees: []
created_at: 2024-07-04T18:15:32Z
updated_at: 2024-08-10T16:49:26Z
url: https://github.com/astral-sh/ruff/issues/12189
synced_at: 2026-01-10T11:09:54Z
```

# SIM108 suggested for code block that could have been simplified to an `or`

---

_Issue opened by @brettcannon on 2024-07-04 18:15_

* List of keywords you searched for before creating this issue: SIM108
* A minimal code snippet that reproduces the bug.
```python
if raw_data["subtest"]:
        test_id = raw_data["subtest"]
    else:
        test_id = raw_data["test"]
```


* The command you invoked: pipx run ruff check --fix --select SIM unittestadapter/execution.py
* The current Ruff version: 0.5.0

I would have expected the code block to be simplified to `test_id = raw_data["subtest"] or raw_data["test"]`.


---

_Label `rule` added by @charliermarsh on 2024-07-04 18:17_

---

_Comment by @charliermarsh on 2024-07-04 18:17_

Makes sense, thanks!

---

_Label `accepted` added by @charliermarsh on 2024-07-04 18:17_

---

_Closed by @charliermarsh on 2024-08-10 16:49_

---

_Closed by @charliermarsh on 2024-08-10 16:49_

---
