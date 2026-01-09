---
number: 6913
title: Rule RUF001 causes autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-08-27T10:47:31Z
updated_at: 2023-08-27T13:44:48Z
url: https://github.com/astral-sh/ruff/issues/6913
synced_at: 2026-01-07T13:12:15-06:00
---

# Rule RUF001 causes autofix error

---

_Issue opened by @qarmin on 2023-08-27 10:47_

Ruff 0.0.286 (latest changes from main branch)

```
ruff  *.py --select ALL --no-cache
```

file content:
```
UNICODE_PUNCTUATION = frozenset(
        "＂",
    )
```

error:
```
error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `_unicode_punctuation3203.py`, the rule codes RUF001, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

_unicode_punctuation3203.py:1:1: EXE002 The file is executable but no shebang is present
_unicode_punctuation3203.py:1:1: CPY001 Missing copyright notice at top of file
_unicode_punctuation3203.py:2:10: RUF001 String contains ambiguous `＂` (FULLWIDTH QUOTATION MARK). Did you mean `"` (QUOTATION MARK)?
```



[_unicode_punctuation3203.py.zip](https://github.com/astral-sh/ruff/files/12447784/_unicode_punctuation3203.py.zip)


---

_Comment by @charliermarsh on 2023-08-27 13:44_

I believe this is the same as https://github.com/astral-sh/ruff/issues/4519 and others.

---

_Closed by @charliermarsh on 2023-08-27 13:44_

---

_Label `bug` added by @charliermarsh on 2023-08-27 13:44_

---

_Label `fuzzer` added by @charliermarsh on 2023-08-27 13:44_

---
