```yaml
number: 15467
title: A006 builtin-lambda-argument-shadowing ignores exceptions
type: issue
state: closed
author: Zer0x00
labels:
  - question
assignees: []
created_at: 2025-01-14T00:57:46Z
updated_at: 2025-01-14T01:05:46Z
url: https://github.com/astral-sh/ruff/issues/15467
synced_at: 2026-01-10T11:09:57Z
```

# A006 builtin-lambda-argument-shadowing ignores exceptions

---

_Issue opened by @Zer0x00 on 2025-01-14 00:57_

The [documentation](https://docs.astral.sh/ruff/rules/builtin-lambda-argument-shadowing/) describes, that setting an exception in pyproject.toml should exclude this module from being marked as a lint error.

However setting it like this:
```toml
[tool.ruff.lint.flake8-builtins]
builtins-ignorelist = ["secrets"]
```

doesn't seem to work for A006.

---

_Comment by @charliermarsh on 2025-01-14 00:59_

That setting expects to receive a list of builtins, not a list of modules. So, like `"int"`.

You might want [`per-file-ignores`](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores)?

---

_Label `question` added by @charliermarsh on 2025-01-14 00:59_

---

_Comment by @Zer0x00 on 2025-01-14 01:03_

I'm looking for a way to mute this lint error:
```
> ruff check .
src/secrets.py:1:1: A005 Module `secrets` shadows a Python standard-library module
Found 1 error.
```

---

_Comment by @Zer0x00 on 2025-01-14 01:05_

Oh, it seems like I got somehow confused. It is not A006 but A005 😕 

---

_Closed by @Zer0x00 on 2025-01-14 01:05_

---
