---
number: 13676
title: "Check whether {filename} conflicts with standard library"
type: issue
state: closed
author: Riezebos
labels: []
assignees: []
created_at: 2024-10-08T09:44:24Z
updated_at: 2024-10-09T07:41:34Z
url: https://github.com/astral-sh/ruff/issues/13676
synced_at: 2026-01-07T13:12:15-06:00
---

# Check whether {filename} conflicts with standard library

---

_Issue opened by @Riezebos on 2024-10-08 09:44_

I think this would be a useful rule, I found a few other rules that use `{filename}` (e.g. INP001) but as far as I can tell this one doesn't exist.

The rule would check whether `{filename}` is the same as any module in the standard library. 

Creating a file like that can cause problems, e.g.:
```
   # bisect.py
   ...

   # main.py
   import random

with:

   Traceback (most recent call last):
     File ".../foo.py", line 1, in <module>
       import random
     File "/usr/lib/python3.12/random.py", line 62, in <module>
       from bisect import bisect as _bisect
   ImportError: cannot import name 'bisect' from 'bisect'
```

This rule would help prevent these types of errors.

---

_Comment by @ember91 on 2024-10-08 12:34_

A bit unrelated but Python 3.13 will include an improved error message of this: https://docs.python.org/3.13/whatsnew/3.13.html#improved-error-messages

---

_Comment by @autinerd on 2024-10-09 05:24_

There is A005 https://docs.astral.sh/ruff/rules/builtin-module-shadowing/ (which is currently in preview), does this rule show a violation?

---

_Comment by @Riezebos on 2024-10-09 07:41_

Yes, that's exactly what I was looking for! Sorry for not finding it before.

---

_Closed by @Riezebos on 2024-10-09 07:41_

---
