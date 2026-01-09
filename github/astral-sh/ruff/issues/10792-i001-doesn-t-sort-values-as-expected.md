---
number: 10792
title: "I001 doesn't sort values as expected."
type: issue
state: closed
author: emmceemoore
labels: []
assignees: []
created_at: 2024-04-05T18:17:55Z
updated_at: 2024-04-05T18:29:55Z
url: https://github.com/astral-sh/ruff/issues/10792
synced_at: 2026-01-07T13:12:15-06:00
---

# I001 doesn't sort values as expected.

---

_Issue opened by @emmceemoore on 2024-04-05 18:17_

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

> * List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
>  e.g. "RUF001", "unused variable", "Jupyter notebook"

- "I001", "case sensitive", etc.

> * A minimal code snippet that reproduces the bug.

```python
# before.py

from django.db import (
    models,
    OperationalError,
    transaction,
)


def foo():
    print(f"{models}, {OperationalError}, {transaction}")
```

> * The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```sh
$ ruff check --isolated --select I001 before.py
before.py:1:1: I001 [*] Import block is un-sorted or un-formatted
Found 1 error.
[*] 1 fixable with the `--fix` option.
```

I ran `ruff check --fix before.py` to generate `after.py` (did some file copying).

```sh
$ ruff check --isolated --select I001 after.py
All checks passed!
```

Here's the diff (`before.py` vs `after.py`) of those two files to show what `ruff` "fixed":

![image](https://github.com/astral-sh/ruff/assets/484429/bfb2773f-6859-4f92-9e65-c2676c23b8a1)

:point_right: I would expect for "m" (ASCII: 109) to come before "O" (ASCII: 79) alphabetically even though their ASCII values sort differently.

> * The current Ruff settings (any relevant sections from your `pyproject.toml`).

These don't seem to be applicable. I _did_ try explicitly setting the [default value](https://docs.astral.sh/ruff/settings/#lint_isort_case-sensitive) and that didn't seem to work either.

> * The current Ruff version (`ruff --version`).

```sh
$ ruff --version
ruff 0.3.5
```

---

_Comment by @AlexWaygood on 2024-04-05 18:26_

Hi! By default, I001 sorts things by casing, then alphabetically after that:
- SCREAMING_SNAKE_CASE names (conventionally used for global constants) come first
- CamelCase names (conventionally used for classes) come second
- Anything else comes last

Within each casing category, everything is sorted alphabetically. If you don't want things sorted by casing first, you can set [`isort.order-by-type`](https://docs.astral.sh/ruff/settings/#lint_isort_order-by-type) to `false`. I.e., if you're using a `ruff.toml` file for your configuration:

```toml
[lint.isort]
order-by-type = false
```

Or if you're using a `pyproject.toml` file for your configuration:

```toml
[tool.ruff.lint.isort]
order-by-type = false
```

Or if you're just running Ruff from the command line:

```
$ ruff --config "lint.isort.order-by-type = false"
```

Hope that helps! We could probably document it better the default way in which this rule sorts imports :-)

---

_Comment by @emmceemoore on 2024-04-05 18:29_

Thanks for the pointer on using `order-by-type`! That does what I need. :grin: 

---

_Closed by @emmceemoore on 2024-04-05 18:29_

---
