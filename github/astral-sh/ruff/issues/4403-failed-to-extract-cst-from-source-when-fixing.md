---
number: 4403
title: "`Failed to extract CST from source` when fixing file with assertions"
type: issue
state: closed
author: qarmin
labels:
  - bug
assignees: []
created_at: 2023-05-12T21:15:14Z
updated_at: 2023-05-12T21:35:50Z
url: https://github.com/astral-sh/ruff/issues/4403
synced_at: 2026-01-07T13:12:14-06:00
---

# `Failed to extract CST from source` when fixing file with assertions

---

_Issue opened by @qarmin on 2023-05-12 21:15_

Ruff f5be3d8e5b2e3f3a0c5890075b552371f4061023

Command - `ruff --fix` with config with all rules enabled
```
assert False or False  # [condition-evals-to-constant]
assert True and True  # [condition-evals-to-constant]
```
cause to show error 
```
error: Failed to create fix for PytestCompositeAssertion: Failed to extract CST from source
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_1364702729.py:1:1: D100 Missing docstring in public module
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_1364702729.py:1:1: S101 Use of `assert` detected
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_1364702729.py:1:1: INP001 File `Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_1364702729.py` is part of an implicit namespace package. Add an `__init__.py`.
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_1364702729.py:2:1: S101 Use of `assert` detected
Desktop/RunEveryCommand/ruff/Broken/PY_FILE_TEST_1364702729.py:2:1: PT018 Assertion should be broken down into multiple parts
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-12 21:25_

---

_Label `bug` added by @charliermarsh on 2023-05-12 21:25_

---

_Referenced in [astral-sh/ruff#4405](../../astral-sh/ruff/pulls/4405.md) on 2023-05-12 21:35_

---

_Closed by @charliermarsh on 2023-05-12 21:35_

---
