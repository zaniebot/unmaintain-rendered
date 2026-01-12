```yaml
number: 8464
title: "Place 'r' prefix before 'f' for raw format strings"
type: pull_request
state: merged
author: deepyaman
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/rf-not-fr
created_at: 2023-11-03T05:13:28Z
updated_at: 2023-11-03T21:01:56Z
url: https://github.com/astral-sh/ruff/pull/8464
synced_at: 2026-01-10T23:40:55Z
```

# Place 'r' prefix before 'f' for raw format strings

---

_Pull request opened by @deepyaman on 2023-11-03 05:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Currently, `UP032` applied to raw strings results in format strings with the prefix 'fr'. This gets changed to 'rf' by Ruff format (or Black). In order to avoid that, this PR uses the prefix 'rf' to begin with.

## Test Plan

<!-- How was it tested? -->
Updated the expectation on an existing test.

---

_Marked ready for review by @deepyaman on 2023-11-03 05:29_

---

_Comment by @github-actions[bot] on 2023-11-03 05:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @charliermarsh by @charliermarsh on 2023-11-03 13:57_

---

_Comment by @charliermarsh on 2023-11-03 13:57_

Thanks, that's a great idea. I'll review and merge this later today.

---

_@zanieb approved on 2023-11-03 13:59_

LGTM!

---

_@charliermarsh approved on 2023-11-03 14:56_

---

_Merged by @charliermarsh on 2023-11-03 14:56_

---

_Closed by @charliermarsh on 2023-11-03 14:56_

---

_Comment by @charliermarsh on 2023-11-03 14:56_

Thanks for getting involved! Great PR.

---

_Label `bug` added by @charliermarsh on 2023-11-03 14:56_

---

_Branch deleted on 2023-11-03 21:01_

---
