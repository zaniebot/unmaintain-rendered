---
number: 14147
title: "DJ006 doesn't detect ModelForm.exclude on inherited forms"
type: issue
state: open
author: GitRon
labels:
  - rule
  - type-inference
assignees: []
created_at: 2024-11-07T07:13:08Z
updated_at: 2024-12-19T09:01:19Z
url: https://github.com/astral-sh/ruff/issues/14147
synced_at: 2026-01-10T01:22:54Z
---

# DJ006 doesn't detect ModelForm.exclude on inherited forms

---

_Issue opened by @GitRon on 2024-11-07 07:13_

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

### Issue

I use the `DJ` extension of ruff. I just found out that this doesn't work on forms that don't use ModelForm directly but an inherited class. This relates to [DJ006](https://docs.astral.sh/ruff/rules/django-exclude-with-model-form/#django-exclude-with-model-form-dj006).

### Code example

```python
class MyModelForm(ModelForm):
    pass


class MyFancyForm(MyModelForm):
    class Meta:
        model = MyFancyModel
        exclude = ("my_field",)
```

When I change the inheritance from `MyModelForm`  to `ModelForm`, it complains as expected.

### List of keywords searched for
* DJ006

### Tried out Commands
* ruff check .\apps\billing\forms.py --isolated
* pre-commit run --all-files --hook-stage push 

### Ruff version
0.7.2

### Ruff settings

```toml
[tool.ruff]
# Enable pycodestyle (`E`) and Pyflakes (`F`) codes by default.
lint.select =  [
    "E",       # pycodestyle errors
    "W",       # pycodestyle warnings
    "F",       # Pyflakes
    "D",       # Pydocstyle
    "N",       # pep8-naming
    "I",       # isort
    "B",       # flake8-bugbear
    "A",       # flake8-builtins
    "DTZ",     # flake8-datetimez
    "DJ",      # flake8-django
    "TD",      # flake8-to-do
    "LOG",     # flake8-logging
    "TID",     # flake8-tidy-imports
    "Q",       # flake8-quotes
    "RET",     # flake8-return
    "RSE",     # flake8-raise
    "RUF",     # Ruff-specific rules
    "YTT",     # Avoid non-future-prove usages of "sys"
    "FBT",     # Protects you from the "boolean trap bug"
    "C4",      # Checks for unnecessary conversions
    "PIE",     # Bunch of useful rules
    "SIM",     # Simplifies your code
    "INT",     # Validates your gettext translation strings
    "PERF",    # Perflint
    "PGH",     # No all-purpose "# noqa" and eval validation
    "UP",      # PyUpgrade
    "PLR2004", # Magic numbers
    "BLE",     # Checks for except clauses that catch all exceptions
    "ANN401",  # Checks that function arguments are annotated with a more specific type than Any
    "TRY",     # Clean try/except
    "ERA",     # Commented out code
]
lint.ignore = [
    "N999",     # Project name contains underscore, not fixable
    "A003",     # Django attributes shadow python builtins
    "DJ001",    # Django model text-based fields shouldn't be nullable
    "DJ012",    # Odd ordering of Django model methods
    "RUF012",   # Mutable class attributes should be annotated with `typing.ClassVar`
    "PERF401",  # Use a list comprehension to create a transformed list
    "TD002",    # To-Do authors
    "TD003",    # To-Do issue links
    "FBT001",   # Checks use of boolean arguments in function definitions, the presence of a bool type hint
    "FBT002",   # Checks use of boolean arguments in function definitions, the presence of a boolean default value
    "D1",       # Missing docstring
    "D200",     # fits-on-one-line - if wanted should be in it's own MR -> effects nearly every doc-strings
    "D203",     # one-blank-line-before-class - incompatible to D211 no-blank-line-before-class
    "D205",     # Checks docstring summary lines not separated from the docstring description
    "D212",     # multi-line-summary-first-line - if wanted should be in it's own MR -> effects nearly every doc-string
    "D213",     # multi-line-summary-second-line - incompatible to D212 multi-line-summary-first-line
    "D400",     # Checks docstrings in which the first line does not end in a period -> ! ? should also be allowed
    "D401",     # Checks function docstrings that include the function's signature in the summary line
    "D415",     # Checks first line docstrings doesn't end in a punctuation mark, ".", "?", "!" -> weird behavior
    "RET503",   # Missing explicit `return` at the end of function able to return non-`None` value
    "TRY002",   # Checks for code that raises Exception directly.
    "TRY003",   # Checks for long exception messages that are not defined in the exception class itself.
]

# Allow autofix for all enabled rules (when `--fix`) is provided.
lint.fixable =  [
    "E",       # pycodestyle errors
    "W",       # pycodestyle warnings
    "F",       # Pyflakes
    "D",       # Pydocstyle
    "N",       # pep8-naming
    "I",       # isort
    "B",       # flake8-bugbear
    "A",       # flake8-builtins
    "DTZ",     # flake8-datetimez
    "DJ",      # flake8-django
    "TD",      # flake8-to-do
    "LOG",     # flake8-logging
    "TID",     # flake8-tidy-imports
    "Q",       # flake8-quotes
    "RET",     # flake8-return
    "RSE",     # flake8-raise
    "RUF",     # Ruff-specific rules
    "YTT",     # Avoid non-future-prove usages of "sys"
    "FBT",     # Protects you from the "boolean trap bug"
    "C4",      # Checks for unnecessary conversions
    "PIE",     # Bunch of useful rules
    "SIM",     # Simplifies your code
    "INT",     # Validates your gettext translation strings
    "PERF",    # Perflint
    "PGH",     # No all-purpose "# noqa" and eval validation
    "UP",      # PyUpgrade
    "PLR2004", # Magic numbers
    "BLE",     # Checks for except clauses that catch all exceptions
    "ANN401",  # Checks that function arguments are annotated with a more specific type than Any
    "TRY",     # Clean try/except
    "ERA",     # Commented out code
]
lint.unfixable = []

exclude = [
    ".bzr",
    ".direnv",
    ".eggs",
    ".git",
    ".hg",
    ".mypy_cache",
    ".nox",
    ".pants.d",
    ".pytype",
    ".ruff_cache",
    ".svn",
    ".tox",
    ".venv",
    "__pypackages__",
    "_build",
    "buck-out",
    "build",
    "dist",
    "node_modules",
    "venv",
    "*/migrations/*"
]

# For linting and formatting
line-length = 120

# Allow unused variables when underscore-prefixed.
lint.dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

# Assume Python 3.12.
target-version = "py312"

[tool.ruff.format]
# Like Black, use double quotes for strings.
quote-style = "double"

# Like Black, indent with spaces, rather than tabs.
indent-style = "space"

# Like Black, respect magic trailing commas.
skip-magic-trailing-comma = false

# Like Black, automatically detect the appropriate line ending.
line-ending = "auto"

# Exclude Django migration files
exclude = ["migrations/*.py"]
```




---

_Comment by @MichaReiser on 2024-11-07 07:49_

It is related to https://github.com/astral-sh/ruff/issues/12865 and applies to all other Django rules. Ruff doesn't support multi-file analysis. Some rules have some hacky implementations that follow inheritance chains in the same file, but these are more one-offs and it breaks as soon as you move the classes to different files. We're working on a more refined semantic model for ruff that supports multifile analysis but it's still far out. 

---

_Label `rule` added by @MichaReiser on 2024-11-07 07:49_

---

_Label `type-inference` added by @MichaReiser on 2024-11-07 07:49_

---

_Comment by @GitRon on 2024-11-07 07:51_

@MichaReiser oh, thx! didn't know!

---

_Comment by @swebra on 2024-12-18 21:56_

@MichaReiser is there an issue or discussion that can be followed to track the general release of multi-file analysis?

---

_Comment by @MichaReiser on 2024-12-19 09:01_

There's https://github.com/astral-sh/ruff/issues/7447

---
