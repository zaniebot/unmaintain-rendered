---
number: 6186
title: Raw string formatting should not remove quote escape backslahes
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
assignees: []
created_at: 2023-07-31T08:42:16Z
updated_at: 2023-07-31T08:57:35Z
url: https://github.com/astral-sh/ruff/issues/6186
synced_at: 2026-01-07T13:12:15-06:00
---

# Raw string formatting should not remove quote escape backslahes

---

_Issue opened by @konstin on 2023-07-31 08:42_

Currently, `print(r"\"" + "|" + r"\"''")` gets formatted as `print(r'"' + "|" + r"\"''")`, but raw strings keep the backslash so they not equivalent:

```
> print(r"\"" + "|" + r"\"''")
\"|\"''
> print(r'"' + "|" + r"\"''")
"|\"''
```

---

_Label `bug` added by @konstin on 2023-07-31 08:42_

---

_Label `formatter` added by @konstin on 2023-07-31 08:42_

---

_Referenced in [astral-sh/ruff#6166](../../astral-sh/ruff/pulls/6166.md) on 2023-07-31 08:45_

---

_Comment by @MichaReiser on 2023-07-31 08:48_

Is this the same as https://github.com/astral-sh/ruff/issues/5941?

---

_Comment by @konstin on 2023-07-31 08:57_

yes :see_no_evil: 

---

_Closed by @konstin on 2023-07-31 08:57_

---
