---
number: 11037
title: "[Panic] Ruff VSCode extension crashes every time the word `def` is written in a class"
type: issue
state: closed
author: lmanc
labels:
  - bug
  - parser
assignees: []
created_at: 2024-04-19T12:56:48Z
updated_at: 2024-04-19T14:45:39Z
url: https://github.com/astral-sh/ruff/issues/11037
synced_at: 2026-01-07T13:12:15-06:00
---

# [Panic] Ruff VSCode extension crashes every time the word `def` is written in a class

---

_Issue opened by @lmanc on 2024-04-19 12:56_

Hi, today while using the Ruff VSCode extension, Ruff crashed with the following message:

```
Ruff: Lint failed ( error: Ruff crashed. If you could open an issue at: https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D ...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative! thread 'main' panicked at D:\a\ruff\ruff\crates\ruff_text_size\src\range.rs:48:9: assertion failed: start.raw <= end.raw note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace )
```

I'm keeping the title as the message suggests. For the other parts:

- "quoting the executed command", since I'm using the VSCode extension, I'm not executing it via CLI. I'm uncertain about the "implicit" commands that are run under the hood.
- "relevant file contents", Ruff crashes every time I start writing a new function with the keyword `def` in this Django REST Framework view. I can't share the entire file, but here's an excerpt:
  ```python
  # Imports and more classes above
  
  class WiseSettingsGroupsView(APIView):
      def get(self, request):
          # snip
          return Response(data)
          
      def serialize_data(self):
          # the function that make Ruff crash as soon as I type `def` here
          
  # Other classes
  ```
- "[...] and pyproject.toml settings", here they are:
  ```toml
  # Include Jupyter notebooks
  include = ["*.py", "*.pyi", "**/pyproject.toml", "*.ipynb"]
  
  # Exclude a variety of commonly ignored directories.
  exclude = [
      ".bzr",
      ".direnv",
      ".eggs",
      ".git",
      ".git-rewrite",
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
  ]
  
  line-length = 99 # Used by both linter and E501
  indent-width = 4
  
  [lint.pycodestyle]
  max-doc-length = 80
  
  [lint]
  select = [
      "A",     # flake8-builtins
  #   "ANN",   # flake8-annotations
      "ARG",   # flake8-unused-arguments
      "B",     # flake8-bugbear
      "BLE",   # flake8-blind-except
      "COM",   # flake8-commas
      "C4",    # flake8-comprehensions
      "C90",   # mccabe
      "D",     # pydocstyle
      'DJ',    # flake8-django
      "E",     # pycodestyle errors
      "ERA",   # eradicate
      "EM",    # flake8-errmsg
      "F",     # pyflakes
      "FBT",   # flake8-boolean-trap
      "FLY",   # flynt
      "FURB",  # refurb
      "G",     # flake8-logging-format
      "I",     # isort
      "ICN",   # flake8-import-conventions
      "INP",   # flake8-no-pep420
      "ISC",   # flake8-implicit-str-concat
      "N",     # pep8-naming
      "PD",    # pandas-vet
      "PERF",  # Perflint
      "PIE",   # flake8-pie
      "PL",    # Pylint
      "PTH",   # flake8-use-pathlib
      "PT",    # flake8-pytest-style
      "RET",   # flake8-return
      "RSE",   # flake8-raise
      "RUF",   # Ruff-specific rules
      "S",     # flake8-bandit
      "SIM",   # flake8-simplify
      "SLF",   # flake8-self
      "TID",   # flake8-tidy-imports
      "TRY",   # tryceratops
      "T10",   # flake8-debugger
      "UP",    # pyupgrade
      "W",     # pycodestyle warnings
  ]
  
  ignore = [
      "ANN101",  # missing-type-self
      "ANN102",  # missing-type-cls
      "ANN204",  # missing-return-type-special-method
      "COM812",  # missing-trailing-comma
      "D100",    # missing docstring in public module
      "D104",    # missing docstring in public package
      "D106",    # missing docstring in public nested class
      "PLR0913", # too-many-arguments
      "W191",    # indentation contains tabs
  ]
  
  # Allow fix for all enabled rules (when `--fix`) is provided.
  fixable = ["ALL"]
  unfixable = []
  
  [extend-per-file-ignores]
  "{tests/*,test*.py}" = [
      "ANN",    # flake8-annotations
      "INP001", # implicit-namespace-package
      "N802",   # invalid-function-name
      "PD901",  # pandas-df-variable-name
      "S",      # flake8-bandit
      "S301",   # suspicious-pickle-usage
      "SLF001", # private-member-access
  ]
  
  "{admin.py,models.py,serializers.py,views.py}" = [
      "RUF012",    # mutable-class-default
  ]
  
  [format]
  # Use single quotes for strings.
  quote-style = "single"
  
  # Like Black, indent with spaces, rather than tabs.
  indent-style = "space"
  
  # Like Black, respect magic trailing commas.
  skip-magic-trailing-comma = false
  
  # Like Black, automatically detect the appropriate line ending.
  line-ending = "auto"
  
  [pydocstyle]
  convention = "numpy"
  ```


---

_Renamed from "[Panic]" to "[Panic] Ruff crashes when adding a function to a Django REST Framework view" by @lmanc on 2024-04-19 13:00_

---

_Comment by @MichaReiser on 2024-04-19 13:02_

Thanks for opening the issue and the in detail description.

@dhruvmanila Is my assumption correct that this was probably fixed by https://github.com/astral-sh/ruff/pull/11032 ?

---

_Comment by @lmanc on 2024-04-19 13:03_

I've noticed that it happens every time I use the keyword `def` in any class.

---

_Renamed from "[Panic] Ruff crashes when adding a function to a Django REST Framework view" to "[Panic] Ruff VSCode extensin crashes every time the word `def` is written in a class" by @lmanc on 2024-04-19 13:04_

---

_Renamed from "[Panic] Ruff VSCode extensin crashes every time the word `def` is written in a class" to "[Panic] Ruff VSCode extension crashes every time the word `def` is written in a class" by @lmanc on 2024-04-19 13:04_

---

_Comment by @lmanc on 2024-04-19 13:08_

And it crashes as soon as I type the space after `def`.

---

_Comment by @dhruvmanila on 2024-04-19 13:35_

Yes, it's fixed on the latest extension version. Can you try it out? I can confirm it's not happening now.

---

_Label `bug` added by @charliermarsh on 2024-04-19 13:52_

---

_Label `parser` added by @charliermarsh on 2024-04-19 13:52_

---

_Comment by @lmanc on 2024-04-19 14:30_

I updated the extension to `v2024.20.0` and restarted all the extensions on VSCode. Now it's not happening to me either.

Impressively fast, thanks!

---

_Closed by @MichaReiser on 2024-04-19 14:45_

---

_Referenced in [astral-sh/ruff#11105](../../astral-sh/ruff/issues/11105.md) on 2024-04-23 15:26_

---
