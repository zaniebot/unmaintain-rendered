```yaml
number: 1011
title: "Modules in imports don't have a \"type\""
type: issue
state: closed
author: Gankra
labels:
  - server
assignees: []
created_at: 2025-08-15T14:58:59Z
updated_at: 2025-11-22T16:06:18Z
url: https://github.com/astral-sh/ty/issues/1011
synced_at: 2026-01-12T15:54:24Z
```

# Modules in imports don't have a "type"

---

_@Gankra_

This is really minor but it's also spooky/weird to me so I want to write this down. If you hover over a reference to a module, we know that it's of type module:

<img width="352" height="82" alt="Image" src="https://github.com/user-attachments/assets/f1d8dc72-b5a1-47ea-af4c-382c6167251a" />

If you hover over a module in an import, we don't:

<img width="369" height="70" alt="Image" src="https://github.com/user-attachments/assets/c3d85245-e15f-41af-9483-06456a38ab3d" />

Specifically `GotoTarget::inferred_type` (goto-type-definition) doesn't work on this symbol, while all the other goto-/docstring features do.

---

_Label `server` added by @Gankra on 2025-08-15 14:59_

---

_Added to milestone `GA` by @Gankra on 2025-08-15 14:59_

---

_Comment by @Gankra on 2025-08-15 15:35_

I believe this is a duplicate of #144 (need to look closer)

---

_Comment by @MichaReiser on 2025-11-14 09:04_

Related to https://github.com/astral-sh/ty/issues/1082

---

_Closed by @Gankra on 2025-11-22 16:06_

---
