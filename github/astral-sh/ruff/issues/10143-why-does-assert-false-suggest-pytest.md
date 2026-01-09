---
number: 10143
title: Why does assert False suggest pytest?
type: issue
state: closed
author: kaddkaka
labels:
  - question
assignees: []
created_at: 2024-02-27T06:52:10Z
updated_at: 2024-02-28T01:00:29Z
url: https://github.com/astral-sh/ruff/issues/10143
synced_at: 2026-01-07T13:12:15-06:00
---

# Why does assert False suggest pytest?

---

_Issue opened by @kaddkaka on 2024-02-27 06:52_

Upon linting a python file with only this content, ruff suggests to use pytest, why?

```py
assert False
```

`` ruff: Assertion always fails, replace with `pytest.fail()` [PT015] ``

There is nothing in filename that suggests that this is a test, so, why?

---

_Comment by @mikeleppane on 2024-02-27 07:33_

Hi! If you enable this rule, it does not care about the file name or whether it is a normal source or a test file. You could specify and include PT rules only for test files. However, I agree, there may be room for improvement. We could try to figure out if it is a test file based on the conventions [Python test discovery](https://docs.pytest.org/en/7.2.x/explanation/goodpractices.html#conventions-for-python-test-discovery). I'm not 100% sure if there are some edge cases on top of this. 

---

_Comment by @kaddkaka on 2024-02-27 07:44_

Using standard pytest discovery seems reasonable. Including understanding pytest ini options such as

* `pyproject.toml`:

```toml
[tool.pytest.ini_options]
addopts = """\
--ignore=stupid_file.py"""
```

---

_Comment by @MichaReiser on 2024-02-27 08:47_

For now, you can ignore the rule yourself by either ignoring the rule for all non test files using [`per-file-ignores`](https://docs.astral.sh/ruff/settings/#lint_per-file-ignores) or you can create a `ruff.toml` in your test directory (assuming all tests are in one directory), extend your root configuration by setting `extend = "../pyproject.toml"`, and enabling the pytest rules `extend-select = ["PT"]`

---

_Label `question` added by @MichaReiser on 2024-02-27 08:47_

---

_Comment by @charliermarsh on 2024-02-28 01:00_

I think this can be tracked in https://github.com/astral-sh/ruff/issues/8794.

---

_Closed by @charliermarsh on 2024-02-28 01:00_

---
