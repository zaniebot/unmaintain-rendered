```yaml
number: 10851
title: "[`pygrep-hooks`] Improve `blanket-noqa` error message (`PGH004`)"
type: pull_request
state: merged
author: augustelalande
labels:
  - rule
assignees: []
merged: true
base: main
head: improve-pgh004
created_at: 2024-04-09T23:23:20Z
updated_at: 2024-04-10T15:10:20Z
url: https://github.com/astral-sh/ruff/pull/10851
synced_at: 2026-01-12T15:55:33Z
```

# [`pygrep-hooks`] Improve `blanket-noqa` error message (`PGH004`)

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Improve `blanket-noqa` error message in cases where codes are provided but not detected due to formatting issues. Namely `# noqa X100` (missing colon) or `noqa : X100` (space before colon). The behavior is similar to `NQA002` and `NQA003` from `flake8-noqa` mentioned in #850. The idea to merge the rules into `PGH004` was suggested by @MichaReiser https://github.com/astral-sh/ruff/pull/10325#issuecomment-2025535444.

## Test Plan

Test cases added to fixture.


---

_Comment by @github-actions[bot] on 2024-04-09 23:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-10 03:31_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-04-10 03:40_

---

_Label `rule` added by @charliermarsh on 2024-04-10 03:40_

---

_@charliermarsh approved on 2024-04-10 04:20_

Thanks!

---

_Merged by @charliermarsh on 2024-04-10 04:30_

---

_Closed by @charliermarsh on 2024-04-10 04:30_

---

_Branch deleted on 2024-04-10 15:10_

---
