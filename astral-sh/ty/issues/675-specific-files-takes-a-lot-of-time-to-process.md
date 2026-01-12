```yaml
number: 675
title: Specific files, takes a lot of time to process
type: issue
state: closed
author: qarmin
labels:
  - performance
assignees: []
created_at: 2025-06-18T07:02:16Z
updated_at: 2025-07-15T07:41:15Z
url: https://github.com/astral-sh/ty/issues/675
synced_at: 2026-01-12T15:54:23Z
```

# Specific files, takes a lot of time to process

---

_@qarmin_

With command
```
ty check b.py
```
Specific files like

[slow_3s.zip](https://github.com/user-attachments/files/20790338/slow_3s.zip)

or 

[more_than_30s.zip](https://github.com/user-attachments/files/20790353/more_than_30s.zip)

takes several seconds to complete

ruff a2cd6df429a3e76880a48a5eb8816a86c36927ed

---

_Label `performance` added by @AlexWaygood on 2025-06-18 07:15_

---

_Added to milestone `Beta` by @MichaReiser on 2025-06-18 07:35_

---

_Comment by @qarmin on 2025-07-15 07:41_

Cannot reproduce with the latest ty version

---

_Closed by @qarmin on 2025-07-15 07:41_

---
