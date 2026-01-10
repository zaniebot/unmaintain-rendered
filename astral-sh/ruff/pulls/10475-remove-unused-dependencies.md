```yaml
number: 10475
title: Remove unused dependencies
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
assignees: []
merged: true
base: main
head: shear-dependencies
created_at: 2024-03-19T16:03:08Z
updated_at: 2024-03-19T16:33:48Z
url: https://github.com/astral-sh/ruff/pull/10475
synced_at: 2026-01-10T22:47:02Z
```

# Remove unused dependencies

---

_Pull request opened by @MichaReiser on 2024-03-19 16:03_

## Summary
I used `cargo-shear` (see [tweet](https://twitter.com/boshen_c/status/1770106165923586395)) to remove some unused dependencies that `cargo udeps` wasn't reporting.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

`cargo test`


---

_Label `internal` added by @MichaReiser on 2024-03-19 16:06_

---

_Comment by @github-actions[bot] on 2024-03-19 16:22_

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

_@charliermarsh approved on 2024-03-19 16:31_

---

_Merged by @MichaReiser on 2024-03-19 16:33_

---

_Closed by @MichaReiser on 2024-03-19 16:33_

---

_Branch deleted on 2024-03-19 16:33_

---
