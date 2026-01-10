```yaml
number: 21251
title: "[ty] Fix caching of imported modules in playground"
type: pull_request
state: merged
author: MichaReiser
labels:
  - playground
  - ty
assignees: []
merged: true
base: main
head: micha/fix-playground-caching
created_at: 2025-11-03T03:28:07Z
updated_at: 2025-11-03T15:00:31Z
url: https://github.com/astral-sh/ruff/pull/21251
synced_at: 2026-01-10T16:53:55Z
```

# [ty] Fix caching of imported modules in playground

---

_Pull request opened by @MichaReiser on 2025-11-03 03:28_

## Summary

Force a new Pyodide instance whenever a file changes. 

This ensures that imported modules aren't cached by Pyodide. 

## Test Plan

https://github.com/user-attachments/assets/ef1c7e32-2d8f-42f4-b85d-3424f5ac51d9





---

_Label `playground` added by @MichaReiser on 2025-11-03 03:28_

---

_Label `ty` added by @MichaReiser on 2025-11-03 03:28_

---

_Label `playground` added by @MichaReiser on 2025-11-03 03:28_

---

_Label `ty` added by @MichaReiser on 2025-11-03 03:28_

---

_Review requested from @sharkdp by @MichaReiser on 2025-11-03 03:36_

---

_@sharkdp approved on 2025-11-03 14:46_

I don't understand any of this, but thank you! (there's a unused-symbol warning)

---

_Merged by @MichaReiser on 2025-11-03 15:00_

---

_Closed by @MichaReiser on 2025-11-03 15:00_

---

_Branch deleted on 2025-11-03 15:00_

---
