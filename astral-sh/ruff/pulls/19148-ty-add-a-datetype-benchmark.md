```yaml
number: 19148
title: "[ty] Add a `DateType` benchmark"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - performance
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/datetype-bench
created_at: 2025-07-04T16:11:16Z
updated_at: 2025-07-04T20:11:49Z
url: https://github.com/astral-sh/ruff/pull/19148
synced_at: 2026-01-12T15:56:33Z
```

# [ty] Add a `DateType` benchmark

---

_@AlexWaygood_

## Summary

The [`DateType`](https://github.com/glyph/DateType) library has some very large protocols in it. Currently we type-check it quite quickly, but the current version of https://github.com/astral-sh/ruff/pull/18659 makes our execution time on this library pathologically slow. That PR doesn't seem to have a big impact on any of our current benchmarks, however, so it seems we have some missing coverage in this area; I therefore propose that we add `DateType` as a benchmark.

Currently the benchmark runs pretty quickly (about half the runtime of attrs, which is our fastest real-world benchmark currently), and the library has 0 third-party dependencies, so the benchmark is quick to setup.

## Test Plan

`cargo bench -p ruff_benchmark --bench=ty`


---

_Review requested from @carljm by @AlexWaygood on 2025-07-04 16:11_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-07-04 16:11_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-07-04 16:11_

---

_Label `performance` added by @AlexWaygood on 2025-07-04 16:11_

---

_Label `testing` added by @AlexWaygood on 2025-07-04 16:11_

---

_Label `ty` added by @AlexWaygood on 2025-07-04 16:11_

---

_@AlexWaygood reviewed on 2025-07-04 16:11_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/ty.rs`:503 on 2025-07-04 16:11_

we don't emit any diagnostics on this library (it's well-typed!), so this assertion didn't pass as-is as it expects at least 1 diagnostic

---

_Comment by @github-actions[bot] on 2025-07-04 16:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2025-07-04 17:47_

---

_Merged by @AlexWaygood on 2025-07-04 20:11_

---

_Closed by @AlexWaygood on 2025-07-04 20:11_

---

_Branch deleted on 2025-07-04 20:11_

---
