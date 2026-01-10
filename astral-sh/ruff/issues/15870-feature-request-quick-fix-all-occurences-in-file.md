---
number: 15870
title: "feature request: quick fix all occurences in file"
type: issue
state: open
author: beauxq
labels:
  - wish
  - server
assignees: []
created_at: 2025-02-01T10:31:42Z
updated_at: 2025-02-03T09:27:55Z
url: https://github.com/astral-sh/ruff/issues/15870
synced_at: 2026-01-10T01:22:57Z
---

# feature request: quick fix all occurences in file

---

_Issue opened by @beauxq on 2025-02-01 10:31_

### Description

Using Ruff in VSCode,
when there is a "Quick Fix" menu item to fix a specific problem,
I would like another menu item to fix all of that specific diagnostic in the whole file.

For example, when I'm using `Q000`, there might be hundreds of strings in the file all violating the rule.
There is a menu item for each one: "Replace single quotes with double quotes"
I would like a Quick Fix menu item to replace all the single quotes with double quotes in the whole file.

---

_Label `server` added by @MichaReiser on 2025-02-03 09:25_

---

_Label `wish` added by @MichaReiser on 2025-02-03 09:25_

---

_Comment by @MichaReiser on 2025-02-03 09:27_

This sounds reasonable and I think is a feature of other linters, e.g. ESLint

<img width="674" alt="Image" src="https://github.com/user-attachments/assets/5ea29e68-6e2f-4ad2-a160-0fe8464c3082" />

---
