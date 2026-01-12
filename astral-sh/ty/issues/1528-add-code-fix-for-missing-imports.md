```yaml
number: 1528
title: Add code-fix for missing imports
type: issue
state: closed
author: Gankra
labels:
  - feature
  - server
  - imports
  - completions
assignees: []
created_at: 2025-11-11T21:25:52Z
updated_at: 2025-11-11T21:27:15Z
url: https://github.com/astral-sh/ty/issues/1528
synced_at: 2026-01-12T15:54:25Z
```

# Add code-fix for missing imports

---

_@Gankra_

The auto-importer is great but I often find myself typing (or pasting) a symbol and then noticing it's not imported, and then it's "too late" for the auto-importer to help. In rust-analyzer you get these two options to fix the import which cover the usecase well.

<img width="396" height="125" alt="Image" src="https://github.com/user-attachments/assets/8d29c7b2-2328-4a25-b661-8ed9675c68a1" />

I assume all the auto-import machinery means we have the info available, and we "just" need to plumb it into code-fix machinery.



---

_Label `feature` added by @Gankra on 2025-11-11 21:25_

---

_Label `server` added by @Gankra on 2025-11-11 21:25_

---

_Label `imports` added by @Gankra on 2025-11-11 21:25_

---

_Label `completions` added by @Gankra on 2025-11-11 21:25_

---

_Closed by @AlexWaygood on 2025-11-11 21:27_

---
