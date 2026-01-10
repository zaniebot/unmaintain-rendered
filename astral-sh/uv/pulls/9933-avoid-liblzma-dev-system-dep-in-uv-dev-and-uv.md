```yaml
number: 9933
title: "Avoid `liblzma-dev` system dep in uv-dev and uv-bench"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/static-lzma
created_at: 2024-12-16T12:44:22Z
updated_at: 2024-12-17T15:12:39Z
url: https://github.com/astral-sh/uv/pull/9933
synced_at: 2026-01-10T12:00:01Z
```

# Avoid `liblzma-dev` system dep in uv-dev and uv-bench

---

_Pull request opened by @konstin on 2024-12-16 12:44_

Enable `lzma-sys/static` through the performance feature not only in uv, but in uv-dev and uv-bench too, to avoid the system dependency on `liblzma-dev`.

Ref #9880


---

_@charliermarsh reviewed on 2024-12-16 16:58_

I really thought we already did this... Maybe this feature wasn't being enabled for `uv-dev`, and that's why _only_ that job was failing?

---

_@charliermarsh reviewed on 2024-12-16 17:07_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:156 on 2024-12-16 17:07_

We should also remove this from the benchmark job.

---

_Comment by @konstin on 2024-12-16 19:52_

It's part of the performance features currently, maybe that's why? (We can move it to a new default feature if it's a problem for the distro people)

---

_Comment by @charliermarsh on 2024-12-16 19:53_

Oh yeah, can you hook `uv-dev` up to the performance features please then?

---

_Comment by @charliermarsh on 2024-12-16 19:53_

Rather than adding this everywhere.

---

_Renamed from "Avoid `liblzma-dev` system dep with static crate" to "Avoid `liblzma-dev` system dep with static crate in uv-dev" by @konstin on 2024-12-17 08:32_

---

_@charliermarsh approved on 2024-12-17 13:28_

---

_Renamed from "Avoid `liblzma-dev` system dep with static crate in uv-dev" to "Avoid `liblzma-dev` system dep in uv-dev and uv-bench" by @konstin on 2024-12-17 13:53_

---

_Merged by @konstin on 2024-12-17 15:12_

---

_Closed by @konstin on 2024-12-17 15:12_

---

_Branch deleted on 2024-12-17 15:12_

---

_Label `internal` added by @konstin on 2024-12-17 15:12_

---
