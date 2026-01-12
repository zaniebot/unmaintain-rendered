```yaml
number: 80
title: Provide basic intelli-sense and diagnostics for non-project files
type: issue
state: closed
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-03-14T10:30:27Z
updated_at: 2025-11-13T17:51:31Z
url: https://github.com/astral-sh/ty/issues/80
synced_at: 2026-01-12T15:54:22Z
```

# Provide basic intelli-sense and diagnostics for non-project files

---

_@MichaReiser_

LSP clients can open a file that doesn't belong to the current project. Red Knot should still provide basic intelli sense support and diagnostics.

---

_Label `server` added by @MichaReiser on 2025-03-14 10:30_

---

_Renamed from "[red-knot] Provide basic intelli-sense and diagnostics for non-project files" to "Provide basic intelli-sense and diagnostics for non-project files" by @MichaReiser on 2025-05-07 15:13_

---

_Removed from milestone `Beta` by @MichaReiser on 2025-08-15 06:59_

---

_Added to milestone `GA` by @MichaReiser on 2025-08-15 06:59_

---

_Comment by @Gankra on 2025-11-13 17:49_

Update: basically every ty feature works in a random python file... except we don't seem to ever produce diagnostics.

(tested by opening rust's `x.py` while in the ruff workspace)

---

_Assigned to @Gankra by @Gankra on 2025-11-13 17:50_

---

_Comment by @MichaReiser on 2025-11-13 17:51_

>  except we don't seem to ever produce diagnostics.

That's good :)

---

_Closed by @MichaReiser on 2025-11-13 17:51_

---
