```yaml
number: 13111
title: Add note about how Ruff handles PYI files wrt target version
type: pull_request
state: merged
author: calumy
labels:
  - documentation
assignees: []
merged: true
base: main
head: pyi-target-version-note
created_at: 2024-08-26T15:32:41Z
updated_at: 2024-08-27T17:37:44Z
url: https://github.com/astral-sh/ruff/pull/13111
synced_at: 2026-01-10T21:38:32Z
```

# Add note about how Ruff handles PYI files wrt target version

---

_Pull request opened by @calumy on 2024-08-26 15:32_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

There has been some confusion about Ruff's target version not being respected in PYI files (https://github.com/astral-sh/ruff/issues/12256 and https://github.com/astral-sh/ruff/issues/9285). This PR adds a line to the docs to clarify Ruff's reason for ignoring target version in PYI files.

Closes #12256.

## Test Plan

N/A - Docs change


---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-08-26 15:47_

---

_Label `documentation` added by @dhruvmanila on 2024-08-26 15:47_

---

_Comment by @github-actions[bot] on 2024-08-26 22:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood approved on 2024-08-27 17:26_

Thanks!

---

_Merged by @AlexWaygood on 2024-08-27 17:28_

---

_Closed by @AlexWaygood on 2024-08-27 17:28_

---
