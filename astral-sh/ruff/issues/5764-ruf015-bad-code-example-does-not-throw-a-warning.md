---
number: 5764
title: RUF015 bad code example does not throw a warning
type: issue
state: closed
author: jschippergoauto
labels:
  - bug
assignees: []
created_at: 2023-07-14T18:26:55Z
updated_at: 2023-07-21T00:08:10Z
url: https://github.com/astral-sh/ruff/issues/5764
synced_at: 2026-01-10T01:22:44Z
---

# RUF015 bad code example does not throw a warning

---

_Issue opened by @jschippergoauto on 2023-07-14 18:26_

Ruff version: 0.0.278
Command: `ruff --config=ruff.toml file.py`
`ruff.toml`:
```toml
extend-select = [
  "RUF",
]
```
Code snippet:
```python
head = list(range(1000000000000))[0]
```

The code snippet was taken from the [Ruff documentation](https://beta.ruff.rs/docs/rules/unnecessary-iterable-allocation-for-first-element/)

I expected it to give me a RUF015 warning but instead it completely ignores it. Changing the snippet to the following does throw a warning.
```python
x = range(1000000000000)
head = list(x)[0]
```

---

_Label `bug` added by @charliermarsh on 2023-07-14 19:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-07-14 20:47_

---

_Referenced in [astral-sh/ruff#5767](../../astral-sh/ruff/pulls/5767.md) on 2023-07-14 23:37_

---

_Closed by @charliermarsh on 2023-07-21 00:08_

---
