```yaml
number: 5154
title: "E703: semicolon followed by ellipsis causes syntax error"
type: issue
state: closed
author: addisoncrump
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-06-17T01:17:56Z
updated_at: 2023-06-19T04:19:43Z
url: https://github.com/astral-sh/ruff/issues/5154
synced_at: 2026-01-12T15:54:45Z
```

# E703: semicolon followed by ellipsis causes syntax error

---

_@addisoncrump_

This one is fairly straightforward, and potentially indicates other issues with E703.

The original source:

```py
while 1:
  1;...
```

And the ruff output:

```
error: Autofix introduced a syntax error in `minimized-from-crash-70aec6f79bd62d88` with rule codes E703: invalid syntax. Got unexpected token '.' at byte offset 14
---
while 1:
  1...

---
minimized-from-crash-70aec6f79bd62d88:2:4: E703 Statement ends with an unnecessary semicolon
  |
1 | while 1:
2 |   1;...
  |    ^ E703
  |
  = help: Remove unnecessary semicolon

Found 1 error.
```

---

_Label `bug` added by @charliermarsh on 2023-06-17 02:07_

---

_Label `good first issue` added by @charliermarsh on 2023-06-17 02:07_

---

_Comment by @charliermarsh on 2023-06-17 02:08_

We might just want to remove this fix for now. It'll be made obsolete by the formatter anyway, and it's probably tricky to get right at the moment.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-19 03:49_

---

_Closed by @charliermarsh on 2023-06-19 04:19_

---
