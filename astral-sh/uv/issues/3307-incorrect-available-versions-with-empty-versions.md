---
number: 3307
title: Incorrect available versions with empty versions range
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2024-04-29T13:06:47Z
updated_at: 2024-10-04T16:14:20Z
url: https://github.com/astral-sh/uv/issues/3307
synced_at: 2026-01-10T01:23:26Z
---

# Incorrect available versions with empty versions range

---

_Issue opened by @konstin on 2024-04-29 13:06_

I requested `ruff>=0.4.3,<0.5`, while 0.4.2 is currently the latest version. This shows:
```
  x No solution found when resolving dependencies:
  `-> Because only the following versions of ruff are available:
          ruff<0.4.3
          ruff>=0.5
      and gha-api-test depends on ruff>=0.4.3,<0.5, we can conclude that the requirements are unsatisfiable.
```
That is incorrect, `ruff>=0.5` is not available.

---

_Label `error messages` added by @konstin on 2024-04-29 13:06_

---

_Comment by @zanieb on 2024-04-29 14:28_

That's... weird. Thanks!

---

_Assigned to @zanieb by @zanieb on 2024-04-29 14:28_

---

_Comment by @zanieb on 2024-10-04 16:10_

Still wrong

```
❯ uv pip install 'ruff>=0.5.8,<0.6'
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of ruff are available:
          ruff<0.5.8
          ruff>0.6
      and you require ruff>=0.5.8,<0.6, we can conclude that your requirements are unsatisfiable.
```

---

_Comment by @zanieb on 2024-10-04 16:13_

Actually not still wrong!

```
❯ uv pip install 'ruff>=0.6.10,<0.7'
  × No solution found when resolving dependencies:
  ╰─▶ Because only ruff<=0.6.9 is available and you require ruff>=0.6.10,<0.7, we can conclude that your requirements are
      unsatisfiable.
```

---

_Closed by @zanieb on 2024-10-04 16:13_

---

_Comment by @zanieb on 2024-10-04 16:14_

Solved in #6118 

---
