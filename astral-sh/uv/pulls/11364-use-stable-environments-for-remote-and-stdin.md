```yaml
number: 11364
title: Use stable environments for remote and stdin scripts
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - no-build
assignees: []
merged: true
base: main
head: charlie/always-stable-dir
created_at: 2025-02-09T19:58:41Z
updated_at: 2025-02-12T00:54:47Z
url: https://github.com/astral-sh/uv/pull/11364
synced_at: 2026-01-12T16:09:48Z
```

# Use stable environments for remote and stdin scripts

---

_@charliermarsh_

## Summary

This is a follow-on to #11347 to use a stable directory for remote and stdin scripts. The annoying piece here was figuring out what to use as the cache key. For remote scripts, I'm using the URL; for stdin scripts, there isn't any identifying information, so I'm just using a hash of the metadata.


---

_Label `enhancement` added by @charliermarsh on 2025-02-09 19:58_

---

_Label `no-build` added by @charliermarsh on 2025-02-09 20:02_

---

_@zanieb approved on 2025-02-11 14:23_

---

_Merged by @charliermarsh on 2025-02-12 00:54_

---

_Closed by @charliermarsh on 2025-02-12 00:54_

---

_Branch deleted on 2025-02-12 00:54_

---
