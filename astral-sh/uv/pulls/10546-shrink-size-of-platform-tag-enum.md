```yaml
number: 10546
title: Shrink size of platform tag enum
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/shrink-tags
created_at: 2025-01-12T20:44:14Z
updated_at: 2025-01-14T03:14:00Z
url: https://github.com/astral-sh/uv/pull/10546
synced_at: 2026-01-10T11:44:57Z
```

# Shrink size of platform tag enum

---

_Pull request opened by @charliermarsh on 2025-01-12 20:44_


## Summary

Reduces it from 56 bytes to 16 bytes.


---

_Label `performance` added by @charliermarsh on 2025-01-12 20:47_

---

_@charliermarsh reviewed on 2025-01-12 20:48_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/platform_tag.rs`:59 on 2025-01-12 20:48_

It's simpler to just store this as `"12_x86_64"` rather than `"12"` and `Arch::X86_64`. We don't use the structured values anywhere (these variants are rare).

---

_Comment by @codspeed-hq[bot] on 2025-01-12 20:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fshrink-tags)

### Merging #10546 will **improve performances by 31.99%**

<sub>Comparing <code>charlie/shrink-tags</code> (c60b991) with <code>main</code> (3fd090b)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/shrink-tags` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` build_platform_tags[burntsushi-archlinux] `` | 368.4 µs | 279.1 µs | +31.99% |


---

_@konstin approved on 2025-01-13 10:37_

---

_@zanieb approved on 2025-01-13 18:05_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/lib.rs`:1347 on 2025-01-13 18:29_

nice!

---

_@konstin approved on 2025-01-13 18:30_

---

_Merged by @charliermarsh on 2025-01-14 03:14_

---

_Closed by @charliermarsh on 2025-01-14 03:14_

---

_Branch deleted on 2025-01-14 03:14_

---
