```yaml
number: 18957
title: "[ty] Add regression-benchmark for attribute-assignment hang"
type: pull_request
state: merged
author: sharkdp
labels:
  - performance
  - testing
assignees: []
merged: true
base: main
head: david/regression-benchmark-627
created_at: 2025-06-26T13:01:43Z
updated_at: 2025-06-26T13:21:10Z
url: https://github.com/astral-sh/ruff/pull/18957
synced_at: 2026-01-12T15:56:28Z
```

# [ty] Add regression-benchmark for attribute-assignment hang

---

_@sharkdp_

## Summary

Adds a new micro-benchmark as a regression test for https://github.com/astral-sh/ty/issues/627.

## Test Plan

Ran the benchmark on the parent commit of https://github.com/astral-sh/ruff/commit/89d915a1e34144051815dfcbe60ec2cdeb29909e, and verified that it took > 1s, while it takes ~10 ms after the fix.

---

_Label `internal` added by @sharkdp on 2025-06-26 13:01_

---

_Label `performance` added by @sharkdp on 2025-06-26 13:01_

---

_Label `testing` added by @sharkdp on 2025-06-26 13:01_

---

_Comment by @github-actions[bot] on 2025-06-26 13:14_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` removed by @MichaReiser on 2025-06-26 13:18_

---

_@MichaReiser approved on 2025-06-26 13:18_

---

_Merged by @sharkdp on 2025-06-26 13:21_

---

_Closed by @sharkdp on 2025-06-26 13:21_

---

_Branch deleted on 2025-06-26 13:21_

---
