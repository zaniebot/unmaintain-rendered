```yaml
number: 17767
title: "[`flake8-use-pathlib`] Don't handle bytes strings in `PTH123`"
type: pull_request
state: closed
author: LaBatata101
labels: []
assignees: []
base: main
head: fix-PTH123
created_at: 2025-05-01T13:37:34Z
updated_at: 2025-05-01T14:44:11Z
url: https://github.com/astral-sh/ruff/pull/17767
synced_at: 2026-01-12T15:56:05Z
```

# [`flake8-use-pathlib`] Don't handle bytes strings in `PTH123`

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

As discussed in #17699 we should not be handling bytes strings here.

This should be merged after #17712.
<!-- What's the purpose of the change? What does it do, and why? -->



---

_Comment by @github-actions[bot] on 2025-05-01 13:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @ntBre on 2025-05-01 14:44_

Thanks again! I think we handled this in a last commit to #17712.

---

_Closed by @ntBre on 2025-05-01 14:44_

---
