```yaml
number: 10370
title: Re-enable zlib-ng on all platforms (except s390x, PowerPC, and FreeBSD)
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/zlib-ng
created_at: 2025-01-07T16:28:42Z
updated_at: 2025-01-07T18:04:36Z
url: https://github.com/astral-sh/uv/pull/10370
synced_at: 2026-01-10T11:44:45Z
```

# Re-enable zlib-ng on all platforms (except s390x, PowerPC, and FreeBSD)

---

_Pull request opened by @charliermarsh on 2025-01-07 16:28_

PowerPC seems to build without errors if we upgrade `zlib-ng`, but upgrading `zlib-ng` causes Windows to break (https://github.com/rust-lang/libz-sys/issues/225), and Cargo doesn't let us include two different versions.

s390x fails because it can't find `stfle`. It's possible that we could fix this by by upgrading our manylinux version and/or by upgrading GCC (which may necessitate upgrading our manylinux version), but I don't know if it's fixable without one of those things? And it's not worth bumping compatibility for that reason. \cc @konstin 

---

_Marked ready for review by @charliermarsh on 2025-01-07 17:55_

---

_Renamed from "Attempt to enable zlib-ng on all platforms" to "Re-enable zlib-ng on all platforms (except s903x, PowerPC, and FreeBSD)" by @charliermarsh on 2025-01-07 17:55_

---

_Label `performance` added by @charliermarsh on 2025-01-07 17:55_

---

_Renamed from "Re-enable zlib-ng on all platforms (except s903x, PowerPC, and FreeBSD)" to "Re-enable zlib-ng on all platforms (except s390x, PowerPC, and FreeBSD)" by @charliermarsh on 2025-01-07 17:57_

---

_Comment by @charliermarsh on 2025-01-07 18:00_

@Gankra may also find this interesting.

---

_@Gankra approved on 2025-01-07 18:00_

---

_Merged by @charliermarsh on 2025-01-07 18:04_

---

_Closed by @charliermarsh on 2025-01-07 18:04_

---

_Branch deleted on 2025-01-07 18:04_

---
