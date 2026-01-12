```yaml
number: 15099
title: "Fix `RUF200` doc to have name and email in single object"
type: pull_request
state: merged
author: guptaarnav
labels:
  - documentation
assignees: []
merged: true
base: main
head: RUF200-authors-listing
created_at: 2024-12-22T18:24:07Z
updated_at: 2024-12-23T05:00:17Z
url: https://github.com/astral-sh/ruff/pull/15099
synced_at: 2026-01-12T15:55:50Z
```

# Fix `RUF200` doc to have name and email in single object

---

_@guptaarnav_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Closes #14975 by modifying the docstring of the InvalidPyprojectToml rule.  Previously the docs were incorrectly stating that author name and emails must be individual items in the authors list, rather than part of a single object for each respective author.  

## Test Plan
This was a docstring change, no tests needed.


---

_Comment by @github-actions[bot] on 2024-12-22 18:30_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "InvalidPyprojectToml doc now has name and email as single obj in docstring" to "Fix `RUF200` doc to have name and email in single object" by @dhruvmanila on 2024-12-23 04:59_

---

_Label `documentation` added by @dhruvmanila on 2024-12-23 04:59_

---

_@dhruvmanila approved on 2024-12-23 04:59_

---

_Comment by @dhruvmanila on 2024-12-23 05:00_

Thank you!

---

_Merged by @dhruvmanila on 2024-12-23 05:00_

---

_Closed by @dhruvmanila on 2024-12-23 05:00_

---
