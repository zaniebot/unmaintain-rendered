---
number: 5621
title: "[Linter panic] IPykernel Debugger Module"
type: issue
state: closed
author: blink1073
labels:
  - bug
assignees: []
created_at: 2023-07-08T17:51:06Z
updated_at: 2023-07-08T19:05:45Z
url: https://github.com/astral-sh/ruff/issues/5621
synced_at: 2026-01-07T13:12:15-06:00
---

# [Linter panic] IPykernel Debugger Module

---

_Issue opened by @blink1073 on 2023-07-08 17:51_


* A minimal code snippet that reproduces the bug.
https://github.com/ipython/ipykernel/blob/1270d5a6e17845e6b22361e43325dd2de59d6392/ipykernel/debugger.py

* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
`ruff --fix`

* The current Ruff settings (any relevant sections from your `pyproject.toml`).

```

[tool.ruff]
target-version = "py37"
line-length = 100
select = [
  "A",
  "B",
  "C",
  "DTZ",
  "E",
  "EM",
  "F",
  "FBT",
  "I",
  "ICN",
  "ISC",
  "N",
  "PLC",
  "PLE",
  "PLW",
  "Q",
  "RUF",
  "S",
  "SIM",
  "T",
  "TID",
  "UP",
  "W",
  "YTT",
]
ignore = [
  # Allow non-abstract empty methods in abstract base classes
  "B027",
  # Ignore McCabe complexity
  "C901",
  # Allow boolean positional values in function calls, like `dict.get(... True)`
  "FBT003",
  # Use of `assert` detected
  "S101",
  # Line too long
  "E501",
  # Relative imports are banned
  "TID252",
  # Boolean ... in function definition
  "FBT001", "FBT002",
  # Module level import not at top of file
  "E402",
  # A001/A002/A003 .. is shadowing a python builtin
  "A001", "A002", "A003",
  # Possible hardcoded password
  "S105", "S106",
  # Q000 Single quotes found but double quotes preferred
  "Q000",
  # N806 Variable `B` in function should be lowercase
  "N806",
  # T201 `print` found
  "T201",
  # N802 Function name `CreateWellKnownSid` should be lowercase
  "N802", "N803",
  # C408 Unnecessary `dict` call (rewrite as a literal)
  "C408",
  # N801 Class name `directional_link` should use CapWords convention
  "N801",
  # SIM105 Use `contextlib.suppress(ValueError)` instead of try-except-pass
  "SIM105",
  # S110 `try`-`except`-`pass` detected
  "S110",
]
unfixable = [
  # Don't touch print statements
  "T201",
  # Don't touch noqa lines
  "RUF100",
]

[tool.ruff.per-file-ignores]
# B011 Do not call assert False since python -O removes these calls
# F841 local variable 'foo' is assigned to but never used
# C408 Unnecessary `dict` call
# E402 Module level import not at top of file
# T201 `print` found
# B007 Loop control variable `i` not used within the loop body.
# N802 Function name `assertIn` should be lowercase
# F841 Local variable `t` is assigned to but never used
# EM101 Exception must not use a string literal, assign to variable first
# PLR2004 Magic value used in comparison
# PLW0603 Using the global statement to update ...
# PLW2901 `for` loop variable ...
# PLC1901 `stderr == ""` can be simplified to `not stderr` as an empty string is falsey
# B018 Found useless expression. Either assign it to a variable or remove it.
# S603 `subprocess` call: check for execution of untrusted input
"ipykernel/tests/*" = ["B011", "F841", "C408", "E402", "T201", "B007", "N802", "F841", "EM101",
    "EM102", "EM103", "PLR2004", "PLW0603", "PLW2901", "PLC1901", "B018", "S603"]
```

* The current Ruff version (`ruff --version`).  `276`

```
panicked at 'range start index 2 out of range for slice of length 1', crates/ruff/src/rules/isort/block.rs:159:36
Backtrace:    0: <unknown>
   1: <unknown>
   2: <unknown>
   3: <unknown>
   4: <unknown>
   5: <unknown>
   6: <unknown>
   7: <unknown>
   8: <unknown>
   9: <unknown>
  10: <unknown>
  11: <unknown>
  12: <unknown>
  13: <unknown>
  14: <unknown>
  15: <unknown>
  16: <unknown>
  17: <unknown>
  18: <unknown>
  19: <unknown>
  20: <unknown>
  21: <unknown>
  22: <unknown>
  23: <unknown>
  24: <unknown>
  25: <unknown>
  26: <unknown>
```


---

_Comment by @charliermarsh on 2023-07-08 18:05_

Thanks, will take a look!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-08 18:10_

---

_Label `bug` added by @charliermarsh on 2023-07-08 18:10_

---

_Comment by @charliermarsh on 2023-07-08 18:13_

If you need an immediate workaround, you can remove the `# isort: skip` comments. But I'll make sure this is fixed in the next release.

---

_Comment by @blink1073 on 2023-07-08 18:14_

We're fine to ignore the warning for now, thanks!

---

_Referenced in [astral-sh/ruff#5623](../../astral-sh/ruff/pulls/5623.md) on 2023-07-08 18:55_

---

_Closed by @charliermarsh on 2023-07-08 19:05_

---
