```yaml
number: 14149
title: Which rule should take precedence, S101 or PT009?
type: issue
state: closed
author: jayce-kim1123
labels:
  - question
assignees: []
created_at: 2024-11-07T09:02:03Z
updated_at: 2024-11-07T23:40:02Z
url: https://github.com/astral-sh/ruff/issues/14149
synced_at: 2026-01-12T15:54:53Z
```

# Which rule should take precedence, S101 or PT009?

---

_@jayce-kim1123_

Hi there
I have a question about the priority of linting rules.

This is the original code.
```
# test_something.py

self.assertEqual(response.status_code, status.HTTP_200_OK)
```

I received the 'Use a regular assert instead of unittest-style assertEqual Ruff(PT009)'
Based on this warning, i modified the code as below.

```
# test_something.py

assert response.status_code == status.HTTP_200_OK
```

I received the ‘Use of assert detected(S101)’ warning after modifying my code according to PT009.

I would like to ignore one of these warnings without using an if.
Which rule should I follow?

Thanks for your time and for maintaining this awesome project.

---
Keywords searched: “S101”, “PT009”, “unittest”

---
Ruff version: 0.7.1

---
Ruff settings:
```
# pyproject.toml

[tool.ruff]
include = [
]

exclude = [
    "oniondesk/call_log/**/*.py",
    ".bzr",
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

line-length = 120

indent-width = 4

target-version = "py37"

[tool.ruff.lint]
select = [
    "F",
    "E",
    "W",
    "I",
    "N",
    "D",
    "UP",
    "YTT",
    "ANN",
    "ASYNC",
    "S",
    "BLE",
    "FBT",
    "B",
    "A",
    "COM",
    "C",
    "DTZ",
    "T10",
    "DJ",
    "EM",
    "EXE",
    "FA",
    "ISC",
    "ICN",
    "LOG",
    "G",
    "PIE",
    "T20",
    "PYI",
    "PT",
    "Q",
    "RSE",
    "RET",
    "SLF",
    "SLOT",
    "SIM",
    "TID",
    "TCH",
    "INT",
    "ARG",
    "PTH",
    "TD",
    "PD",
    "PGH",
    "PLC",
    "PLE",
    "PLR",
    "PLW",
    "TRY",
    "FLY",
    "NPY",
    "PERF",
    "FURB",
]

ignore = [
    "D100",
    "D101",
    "D102",
    "D103",
    "D104",
    "D105",
    "D106",
    "D107",
    "D203",
    "D213",
    "S110",
    "S112",
    "CPY001",
    "LOG002",
    "PYI",
    "TD003",
    "FIX",
    "ERA001",
    "PLC0206",
    "PLC0415",
    "FAST",
    "AIR",
    "ISC001",
    "DOC",
    "RUF",
]

fixable = [
    "F",
    "E",
    "W",
    "I001",
    "D",
    "UP",
    "COM",
    "Q",
]

unfixable = []

dummy-variable-rgx = "^(_+|(_+[a-zA-Z0-9_]*[a-zA-Z0-9]+?))$"

[tool.ruff.format]
quote-style = "double"

indent-style = "space"

skip-magic-trailing-comma = false

line-ending = "auto"

docstring-code-format = false

docstring-code-line-length = "dynamic"
```

---

_Comment by @Daverball on 2024-11-07 12:25_

You can safely ignore S101 in test code, it's only really relevant for runtime code and only if you are worried that your code will break in unexpected ways if those assertions are removed during compilation. E.g. for static typing it's quite common to use `assert`, but you tend to not care if those asserts are optimized away, in fact you may actually want them to be removed for better performance, and only be active during testing.

If you wanted to ignore this code in just your tests you would need to add a `extend-per-file-ignores` section to your `pyproject.toml`.

E.g. like this if all your tests live in a `tests` folder in the root of your project:
```toml
[tool.ruff.lint.extend-per-file-ignores]
"tests/**/*.py" = ["S101"]
```

There's probably other codes you don't want to enable for tests.

---

_Label `question` added by @MichaReiser on 2024-11-07 12:26_

---

_Comment by @jayce-kim1123 on 2024-11-07 23:39_

it works!
I appreciate your help 👍 

---

_Closed by @jayce-kim1123 on 2024-11-07 23:40_

---
