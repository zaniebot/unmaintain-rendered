```yaml
number: 11475
title: "Use struct fields in `TokenValue` enum"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser-phase-2
head: dhruv/token-value-fields
created_at: 2024-05-20T10:40:55Z
updated_at: 2024-05-27T16:02:05Z
url: https://github.com/astral-sh/ruff/pull/11475
synced_at: 2026-01-12T15:55:38Z
```

# Use struct fields in `TokenValue` enum

---

_@dhruvmanila_

## Summary

This PR updates certain `TokenValue` enum variants to use struct fields instead of tuple variants. The main reason is to avoid a large diff for test snapshots, so it becomes easier to diagnose any issues. This is temporary and will be updated once everything is finalized.


---

_Marked ready for review by @dhruvmanila on 2024-05-21 05:27_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-05-21 05:27_

---

_Label `internal` added by @dhruvmanila on 2024-05-21 05:27_

---

_@MichaReiser approved on 2024-05-27 10:21_

---

_Merged by @dhruvmanila on 2024-05-27 16:02_

---

_Closed by @dhruvmanila on 2024-05-27 16:02_

---

_Branch deleted on 2024-05-27 16:02_

---
