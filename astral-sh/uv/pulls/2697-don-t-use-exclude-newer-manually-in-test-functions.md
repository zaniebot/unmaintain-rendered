```yaml
number: 2697
title: "Don't use exclude newer manually in test functions"
type: pull_request
state: merged
author: konstin
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: konsti/dont-use-exclude-newer-explictly
created_at: 2024-03-27T17:57:50Z
updated_at: 2024-04-01T14:07:26Z
url: https://github.com/astral-sh/uv/pull/2697
synced_at: 2026-01-12T16:05:10Z
```

# Don't use exclude newer manually in test functions

---

_@konstin_

With this change, all usages of `EXCLUDE_NEWER` are now in command wrappers, not in the test functions themselves. 

For the venv test, i refactored them into the same kind of test context abstraction that the other test modules have in the second commit.

The third commit makes`"INSTA_FILTERS` "private", removing the last remaining individual usage.

Pending windows CI :crossed_fingers: 

---

_Review requested from @zanieb by @konstin on 2024-03-27 17:57_

---

_@zanieb approved on 2024-03-27 17:59_

Thanks!

---

_Label `internal` added by @zanieb on 2024-03-27 17:59_

---

_Label `testing` added by @zanieb on 2024-03-27 17:59_

---

_Marked ready for review by @konstin on 2024-03-28 10:12_

---

_Merged by @konstin on 2024-04-01 14:07_

---

_Closed by @konstin on 2024-04-01 14:07_

---

_Branch deleted on 2024-04-01 14:07_

---
