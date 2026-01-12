```yaml
number: 10126
title: "Move installable targets out of `uv-resolver` crate"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
  - no-build
  - no-test
assignees: []
merged: true
base: main
head: charlie/installable
created_at: 2024-12-23T19:26:59Z
updated_at: 2024-12-25T00:01:52Z
url: https://github.com/astral-sh/uv/pull/10126
synced_at: 2026-01-12T16:09:08Z
```

# Move installable targets out of `uv-resolver` crate

---

_@charliermarsh_

## Summary

The proximate motivation is that I want to add new variant for scripts, but `uv-resolver` can't depend on `uv-scripts` without creating a circular dependency. However, I think this _does_ just make more sense -- the resolver crate shouldn't be coupled to the various kinds of workspaces, and these details are mostly encoded in `projects/lock.rs` and similar files.


---

_Label `internal` added by @charliermarsh on 2024-12-23 19:27_

---

_Converted to draft by @charliermarsh on 2024-12-23 22:45_

---

_Label `no-build` added by @charliermarsh on 2024-12-24 00:46_

---

_Label `no-test` added by @charliermarsh on 2024-12-24 00:46_

---

_Marked ready for review by @charliermarsh on 2024-12-24 23:39_

---

_Merged by @charliermarsh on 2024-12-25 00:01_

---

_Closed by @charliermarsh on 2024-12-25 00:01_

---

_Branch deleted on 2024-12-25 00:01_

---
