```yaml
number: 9014
title: "[Fix error]Unpacking an empty dict with PIE804"
type: issue
state: closed
author: Blumenkind111
labels:
  - bug
assignees: []
created_at: 2023-12-05T20:13:30Z
updated_at: 2023-12-05T21:30:30Z
url: https://github.com/astral-sh/ruff/issues/9014
synced_at: 2026-01-10T11:09:51Z
```

# [Fix error]Unpacking an empty dict with PIE804

---

_Issue opened by @Blumenkind111 on 2023-12-05 20:13_

Hi all,

The following code seems to be unformattable by ruff with Rule PIE804

> def test_function(*args, **kwargs):
>     pass
> 
> test_function(
>         arg1=1016,
>         arg2=25.4,
>         **{},
> )

The error is: `error: Fix introduced a syntax error. Reverting all changes.`

I could break it down to this setting: `ruff check testfile.py --select=PIE804 --fix`

version is ruff 0.1.7

It's quite obvious, that ruff fails to unpack the empty dict into keyword-arguments, but when reformatting an old codebase it would be nice, if this (admittedly quite sketchy piece of code) would not crash the fix for the complete file. 

---

_Label `bug` added by @charliermarsh on 2023-12-05 20:25_

---

_Comment by @charliermarsh on 2023-12-05 20:25_

Thanks for filing!

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-05 20:27_

---

_Comment by @charliermarsh on 2023-12-05 20:27_

(The issue is the trailing comma.)

---

_Closed by @charliermarsh on 2023-12-05 21:30_

---
