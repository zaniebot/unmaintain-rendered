```yaml
number: 11573
title: "FURB118: Should not replace lambda with operator.itemgetter when the itemgetter use the lambda arguments"
type: issue
state: closed
author: nasyxx
labels:
  - bug
  - help wanted
assignees: []
created_at: 2024-05-27T18:07:45Z
updated_at: 2024-05-28T01:34:53Z
url: https://github.com/astral-sh/ruff/issues/11573
synced_at: 2026-01-12T15:54:51Z
```

# FURB118: Should not replace lambda with operator.itemgetter when the itemgetter use the lambda arguments

---

_@nasyxx_

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

`operator.itemgetter` cannot be used to replace a lambda function for indexing with lambda arguments.

```python
# xxx.py
xs = [1, 2, 3, 4, 5]

xm = map(lambda x: x[5-x], xs)
```

```
xxx.py:
  3:10 FURB118 [*] Use `operator.itemgetter(5-x)` instead of defining a lambda
```

Here if replaced with `operator.itemgetter(5-x)`, the x would be a undefined variable

```
ruff: 0.4.5
python: 3.12.2
```


---

_Comment by @charliermarsh on 2024-05-27 18:58_

Makes sense, thanks.

---

_Label `bug` added by @charliermarsh on 2024-05-27 18:58_

---

_Label `help wanted` added by @charliermarsh on 2024-05-27 20:10_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-28 01:23_

---

_Closed by @charliermarsh on 2024-05-28 01:29_

---

_Comment by @charliermarsh on 2024-05-28 01:34_

Fixed in the next release.

---
