---
number: 7348
title: "CLI: Display non-default ignore file option"
type: issue
state: open
author: zanieb
labels:
  - cli
assignees: []
created_at: 2023-09-13T16:04:35Z
updated_at: 2023-09-13T16:04:35Z
url: https://github.com/astral-sh/ruff/issues/7348
synced_at: 2026-01-07T13:12:15-06:00
---

# CLI: Display non-default ignore file option

---

_Issue opened by @zanieb on 2023-09-13 16:04_

We currently have `--respect-gitignore` / `--no-respect-gitignore` flags which toggle automatic exclusion of files listed in a `.gitignore` file and “other standard ignore files”. The CLI help menu displays `--respect-gitignore` but it is already the default behavior. Instead, it should display the negative option to clarify the default behavior.

---

_Label `cli` added by @zanieb on 2023-09-13 16:04_

---
