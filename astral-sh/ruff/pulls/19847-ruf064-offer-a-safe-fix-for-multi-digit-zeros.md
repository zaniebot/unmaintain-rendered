```yaml
number: 19847
title: "RUF064: offer a safe fix for multi-digit zeros"
type: pull_request
state: merged
author: RazerM
labels: []
assignees: []
merged: true
base: main
head: fix/non-octal-permissions-multi-digit-zero
created_at: 2025-08-10T19:57:23Z
updated_at: 2025-08-10T22:26:27Z
url: https://github.com/astral-sh/ruff/pull/19847
synced_at: 2026-01-12T15:56:48Z
```

# RUF064: offer a safe fix for multi-digit zeros

---

_@RazerM_

Fixes #19010

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

See #19010. `0` was not considered a violation, but `000` was. The latter will now be fixed to `0o000`.

## Test Plan

Existing test updated.


---

_Comment by @github-actions[bot] on 2025-08-10 20:07_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dylwil3 approved on 2025-08-10 20:16_

Thank you!

---

_Merged by @dylwil3 on 2025-08-10 20:35_

---

_Closed by @dylwil3 on 2025-08-10 20:35_

---

_Branch deleted on 2025-08-10 22:26_

---
