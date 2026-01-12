```yaml
number: 7160
title: Fix B006 when function docstring is followed by whitespace but no newline
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: fix/b006
created_at: 2023-09-05T15:01:27Z
updated_at: 2023-09-05T16:10:58Z
url: https://github.com/astral-sh/ruff/pull/7160
synced_at: 2026-01-12T15:55:23Z
```

# Fix B006 when function docstring is followed by whitespace but no newline

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/7155

---

_Comment by @github-actions[bot] on 2023-09-05 15:19_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Label `bug` added by @zanieb on 2023-09-05 15:49_

---

_@charliermarsh approved on 2023-09-05 15:52_

This looks right to me.

---

_@zanieb reviewed on 2023-09-05 15:55_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_bugbear/rules/mutable_argument_default.rs`:178 on 2023-09-05 15:55_

Do we care about the cost of calculating the `full_line_end` twice here? (we do it on L183 as well)

I presume not since this is a rare case.

---

_Marked ready for review by @zanieb on 2023-09-05 15:55_

---

_Merged by @zanieb on 2023-09-05 16:10_

---

_Closed by @zanieb on 2023-09-05 16:10_

---

_Branch deleted on 2023-09-05 16:10_

---
