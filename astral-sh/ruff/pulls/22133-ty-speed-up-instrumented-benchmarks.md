```yaml
number: 22133
title: "[ty] Speed-up instrumented benchmarks"
type: pull_request
state: merged
author: MichaReiser
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: micha/instrumented-depot
created_at: 2025-12-21T21:04:38Z
updated_at: 2025-12-22T14:06:24Z
url: https://github.com/astral-sh/ruff/pull/22133
synced_at: 2026-01-12T15:57:42Z
```

# [ty] Speed-up instrumented benchmarks

---

_@MichaReiser_

The ty-instrumented benchmarks are now our slowest job after https://github.com/astral-sh/ruff/pull/22126.

This PR speeds up the instrumented benchmarks by:

* Using a depot-4 runner to build the benchmarks
* Split the `instrumented` features into `ty` and `ruff` to reduce the build time
* Shard the instrumented benchmark across two runners

This cuts the:

* ty build times from 3min 36s (with warm cache) to a little more than 2 min (cold cache)
* ruff build times from 3min 30s (warm cache) to around 3min (cold cache)
* Cuts the ty instrumented runtime from 9min 45s (warm cache) to 7 min (cold cache). The benchmarks now complete before the macos tests.

---

_Label `testing` added by @MichaReiser on 2025-12-21 21:04_

---

_Label `ty` added by @MichaReiser on 2025-12-21 21:04_

---

_Comment by @astral-sh-bot[bot] on 2025-12-21 21:22_


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

_Marked ready for review by @MichaReiser on 2025-12-22 08:29_

---

_Renamed from "[ty] Use depot runners for instrumented benchmarks" to "[ty] Speed-up instrumented benchmarks" by @MichaReiser on 2025-12-22 08:32_

---

_Merged by @MichaReiser on 2025-12-22 14:06_

---

_Closed by @MichaReiser on 2025-12-22 14:06_

---

_Branch deleted on 2025-12-22 14:06_

---
