---
number: 7784
title: "Invalid fix for `UP025` when implicitly concatenated with f-string"
type: issue
state: closed
author: dhruvmanila
labels:
  - bug
  - fixes
assignees: []
created_at: 2023-10-03T15:56:25Z
updated_at: 2023-11-16T02:30:43Z
url: https://github.com/astral-sh/ruff/issues/7784
synced_at: 2026-01-07T13:12:15-06:00
---

# Invalid fix for `UP025` when implicitly concatenated with f-string

---

_Issue opened by @dhruvmanila on 2023-10-03 15:56_

Note that this is unrelated to the new f-string changes.

If a unicode string is implicitly concatenated with a f-string, then the fix removes the first character of the string instead of the prefix:

```python
u"foo" f"{bar}"
```

The fix:
```console
$ pipx run "ruff==0.0.291" check --select=UP  ~/playground/ruff/fstring.py --ecosystem-ci
/Users/dhruv/playground/ruff/fstring.py:1:3: UP025 [*] Remove unicode literals from strings
ℹ Fix
1   |-u"foo" f"{bar}"
  1 |+u"oo" f"{bar}"

Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

This is because the "foo" string is part of the outer f-string expression and the range of the string constant is within the quotes.

---

_Label `bug` added by @dhruvmanila on 2023-10-03 15:56_

---

_Label `autofix` added by @dhruvmanila on 2023-10-03 15:56_

---

_Comment by @harupy on 2023-10-10 01:45_

@dhruvmanila Can I work on this?

---

_Comment by @dhruvmanila on 2023-10-10 13:51_

I would recommend to wait for this a bit as we could change the AST representation for an implicit string concatenation node which then would make this easier to solve.

---

_Comment by @harupy on 2023-10-10 13:58_

@dhruvmanila Sounds good!

---

_Referenced in [astral-sh/ruff#8709](../../astral-sh/ruff/pulls/8709.md) on 2023-11-16 02:19_

---

_Closed by @charliermarsh on 2023-11-16 02:30_

---
