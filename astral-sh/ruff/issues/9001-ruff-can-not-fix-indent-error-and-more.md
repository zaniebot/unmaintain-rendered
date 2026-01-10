```yaml
number: 9001
title: ruff can not fix indent error and more whitespaces on function parameters (but black works well)
type: issue
state: closed
author: lgf4591
labels:
  - question
assignees: []
created_at: 2023-12-05T00:02:10Z
updated_at: 2023-12-06T00:21:32Z
url: https://github.com/astral-sh/ruff/issues/9001
synced_at: 2026-01-10T11:09:51Z
```

# ruff can not fix indent error and more whitespaces on function parameters (but black works well)

---

_Issue opened by @lgf4591 on 2023-12-05 00:02_


# Code
```python
def main(   num   ):
            print(f"the num is: {num}")
```

# ruff config
```toml
[tool.black]
target-version = ["py37"]
line-length = 120
skip-string-normalization = true

[tool.ruff]
target-version = "py37"
line-length = 120
indent-width = 2
select = [
  "A",
  "ARG",
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
  "PLR",
  "PLW",
  "Q",
  "RUF",
  "S",
  "T",
  "TID",
  "UP",
  "W",
  "YTT",
]
ignore = [
  # Allow non-abstract empty methods in abstract base classes
  "B027",
  # Allow boolean positional values in function calls, like `dict.get(... True)`
  "FBT003",
  # Ignore checks for possible passwords
  "S105",
  "S106",
  "S107",
  # Ignore complexity
  "C901",
  "PLR0911",
  "PLR0912",
  "PLR0913",
  "PLR0915",
  # T201 `print` found
  "T201",
]
unfixable = [
  # Don't touch unused imports
  "F401",
]
```

---

_Comment by @charliermarsh on 2023-12-05 00:03_

Have you tried running `ruff format`, instead of `ruff check`?

---

_Label `question` added by @charliermarsh on 2023-12-05 00:03_

---

_Comment by @lgf4591 on 2023-12-05 09:20_

yes, but it doesn't work

---

_Comment by @MichaReiser on 2023-12-05 09:30_

I tried to reproduce the issue with the given configuration using `ruff format` but haven't been able to. 

```bash
~/astral/test
➜ ls
main.py		pyproject.toml

~/astral/test
➜ ../ruff/target/debug/ruff format
warning: The following rules may cause conflicts when used with the formatter: `ISC001`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
warning: Detected debug build without --no-cache.
1 file reformatted

~/astral/test
➜ bat main.py
───────┬─────────────────────────────────────────────────────────────────────────────────────────────
       │ File: main.py
───────┼─────────────────────────────────────────────────────────────────────────────────────────────
   1   │ def main(num):
   2   │   print(f"the num is: {num}")
───────┴─────────────────────────────────────────────────────────────────────────────────────────────
```

Can you list the content of your directory and share how you invoke ruff?

---

_Comment by @lgf4591 on 2023-12-05 11:40_

thx!, I tested it in another PC and it works well with "ruff format ." , then I reinstall my vscode on my PC, it work well now

by the way, why "ruff check . --fix" command doesn't work?

---

_Closed by @lgf4591 on 2023-12-05 11:42_

---

_Comment by @MichaReiser on 2023-12-06 00:21_

Hmm that's surprising. Not sure what's happening. Have you checked that both computers use the same ruff version? 

Or do you have a user-level configuration on the computer where formatting isn't working (only if you aren't using the latest version of ruff). You can use the `--show-settings` option for debugging. The output is rather bad (we plan to improve it), but it prints the name of all configurations ruff takes into consideration:

```bash
ruff check --show-setting
```

---
