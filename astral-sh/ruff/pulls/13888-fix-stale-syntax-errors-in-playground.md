```yaml
number: 13888
title: Fix stale syntax errors in playground
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
assignees: []
merged: true
base: main
head: micha/fix-stale-syntax-diagnostics
created_at: 2024-10-23T12:25:21Z
updated_at: 2024-10-23T12:30:11Z
url: https://github.com/astral-sh/ruff/pull/13888
synced_at: 2026-01-12T15:55:46Z
```

# Fix stale syntax errors in playground

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/13877

The problem was that syntax errors have no code. This resulted in two diagnostic-lines to have the same key which isn't allowed in React.

## Test Plan

Tested locally that syntax errors disappear from the diagnostics panel when they are fixed.


---

_Label `playground` added by @MichaReiser on 2024-10-23 12:25_

---

_Merged by @MichaReiser on 2024-10-23 12:30_

---

_Closed by @MichaReiser on 2024-10-23 12:30_

---

_Branch deleted on 2024-10-23 12:30_

---
