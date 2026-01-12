```yaml
number: 17583
title: Fix stale diagnostics in Ruff playground
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
assignees: []
merged: true
base: main
head: micha/fix-duplicate-key-error
created_at: 2025-04-23T13:22:47Z
updated_at: 2025-04-23T13:47:56Z
url: https://github.com/astral-sh/ruff/pull/17583
synced_at: 2026-01-12T15:56:02Z
```

# Fix stale diagnostics in Ruff playground

---

_@MichaReiser_

## Summary

Fix stale diagnostics in the Ruff playground.

It was possible that diagnostics accumulated when two diagnostics had the same code, row, and column because 
we used that information as the React key (which has to be unique).

This PR uses a disambiguator if two diagnostics have the same code, row, and column information.

## Test Plan


https://github.com/user-attachments/assets/450ab9e3-a6de-4ad1-94fe-cf2f7031dc91




---

_Label `playground` added by @MichaReiser on 2025-04-23 13:23_

---

_Merged by @MichaReiser on 2025-04-23 13:47_

---

_Closed by @MichaReiser on 2025-04-23 13:47_

---

_Branch deleted on 2025-04-23 13:47_

---
