---
number: 11828
title: F811 is not raised when it is needed
type: issue
state: closed
author: Denis-Alexeev
labels:
  - bug
assignees: []
created_at: 2024-06-10T20:21:35Z
updated_at: 2024-07-26T02:16:48Z
url: https://github.com/astral-sh/ruff/issues/11828
synced_at: 2026-01-07T13:12:15-06:00
---

# F811 is not raised when it is needed

---

_Issue opened by @Denis-Alexeev on 2024-06-10 20:21_

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
Ruff does not raise F811 error when unused redefinition exists.

Keywords:
"F811", "redefinition"

Code snippet:
```python
"""module."""
class AAA:
    """ddd."""

    def world(self: "AAA") -> None:
        """Docstring."""

    hello = world

    def hello(self: "AAA") -> None:
        """Docstring."""

``` 
Command:
`ruff check --config /path/to/config /path/to/python/file.py`

ruff.toml:
```
target-version = "py312"

[lint]
select = ["ALL"]
```

ruff version:
ruff 0.4.8

---

_Label `bug` added by @charliermarsh on 2024-06-10 20:28_

---

_Comment by @charliermarsh on 2024-06-12 02:46_

Not clear why it's not being raised here, but it should be AFAICT.

---

_Comment by @ukyen8 on 2024-06-13 08:43_

@charliermarsh I think the shadowed binding can also be an Assignment. At the moment, Ruff checks shadowed binding for class, function, import, from import.

---

_Comment by @ukyen8 on 2024-06-17 21:41_

@charliermarsh I like using Ruff. I'd love to contribute to ruff and would like to work on this issue. Is it okay if I take this on?


---

_Comment by @dhruvmanila on 2024-06-18 04:55_

@ukyen8 Yeah, go for it. The code for this rule logic is at https://github.com/astral-sh/ruff/blob/c53d55a483f0f81f0dcdc67f762bbf2c71e96773/crates/ruff_linter/src/checkers/ast/analyze/deferred_scopes.rs#L190-L309

 Don't hesitate to ask any questions if you're stuck either here or on Discord :)

---

_Referenced in [astral-sh/ruff#11961](../../astral-sh/ruff/pulls/11961.md) on 2024-06-21 09:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-13 17:26_

---

_Closed by @charliermarsh on 2024-07-26 02:16_

---

_Comment by @charliermarsh on 2024-07-26 02:16_

Fixed in https://github.com/astral-sh/ruff/pull/11961, forgot to close.

---
