---
number: 17313
title: D212 and D213 show warnings even when ignored
type: issue
state: closed
author: nunokaeru
labels:
  - question
assignees: []
created_at: 2025-04-09T13:59:00Z
updated_at: 2025-04-10T06:59:22Z
url: https://github.com/astral-sh/ruff/issues/17313
synced_at: 2026-01-07T13:12:16-06:00
---

# D212 and D213 show warnings even when ignored

---

_Issue opened by @nunokaeru on 2025-04-09 13:59_

### Summary

Creating an issue as a follow up on comment https://github.com/astral-sh/ruff/issues/8470#issuecomment-2785786287


```
(<redacted>-py) nuno@fedora:~/github/<redacted>$ uvx ruff --version
Installed 1 package in 3ms
ruff 0.11.4
(<redacted>-py) nuno@fedora:~/github/<redacted>$ uvx ruff check .
warning: `multi-line-summary-first-line` (D212) and `multi-line-summary-second-line` (D213) are incompatible. Ignoring `multi-line-summary-second-line`.
All checks passed!
```

The <redacted> python repository is composed of the main app and two editable packages.

<redacted>:
```toml
[tool.uv]
package = false

[tool.uv.sources]
squishtests-pkg = { path = "squishtests", editable = true }
translations = { path = "translations", editable = true }

[tool.ruff]
line-length = 120

[tool.ruff.lint]
# Supported rules https://github.com/charliermarsh/ruff/#supported-rules
ignore = ["F", "E", "W"] # Ignore everything not in /squishtests
```

`translations = { path = "translations", editable = true }` does not have any `[tool.ruff]` sections, except for line-length.

squishtests:
```toml
[tool.ruff]
line-length = 120

# Supported rules https://github.com/charliermarsh/ruff/#supported-rules
lint.select = ["ALL"]

lint.ignore = [
    "ANN",    # flake8-annotations: Allow untyped code
    "E501",   # Line lenght higher than 88 characters
    "EM",     # Assigning a string to a variable before exceptions does not provide much benefit
    "D",      # pydocstyle: Allow undocummented code
    "S101",   # S101 - Usage of assert
    "INP001", # Implicit namespace package -> add __init__.py to fix
    "COM812", # Use a trailing comma when calling methods
    "PTH",    # TODO: Fix usage of os.path
    "B023",   # Function definition does not bind loop variable `items` (our squish objects do not need this)
    "ARG",    # TODO: Arguments not being used (check if we can just ignore context but not other args)
    "ERA",    # TODO: commented out code
    "C90",    # TODO: Complexity of functions, needs proper care
    "G004",   # Logging with f-strings
    "TCH",    # Imports used for TYPE CHECKING
    "TRY003", # We want to send strings to exceptions
    "S311",   # Standard pseudo-random generators are not suitable for cryptographic purposes
    "S603",   # Bandit's rule where string literals are considered okay, but dynamically-computed values are not.
    "TD003",  # Add issue link to TODO - We do not use issues
    "TD004",  # Missing colon in TODO
    "FIX002", # Line contains TODO
    "UP007",  # Use X | Y or ( X | None ) in type hints, I prefer using Optional[X] instead
    "RUF012", # Mutable class attributes should be annotated with `typing.ClassVar`
    "ISC001", # single-line-implicit-string-concatenation
    "T100",   # debugpy - need it for debugging
    "A004",   # Shadowing a python builtin - Unfortunatelly needed because squish has its own 'test' and 'object' modules
    "PLW1508",# Invalid type for environment variable default; expected `str` or `None` - I like environ.get(foo, False)
]

# Assume Python 3.10
target-version = "py310"

[tool.ruff.lint.isort]
known-third-party = ["squish", "objectmaphelper", "test", "remotesystem"]

[tool.ruff.lint.per-file-ignores]
# F401 - Error of unused import (shared.scripts.envvars)
# F821 - Undefined imports of squish methods and @Given, @When decorators
# F811 - Redefinition of methods with same name (steps methods)
# S105 - Checks for hardcoded passwords
# F403 - File names.py should not exist, but it throws * import error

"**steps.py" = ["F811", "F821"]
"**page_actions.py" = ["F811", "F821"]
"**test.py" = ["F821"]
"suite_**bdd_hooks.py" = ["F811", "F821"]
"**tst_lock_screen**steps.py" = ["S105"]
"names.py" = ["ALL"]
"**test.feature" = ["ALL"]

[tool.ruff.lint.mccabe]
max-complexity = 5

```


### Version

0.11.4

---

_Comment by @MichaReiser on 2025-04-09 14:02_

Thanks for opening a new issue.

Is the configuration in `squishtests` a `pyproject.toml`?

Can you try running ruff with `uvx ruff check . -v` to see if there are any useful logs

---

_Label `question` added by @MichaReiser on 2025-04-09 14:02_

---

_Comment by @nunokaeru on 2025-04-09 17:22_

1. Yes all of <redacted>, translations and squishtests use pyproject.toml.
2. Will be slow at responding due to timezone differences, will look into creating a simple test repository where the issue is present tomorrow and share here.

---

_Comment by @nunokaeru on 2025-04-10 06:43_

Closing the issue as I found out we have a `pyproject.toml` file in our repo a few folders deep that I didn't know about. Sorry for the noise.

It had this configuration;
```
ignore = [
    "F",
    "E",
    "W",
    "D10",
    "D203",
added this line now  -->   "D212",
    "T201",
    "TD003",
    "FIX002",
    "TRY",
    "EM102",
    "COM812",
    "FBT",
]
```

---

_Closed by @nunokaeru on 2025-04-10 06:43_

---

_Comment by @MichaReiser on 2025-04-10 06:59_

No worries. Our logging here could be better and mention from which configuration file the conflict comes from. This is something we are aware of and want to improve.

---
