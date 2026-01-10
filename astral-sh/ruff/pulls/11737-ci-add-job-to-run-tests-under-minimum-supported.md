```yaml
number: 11737
title: "CI: add job to run tests under minimum supported rust version (msrv)"
type: pull_request
state: merged
author: MichaelOultram-pexip
labels:
  - ci
assignees: []
merged: true
base: main
head: msrv
created_at: 2024-06-04T14:56:48Z
updated_at: 2024-06-04T19:14:59Z
url: https://github.com/astral-sh/ruff/pull/11737
synced_at: 2026-01-10T21:56:00Z
```

# CI: add job to run tests under minimum supported rust version (msrv)

---

_Pull request opened by @MichaelOultram-pexip on 2024-06-04 14:56_

## Summary

This change adds a GitHub Actions CI job to check that the project builds and test pass under the declared minimum supported rust compiler. I have bumped the msrv to 1.74 as that is the lowest version I could get this project to build on.

## Test Plan

The CI job has run on this PR, and will also run on the main branch.


---

_Comment by @github-actions[bot] on 2024-06-04 15:16_

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

_@charliermarsh approved on 2024-06-04 19:14_

Thanks, this makes sense to me.

---

_Merged by @charliermarsh on 2024-06-04 19:14_

---

_Closed by @charliermarsh on 2024-06-04 19:14_

---

_Label `internal` added by @charliermarsh on 2024-06-04 19:14_

---

_Label `internal` removed by @charliermarsh on 2024-06-04 19:14_

---

_Label `ci` added by @charliermarsh on 2024-06-04 19:14_

---
