```yaml
number: 16146
title: Exclude subfolders not working when using ruff format
type: issue
state: closed
author: akshaybabloo
labels:
  - question
assignees: []
created_at: 2025-02-13T23:06:50Z
updated_at: 2025-02-14T11:44:46Z
url: https://github.com/astral-sh/ruff/issues/16146
synced_at: 2026-01-12T15:54:55Z
```

# Exclude subfolders not working when using ruff format

---

_@akshaybabloo_

### Description

I am trying to exclude a subfolder when I use `ruff format`. Here is my configuration:

```toml
# ruff.toml

line-length = 88
target-version = "py310"

[lint]
select = [
    "A",    # Builtins
    "B",    # Bugbear
    "D",    # pyDocStyle
    "E",    # pyCodeStyle
    "F",    # Pyflakes
    "G",    # Logging format
    "N",    # Naming
    "S",    # Bandit (security)
    "BLE",  # Blind Except
    "CPY",  # Copyright
    "DTZ",  # Date Time
    "ICN",  # Imports
    "TID",  # Tidy imports
    "LOG",  # Logging
    "PIE",  # Unnecessary things
    "PT",   # PyTest Style
    "Q",    # Quotes
    "ARG",  # Arguments
    "PTH",  # Pathlib vs OS
    "DOC",  # pyDocLint
]

ignore = [
    "E203",   # whitespace-before-punctuation
    "E501",   # line too long
    "S101",   # assert detected
    "D407",   # Missing dashed underline after section
    "D203",   # One blank line before class doc
    "D212",   # Multi line docstring - 1st line
    "D213",   # Multi line docstring - 2nd line
    "D413",   # Missing blank line after last section
    "PTH123", # `open()` should be replaced by `Path.open()`
    "G004",   # Logging uses f-string
    "D401",   # First line should be in imperative mood
    "D200",   # One-line docstring should fit on one line with quotes

    # TODO: Following rules needs to be fixed
    "D101",   # Missing docstring in public class
    "D102",   # Missing docstring in public method
    "D104",   # Missing docstring in public package
    "D107",   # Missing docstring in __init__
    "D103",   # Missing docstring in public function
    "D100",   # Missing docstring in public module
    "D205",   # 1 blank line required between summary line and description
    "D202",
    "B011",   # Do not use `assert` to raise exceptions
    "PT015",  # Use `assert` instead of `raise` in tests
    "PIE790", # Unnecessary `pass` statement
    "D415",   # First line should end with a period, question mark, or exclamation point
    "D400",   # First line should end with a period, question mark, or exclamation point
    "PTH118", # `os.path` should be replaced by `Path`
    "A002",   # Function argument `set` is shadowing a Python builtin
    "A001",
    "B007",   # Loop control
    "PTH110", # `os.path` should be replaced by `Path`
    "PTH103", # `os.path` should be replaced by `Path`
    "ARG001", # Unused argument
]

exclude = ["images/*", "pipelines/*", "tools/*", "*.pyi", "test_legacy/*", "deprecated_logged_data_official_names/*.py"]
fixable = ["ALL"]

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[format]
skip-magic-trailing-comma = false  # Like Black, respect magic trailing commas.
line-ending = "auto"               # Like Black, automatically detect the appropriate line ending.

[lint.mccabe]
max-complexity = 10                # Unlike Flake8, default to a complexity level of 10.

```

I want to ignore `ignore_py` and it's folders. I tried `ignore_py/*`, `ignore_py/**` and `ignore_py/**/` none of these worked.

**Version:** 0.9.6

---

_Comment by @dylwil3 on 2025-02-14 04:01_

Could you try once more but put `exclude = ["ignore_py/*"]` under `[format]` or at the top level? Apologies if you've already tried that and I'm misunderstanding.

---

_Label `question` added by @dylwil3 on 2025-02-14 04:02_

---

_Comment by @MichaReiser on 2025-02-14 07:24_

This is probably related to https://github.com/astral-sh/ruff/issues/8267

---

_Comment by @MichaReiser on 2025-02-14 07:25_

Oh, never mind. @dylwil3's right. I only read the title and jumped right to conclusion. You'll need an `exclude` in the `format` section or you can move the `exclude` to the top level if it should apply to both format and lint

---

_Comment by @akshaybabloo on 2025-02-14 11:44_

Oh got it. I will giive that a try.

---

_Closed by @akshaybabloo on 2025-02-14 11:44_

---
