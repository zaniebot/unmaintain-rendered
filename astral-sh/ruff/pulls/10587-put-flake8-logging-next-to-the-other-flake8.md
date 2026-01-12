```yaml
number: 10587
title: Put flake8-logging next to the other flake8 plugins in registry
type: pull_request
state: merged
author: FFY00
labels:
  - documentation
assignees: []
merged: true
base: main
head: flake8-logging-registry-order
created_at: 2024-03-25T20:15:42Z
updated_at: 2024-03-25T20:29:32Z
url: https://github.com/astral-sh/ruff/pull/10587
synced_at: 2026-01-12T15:55:32Z
```

# Put flake8-logging next to the other flake8 plugins in registry

---

_@FFY00_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This is just a nitpicky improvement, but I thought it'd be a good opportunity to look at the ruff source.

> The rules list in the documentation is generated using the registry order. Currently, flake8-logging is separated from the rest of the flake8 plugins. This patch puts it next to them.

https://docs.astral.sh/ruff/rules/

If it makes sense, we could alternatively just sort the linters in https://github.com/astral-sh/ruff/blob/main/crates/ruff_dev/src/generate_rules_table.rs.

## Test Plan

<!-- How was it tested? -->


---

_@charliermarsh approved on 2024-03-25 20:16_

Thank you!

---

_Label `documentation` added by @charliermarsh on 2024-03-25 20:16_

---

_Comment by @charliermarsh on 2024-03-25 20:17_

I think the sorting there is intentional (Flake8 stuff, followed by the more specific linters like the `numpy` rules, followed by the Ruff set), so this fix seems right.

---

_Merged by @charliermarsh on 2024-03-25 20:23_

---

_Closed by @charliermarsh on 2024-03-25 20:23_

---

_Comment by @github-actions[bot] on 2024-03-25 20:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
