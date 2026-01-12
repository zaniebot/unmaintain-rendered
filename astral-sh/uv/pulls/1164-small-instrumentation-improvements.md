```yaml
number: 1164
title: Small instrumentation improvements
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/improve-instrumentation
created_at: 2024-01-29T10:53:18Z
updated_at: 2024-01-29T10:55:20Z
url: https://github.com/astral-sh/uv/pull/1164
synced_at: 2026-01-12T16:04:28Z
```

# Small instrumentation improvements

---

_@konstin_

Less verbose span fields for `Dist`s by using the display impl and no more min length in the tracing durations plot config for comparability (we lose spans due to a speedup otherwise). Both wait points in the solver loop are now instrumented so we can inspect what we're waiting for to progress in the solver.

---

_Label `internal` added by @konstin on 2024-01-29 10:53_

---

_Merged by @konstin on 2024-01-29 10:55_

---

_Closed by @konstin on 2024-01-29 10:55_

---

_Branch deleted on 2024-01-29 10:55_

---
