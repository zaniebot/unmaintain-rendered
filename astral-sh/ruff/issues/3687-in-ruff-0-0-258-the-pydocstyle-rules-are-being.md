```yaml
number: 3687
title: "In `ruff` 0.0.258 the `pydocstyle` rules are being enabled and can't be disabled with `[tool.ruff.pydocstyle]` defined. "
type: issue
state: closed
author: bgianfo
labels: []
assignees: []
created_at: 2023-03-23T17:50:04Z
updated_at: 2023-03-23T17:54:20Z
url: https://github.com/astral-sh/ruff/issues/3687
synced_at: 2026-01-10T11:09:46Z
```

# In `ruff` 0.0.258 the `pydocstyle` rules are being enabled and can't be disabled with `[tool.ruff.pydocstyle]` defined. 

---

_Issue opened by @bgianfo on 2023-03-23 17:50_

After upgrading we noticed that these D<XXX> rules (pydocstyle) are being enabled and suppressing them in the `ignore` section doesn't seem to take effect? 

It seems the presence of this line in the `pyproject.toml` somehow enables the rule even though the ruleset isn't enabled?

```toml
[tool.ruff.pydocstyle]
convention = "google"
```

Repro steps: 
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.

```bash
# ruff . --statistics
   2	UP031	[*] Use format specifiers instead of percent format
1515	D203 	[*] 1 blank line required before class docstring
 337	D213 	[*] Multi-line docstring summary should start at the second line
4915	D400 	[*] First line should end with a period
1814	D401 	[ ] First line of docstring should be in imperative mood: "Main entry point for the script."
 136	D404 	[ ] First word of the docstring should not be "This"
4633	D407 	[*] Missing dashed underline after section ("Args")
   6	D409 	[*] Section underline should match the length of its name ("Returns")
   1	D413 	[*] Missing blank line after last section ("Args")
```

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

```toml
[tool.ruff]
# Support Python 3.8+.
target-version = "py38"

select = [
    "E",   # pycodestyle
    "EXE", # flake8-executable
    "F",   # pyflakes
    "A",   # flake8-builtins
    "B",   # flake8-bugbear
    "W",   # pycodestyle
    "PGH", # pygrep-hooks
    "PIE", # flake8-pie
    "YTT", # flake8-2020
    "PLC", # pylint
    "PLE", # pylint
    "PLR", # pylint
    "PLW", # pylint
    "RUF", # ruff rules
    "UP",  # pyupgrade
    # TODO:
    # "D",   # pydocstyle
    # "I",   # isort
    # "BLE", # flake8-blind-except
    # "ANN", # flake8-annotations
]

ignore = [
    # Formatting errors.  Formatting is enforced by black.
    # Uncomment once ruff supports this, we can ignore it.
    # "E2",   # Not yet implemented by ruff
    # "E302", # Not yet implemented by ruff
    "E501", # Line too long
    # "W503", # line break before binary operator # Not yet implemented by ruff
    # "W504", # line break after binary operator  # Not yet implemented by ruff
    # This is enforced by isort.
    "E402",   # module level import not at top of file.
    # Temporarily suppressed warnings.
    # TODO: enable these
    "A003",   # Class attribute is shadowing a python builtin
    "B006",   # Do not use mutable data structures for argument defaults
    "B008",   # Do not perform function call in argument defaults
    "B011",   # Do not `assert False`
    "B904",   # Within an `except` clause, raise exceptions with `raise ... from err`
    "E722",   # do not use bare except, specify exception instead
    "E731",   # Do not assign a lamda expression
    "E741",   # ambiguous name
    "F401",   # module imported but unused
    "F403",   # unable to detect undefined names
    "F405",   # future import(s) name after other statements
    "F821",   # undefined name name
    "W605",   # Invalid escape sequence
    "RUF005", # Consider {expr} instead of concatenation
    "RUF006", # Store a reference to the return value of asyncio.<func>
    "RUF100", # Unused noqa directive
    "PGH003", # use specific rule codes when ignoring type issues
    "PLC1901",# `<expression>` can be simplified to `<expression>` as an empty string is falsey
    "PLR0402",# Use in lieu of alias
    "PLR0911",# Too many return statements
    "PLR0912",# Too many branches
    "PLR0913",# Too many arguments to function call 
    "PLR0915",# Too many statements
    "PLR2004",# Magic value used
    "PLR5501",# Consider using elif
    "PLW0602",# Using global without assignment
    "PLW0603",# Using the global statement to update variable is discouraged
    "PLW2901",# Outer for loop variable overwritten by inner assignment target
]

[tool.ruff.pydocstyle]
convention = "google"
```

* The current Ruff version (`ruff --version`).
 0.0.258



---

_Renamed from "In `ruff` 0.0.258 the `pydocstyle` rules are being enabled automatically and can't be disabled. " to "In `ruff` 0.0.258 the `pydocstyle` rules are being enabled and can't be disabled with `[tool.ruff.pydocstyle]` defined. " by @bgianfo on 2023-03-23 17:50_

---

_Comment by @charliermarsh on 2023-03-23 17:54_

Hey sorry -- thanks tons for the clear report, this was fixed today in #3685 and will go out in a new release in a bit.

---

_Closed by @charliermarsh on 2023-03-23 17:54_

---
