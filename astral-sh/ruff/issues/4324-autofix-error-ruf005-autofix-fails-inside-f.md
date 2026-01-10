---
number: 4324
title: "[Autofix error] RUF005 autofix fails inside f-string: `f\"{a() + ['b']}\"`"
type: issue
state: closed
author: konstin
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-05-09T19:01:26Z
updated_at: 2023-05-18T14:33:35Z
url: https://github.com/astral-sh/ruff/issues/4324
synced_at: 2026-01-10T01:22:43Z
---

# [Autofix error] RUF005 autofix fails inside f-string: `f"{a() + ['b']}"`

---

_Issue opened by @konstin on 2023-05-09 19:01_

In ruff https://github.com/charliermarsh/ruff/commit/99a755f936ebd73cdd1bfccbc121c31d4b27751e, the following snippet causes the autofix RUF005 to introduce a syntax error:

```python
f"{a() + ['b']}"
```

```
$ ./ruff --no-cache --select RUF005 --fix code.py

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/charliermarsh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `code.py`, the rule codes RUF005, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

code.py:1:4: RUF005 Consider `[*a(), "b"]` instead of concatenation
Found 1 error.
```

This was originally discovered in https://github.com/Mojino01/hugging_face/blob/3dae0d7b4fb8d7e9383b893e4e1799191bb2ab7b/src/transformers/pipelines/base.py#L1181


---

_Label `bug` added by @konstin on 2023-05-09 19:01_

---

_Label `autofix` added by @konstin on 2023-05-09 19:01_

---

_Label `autofix-error` added by @konstin on 2023-05-09 19:01_

---

_Comment by @charliermarsh on 2023-05-09 19:25_

I think we have a few options for addressing this class of issues:

1. Always skip autofix within f-strings (easiest).
2. Skip autofix within f-strings on a rule-by-rule basis. This is fine, but we're going to have to omit most rules. Even `RUF005` only fails because we go through `unparse_expr`, which changes the quote style of the `'b'` to `"b"`.
3. Improve `unparse_expr` to enforce specific quote styles within f-strings. This would require tracking the quote style of the f-string, threading it down to `unparse_expr`, and respecting it throughout the unparse logic.

I'm honestly tempted to do (1).


---

_Comment by @konstin on 2023-05-09 19:30_

One note to add here is that most of these will valid syntax with [PEP 701](https://peps.python.org/pep-0701/) implemented, currently targeted for python 3.12, i.e. we could very ambitiously allow autofixes in f-strings for projects with minimum python version 3.12.

---

_Referenced in [astral-sh/ruff#4487](../../astral-sh/ruff/pulls/4487.md) on 2023-05-18 03:27_

---

_Closed by @charliermarsh on 2023-05-18 14:33_

---

_Referenced in [astral-sh/ruff#4488](../../astral-sh/ruff/pulls/4488.md) on 2023-05-19 12:52_

---
