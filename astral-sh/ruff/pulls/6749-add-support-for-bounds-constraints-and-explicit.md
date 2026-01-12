```yaml
number: 6749
title: Add support for bounds, constraints, and explicit variance on generic type variables to UP040
type: pull_request
state: merged
author: nathanwhit
labels:
  - rule
assignees: []
merged: true
base: main
head: up040-generic-type-aliases
created_at: 2023-08-22T00:40:02Z
updated_at: 2023-09-15T01:11:25Z
url: https://github.com/astral-sh/ruff/pull/6749
synced_at: 2026-01-12T02:39:09Z
```

# Add support for bounds, constraints, and explicit variance on generic type variables to UP040

---

_Pull request opened by @nathanwhit on 2023-08-22 00:40_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Extends UP040 to support moving type variables with bounds/constraints/variance that are used in type aliases to use PEP-695 syntax.

Part of #4617.

## Test Plan

The existing tests added by #6314 already cover the relevant cases.


---

_Comment by @github-actions[bot] on 2023-08-22 01:06_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Label `rule` added by @charliermarsh on 2023-09-15 00:50_

---

_@charliermarsh approved on 2023-09-15 01:11_

Thanks for this -- looks great -- and very sorry for the lengthy delay in review!

---

_Merged by @charliermarsh on 2023-09-15 01:11_

---

_Closed by @charliermarsh on 2023-09-15 01:11_

---
