```yaml
number: 21397
title: "[ty] Fix Escape handler in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/playground-fix-escape
created_at: 2025-11-12T07:14:47Z
updated_at: 2025-11-12T07:54:16Z
url: https://github.com/astral-sh/ruff/pull/21397
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Fix Escape handler in playground

---

_Pull request opened by @MichaReiser on 2025-11-12 07:14_

## Summary

Fixes the Escape key on the playground. 

The playground sets an escape key handler when within a vendored file to allow navigating back to the user file when pressing escape. 

However, this handler applied to all files, preventing the builtin escape handlers to e.g. close a context menu from not working. 

This PR also fixes another bug where the escape handler triggered a stale instance of the escape handler function (the instance that
was bound when creating the editor the first time rather than the most recent revision stored in `this.props.onBackToUserFile`.

Fixes https://github.com/astral-sh/ty/issues/1521 
Fixes https://github.com/astral-sh/ty/issues/1263

## Test Plan

https://github.com/user-attachments/assets/a87d5acf-e761-4a7b-b5d3-f8253779c00d




---

_Label `playground` added by @MichaReiser on 2025-11-12 07:14_

---

_Label `ty` added by @MichaReiser on 2025-11-12 07:14_

---

_Merged by @MichaReiser on 2025-11-12 07:54_

---

_Closed by @MichaReiser on 2025-11-12 07:54_

---

_Branch deleted on 2025-11-12 07:54_

---
