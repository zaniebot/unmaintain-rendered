```yaml
number: 1326
title: "Add a diagnostic that errors when a user accesses an instance method off a type[] type"
type: issue
state: open
author: carljm
labels:
  - unsoundness
assignees: []
created_at: 2025-10-08T16:59:38Z
updated_at: 2025-10-08T22:18:56Z
url: https://github.com/astral-sh/ty/issues/1326
synced_at: 2026-01-12T15:54:25Z
```

# Add a diagnostic that errors when a user accesses an instance method off a type[] type

---

_@carljm_

This is unsound, as described in https://github.com/microsoft/pyright/issues/11007

Depending on ecosystem impact, this maybe has to be opt-in?

---

_Label `unsoundness` added by @carljm on 2025-10-08 16:59_

---

_Renamed from "Error on accessing instance methods off a type[] type" to "Add a diagnostic that errors when a user accesses an instance method off a type[] type" by @AlexWaygood on 2025-10-08 22:18_

---
