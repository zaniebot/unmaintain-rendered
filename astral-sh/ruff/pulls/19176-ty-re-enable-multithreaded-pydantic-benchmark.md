```yaml
number: 19176
title: "[ty] Re-enable multithreaded pydantic benchmark"
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
head: david/reenable-multithreaded-pydantic
created_at: 2025-07-07T12:04:33Z
updated_at: 2025-07-07T12:28:17Z
url: https://github.com/astral-sh/ruff/pull/19176
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Re-enable multithreaded pydantic benchmark

---

_Pull request opened by @sharkdp on 2025-07-07 12:04_

## Summary

I played with those numbers a bit locally and `sample_size=3, sample_count=8` seemed like a rather stable setup. This means a single sample consistents of 3 iterations of checking pydantic multithreaded. And this is repeated 8 times for statistics. A single check took ~300 ms previously on the runners, so this should only take 7 s.

---

_Label `internal` added by @sharkdp on 2025-07-07 12:04_

---

_Label `performance` added by @sharkdp on 2025-07-07 12:04_

---

_Label `ty` added by @sharkdp on 2025-07-07 12:04_

---

_@MichaReiser approved on 2025-07-07 12:08_

Thank you

---

_@MichaReiser reviewed on 2025-07-07 12:09_

---

_Review comment by @MichaReiser on `crates/ruff_benchmark/benches/ty_walltime.rs`:245 on 2025-07-07 12:09_

For the future: divan also supports ignoring benchmarks (so that they can still be run locally)

---

_Comment by @github-actions[bot] on 2025-07-07 12:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Merged by @sharkdp on 2025-07-07 12:28_

---

_Closed by @sharkdp on 2025-07-07 12:28_

---

_Branch deleted on 2025-07-07 12:28_

---
