```yaml
number: 5530
title: "Syntax error when `RUF010` autofixes `f\"{str({})}\"`"
type: issue
state: closed
author: harupy
labels:
  - bug
assignees: []
created_at: 2023-07-05T11:26:28Z
updated_at: 2023-07-05T19:22:24Z
url: https://github.com/astral-sh/ruff/issues/5530
synced_at: 2026-01-12T15:54:45Z
```

# Syntax error when `RUF010` autofixes `f"{str({})}"`

---

_@harupy_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Reproduction

```sh
> ruff --version
ruff 0.0.275

> echo 'f"{str({})}"' | ruff --select RUF010 --stdin-filename a.py --fix -

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `a.py`, the rule codes RUF010, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

f"{str({})}"
```




---

_Renamed from "Syntax error when `RUF010` autofixes `'f"{str({})}"'`" to "Syntax error when `RUF010` autofixes `f"{str({})}"`" by @harupy on 2023-07-05 11:27_

---

_Comment by @charliermarsh on 2023-07-05 13:33_

Oh boy. Did this trigger on a real project? Just curious how common it is.

---

_Label `bug` added by @charliermarsh on 2023-07-05 13:33_

---

_Comment by @harupy on 2023-07-05 13:42_

> Did this trigger on a real project?

Yes, on https://github.com/mlflow/mlflow.


> Just curious how common it is.

I think it's uncommon :)

---

_Comment by @charliermarsh on 2023-07-05 13:43_

Haha we should still fix it definitely, was just looking for examples :)

---

_Comment by @harupy on 2023-07-05 13:49_

The original code looked like this:

```python
"foo bar baz {}".format(str({}))
```

I first applied `UP032`:

```python
f"foo bar baz {str({})}"
```

Then, applied `RUF010` to remove `str()` and hit the error.


---

_Comment by @harupy on 2023-07-05 13:55_

`f"{{}!s}"` is invalid, but `f"{{}}"` is.

```python
>>> f"{{}}"
'{}'
>>> f"{{}!s}"
  File "<stdin>", line 1
SyntaxError: f-string: single '}' is not allowed
```

---

_Comment by @charliermarsh on 2023-07-05 14:02_

Hmm yeah but they have different meanings, in the first case it's being interpreted as the literal braces whereas in the second it's an expression.

E.g. consider if the original example was: `"foo bar baz {}".format(str({"a": 1}))`. Then `f"foo bar baz {{"a": 1}}"` would be invalid syntax.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-05 19:07_

---

_Closed by @charliermarsh on 2023-07-05 19:22_

---
