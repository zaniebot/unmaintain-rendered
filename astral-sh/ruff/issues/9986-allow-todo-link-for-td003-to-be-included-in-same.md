---
number: 9986
title: Allow TODO Link for TD003 to be included in same line
type: issue
state: closed
author: severus-sn4pe
labels: []
assignees: []
created_at: 2024-02-14T15:04:50Z
updated_at: 2024-02-14T18:07:17Z
url: https://github.com/astral-sh/ruff/issues/9986
synced_at: 2026-01-10T01:22:49Z
---

# Allow TODO Link for TD003 to be included in same line

---

_Issue opened by @severus-sn4pe on 2024-02-14 15:04_

TD003 checks that a TODO comment is associated with a link to a relevant issue or ticket.
It requires the link or issue code to be on the line **after** the comment:

```
# TODO: Add parameters
# ISSUE-123
```

Problem: having the link/issue code inline the TODO comment, reports a TD003 error:
```
# TODO: ISSUE-123 Add parameters
```

flake8-todos (from which TD003 is derived) supports this. 
It would be great, if inline-links within the TODO could be allowed. 

Some tools, like IntelliJ IDEA can track open TODO's in a project and having the link/issue-code on the same line allows easier locating a specific todo.








---

_Comment by @dhruvmanila on 2024-02-14 18:07_

Thanks! This should be fixed by https://github.com/astral-sh/ruff/pull/9627

Merging this with https://github.com/astral-sh/ruff/issues/8061

---

_Closed by @dhruvmanila on 2024-02-14 18:07_

---

_Referenced in [astral-sh/ruff#16519](../../astral-sh/ruff/issues/16519.md) on 2025-03-06 14:18_

---
