```yaml
number: 13743
title: "Propagate credentials to files on devpi indexes ending in `/+simple`"
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/devpi-s
created_at: 2025-05-30T20:51:18Z
updated_at: 2025-06-02T00:20:58Z
url: https://github.com/astral-sh/uv/pull/13743
synced_at: 2026-01-10T11:10:42Z
```

# Propagate credentials to files on devpi indexes ending in `/+simple`

---

_Pull request opened by @zanieb on 2025-05-30 20:51_

Closes https://github.com/astral-sh/uv/issues/13737

Not much to say here — devpi is common enough that we should support their weird `/+simple` URL format.

---

_Label `bug` added by @zanieb on 2025-05-30 20:51_

---

_Comment by @codspeed-hq[bot] on 2025-05-30 21:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Fdevpi-s)

### Merging #13743 will **improve performances by 15.79%**

<sub>Comparing <code>zb/devpi-s</code> (f641e6e) with <code>main</code> (13a86a2)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 110 ns | 95 ns | +15.79% |


---

_Marked ready for review by @zanieb on 2025-05-30 21:28_

---

_Review requested from @Gankra by @zanieb on 2025-05-30 21:28_

---

_Assigned to @Gankra by @zanieb on 2025-05-30 21:28_

---

_@charliermarsh approved on 2025-06-02 00:15_

---

_Merged by @charliermarsh on 2025-06-02 00:20_

---

_Closed by @charliermarsh on 2025-06-02 00:20_

---

_Branch deleted on 2025-06-02 00:20_

---
