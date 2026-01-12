```yaml
number: 4652
title: "fix the issue with pruning the last package in `pip tree`"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - bug
assignees: []
merged: true
base: main
head: pip-tree-prune-fix
created_at: 2024-06-29T20:55:45Z
updated_at: 2024-06-29T22:45:35Z
url: https://github.com/astral-sh/uv/pull/4652
synced_at: 2026-01-12T16:06:22Z
```

# fix the issue with pruning the last package in `pip tree`

---

_@ChannyClaus_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
resolves https://github.com/astral-sh/uv/issues/4651
(pruning needs to happen at the parent level so that the number of children being used to figure out the output is correct)

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
added a test that would've caught this bug 🌵 
<!-- How was it tested? -->


---

_@zanieb approved on 2024-06-29 22:45_

Thanks!

---

_Label `bug` added by @zanieb on 2024-06-29 22:45_

---

_Merged by @zanieb on 2024-06-29 22:45_

---

_Closed by @zanieb on 2024-06-29 22:45_

---
