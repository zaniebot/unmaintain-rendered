```yaml
number: 11576
title: "Remove some unused `pub` functions"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/unused
created_at: 2024-05-28T02:11:21Z
updated_at: 2024-05-28T13:56:52Z
url: https://github.com/astral-sh/ruff/pull/11576
synced_at: 2026-01-12T15:55:38Z
```

# Remove some unused `pub` functions

---

_@charliermarsh_

## Summary

I left anything in `red-knot`, any `with_` methods, etc.

---

_Review requested from @MichaReiser by @charliermarsh on 2024-05-28 02:11_

---

_Review requested from @dhruvmanila by @charliermarsh on 2024-05-28 02:11_

---

_Label `internal` added by @charliermarsh on 2024-05-28 02:11_

---

_Comment by @github-actions[bot] on 2024-05-28 02:34_

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

_@AlexWaygood reviewed on 2024-05-28 03:17_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/lib.rs`:378 on 2024-05-28 03:17_

It's possible @dhruvmanila has plans for this one — he added it two weeks ago in https://github.com/astral-sh/ruff/commit/025768d30344b275e9caa2f72a17db8fa4a63ccd

---

_@dhruvmanila reviewed on 2024-05-28 03:42_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lib.rs`:378 on 2024-05-28 03:42_

Yeah, I'm using this in the new parser PRs. It's fine to remove it here as it has changed a bit, so I'll resolve it during the rebase.

---

_@dhruvmanila approved on 2024-05-28 03:46_

---

_@AlexWaygood approved on 2024-05-28 06:29_

---

_Merged by @charliermarsh on 2024-05-28 13:56_

---

_Closed by @charliermarsh on 2024-05-28 13:56_

---

_Branch deleted on 2024-05-28 13:56_

---
