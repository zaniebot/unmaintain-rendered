```yaml
number: 11094
title: "(📚) fix docs for `invalid-X-returns`"
type: pull_request
state: merged
author: KotlinIsland
labels:
  - documentation
assignees: []
merged: true
base: main
head: integer
created_at: 2024-04-23T00:09:07Z
updated_at: 2024-04-23T00:21:03Z
url: https://github.com/astral-sh/ruff/pull/11094
synced_at: 2026-01-12T15:55:34Z
```

# (📚) fix docs for `invalid-X-returns`

---

_@KotlinIsland_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

There is no class `integer` in python, nor is there a type `integer`, so I updated the docs to remove the backticks on these references, such that it is the representation of an integer, and not a reference.

## Test Plan

<!-- How was it tested? -->


---

_@charliermarsh approved on 2024-04-23 00:13_

Thanks, I caught a few of these but clearly not all, appreciate it.

---

_Label `documentation` added by @charliermarsh on 2024-04-23 00:13_

---

_Merged by @charliermarsh on 2024-04-23 00:17_

---

_Closed by @charliermarsh on 2024-04-23 00:17_

---

_Comment by @github-actions[bot] on 2024-04-23 00:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
