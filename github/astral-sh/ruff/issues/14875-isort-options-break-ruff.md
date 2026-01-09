---
number: 14875
title: Isort options break ruff
type: issue
state: closed
author: berislavlopac-wpp
labels:
  - question
  - isort
  - configuration
assignees: []
created_at: 2024-12-09T14:38:26Z
updated_at: 2024-12-09T17:26:01Z
url: https://github.com/astral-sh/ruff/issues/14875
synced_at: 2026-01-07T13:12:16-06:00
---

# Isort options break ruff

---

_Issue opened by @berislavlopac-wpp on 2024-12-09 14:38_

When including (some of) the documented options under the `[tool.ruff.lint.isort]` section in `pyproject.toml`, ruff breaks being unable to parse the file. Specifically:

Relevant part of `pyproject.toml`:

```toml
[tool.ruff]
line-length = 100
output-format = "grouped"

[tool.ruff.format]
docstring-code-format = true

[tool.ruff.lint]
# Refer to this rules if you need to config any submodule with ruff: https://docs.astral.sh/ruff/rules/
select = [
    "E", # pycodestyle errors,
    "W", # pycodestyle warnings
    "F", # pyflakes
    #    "D", # compliance with docstring conventions
    "I",   # isort; uncomment this once it's fully configurable in ruff
    "N", # pep8-naming
    "T20", # flake8-print
    "Q", # flake8-quotes
    "B", # flake8-bugbear
    "C", # flake8-comprehensions
    "S", # flake8-bandit
    "ANN", # flake8-annotations
    "COM", # flake8-commas
    #    "A",   # flake8-builtins, TBD(discussed), many uses builtin names for variables
    "C4", # flake8-comprehensions
    "DTZ", # flake8-datetimez
    "PIE", # flake8-pie
    "SIM", # flake8-simplify
    "TID", # flake8-tidy-imports
    #    "TCH", # flake8-type-checking, enable once we agree, it may be too strict (although it helps even with circular imports)
    "C90", # mccabe
    "UP", # pyupgrade
    "ERA", # eradicate, checks for commented out code
    #    "ARG", # flake8-unused-arguments  (not all options available https://pypi.org/project/flake8-unused-arguments/)
]

ignore = [
    "COM812", # may cause conflicts when used with the formatter
]

[tool.ruff.lint.isort]
combine_as_imports = true
known_first_party = ["src", "tests"]
order_by_type = false
```

Command used:
```shell
$ ruff check .
```

Output:
```shell
ruff failed
  Cause: Failed to parse /home/[...]/pyproject.toml
  Cause: TOML parse error at line 64, column 1
   |
64 | [tool.ruff.lint]
   | ^^^^^^^^^^^^^^^^
unknown field `combine_as_imports`, expected one of `force-wrap-aliases`, `force-single-line`, `single-line-exclusions`, `combine-as-imports`, `split-on-trailing-comma`, `order-by-type`, `force-sort-within-sections`, `case-sensitive`, `force-to-top`, `known-first-party`, `known-third-party`, `known-local-folder`, `extra-standard-library`, `relative-imports-order`, `required-imports`, `classes`, `constants`, `variables`, `no-lines-before`, `lines-after-imports`, `lines-between-types`, `forced-separate`, `section-order`, `default-section`, `no-sections`, `detect-same-package`, `from-first`, `length-sort`, `length-sort-straight`, `sections`
```

Ruff is version `0.8.2`, Python is version `3.11.10`.

Any ideas how to sort this out?

---

_Comment by @berislavlopac-wpp on 2024-12-09 14:41_

OK ignore me, I'm an idiot - the options had `_` instead of `-` in their names. 🤦 

---

_Closed by @berislavlopac-wpp on 2024-12-09 14:41_

---

_Label `question` added by @AlexWaygood on 2024-12-09 17:26_

---

_Label `isort` added by @AlexWaygood on 2024-12-09 17:26_

---

_Label `configuration` added by @AlexWaygood on 2024-12-09 17:26_

---
