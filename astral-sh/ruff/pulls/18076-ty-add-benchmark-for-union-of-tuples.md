```yaml
number: 18076
title: "[ty] Add benchmark for union of tuples"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - performance
  - ty
assignees: []
merged: true
base: main
head: david/benchmark-union-of-tuples
created_at: 2025-05-13T19:11:25Z
updated_at: 2025-05-13T20:14:32Z
url: https://github.com/astral-sh/ruff/pull/18076
synced_at: 2026-01-12T15:56:11Z
```

# [ty] Add benchmark for union of tuples

---

_@sharkdp_

## Summary

Add a micro-benchmark for the code pattern observed in https://github.com/astral-sh/ty/issues/362.

This currently takes around 1 second on my machine.

## Test Plan

```bash
cargo bench -p ruff_benchmark -- 'ty_micro\[many_tuple' --sample-size 10
```


---

_Label `internal` added by @sharkdp on 2025-05-13 19:11_

---

_Label `performance` added by @sharkdp on 2025-05-13 19:11_

---

_Label `ty` added by @sharkdp on 2025-05-13 19:11_

---

_Comment by @github-actions[bot] on 2025-05-13 19:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser approved on 2025-05-13 19:51_

---

_Merged by @sharkdp on 2025-05-13 20:14_

---

_Closed by @sharkdp on 2025-05-13 20:14_

---

_Branch deleted on 2025-05-13 20:14_

---
