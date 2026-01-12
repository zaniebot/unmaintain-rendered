```yaml
number: 9970
title: Support 32-bit OS on 64-bit host
type: pull_request
state: merged
author: konstin
labels:
  - bug
assignees: []
merged: true
base: main
head: konsti/32-os-on64-bit-host-but-its-arm
created_at: 2024-12-17T12:46:55Z
updated_at: 2024-12-17T14:17:40Z
url: https://github.com/astral-sh/uv/pull/9970
synced_at: 2026-01-12T16:09:04Z
```

# Support 32-bit OS on 64-bit host

---

_@konstin_

When using a 32-bit OS on 64-bit host, almost all Python std methods will report a 64-bit aarch64, but we most not install 64-bit executables since Python is actually 32-bit, identifiable through `struct.calcsize("P") == 4`.

Porting https://github.com/pypa/packaging/blob/4dc334c86d43f83371b194ca91618ed99e0e49ca/src/packaging/tags.py#L539-L543 to uv.

Tested on a raspberry pi 4 with a 64-bit host raspbian and `docker run -it --rm -v arm32v7/ubuntu` as 32-bit "host".

Fixes #9842

---

_Label `bug` added by @konstin on 2024-12-17 12:46_

---

_Renamed from "Support 32-bit OS on 64-bit ARM host" to "Support 32-bit OS on 64-bit host" by @konstin on 2024-12-17 12:47_

---

_@charliermarsh approved on 2024-12-17 13:29_

---

_Merged by @konstin on 2024-12-17 13:35_

---

_Closed by @konstin on 2024-12-17 13:35_

---

_Branch deleted on 2024-12-17 13:35_

---

_Comment by @zanieb on 2024-12-17 14:17_

@konstin thanks for taking this one!

---
