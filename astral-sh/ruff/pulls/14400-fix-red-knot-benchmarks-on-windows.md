```yaml
number: 14400
title: Fix Red Knot benchmarks on Windows
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: micha/fix-benchmarks-on-winodws
created_at: 2024-11-17T16:13:46Z
updated_at: 2024-11-17T16:30:32Z
url: https://github.com/astral-sh/ruff/pull/14400
synced_at: 2026-01-12T15:55:47Z
```

# Fix Red Knot benchmarks on Windows

---

_@MichaReiser_

## Summary

Fix Red Knot benchmarks on windows by replacing `\` path separators with `/`.

## Test Plan

Running `cargo bench -p ruff_benchmark --bench red_knot` no longer fails on windows


---

_Label `red-knot` added by @MichaReiser on 2024-11-17 16:13_

---

_Merged by @MichaReiser on 2024-11-17 16:21_

---

_Closed by @MichaReiser on 2024-11-17 16:21_

---

_Branch deleted on 2024-11-17 16:21_

---

_Comment by @github-actions[bot] on 2024-11-17 16:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
