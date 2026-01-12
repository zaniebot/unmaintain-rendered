```yaml
number: 187
title: Verify file watching with windows UNC paths
type: issue
state: open
author: MichaReiser
labels:
  - windows
assignees: []
created_at: 2025-03-03T14:42:08Z
updated_at: 2025-05-19T07:10:42Z
url: https://github.com/astral-sh/ty/issues/187
synced_at: 2026-01-12T15:54:22Z
```

# Verify file watching with windows UNC paths

---

_@MichaReiser_

Windows knows two representations for file paths: 

* DOS
* UNC 

We should test our file watcher to see if it returns UNC or regular paths if the input path is a DOS or UNC path. 

---

_Label `windows` added by @AlexWaygood on 2025-03-04 11:38_

---

_Label `windows` added by @MichaReiser on 2025-05-07 15:21_

---

_Renamed from "[red-knot] Verify file watching with windows UNC paths" to "Verify file watching with windows UNC paths" by @MichaReiser on 2025-05-07 15:26_

---

_Label `server` added by @AlexWaygood on 2025-05-11 07:29_

---

_Label `server` removed by @MichaReiser on 2025-05-19 07:10_

---
