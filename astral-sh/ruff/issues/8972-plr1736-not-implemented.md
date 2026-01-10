---
number: 8972
title: PLR1736 not implemented?
type: issue
state: closed
author: Ryang20718
labels:
  - question
assignees: []
created_at: 2023-12-02T21:47:05Z
updated_at: 2023-12-02T22:04:59Z
url: https://github.com/astral-sh/ruff/issues/8972
synced_at: 2026-01-10T01:22:48Z
---

# PLR1736 not implemented?

---

_Issue opened by @Ryang20718 on 2023-12-02 21:47_

Error:

```
        ruff failed
          Cause: Failed to parse `/home/ryang/repos/av/.ruff.toml`: TOML parse error at line 1, column 1
          |
        1 | # Enable pylint error, pylint warning, flake8-copyright
          | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        Unknown rule selector: `PLR1736`
```

In my ruff.toml, i have

```
# Enable pylint error, pylint warning, flake8-copyright
select = ["PLE", "PLW", "CPY", "E"]

extend-select = [
  "W291",  # trailing-whitespace
  "PLC0414",  # useless-import-alias
  "PLC2401",  # non-ascii-name
  "PLC3002",  # unnecessary-direct-lambda-call
  "F706",  # return-outside-function
  "F704",  # yield-outside-function
  "PLR0124",  # comparison-with-itself
  "PLR0206",  # property-with-parameters
  "PLR1704",  # redefined-argument-from-local
  "PLR1711",  # useless-return
  "C416",  # unnecessary-comprehension
  "B033",  # duplicate-value
  "F401",  # unused-import
  "F841",  # unused-variable
  "SIM118",  # consider-iterating-dictionary
  # "C3001",  # unnecessary-lambda-assignment
  # "C3002",  # unnecessary-direct-lambda-call
  "N815",  # invalid-name
  "PLC0105",  # typevar-name-incorrect-variance
  "PLR0133",  # comparison-of-constants
  "PLR1704",  # redefined-argument-from-local
  "PLR1714",  # consider-using-in
  "PLR1736",  # unnecessary-list-index-lookup
  "C417",  # consider-using-generator
  "C405",  # use-list-literal
  "C406",  # use-dict-literal
  "B006",  # dangerous-default-value
  "B018",  # pointless-statement
  "ARG001",  # unused-argument
  "T100",  # forgotten-debug-statement
]
```


Command to repro:
`ruff <any file> --fix`

```
Ruff version
0.1.6
```




---

_Comment by @charliermarsh on 2023-12-02 22:04_

`PLR1736` is implemented but it hasn't been released yet! So it exists on `main` but not in the latest version you can install from PyPI. That selector will work when we cut our next release, probably early next week.

---

_Closed by @charliermarsh on 2023-12-02 22:04_

---

_Label `question` added by @charliermarsh on 2023-12-02 22:04_

---
