```yaml
number: 8944
title: Enable Preview mode for formatter benchmarks
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: formatter-benchmark-enable-preview
created_at: 2023-12-01T07:56:11Z
updated_at: 2023-12-01T10:03:00Z
url: https://github.com/astral-sh/ruff/pull/8944
synced_at: 2026-01-12T15:55:27Z
```

# Enable Preview mode for formatter benchmarks

---

_@MichaReiser_

## Summary

This PR enables preview mode for the formatter benchmark.

The reasoning for enabling preview mode is that most (significant) changes to the formatter are made to preview mode. 
The "stable" style may get smaller bugfixes or we introduce new branching for preview styles. However, neither of these should have a significant impact on performance (a bug fix might, but I consider it less likely). 
That's why benchmarking with preview style helps us catch significant regressions before shortly before a release when we promote a style to stable. 

## Test Plan

Codspeed


---

_Comment by @MichaReiser on 2023-12-01 07:56_

Current dependencies on/for this PR:
* main
  * **PR #8944** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8944?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8944?utm_source=stack-comment).

---

_Label `internal` added by @MichaReiser on 2023-12-01 07:59_

---

_Comment by @github-actions[bot] on 2023-12-01 08:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@konstin approved on 2023-12-01 08:30_

---

_Review requested from @zanieb by @konstin on 2023-12-01 08:30_

---

_Merged by @MichaReiser on 2023-12-01 10:02_

---

_Closed by @MichaReiser on 2023-12-01 10:02_

---

_Branch deleted on 2023-12-01 10:03_

---
