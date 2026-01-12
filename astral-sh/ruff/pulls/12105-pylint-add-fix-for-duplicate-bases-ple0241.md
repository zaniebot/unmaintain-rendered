```yaml
number: 12105
title: "[`pylint`] Add fix for `duplicate-bases` (`PLE0241`)"
type: pull_request
state: merged
author: Peiffap
labels:
  - fixes
assignees: []
merged: true
base: main
head: gp/autofix-PLE0241
created_at: 2024-06-29T14:44:09Z
updated_at: 2024-06-29T22:18:53Z
url: https://github.com/astral-sh/ruff/pull/12105
synced_at: 2026-01-12T15:55:40Z
```

# [`pylint`] Add fix for `duplicate-bases` (`PLE0241`)

---

_@Peiffap_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

This adds a fix for the `duplicate-bases` rule that removes the duplicate base from the class definition.

## Test Plan

<!-- How was it tested? -->

`cargo nextest run duplicate_bases`, `cargo insta review`.


---

_Comment by @charliermarsh on 2024-06-29 17:42_

Thanks!

---

_Label `fixes` added by @charliermarsh on 2024-06-29 17:44_

---

_@charliermarsh approved on 2024-06-29 17:47_

---

_Merged by @charliermarsh on 2024-06-29 17:48_

---

_Closed by @charliermarsh on 2024-06-29 17:48_

---

_Branch deleted on 2024-06-29 22:18_

---
