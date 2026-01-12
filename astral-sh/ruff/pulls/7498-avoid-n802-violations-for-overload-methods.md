```yaml
number: 7498
title: "Avoid N802 violations for @overload methods"
type: pull_request
state: merged
author: JonathanPlasse
labels:
  - bug
assignees: []
merged: true
base: main
head: n802-ignore-overload-decorated-function
created_at: 2023-09-18T18:14:49Z
updated_at: 2023-09-18T18:33:01Z
url: https://github.com/astral-sh/ruff/pull/7498
synced_at: 2026-01-12T15:55:24Z
```

# Avoid N802 violations for @overload methods

---

_@JonathanPlasse_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
- Close #7479

The `@override` was already implemented

## Test Plan

Tested the code in the issue. After removing all the noqa's, only one occurrence of `BadName()` raised a violation.
Added a fixture

<!-- How was it tested? -->


---

_@charliermarsh approved on 2023-09-18 18:25_

---

_Label `bug` added by @charliermarsh on 2023-09-18 18:25_

---

_Merged by @charliermarsh on 2023-09-18 18:32_

---

_Closed by @charliermarsh on 2023-09-18 18:32_

---

_Branch deleted on 2023-09-18 18:33_

---
