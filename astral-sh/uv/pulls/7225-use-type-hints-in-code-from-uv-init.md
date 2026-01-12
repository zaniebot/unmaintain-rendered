```yaml
number: 7225
title: "Use type hints in code from `uv init`"
type: pull_request
state: merged
author: johnslavik
labels:
  - enhancement
assignees: []
merged: true
base: main
head: bswck/init-script-typehinted
created_at: 2024-09-09T18:10:54Z
updated_at: 2024-09-14T07:00:51Z
url: https://github.com/astral-sh/uv/pull/7225
synced_at: 2026-01-12T16:07:44Z
```

# Use type hints in code from `uv init`

---

_@johnslavik_

Let's promote type hints!

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
The generated script now annotates the return type of the dummy function `hello()`.

## Test Plan

<!-- How was it tested? -->
All existing tests have been synced with this update.


---

_@charliermarsh approved on 2024-09-09 18:40_

---

_@zanieb approved on 2024-09-09 20:36_

---

_Comment by @zanieb on 2024-09-09 20:36_

Thanks!

---

_Merged by @zanieb on 2024-09-09 20:37_

---

_Closed by @zanieb on 2024-09-09 20:37_

---

_Label `enhancement` added by @zanieb on 2024-09-09 20:37_

---

_Branch deleted on 2024-09-14 07:00_

---
