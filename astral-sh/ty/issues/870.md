```yaml
number: 870
title: Fallback to resolve a module relative to the importing file
type: issue
state: closed
author: MichaReiser
labels:
  - server
  - imports
assignees: []
created_at: 2025-07-22T14:11:28Z
updated_at: 2025-11-14T00:14:28Z
url: https://github.com/astral-sh/ty/issues/870
synced_at: 2026-01-10T02:06:24Z
```

# Fallback to resolve a module relative to the importing file

---

_Issue opened by @MichaReiser on 2025-07-22 14:11_

Most users using ty as a LSP probably won't bother configuring all the search paths. However, ty might fail to resolve many imports if they don't, depending on their project structure. 

To improve their experience, consider implementing step 2 mentioned in [pyrefly's implicit search paths](https://pyrefly.org/en/docs/import-resolution/#implicit-search-path) documentation. Pyright implements a similar fallback. 

---

_Label `server` added by @MichaReiser on 2025-07-22 14:11_

---

_Label `imports` added by @MichaReiser on 2025-07-22 14:11_

---

_Closed by @carljm on 2025-11-14 00:14_

---
