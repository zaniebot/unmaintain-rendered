```yaml
number: 1749
title: "Don't preserve timestamp in streaming unzip"
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/dont-preserve-timestamp-streaming
created_at: 2024-02-20T12:27:25Z
updated_at: 2024-02-20T14:54:30Z
url: https://github.com/astral-sh/uv/pull/1749
synced_at: 2026-01-12T16:04:42Z
```

# Don't preserve timestamp in streaming unzip

---

_@konstin_

## Summary

Don't preserve mtime to work around alexcrichton/tar-rs#349. Same as #634 except for the streaming unzip.

Fixes #1748.

## Test Plan

Added the tomli source dist as test case.


---

_Label `bug` added by @konstin on 2024-02-20 12:27_

---

_@konstin reviewed on 2024-02-20 12:28_

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:95 on 2024-02-20 12:28_

@charliermarsh Do you remember why we have that here but not in the sync impl? Docs say "This flag is disabled by default" in both impls.

---

_@charliermarsh reviewed on 2024-02-20 13:48_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:95 on 2024-02-20 13:48_

I think this is a mistake / typo. I probably meant for this to be mtime.

---

_@charliermarsh reviewed on 2024-02-20 13:48_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:95 on 2024-02-20 13:48_

We probably should be preserving permissions, right?

---

_@charliermarsh approved on 2024-02-20 14:46_

---

_@konstin reviewed on 2024-02-20 14:49_

---

_Review comment by @konstin on `crates/uv-extract/src/stream.rs`:95 on 2024-02-20 14:49_

After https://github.com/astral-sh/uv/pull/1743, i think we should preserve the execute permission, but everything else (read, write and owner, as far as i can tell) should always be set to os defaults. We can adapt the same logic as for wheel for tars (#1743).

---

_@charliermarsh reviewed on 2024-02-20 14:50_

---

_Review comment by @charliermarsh on `crates/uv-extract/src/stream.rs`:95 on 2024-02-20 14:50_

Yeah I was wondering if we were missing that for tars.

---

_Merged by @charliermarsh on 2024-02-20 14:54_

---

_Closed by @charliermarsh on 2024-02-20 14:54_

---

_Branch deleted on 2024-02-20 14:54_

---
