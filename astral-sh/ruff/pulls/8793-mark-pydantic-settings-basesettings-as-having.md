```yaml
number: 8793
title: "Mark `pydantic_settings.BaseSettings` as having default copy semantics"
type: pull_request
state: merged
author: Iipin
labels:
  - bug
assignees: []
merged: true
base: main
head: main
created_at: 2023-11-20T19:25:27Z
updated_at: 2023-11-20T19:37:28Z
url: https://github.com/astral-sh/ruff/pull/8793
synced_at: 2026-01-12T15:55:27Z
```

# Mark `pydantic_settings.BaseSettings` as having default copy semantics

---

_@Iipin_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In 2.0, Pydantic has moved the `BaseSettings` class to a separate package called `pydantic-settings` (https://docs.pydantic.dev/2.4/migration/#basesettings-has-moved-to-pydantic-settings), which results in a false positive on `RUF012` (`mutable-class-default`). A simple fix for that would be adding `pydantic_settings.BaseSettings` base to the `has_default_copy_semantics` helper, which I've done in this PR.

Related issue: #5308

## Test Plan

`cargo test`

---

_@charliermarsh approved on 2023-11-20 19:27_

Thanks!

---

_Label `bug` added by @charliermarsh on 2023-11-20 19:27_

---

_Merged by @charliermarsh on 2023-11-20 19:29_

---

_Closed by @charliermarsh on 2023-11-20 19:29_

---

_Comment by @github-actions[bot] on 2023-11-20 19:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
