```yaml
number: 6291
title: "Invalidate `uv.lock` when virtual `dev-dependencies` change"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - lock
assignees: []
merged: true
base: main
head: charlie/res
created_at: 2024-08-21T00:50:25Z
updated_at: 2024-08-21T01:25:39Z
url: https://github.com/astral-sh/uv/pull/6291
synced_at: 2026-01-10T13:09:51Z
```

# Invalidate `uv.lock` when virtual `dev-dependencies` change

---

_Pull request opened by @charliermarsh on 2024-08-21 00:50_

## Summary

For non-virtual workspaces, these are covered by the _members_. But for virtual workspaces, they aren't captured anywhere else in the lock. So, we weren't invalidating `uv.lock` when the dev dependencies changed, which led to a panic.

Closes https://github.com/astral-sh/uv/issues/6288


---

_Label `bug` added by @charliermarsh on 2024-08-21 00:50_

---

_Label `lock` added by @charliermarsh on 2024-08-21 00:50_

---

_Merged by @charliermarsh on 2024-08-21 01:25_

---

_Closed by @charliermarsh on 2024-08-21 01:25_

---

_Branch deleted on 2024-08-21 01:25_

---
