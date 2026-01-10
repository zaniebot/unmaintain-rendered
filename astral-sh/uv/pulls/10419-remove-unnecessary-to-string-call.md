```yaml
number: 10419
title: "Remove unnecessary `.to_string()` call"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/to-string
created_at: 2025-01-08T23:46:33Z
updated_at: 2025-01-09T00:00:36Z
url: https://github.com/astral-sh/uv/pull/10419
synced_at: 2026-01-10T11:44:48Z
```

# Remove unnecessary `.to_string()` call

---

_Pull request opened by @charliermarsh on 2025-01-08 23:46_

_No description provided._

---

_Label `internal` added by @charliermarsh on 2025-01-08 23:46_

---

_Marked ready for review by @charliermarsh on 2025-01-08 23:46_

---

_Comment by @codspeed-hq[bot] on 2025-01-08 23:52_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fto-string)

### Merging #10419 will **improve performances by 29.86%**

<sub>Comparing <code>charlie/to-string</code> (ac9cf07) with <code>main</code> (f65fcf2)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/to-string` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` build_platform_tags[burntsushi-archlinux] `` | 1,247.8 µs | 960.9 µs | +29.86% |


---

_Merged by @charliermarsh on 2025-01-08 23:59_

---

_Closed by @charliermarsh on 2025-01-08 23:59_

---

_Branch deleted on 2025-01-08 23:59_

---

_Label `internal` removed by @charliermarsh on 2025-01-09 00:00_

---

_Label `performance` added by @charliermarsh on 2025-01-09 00:00_

---
