---
number: 8629
title: "🐞 Don't remove nested comments on formatting"
type: issue
state: closed
author: jd-solanki
labels:
  - question
assignees: []
created_at: 2023-11-12T16:37:08Z
updated_at: 2023-11-16T04:06:09Z
url: https://github.com/astral-sh/ruff/issues/8629
synced_at: 2026-01-07T13:12:15-06:00
---

# 🐞 Don't remove nested comments on formatting

---

_Issue opened by @jd-solanki on 2023-11-12 16:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Problem

When I save file in VSCode ruff removes the nested comment `# noqa: A003`

https://github.com/astral-sh/ruff/assets/47495003/e429f3d6-b632-45ca-ac2e-b5ee87d4ec99



## Snippet

```py
class Demo:
    pass
    # id: int # noqa: A003
```

## Command

```shell
ruff play.py
```

## Ruff settings

```toml
# Docs: https://docs.astral.sh/ruff/settings/
select = ["ALL"]
fix = true
target-version = "py311"
line-length = 120
ignore = [
    "D101",   # Missing docstring in public class
    "D104",   # Missing docstring in public package
    "D100",   # Missing docstring in public module
    "ANN201", # Missing return type annotation for public function `get_db`
    "D103",   # Missing docstring in public function
    "INP001", # File `X` is part of an implicit namespace package. Add an `__init__.py`.
    "D102",   # Missing docstring in public method
    "D107",   # Missing docstring in `__init__`
    "ANN101", # Missing type annotation for `self` in method
    "D106",   # Missing docstring in public nested class

    # Comments
    "TD002",  # Missing author in TODO; try: `# TODO(<author_name>): ...` or `# TODO @<author_name>: ...`
    "TD003",  # Missing issue link on the line following this TODO
    "RUF003", # ambiguous-unicode-character-comment (For emoji comments)

    # FastAPI only
    "B008", # Do not perform function call `Depends` in argument defaults
]
[extend-per-file-ignores]
"tests/**" = ["S101", "PLR2004"]
```

## Version

```
ruff 0.1.5
python 3.12
```

---

_Comment by @charliermarsh on 2023-11-12 16:39_

I believe this is intended due to having `RUF100` enabled. It detects that the `# noqa` is unused, and the automatic fix is to remove it. You could mark `RUF100` as an unsafe fix if you'd prefer for it not to run without explicit `--unsafe-fixes`, like:

```toml
extend-unsafe-fixes = ["RUF100"]
```

Can you give that a try, and see if it fits your needs?

---

_Label `question` added by @charliermarsh on 2023-11-12 16:39_

---

_Comment by @charliermarsh on 2023-11-16 04:06_

Closing for now but always happy to revisit if more info comes up.

---

_Closed by @charliermarsh on 2023-11-16 04:06_

---
