---
number: 20853
title: Fixes between E302 and I001 disagree
type: issue
state: closed
author: mcdigman
labels:
  - bug
  - fixes
assignees: []
created_at: 2025-10-14T00:41:57Z
updated_at: 2025-10-14T07:23:24Z
url: https://github.com/astral-sh/ruff/issues/20853
synced_at: 2026-01-07T13:12:16-06:00
---

# Fixes between E302 and I001 disagree

---

_Issue opened by @mcdigman on 2025-10-14 00:41_

[ruff_test_failure.py](https://github.com/user-attachments/files/22894619/ruff_test_failure.py)

Ruff got stuck in an infinite loop and asked me to file a bug report. I made a minimum working example, attached. It appears to be caused by exactly the sequence:

1. An import
2. A blank line
3. 2 (or more) comments
4. A blank line
5. Another comment
6. A function definition.

Here is how it was ran (it does _not_ fail if I run `ruff check` directly):

```
$ pre-commit run --all
Trim Trailing Whitespace.................................................Passed
Fix End of Files.........................................................Passed
Check Yaml...............................................................Passed
Check for added large files..............................................Passed
Check for case conflicts.................................................Passed
Check docstring is first.................................................Passed
Check JSON...........................................(no files to check)Skipped
Check Toml...............................................................Passed
Check Xml............................................(no files to check)Skipped
Check python ast.........................................................Passed
Check for broken symlinks............................(no files to check)Skipped
Check vcs permalinks.....................................................Passed
Debug Statements (Python)................................................Passed
Detect AWS Credentials...................................................Passed
Detect Private Key.......................................................Passed
Mixed line ending........................................................Passed
Pretty format JSON...................................(no files to check)Skipped
Fix requirements.txt.................................(no files to check)Skipped
pyupgrade................................................................Passed
ssort....................................................................Passed
ruff.....................................................................Failed
- hook id: ruff
- exit code: 1

error: Failed to converge after 100 iterations.

This indicates a bug in Ruff. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BInfinite%20loop%5D

...quoting the contents of `tests/ruff_test_failure.py`, the rule codes E302, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

tests/ruff_test_failure.py:10:1: E302 Expected 2 blank lines, found 1
   |
 9 | # scaling on (Nf, Nt, dt, mult) in the second configuration
10 | @pytest.mark.skip
   | ^ E302
11 | def test_ruff_fails() -> None:
12 |     """A test case that causes ruff to have an infinite loop"""
   |
   = help: Add missing blank line(s)

Found 101 errors (100 fixed, 1 remaining).
[*] 1 fixable with the `--fix` option.

mypy.....................................................................Passed
```

Ruff version from .pre-commit-config.yaml:
```
# See https://pre-commit.com for more information
# See https://pre-commit.com/hooks.html for more hooks
repos:
- repo: https://github.com/pre-commit/pre-commit-hooks
  rev: v3.2.0
  hooks:
  - id: trailing-whitespace
  - id: end-of-file-fixer
  - id: check-yaml
  - id: check-added-large-files
  - id: check-case-conflict
  - id: check-docstring-first
  - id: check-json
  - id: check-toml
  - id: check-xml
  - id: check-ast
  - id: check-symlinks
  - id: check-vcs-permalinks
  - id: debug-statements
  - id: detect-aws-credentials
    args: [--allow-missing-credentials]
  - id: detect-private-key
  - id: mixed-line-ending
  - id: pretty-format-json
    args: [--autofix]
  - id: requirements-txt-fixer

- repo: https://github.com/asottile/pyupgrade
  rev: v3.19.1
  hooks:
  - id: pyupgrade
    args: [--keep-percent-format]

- repo: https://github.com/bwhmather/ssort
  rev: 0.14.0
  hooks:
  - id: ssort

- repo: https://github.com/astral-sh/ruff-pre-commit
  rev: v0.11.8
  hooks:
  - id: ruff
    args: [--preview, --fix]

- repo: https://github.com/pre-commit/mirrors-mypy
  rev: v1.16.0
  hooks:
  - id: mypy
    args: [--ignore-missing-imports]
```

pyproject.toml:

```
[project]
name = "GalacticStochastic"
version = "0.0.1"
description = "Tool for getting galactic stochastic background for a given galaxy"
readme = "README.md"
authors = [
  { name="Matthew C. Digman", email="matthew.digman@vanderbilt.edu" }
]
license = { file = "LICENSE" }
dependencies = [
    "numpy",
    "scipy",
    "numba",
    "pytest",
    "h5py"
]
requires-python = ">=3.7"

[build-system]
requires = ["setuptools>=61.0", "wheel"]
build-backend = "setuptools.build_meta"
```

ruff.toml:

```
# Exclude a variety of commonly ignored directories.
exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".git-rewrite",
    ".hg",
    ".ipynb_checkpoints",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pyenv",
    ".pytest_cache",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    ".vscode",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "site-packages",
    "venv",
]

line-length = 500
indent-width = 4

# Assume Python 3.9
target-version = "py39"

[lint]
# Enable Pyflakes (`F`) and a subset of the pycodestyle (`E`) codes by default.
# Unlike Flake8, Ruff doesn't enable pycodestyle warnings (`W`) or
# McCabe complexity (`C901`) by default.
select = ["E", "F", "W", "PERF", "FLY", "TC", "C4", "DTZ", "T10", "DJ", "EM", "FA", "INT", "ISC", "ICN", "LOG", "G", "INP", "PIE", "RSE", "SLOT", "TID", "ARG", "PTH", "I", "W", "D210", "D403", "D202", "D209", "PGH", "PLC", "PLE", "PLW", "FURB", "RUF", "TRY", "A", "B", "BLE", "ASYNC", "YTT", "FAST", "AIR", "S", "PT", "SIM", "PD", "Q", "SLF", "FBT", "RET", "COM", "NPY", "ANN"]
ignore = ["S101", "SIM108", "DOC202"]

# Allow fix for all enabled rules (when `--fix`) is provided.
fixable = ["ALL"]
unfixable = []

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[lint.flake8-quotes]
inline-quotes="single"

[lint.flake8-copyright]
author = "Matthew C. Digman"
min-file-size = 1024

[lint.per-file-ignores]
"tests/*" = ["C90", "FBT", "PLR"]
"speed_tests/*" = ["C90", "FBT", "ANN", "PLR"]

[lint.pydocstyle]
convention = "numpy"


[format]
# use single quotes for strings.
quote-style = "single"

# indent with spaces, rather than tabs.
indent-style = "space"

# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = false

# Like Black, automatically detect the appropriate line ending.
line-ending = "auto"

# Enable auto-formatting of code examples in docstrings. Markdown,
# reStructuredText code/literal blocks and doctests are all supported.
#
# This is currently disabled by default, but it is planned for this
# to be opt-out in the future.
docstring-code-format = false

# Set the line length limit used when formatting code snippets in
# docstrings.
#
# This only has an effect when the `docstring-code-format` setting is
# enabled.
docstring-code-line-length = "dynamic"
```

---

_Label `bug` added by @MichaReiser on 2025-10-14 06:42_

---

_Comment by @MichaReiser on 2025-10-14 06:46_

Thank you for the detailed report! 

The shared example is

```py
"""Test that ruff fails"""


import pytest

# whatever
# whatever 2

# whatever 3
@pytest.mark.skip
def test_ruff_fails() -> None:
    """A test case that causes ruff to have an infinite loop"""
    return
```

E302 reports a missing empty line between `# whatever 2` and `@pytest.mark.skip`

The fix adds an extra blank line between `import pytest` and `# whatever`, so that the resulting file looks like this:

```diff
  """Test that ruff fails"""
  
  
  import pytest
+ 

  # whatever
  # whatever 2
  
  # whatever 3
  @pytest.mark.skip
  def test_ruff_fails() -> None:
      """A test case that causes ruff to have an infinite loop"""
      return
```

But isort doesn't like that and removes the blank line again :) 

The formatter formats the input file to


```diff
  """Test that ruff fails"""
  
  
  import pytest

  # whatever
  # whatever 2

+   
  # whatever 3
  @pytest.mark.skip
  def test_ruff_fails() -> None:
      """A test case that causes ruff to have an infinite loop"""
      return
```

We should update `E302`'s fix to also insert the blank line there

https://play.ruff.rs/2c41c0c0-2ce5-4237-a607-fb01b401ab4f

---

_Renamed from "[Infinite loop]" to "Fixes between E302 and I001 disagree" by @MichaReiser on 2025-10-14 07:22_

---

_Label `fixes` added by @MichaReiser on 2025-10-14 07:22_

---

_Comment by @MichaReiser on 2025-10-14 07:23_

I think this is the same one as https://github.com/astral-sh/ruff/issues/12611

---

_Closed by @MichaReiser on 2025-10-14 07:23_

---

_Referenced in [astral-sh/ruff#12611](../../astral-sh/ruff/issues/12611.md) on 2025-10-14 07:24_

---
