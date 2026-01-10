```yaml
number: 11514
title: "[`flake8-logging-format`] Fix the autofix title in `logging-warn` (`G010`)"
type: pull_request
state: merged
author: monk-time
labels: []
assignees: []
merged: true
base: main
head: autofix-g010-typo
created_at: 2024-05-23T12:18:07Z
updated_at: 2024-05-24T09:58:00Z
url: https://github.com/astral-sh/ruff/pull/11514
synced_at: 2026-01-10T22:05:26Z
```

# [`flake8-logging-format`] Fix the autofix title in `logging-warn` (`G010`)

---

_Pull request opened by @monk-time on 2024-05-23 12:18_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Rule `logging-warn` (`G010`) prescribes a change from `warn` to `warning` and has a corresponding autofix, but the autofix is mistakenly titled ```"Convert to `warn`"``` instead of ```"Convert to `warning`"``` (the latter is what the autofix actually does). Seems to be a plain typo.

## Test Plan

I apologize for not being able to properly test this at the moment, the change was made by grepping the repo and fixing the typo in code as well as in the `.snap` file.


---

_Comment by @github-actions[bot] on 2024-05-23 12:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@dhruvmanila approved on 2024-05-24 07:42_

Thank you!

> I apologize for not being able to properly test this at the moment, the change was made by grepping the repo and fixing the typo in code as well as in the .snap file.

Well, the CI is green so everything must have been updated ;)

---

_Merged by @dhruvmanila on 2024-05-24 07:43_

---

_Closed by @dhruvmanila on 2024-05-24 07:43_

---

_Branch deleted on 2024-05-24 09:58_

---
