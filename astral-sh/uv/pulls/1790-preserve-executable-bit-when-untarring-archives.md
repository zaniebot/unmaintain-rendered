```yaml
number: 1790
title: Preserve executable bit when untarring archives
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/tar
created_at: 2024-02-21T01:26:42Z
updated_at: 2024-02-21T14:18:45Z
url: https://github.com/astral-sh/uv/pull/1790
synced_at: 2026-01-10T15:33:24Z
```

# Preserve executable bit when untarring archives

---

_Pull request opened by @charliermarsh on 2024-02-21 01:26_

## Summary

Closes https://github.com/astral-sh/uv/issues/1767.

## Test Plan

Not sure -- I don't think has come up yet?


---

_Review requested from @konstin by @charliermarsh on 2024-02-21 01:26_

---

_Label `bug` added by @charliermarsh on 2024-02-21 01:26_

---

_Review comment by @zanieb on `crates/uv-extract/src/stream.rs`:111 on 2024-02-21 01:28_

Do we need a license?

---

_@zanieb reviewed on 2024-02-21 01:28_

---

_@charliermarsh reviewed on 2024-02-21 01:30_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:111 on 2024-02-21 01:30_

I think it's sufficient to link to the source here.

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:112 on 2024-02-21 10:41_

Could we un-nest this function?

---

_@konstin reviewed on 2024-02-21 10:45_

We need to do this in `sync::archive`, too (or remove that branch).

---

_@konstin approved on 2024-02-21 14:13_

> We need to do this in sync::archive, too (or remove that branch).

Fixed by #1809

---

_Comment by @charliermarsh on 2024-02-21 14:15_

Thanks!

---

_Merged by @charliermarsh on 2024-02-21 14:18_

---

_Closed by @charliermarsh on 2024-02-21 14:18_

---

_Branch deleted on 2024-02-21 14:18_

---
