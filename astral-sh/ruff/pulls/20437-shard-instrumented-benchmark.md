```yaml
number: 20437
title: Shard instrumented benchmark
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
assignees: []
merged: true
base: main
head: micha/shard-instrumented
created_at: 2025-09-16T16:28:08Z
updated_at: 2025-09-19T08:25:06Z
url: https://github.com/astral-sh/ruff/pull/20437
synced_at: 2026-01-12T15:57:01Z
```

# Shard instrumented benchmark

---

_@MichaReiser_

Shard the instrumented benchmarks and only run the ruff/ty benchmarks when there were corresponding code changes.

I tested that the corresponding benchmarks run when making changes to ruff or ty

* https://github.com/astral-sh/ruff/pull/20450
* https://github.com/astral-sh/ruff/pull/20451

This PR increases the overall CI time when making changes that apply to both ruff and ty, as it now requires building the shared crates twice. However, it should reduce CI time when only changing code for one project because the benchmark jobs now only build the relevant benchmarks (e.g. ty doesn't need to build `ruff_linter` and `ruff`).

Sharding the walltime benchmarks is a bit more involved because codspeed doesn't support filtering the benchmarks by name but this is something they plan to add soon.

---

_Label `testing` added by @MichaReiser on 2025-09-16 16:28_

---

_Comment by @github-actions[bot] on 2025-09-16 16:46_

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

_Closed by @MichaReiser on 2025-09-17 10:47_

---

_Reopened by @MichaReiser on 2025-09-17 10:47_

---

_Marked ready for review by @MichaReiser on 2025-09-17 11:23_

---

_@MichaReiser reviewed on 2025-09-17 11:24_

---

_Review comment by @MichaReiser on `.github/workflows/ci.yaml`:909 on 2025-09-17 11:24_

I tried to use a matrix to reduce code duplication but matrix doesn't provide an easy way to conditionally skip an entire job. 

---

_Comment by @MichaReiser on 2025-09-19 08:25_

Let's give this a try. It's easy enough to revert

---

_Merged by @MichaReiser on 2025-09-19 08:25_

---

_Closed by @MichaReiser on 2025-09-19 08:25_

---

_Branch deleted on 2025-09-19 08:25_

---
