---
number: 12321
title: "New rule: Validate form feeds in leading white space"
type: issue
state: closed
author: dscorbett
labels:
  - rule
assignees: []
created_at: 2024-07-14T18:18:14Z
updated_at: 2025-02-10T00:23:49Z
url: https://github.com/astral-sh/ruff/issues/12321
synced_at: 2026-01-07T13:12:15-06:00
---

# New rule: Validate form feeds in leading white space

---

_Issue opened by @dscorbett on 2024-07-14 18:18_

A form feed [has an undefined effect in leading white space except at the start of the line](https://docs.python.org/3/reference/lexical_analysis.html#indentation). A rule to validate that a form feed does not appear after indentation could be useful. A related rule is E113, but that doesn’t handle form feeds.

In practice, a form feed resets the indentation, which can be very misleading.

```console
$ printf 'if False:\n    print("F")\n    \fprint("T")\n' >form-feed.py
$ cat form-feed.py
if False:
    print("F")
    
    print("T")
$ python form-feed.py
T
```

---

_Comment by @MichaReiser on 2024-07-15 06:22_

This seems reasonable and should be very cheap to implement with `memfind`. 

---

_Label `rule` added by @MichaReiser on 2024-07-15 06:22_

---

_Referenced in [astral-sh/ruff#16049](../../astral-sh/ruff/pulls/16049.md) on 2025-02-09 00:39_

---

_Closed by @ntBre on 2025-02-10 00:23_

---

_Closed by @ntBre on 2025-02-10 00:23_

---
