```yaml
number: 5494
title: False positive in PERF402 when append is more complex. 
type: issue
state: closed
author: bgianfo
labels:
  - bug
assignees: []
created_at: 2023-07-03T20:19:59Z
updated_at: 2023-07-04T18:21:07Z
url: https://github.com/astral-sh/ruff/issues/5494
synced_at: 2026-01-10T11:09:47Z
```

# False positive in PERF402 when append is more complex. 

---

_Issue opened by @bgianfo on 2023-07-03 20:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.
-->

The new rule [PERF402](https://beta.ruff.rs/docs/rules/manual-list-copy/) suggests to convert lists to trivial copies in cases where it's not really possible. Ideally the tool would not report such false positives. 

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.

```python
 # In this example you can't trivially convert this to a `list.copy` as the rule suggests.  
 for r in record_dict["values"]:
        list_of_stuff[r["other_value"]].append(r)
```


* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

	`ruff --select=PERF402 example.py` 

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

 	`PERF` ruleset is enabled

* The current Ruff version (`ruff --version`).

 	version = `0.0.276`


---

_Label `bug` added by @charliermarsh on 2023-07-03 20:20_

---

_Comment by @charliermarsh on 2023-07-03 20:21_

Makes sense, we should probably require that the left-hand side expression is a static name (or at least an expression that doesn't depend on the iteration key). Thanks!

---

_Label `help wanted` added by @charliermarsh on 2023-07-03 20:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-04 17:59_

---

_Label `help wanted` removed by @charliermarsh on 2023-07-04 18:02_

---

_Closed by @charliermarsh on 2023-07-04 18:21_

---
