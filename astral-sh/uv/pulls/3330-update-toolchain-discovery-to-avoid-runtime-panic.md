```yaml
number: 3330
title: Update toolchain discovery to avoid runtime panic
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/toolchain-panic
created_at: 2024-05-01T17:27:17Z
updated_at: 2024-05-02T11:44:17Z
url: https://github.com/astral-sh/uv/pull/3330
synced_at: 2026-01-10T14:37:54Z
```

# Update toolchain discovery to avoid runtime panic

---

_Pull request opened by @zanieb on 2024-05-01 17:27_

Split out of https://github.com/astral-sh/uv/pull/3266

If `UV_BOOTSTRAP_DIR` and `CARGO_MANIFEST_DIR` are both unset, we currently panic. This isn't good once we start to use managed toolchains in production. We'll need to change this more later once the toolchain directory is more user-facing.

---

_Label `internal` added by @zanieb on 2024-05-01 17:27_

---

_Comment by @codspeed-hq[bot] on 2024-05-01 17:29_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb/toolchain-panic)

### Merging #3330 will **not alter performance**

<sub>Comparing <code>zb/toolchain-panic</code> (98150aa) with <code>main</code> (614c073)</sub>



### Summary

`✅ 12` untouched benchmarks






---

_Marked ready for review by @zanieb on 2024-05-01 17:40_

---

_@konstin approved on 2024-05-02 08:21_

---

_Merged by @zanieb on 2024-05-02 11:44_

---

_Closed by @zanieb on 2024-05-02 11:44_

---

_Branch deleted on 2024-05-02 11:44_

---
