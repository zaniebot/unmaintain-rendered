```yaml
number: 22311
title: Clean up pre-commit config
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ci
assignees: []
merged: true
base: main
head: clean-up-precommit
created_at: 2025-12-30T22:07:01Z
updated_at: 2025-12-31T07:36:20Z
url: https://github.com/astral-sh/ruff/pull/22311
synced_at: 2026-01-10T16:36:19Z
```

# Clean up pre-commit config

---

_Pull request opened by @MatthewMckee4 on 2025-12-30 22:07_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

`clippy` and `dev-generate-all` don't exist in the `pre-commit-config.yaml` anymore.

And I figured `ci` is not needed if we already control what to skip in ci.


---

_Comment by @astral-sh-bot[bot] on 2025-12-30 22:18_


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

_@MichaReiser approved on 2025-12-31 07:35_

---

_Label `ci` added by @MichaReiser on 2025-12-31 07:35_

---

_Merged by @MichaReiser on 2025-12-31 07:36_

---

_Closed by @MichaReiser on 2025-12-31 07:36_

---
