```yaml
number: 12245
title: Avoid raising S310 on safe f-strings
type: issue
state: closed
author: dscorbett
labels:
  - rule
  - help wanted
assignees: []
created_at: 2024-07-08T16:19:17Z
updated_at: 2024-07-13T20:57:06Z
url: https://github.com/astral-sh/ruff/issues/12245
synced_at: 2026-01-12T15:54:51Z
```

# Avoid raising S310 on safe f-strings

---

_@dscorbett_

Given this input file (s310_fstring.py):
```python
from urllib.request import urlopen
foo = "foo"
urlopen(f"https://www.example.com/{foo}")
```
Ruff reports a violation of rule S310.
```console
$ ruff check --select S310 --output-format concise s310_fstring.py
s310_fstring.py:3:1: S310 Audit URL open for permitted schemes. Allowing use of `file:` or custom schemes is often unexpected.
Found 1 error.
```
Even though the argument is an f-string, Ruff should be able to detect that it begins with a permitted scheme. Compare #8040.

---

_Comment by @dhruvmanila on 2024-07-09 12:09_

Yeah, I think this makes sense. We can check for the first f-string literal part and verify if it begins with `http` or `https` similar to string literal.

---

_Label `rule` added by @dhruvmanila on 2024-07-09 12:09_

---

_Label `help wanted` added by @dhruvmanila on 2024-07-09 12:09_

---

_Closed by @charliermarsh on 2024-07-13 20:57_

---
