```yaml
number: 16584
title: Docker builds target rust toolchain with bundled musl v1.2.5
type: pull_request
state: merged
author: samypr100
labels: []
assignees: []
merged: true
base: main
head: docker-musl-125
created_at: 2025-11-03T23:54:18Z
updated_at: 2025-11-05T01:46:52Z
url: https://github.com/astral-sh/uv/pull/16584
synced_at: 2026-01-10T06:28:12Z
```

# Docker builds target rust toolchain with bundled musl v1.2.5

---

_Pull request opened by @samypr100 on 2025-11-03 23:54_

## Summary

Addresses https://github.com/astral-sh/uv/issues/8450 for docker builds

## Test Plan

* Ubuntu Tests [CI Run](https://github.com/samypr100/uv/actions/runs/19054484993/job/54421786118) with `nightly-2025-11-02` set in root `rust-toolchain.toml`
* Manually test functionality using locally built binaries from cargo-zigbuild

---

_Marked ready for review by @samypr100 on 2025-11-04 02:48_

---

_Review requested from @zanieb by @konstin on 2025-11-04 16:24_

---

_Review requested from @Gankra by @konstin on 2025-11-04 16:24_

---

_@Gankra approved on 2025-11-04 18:52_

rad! this is basically exactly the solution we were discussing shipping.

---

_Merged by @Gankra on 2025-11-04 18:53_

---

_Closed by @Gankra on 2025-11-04 18:53_

---

_Comment by @zanieb on 2025-11-04 19:10_

Wow thanks Sam!

---

_Comment by @charliermarsh on 2025-11-04 19:11_

@samypr100 you rock!

---

_Branch deleted on 2025-11-05 01:46_

---
