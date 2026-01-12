```yaml
number: 6986
title: "docs: :memo: Updating the example for `assert(S101)`"
type: pull_request
state: merged
author: Anselmoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: issue-6984
created_at: 2023-08-29T17:35:36Z
updated_at: 2023-08-29T21:08:08Z
url: https://github.com/astral-sh/ruff/pull/6986
synced_at: 2026-01-12T15:55:23Z
```

# docs: :memo: Updating the example for `assert(S101)`

---

_@Anselmoo_





<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Rewriting the `if`-comparison to focus on the meaning of rule ` assert S101`.

Fixes #6984

## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @Anselmoo on 2023-08-29 17:45_

---

_Comment by @github-actions[bot] on 2023-08-29 17:55_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb reviewed on 2023-08-29 18:28_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_bandit/rules/assert_used.rs`:28 on 2023-08-29 18:28_

Nit: We should probably have an empty line
```suggestion
///     raise ValueError("Expected positive value.")
///
/// # or even better:
```

---

_Comment by @zanieb on 2023-08-29 18:28_

Thanks for contributing!

---

_Label `documentation` added by @zanieb on 2023-08-29 18:28_

---

_@zanieb approved on 2023-08-29 21:08_

---

_Merged by @zanieb on 2023-08-29 21:08_

---

_Closed by @zanieb on 2023-08-29 21:08_

---
