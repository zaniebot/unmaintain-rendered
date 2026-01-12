```yaml
number: 8504
title: Detect runtime-evaluated base classes defined in the current file
type: pull_request
state: closed
author: evanrittenhouse
labels: []
assignees: []
base: main
head: evanrittenhouse/runtime_eval_base_class
created_at: 2023-11-06T04:39:45Z
updated_at: 2023-11-10T03:33:44Z
url: https://github.com/astral-sh/ruff/pull/8504
synced_at: 2026-01-10T23:40:55Z
```

# Detect runtime-evaluated base classes defined in the current file

---

_Pull request opened by @evanrittenhouse on 2023-11-06 04:39_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Detect runtime-evaluated base classes defined in the file we're currently linting. Closes #8250 

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-11-06 04:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @evanrittenhouse on 2023-11-06 04:57_

Going to fix the Clippy error in the morning

---

_Comment by @charliermarsh on 2023-11-09 02:33_

@evanrittenhouse - Do you mind giving me commit privileges on this branch?

---

_Comment by @charliermarsh on 2023-11-09 03:38_

I re-opened here with you as co-author: https://github.com/astral-sh/ruff/pull/8572

---

_Closed by @charliermarsh on 2023-11-09 03:38_

---

_Comment by @evanrittenhouse on 2023-11-10 03:33_

Sounds good. Should've added some tests, totally spaced.

---
