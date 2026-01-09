---
number: 16920
title: uv 0.9.14 installation fails
type: issue
state: closed
author: kasium
labels:
  - bug
assignees: []
created_at: 2025-12-02T08:49:14Z
updated_at: 2025-12-02T08:54:52Z
url: https://github.com/astral-sh/uv/issues/16920
synced_at: 2026-01-07T13:12:19-06:00
---

# uv 0.9.14 installation fails

---

_Issue opened by @kasium on 2025-12-02 08:49_

### Summary

I have no idea why this occurs, but suddenly the installation of uv itself fails with
```
hint: You're on Linux (`manylinux_2_31_x86_64`), but `uv` (v0.9.14) only has wheels for the following platforms: `manylinux_2_17_ppc64le`, `manylinux_2_17_s390x`, `manylinux_2_28_aarch64`, `manylinux2014_ppc64le`, `manylinux2014_s390x`, `musllinux_1_1_i686`, `macosx_10_12_x86_64`, `macosx_11_0_arm64`; consider adding your platform to `tool.uv.required-environments` to ensure uv resolves to a version with compatible wheels
```

0.9.13 works w/o issues. I don't have any environments settings active.

### Platform

SUSE Linux Enterprise Server 15 SP5

### Version

uv 0.9.13 to install 0.9.14

### Python version

3.14.0

---

_Label `bug` added by @kasium on 2025-12-02 08:49_

---

_Comment by @kasium on 2025-12-02 08:54_

Issue on my side; sorry for the noise

---

_Closed by @kasium on 2025-12-02 08:54_

---
