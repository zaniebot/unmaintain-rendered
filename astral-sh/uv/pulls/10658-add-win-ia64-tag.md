```yaml
number: 10658
title: "Add `win_ia64` tag"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/ia64
created_at: 2025-01-15T22:43:55Z
updated_at: 2025-01-15T23:14:51Z
url: https://github.com/astral-sh/uv/pull/10658
synced_at: 2026-01-10T11:45:03Z
```

# Add `win_ia64` tag

---

_Pull request opened by @charliermarsh on 2025-01-15 22:43_

## Summary

No practical effect, but it is a valid tag.


---

_Marked ready for review by @charliermarsh on 2025-01-15 22:43_

---

_Label `enhancement` added by @charliermarsh on 2025-01-15 22:44_

---

_@zanieb approved on 2025-01-15 22:46_

---

_Comment by @codspeed-hq[bot] on 2025-01-15 23:10_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fia64)

### Merging #10658 will **degrade performances by 16.28%**

<sub>Comparing <code>charlie/ia64</code> (5d3480d) with <code>main</code> (37e31c3)</sub>



### Summary

`❌ 2` regressions  
`✅ 12` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Fia64)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/ia64` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` build_platform_tags[burntsushi-archlinux] `` | 279.1 µs | 324.2 µs | -13.9% |
| ❌ | `` wheelname_tag_compatibility[flyte-long-incompatible] `` | 1.2 µs | 1.5 µs | -16.28% |


---

_Merged by @charliermarsh on 2025-01-15 23:14_

---

_Closed by @charliermarsh on 2025-01-15 23:14_

---

_Branch deleted on 2025-01-15 23:14_

---
