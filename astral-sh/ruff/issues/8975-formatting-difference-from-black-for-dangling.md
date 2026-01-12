```yaml
number: 8975
title: formatting difference from black for dangling commas (tuples)
type: issue
state: closed
author: r-downing
labels:
  - formatter
assignees: []
created_at: 2023-12-02T23:50:37Z
updated_at: 2023-12-03T00:31:29Z
url: https://github.com/astral-sh/ruff/issues/8975
synced_at: 2026-01-12T15:54:48Z
```

# formatting difference from black for dangling commas (tuples)

---

_@r-downing_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

version info:
```
$ python --version
Python 3.11.4

$ ruff --version
ruff 0.1.6

$ black --version
black, 23.11.0 (compiled: yes)
Python (CPython) 3.11.4
```

Example file:
```python
print("hello"),  # dangling comma makes this a tuple

1,  # useless tuple
```

After running `black test.py`, file is unchanged.

After running `ruff format test.py`, parens are added:
```python
(print("hello"),)  # dangling comma makes this a tuple

(1,)  # useless tuple
```

Formatter shouldn't remove the commas because this would actually change the meaning of the line (even if the comma is useless or unintended) but the adding parenthesis behavior is inconsistent with black's behavior.


---

_Comment by @charliermarsh on 2023-12-03 00:31_

Thanks for the clear issue. This is actually a documented violation (https://docs.astral.sh/ruff/formatter/black/#single-element-tuples-are-always-parenthesized) -- we think it's clearer to parenthesize single-element tuples since it gives them visual distinction (they're very often a mistake).

---

_Closed by @charliermarsh on 2023-12-03 00:31_

---

_Label `formatter` added by @charliermarsh on 2023-12-03 00:31_

---
