---
number: 2529
title: Q000 different quotes in string
type: issue
state: closed
author: FieryDruid
labels: []
assignees: []
created_at: 2023-02-03T11:46:41Z
updated_at: 2023-02-03T12:39:11Z
url: https://github.com/astral-sh/ruff/issues/2529
synced_at: 2026-01-07T13:12:14-06:00
---

# Q000 different quotes in string

---

_Issue opened by @FieryDruid on 2023-02-03 11:46_

ruff  `0.0.240`

I use single quotes for strings, settings from `.toml` file:
```toml
[tool.ruff.flake8-quotes]
docstring-quotes = "double"
inline-quotes = "single"
multiline-quotes = "double"
```
Case:
```python
test_1 = 'hello'
test_2 = "hello"
test_3 = "hello 'world'"
```
On this case `ruff` show error only for `test_2` string. I think it is because i use single quotes inside string in `test_3`


---

_Comment by @ngnpope on 2023-02-03 12:25_

This is intentional. In `flake8-quotes`, they made the [decision](https://github.com/zheller/flake8-quotes#major-update-in-200) to avoid escaping quotes as per PEP 8. In other words, if single quotes is configured, `"hello 'world'"` is preferable to `'hello \'world\''`.

If you really want the latter, you can set `avoid-escape = false` as per the [documentation](https://github.com/charliermarsh/ruff#avoid-escape).

---

_Closed by @charliermarsh on 2023-02-03 12:38_

---

_Comment by @FieryDruid on 2023-02-03 12:39_

Thanks!

---
