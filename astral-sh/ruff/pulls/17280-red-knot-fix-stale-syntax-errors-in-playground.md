```yaml
number: 17280
title: "[red-knot] Fix stale syntax errors in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/fix-stale-diagnostics
created_at: 2025-04-07T16:25:35Z
updated_at: 2025-04-07T16:39:23Z
url: https://github.com/astral-sh/ruff/pull/17280
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Fix stale syntax errors in playground

---

_@MichaReiser_

## Summary

React requires that `key`s are unique but the constructed key for diagnostics wasn't guaranteed when two diagnostics had the same name and location. 

This PR fixes this by using a disambiguator map to disambiguate the key.

Fixes #17276

## Test Plan


https://github.com/user-attachments/assets/f3f9d10a-ecc4-4ffe-8676-3633a12e07ce



---

_Label `playground` added by @MichaReiser on 2025-04-07 16:26_

---

_Label `red-knot` added by @MichaReiser on 2025-04-07 16:26_

---

_Merged by @MichaReiser on 2025-04-07 16:39_

---

_Closed by @MichaReiser on 2025-04-07 16:39_

---

_Branch deleted on 2025-04-07 16:39_

---
