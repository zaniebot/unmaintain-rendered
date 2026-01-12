```yaml
number: 12003
title: "Ruff ignores \"quote-style = 'single'\""
type: issue
state: closed
author: jackdesert
labels:
  - question
assignees: []
created_at: 2024-06-24T04:06:14Z
updated_at: 2024-06-26T01:22:03Z
url: https://github.com/astral-sh/ruff/issues/12003
synced_at: 2026-01-12T15:54:51Z
```

# Ruff ignores "quote-style = 'single'"

---

_@jackdesert_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Ruff wants to use double quotes even though pyproject.toml specifies single quotes. 



Keywords: single, single quote, quote style

Code snippet:

```python
print('single')
print("double")
```

Command:

    ruff check --config pyproject.toml sample.py

Output:

```bash
sample.py:1:1: D100 Missing docstring in public module
sample.py:1:7: Q000 [*] Single quotes found but double quotes preferred
Found 2 errors.
[*] 1 fixable with the `--fix` option.
```

Expected Output:

```bash
sample.py:1:1: D100 Missing docstring in public module
sample.py:2:7: Q000 [*] Double quotes found but single quotes preferred
Found 2 errors.
[*] 1 fixable with the `--fix` option.
```


Version:

    ruff 0.4.10

pyproject.toml:

```toml
[tool.ruff]

target-version = 'py311'

# Exclude a variety of commonly ignored directories.
exclude = [
    '.bzr',
    '.direnv',
    '.eggs',
    '.git',
    '.hg',
    '.mypy_cache',
    '.nox',
    '.pants.d',
    '.ruff_cache',
    '.svn',
    '.tox',
    '.venv',
    '__pypackages__',
    '_build',
    'buck-out',
    'build',
    'dist',
    'node_modules',
    'venv',
    'env',
]

# Same as Black.
line-length = 88

[tool.ruff.lint]

# Enable All rules.
# We may not want all rules, but this is a good starting point for you to
# see the full capabilities of ruff
select = ['ALL']

unfixable = []

per-file-ignores = {}

# Putting an entry in `ignore` will ignore all errors that start with that string.
# For example, adding `T` will ignore all errors beginning with `T`,
# whereas adding `T20` will only ignore the `T20` error
# (and probably any errors that start with `T20`)
ignore = ['INP001', # implicit-namespace-package
          'T20',    # flake8-print (warns when there are print statements)

          # One of either D212 or D213 must be disabled.
          # (I prefer disabling D212 (Multi-line docstring summary should start at the first line)
          #  because I like text inside a docstring
          #  to start the line below the three ''')
          #'D213', # See https://github.com/charliermarsh/ruff/issues/2281
          'D212', # See https://github.com/charliermarsh/ruff/issues/2281
          # One of D203 and D200 must be disabled.
          # No strong preference here.
          # One expects a blank line before a class docstring
          #'D203', # See https://github.com/charliermarsh/ruff/issues/2281
          'D211', # See https://github.com/charliermarsh/ruff/issues/2281


          'D200', # Allow multiline docstring even when it could fit on one line

          'C408', # Allow instantiating dictionary as dict(key=value)
          'G004', # Logging statement uses f-string
          'E501', # Line too long
          'ANN101', # Don't require self to be annotated.
          'PTH', # allow os.path
          'PLR0913', # allow too many arguments
          'COM812', # may cause issues with ruff format
          'ISC001', # may cause issues with ruff format

]

# Allow unused variables when underscore-prefixed.
dummy-variable-rgx = '^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$'

# In order for a particular class to be fixable with `ruff [file] --fix`,
# the class must be in `fixable`.
fixable = ['A', 'B', 'C', 'D', 'E', 'F', 'Q']

# I like the google docstyle
# See https://sphinxcontrib-napoleon.readthedocs.io/en/latest/example_google.html
# and http://google.github.io/styleguide/pyguide.html
# https://gist.github.com/redlotus/3bc387c2591e3e908c9b63b97b11d24e
[tool.ruff.lint.pydocstyle]
convention = 'google'  # Or 'numpy', or 'pep257'


[tool.ruff.lint.mccabe]
# Unlike Flake8, default to a complexity level of 10.
max-complexity = 10

# Configure single quotes
[tool.ruff.format]
quote-style = 'single'

```


---

_Comment by @zanieb on 2024-06-24 04:23_

Hi! The configuration setting you're checking is for `ruff format` — `ruff check` only runs the linter right now (see #8232 for more on that). If you run `ruff format`, your desired quote style should be enforced.

---

_Label `question` added by @zanieb on 2024-06-24 04:23_

---

_Closed by @charliermarsh on 2024-06-26 01:22_

---
