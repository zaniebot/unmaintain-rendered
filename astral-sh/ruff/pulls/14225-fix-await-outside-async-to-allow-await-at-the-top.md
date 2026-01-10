```yaml
number: 14225
title: "Fix `await-outside-async` to allow `await` at the top-level scope of a notebook"
type: pull_request
state: merged
author: harupy
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-await-outside-async
created_at: 2024-11-09T15:13:53Z
updated_at: 2024-11-10T03:18:08Z
url: https://github.com/astral-sh/ruff/pull/14225
synced_at: 2026-01-10T20:50:57Z
```

# Fix `await-outside-async` to allow `await` at the top-level scope of a notebook

---

_Pull request opened by @harupy on 2024-11-09 15:13_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Fix `await-outside-async` to allow `await` at the top-level scope of a notebook.

```python
# foo.ipynb

await asyncio.sleep(1)  # should be allowed
```

## Test Plan

<!-- How was it tested? -->

A unit test

---

_Comment by @github-actions[bot] on 2024-11-09 15:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh approved on 2024-11-09 17:44_

---

_Merged by @charliermarsh on 2024-11-09 17:44_

---

_Closed by @charliermarsh on 2024-11-09 17:44_

---

_Label `bug` added by @charliermarsh on 2024-11-09 17:44_

---

_Comment by @charliermarsh on 2024-11-09 17:44_

Thanks!

---

_Branch deleted on 2024-11-10 03:18_

---
