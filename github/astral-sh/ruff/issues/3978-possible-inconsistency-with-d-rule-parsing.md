---
number: 3978
title: "Possible inconsistency with \"D\" rule parsing docstrings"
type: issue
state: closed
author: syntaxaire
labels:
  - docstring
assignees: []
created_at: 2023-04-14T20:36:05Z
updated_at: 2023-06-06T01:36:57Z
url: https://github.com/astral-sh/ruff/issues/3978
synced_at: 2026-01-07T13:12:14-06:00
---

# Possible inconsistency with "D" rule parsing docstrings

---

_Issue opened by @syntaxaire on 2023-04-14 20:36_

With ruff 0.0.261, using the "D" rule with  `--fix`, the following code:
```python
"""
Do a fix.
Workaround for:
BUG: https://github.com/someproject/somerepo/issues/1337
"""
```
is autocorrected to:
```python
"""Do a fix.
Workaround for:
BUG: https://github.com/someproject/somerepo/issues/1337.
"""
```
Note the addition of a period in line 3. But inserting a newline into the original code causes that period not to be added:
```python
"""
Do a fix.

Workaround for:
BUG: https://github.com/someproject/somerepo/issues/1337
"""
```
is autocorrected to
```python
"""Do a fix.

Workaround for:
BUG: https://github.com/someproject/somerepo/issues/1337
"""
```
Note the lack of a period after the link.

---

_Comment by @syntaxaire on 2023-04-14 20:44_

configuration in `pyproject.toml` is:
```
[tool.ruff]
select = [
  "A", "ARG", "B", "BLE", "C4", "C90", "D", "DTZ", "E", "ERA", "F", "FBT", "G", "I", "ISC", "PGH", "PIE", "PL", "PT", "PTH", "PYI", "Q", "RET", "RSE", "RUF", "S", "SIM", "T10", "T20", "TRY", "UP", "W", "YTT"
]

unfixable =  [
  "ERA001",
  "RUF002",
]
```

---

_Comment by @charliermarsh on 2023-04-14 21:09_

Yeah it's a little challenging to get this right in all cases. Broadly, when checking if the docstring ends in a period, I believe we look for the "summary line", which is considered to be all content up to the first paragraph break.

This is to support docstrings like:

```
"""Here's content and
here's more flowing content onto
the next lines
"""
```

Here, we put the period after `lines`, rather than after `and`. But that clearly leads to strange output in the case above.


---

_Label `docstring` added by @charliermarsh on 2023-04-14 21:09_

---

_Comment by @charliermarsh on 2023-04-14 21:11_

(Not certain what if anything should change -- previous comment is purely informational for now.)

---

_Comment by @charliermarsh on 2023-06-06 01:36_

Closing this as "expected" for now.

---

_Closed by @charliermarsh on 2023-06-06 01:36_

---
