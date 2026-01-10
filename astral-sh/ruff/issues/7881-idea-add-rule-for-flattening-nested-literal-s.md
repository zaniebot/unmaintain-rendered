```yaml
number: 7881
title: "idea: Add rule for flattening nested `Literal`s"
type: issue
state: closed
author: diceroll123
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-10-10T01:34:29Z
updated_at: 2025-02-11T20:22:38Z
url: https://github.com/astral-sh/ruff/issues/7881
synced_at: 2026-01-10T11:09:50Z
```

# idea: Add rule for flattening nested `Literal`s

---

_Issue opened by @diceroll123 on 2023-10-10 01:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

From [PEP 586](https://peps.python.org/pep-0586/#legal-parameters-for-literal-at-type-check-time), `Literal[Literal[Literal[1, 2, 3], "foo"], 5, None]` is equivalent to `Literal[1, 2, 3, "foo", 5, None]`

Currently there's no rule I know of that would unfurl/flatten a literal structure like this. Would be neat!

I came across this while fiddling with #7880. 

---

_Comment by @T-256 on 2023-10-10 03:05_

++ `Union` and `Annotated`.

---

_Comment by @charliermarsh on 2023-10-13 01:25_

Could this not be part of PYI030 itself (https://docs.astral.sh/ruff/rules/unnecessary-literal-union/)?

---

_Label `rule` added by @charliermarsh on 2023-10-13 01:25_

---

_Label `needs-decision` added by @charliermarsh on 2023-10-13 01:25_

---

_Comment by @T-256 on 2023-10-13 01:49_

> Could this not be part of PYI030 itself (https://docs.astral.sh/ruff/rules/unnecessary-literal-union/)?

As its name says that is `Unnecessery union` but here I think we need `Unnecessery nested`.
For `Unnecessery union`, types get joined:
```py
field: Literal[1] | Literal[2]  # FIX: Literal[1, 2] 
```
For `Unnecessery nested`, types get unpacked:
```py
field: Literal[1, Literal[2]]  # FIX: Literal[1, 2]
```

---

_Comment by @diceroll123 on 2023-10-14 23:35_

> Could this not be part of PYI030 itself ([docs.astral.sh/ruff/rules/unnecessary-literal-union](https://docs.astral.sh/ruff/rules/unnecessary-literal-union/))?

I've implemented this in as part of #7934, but of course this is strictly for literals.

---

_Comment by @Avasam on 2023-11-22 15:58_

> Could this not be part of PYI030 itself (https://docs.astral.sh/ruff/rules/unnecessary-literal-union/)?

It could. But can also be a new code. It was not implemented in flake8-pyi for the sole reason it was not deemed worth the effort for typeshed. https://github.com/PyCQA/flake8-pyi/issues/425

Also note the existence of Y061 https://github.com/PyCQA/flake8-pyi/pull/435
Which prefers `Literal[0] | None` over `Literal[0, None]` and is an opinionated and purely aesthetic rule (flake8-pyi doesn't need to support enforcing the second form because they are their own users at typeshed)

---

_Comment by @Avasam on 2025-02-11 20:18_

Just tested on Ruff 0.9.6 and [unnecessary-nested-literal (RUF041)](https://docs.astral.sh/ruff/rules/unnecessary-nested-literal/#unnecessary-nested-literal-ruf041) does the job.

OP's example is autofixed from
```py
Literal[Literal[Literal[1, 2, 3], "foo"], 5, None]
```
to
```py
test: Literal[1, 2, 3, "foo", 5, None]
```
and even, with [redundant-none-literal (PYI061)](https://docs.astral.sh/ruff/rules/redundant-none-literal/#redundant-none-literal-pyi061)
```py
test: Literal[1, 2, 3, "foo", 5] | None
```

---

_Closed by @MichaReiser on 2025-02-11 20:22_

---
