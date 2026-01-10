```yaml
number: 11085
title: "Allow `--exclude-newer` to be set at the test context level"
type: pull_request
state: merged
author: charliermarsh
labels:
  - testing
assignees: []
merged: true
base: main
head: charlie/ex-newer
created_at: 2025-01-29T23:35:58Z
updated_at: 2025-01-30T00:44:22Z
url: https://github.com/astral-sh/uv/pull/11085
synced_at: 2026-01-10T11:10:34Z
```

# Allow `--exclude-newer` to be set at the test context level

---

_Pull request opened by @charliermarsh on 2025-01-29 23:35_

## Summary

This is a lot simpler, since you don't need tor remember to set it on every command.


---

_Label `testing` added by @charliermarsh on 2025-01-29 23:36_

---

_@konstin approved on 2025-01-29 23:39_

---

_@zanieb approved on 2025-01-29 23:57_

---

_Comment by @charliermarsh on 2025-01-30 00:23_

I removed several `env_remove(EnvVars::UV_EXCLUDE_NEWER)` usages too, and replaced them with hard dates.

---

_Merged by @charliermarsh on 2025-01-30 00:44_

---

_Closed by @charliermarsh on 2025-01-30 00:44_

---

_Branch deleted on 2025-01-30 00:44_

---
