```yaml
number: 10996
title: "Add `if` as sequence pattern list terminator"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
assignees: []
merged: true
base: dhruv/parser
head: dhruv/match-seq-pattern
created_at: 2024-04-17T11:39:59Z
updated_at: 2024-04-17T13:05:51Z
url: https://github.com/astral-sh/ruff/pull/10996
synced_at: 2026-01-12T15:55:34Z
```

# Add `if` as sequence pattern list terminator

---

_@dhruvmanila_

## Summary

This fixes a bug for code like:

```python
match subject:
    case a, b if expr: ...
```

Previously, the `:` was the only terminator token so the list parsing wouldn't stop after `b`. The `if` token has been added to the terminator list to fix this.

## Test Plan

Add test cases and verified the snapshots.

---

_Label `bug` added by @dhruvmanila on 2024-04-17 11:39_

---

_Label `parser` added by @dhruvmanila on 2024-04-17 11:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-17 11:40_

---

_Comment by @codspeed-hq[bot] on 2024-04-17 12:04_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/match-seq-pattern)

### Merging #10996 will **not alter performance**

<sub>Comparing <code>dhruv/match-seq-pattern</code> (a0799ca) with <code>dhruv/parser</code> (c30057a)</sub>



### Summary

`✅ 30` untouched benchmarks






---

_@MichaReiser approved on 2024-04-17 12:07_

---

_Merged by @dhruvmanila on 2024-04-17 12:47_

---

_Closed by @dhruvmanila on 2024-04-17 12:47_

---

_Branch deleted on 2024-04-17 12:47_

---

_Comment by @github-actions[bot] on 2024-04-17 13:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
