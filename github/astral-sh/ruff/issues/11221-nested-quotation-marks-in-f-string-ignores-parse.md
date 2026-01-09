---
number: 11221
title: Nested quotation marks in f-string, ignores parse error
type: issue
state: closed
author: andy-lz
labels:
  - parser
assignees: []
created_at: 2024-04-30T21:59:52Z
updated_at: 2024-04-30T22:06:52Z
url: https://github.com/astral-sh/ruff/issues/11221
synced_at: 2026-01-07T13:12:15-06:00
---

# Nested quotation marks in f-string, ignores parse error

---

_Issue opened by @andy-lz on 2024-04-30 21:59_

In Python 3.9.18, the below code will succeed silently with ruff format and ruff check, even though it has a SyntaxError:

```
# file.py
my_str = f"1+1={"2"}"


>> ruff format file.py #succeeds
>> python file.py  # fails with SyntaxError
```

---

_Label `parser` added by @AlexWaygood on 2024-04-30 22:04_

---

_Comment by @AlexWaygood on 2024-04-30 22:06_

Thanks for opening the issue! I'm going to close this as a duplicate of #6591 (the syntax in question here is legal on Python 3.12+, but not on lower versions)

---

_Closed by @AlexWaygood on 2024-04-30 22:06_

---
