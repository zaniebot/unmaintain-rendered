```yaml
number: 8988
title: "Create dedicated `is_*_enabled` functions for each preview style"
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: formatter-preview-functions
created_at: 2023-12-04T05:29:42Z
updated_at: 2023-12-04T05:40:14Z
url: https://github.com/astral-sh/ruff/pull/8988
synced_at: 2026-01-12T15:55:27Z
```

# Create dedicated `is_*_enabled` functions for each preview style

---

_@MichaReiser_

## Summary

Create dedicated `is_*_enabled` functions for each preview style to ease identifying the checks that need to be removed when promoting a specific preview style.

## Test Plan

`cargo test`


---

_Comment by @MichaReiser on 2023-12-04 05:29_

Current dependencies on/for this PR:
* main
  * **PR #8988** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8988?utm_source=stack-comment-icon" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8988?utm_source=stack-comment).

---

_Label `internal` added by @MichaReiser on 2023-12-04 05:30_

---

_Merged by @MichaReiser on 2023-12-04 05:38_

---

_Closed by @MichaReiser on 2023-12-04 05:38_

---

_Branch deleted on 2023-12-04 05:38_

---

_Comment by @github-actions[bot] on 2023-12-04 05:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
