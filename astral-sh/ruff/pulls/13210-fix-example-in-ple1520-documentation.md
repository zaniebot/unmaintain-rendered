```yaml
number: 13210
title: Fix example in PLE1520 documentation
type: pull_request
state: merged
author: Jinior
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2024-09-02T09:45:43Z
updated_at: 2024-09-02T11:19:43Z
url: https://github.com/astral-sh/ruff/pull/13210
synced_at: 2026-01-12T15:55:43Z
```

# Fix example in PLE1520 documentation

---

_@Jinior_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
[PLE1520 singledispatchmethod-function](https://docs.astral.sh/ruff/rules/singledispatchmethod-function/) contained an error in *use_instead* section of the code example. 

The example imported `singledispatchmethod` while it uses `singledispatch`.

## Test Plan

All tests and pre-commit hooks were run as described in the [contributing/development](https://docs.astral.sh/ruff/contributing/#development) section of the docs.

To check the documentation:
1. Locally deploy the documentation as described in: https://docs.astral.sh/ruff/contributing/#mkdocs
1. Check whether the example is now correct.


---

_Comment by @github-actions[bot] on 2024-09-02 09:59_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @MichaReiser on 2024-09-02 11:19_

---

_@MichaReiser approved on 2024-09-02 11:19_

Thanks!

---

_Merged by @MichaReiser on 2024-09-02 11:19_

---

_Closed by @MichaReiser on 2024-09-02 11:19_

---
