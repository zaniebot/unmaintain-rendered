```yaml
number: 3060
title: "[Autofix error] - self.assertIsNotNone one-liner with if/else"
type: issue
state: closed
author: bmrobin
labels:
  - bug
assignees: []
created_at: 2023-02-20T15:09:32Z
updated_at: 2023-02-20T17:49:24Z
url: https://github.com/astral-sh/ruff/issues/3060
synced_at: 2026-01-10T11:09:46Z
```

# [Autofix error] - self.assertIsNotNone one-liner with if/else

---

_Issue opened by @bmrobin on 2023-02-20 15:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

the following one-liner fails to autofix and causes the syntax error message to be thrown

```python
# original code
self.assertIsNotNone(value) if expect_condition else self.assertIsNone(value)

# suitable work-around i did to get unblocked
if expect_condition:
    assert value is not None
else:
    assert value is None
```

this was on `ruff` version `0.0.247`

here are my ruff settings in pyproject.toml

```toml
[tool.ruff]
line-length = 120
target-version = "py310"
format = "grouped"
extend-exclude = [
    "__pycache__",
    "__init__.py",
]
show-source = true
select = [
    "E", "F", # Flake8 (pycodestyle + pyflakes)
    "I",      # isort (import sorting)
    #"ANN",   # flake8-annotations (type annotations)
    # "S",    # flake8-bandit (security)
    "B",      # flake8-bugbear (sneaky silent bugs)
    "A",      # flake8-builtins (reserved keyword usage)
    "PT",     # flake8-pytest-style (pytest-specific linting)
    "Q",      # flake8-quotes (single vs. double quotes)
    "ARG",    # flake8-unused-arguments (unused function arguments)
    "DTZ",    # flake8-datetimez (naive datetime usage)
    "RUF",    # ruff (ruff-specific linting)
]
ignore = [
    "RUF001",
    "RUF002",
    "RUF003",
    "RUF005",
    "E731",
    "PT001",
    "PT006",
    "PT023",
    "B904",
    "E722",
]
```

love the tool, thanks for all the hard (great) work!

---

_Label `bug` added by @charliermarsh on 2023-02-20 16:39_

---

_Comment by @charliermarsh on 2023-02-20 16:39_

Thanks!

---

_Closed by @charliermarsh on 2023-02-20 17:49_

---
