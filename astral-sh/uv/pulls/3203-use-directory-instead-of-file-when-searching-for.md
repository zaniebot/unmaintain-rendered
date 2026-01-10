```yaml
number: 3203
title: Use directory instead of file when searching for uv.toml file
type: pull_request
state: merged
author: haarisr
labels:
  - bug
  - configuration
assignees: []
merged: true
base: main
head: bugfix
created_at: 2024-04-23T08:06:01Z
updated_at: 2024-04-23T18:36:42Z
url: https://github.com/astral-sh/uv/pull/3203
synced_at: 2026-01-10T14:37:54Z
```

# Use directory instead of file when searching for uv.toml file

---

_Pull request opened by @haarisr on 2024-04-23 08:06_

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The function `find_in_directory` joins `uv.toml`. The initial join in `user` is redundant.

Also a small comment fix.

## Test Plan

<!-- How was it tested? -->

Not really sure how to test this. Please suggest if any tests need to be added.

---

_Renamed from "Bugfix" to "Use directory instead of file when searching for uv.toml file" by @haarisr on 2024-04-23 08:09_

---

_@charliermarsh reviewed on 2024-04-23 12:59_

---

_Review comment by @charliermarsh on `crates/uv-workspace/src/workspace.rs`:25 on 2024-04-23 12:59_

Oh, I think this should just be `read_file` and not `find_in_directory`?

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-23 16:02_

---

_Label `bug` added by @charliermarsh on 2024-04-23 16:02_

---

_Label `configuration` added by @charliermarsh on 2024-04-23 16:02_

---

_@haarisr reviewed on 2024-04-23 18:23_

---

_Review comment by @haarisr on `crates/uv-workspace/src/workspace.rs`:25 on 2024-04-23 18:23_

Fixed

---

_@charliermarsh approved on 2024-04-23 18:36_

---

_Merged by @charliermarsh on 2024-04-23 18:36_

---

_Closed by @charliermarsh on 2024-04-23 18:36_

---

_Comment by @charliermarsh on 2024-04-23 18:36_

Thanks!

---
