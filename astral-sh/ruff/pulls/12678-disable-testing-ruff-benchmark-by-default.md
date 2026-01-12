```yaml
number: 12678
title: "Disable testing `ruff_benchmark` by default"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: disable-testing-ruff-benchmark-by-default
created_at: 2024-08-05T06:11:21Z
updated_at: 2024-08-05T06:25:17Z
url: https://github.com/astral-sh/ruff/pull/12678
synced_at: 2026-01-12T15:55:41Z
```

# Disable testing `ruff_benchmark` by default

---

_@MichaReiser_

## Summary

Part of https://github.com/astral-sh/ruff/issues/12662

`ruff_benchmark` contains no tests. Remove it from the crates that are tested by default when running `cargo test`. It remains possible to run the (non-existing) tests with `cargo test -p ruff_benchmark).

## Test Plan

`cargo test`. Verified that `ruff_benchmark` doesn't show up in the list of built crates


---

_Label `internal` added by @MichaReiser on 2024-08-05 06:11_

---

_Merged by @MichaReiser on 2024-08-05 06:15_

---

_Closed by @MichaReiser on 2024-08-05 06:15_

---

_Branch deleted on 2024-08-05 06:15_

---

_Comment by @github-actions[bot] on 2024-08-05 06:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
