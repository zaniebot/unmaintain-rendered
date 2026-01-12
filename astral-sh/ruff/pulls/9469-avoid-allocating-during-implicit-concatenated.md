```yaml
number: 9469
title: Avoid allocating during implicit concatenated string formatting
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: parts-iter
created_at: 2024-01-11T14:32:18Z
updated_at: 2024-01-11T14:58:50Z
url: https://github.com/astral-sh/ruff/pull/9469
synced_at: 2026-01-12T15:55:28Z
```

# Avoid allocating during implicit concatenated string formatting

---

_@MichaReiser_

## Summary

Create a custom `Iterator` type to avoid collecting all string parts in `AnyString::parts`

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-01-11 14:33_

---

_Label `formatter` added by @MichaReiser on 2024-01-11 14:33_

---

_Label `ci` added by @MichaReiser on 2024-01-11 14:33_

---

_Label `ci` removed by @MichaReiser on 2024-01-11 14:33_

---

_Comment by @github-actions[bot] on 2024-01-11 14:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@charliermarsh approved on 2024-01-11 14:48_

---

_Merged by @MichaReiser on 2024-01-11 14:58_

---

_Closed by @MichaReiser on 2024-01-11 14:58_

---

_Branch deleted on 2024-01-11 14:58_

---
