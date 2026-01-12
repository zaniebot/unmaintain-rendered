```yaml
number: 13119
title: "Reject non-PEP 751 filenames in `uv pip compile` and `uv export`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
  - breaking
assignees: []
merged: true
base: release/070
head: charlie/err
created_at: 2025-04-26T14:30:31Z
updated_at: 2025-04-28T20:42:05Z
url: https://github.com/astral-sh/uv/pull/13119
synced_at: 2026-01-12T16:10:32Z
```

# Reject non-PEP 751 filenames in `uv pip compile` and `uv export`

---

_@charliermarsh_

## Summary

We shouldn't let users create files that won't work in subsequent commands.

Closes https://github.com/astral-sh/uv/issues/13117.


---

_Label `error messages` added by @charliermarsh on 2025-04-26 14:45_

---

_Review requested from @Gankra by @charliermarsh on 2025-04-26 14:53_

---

_Review requested from @zanieb by @charliermarsh on 2025-04-26 14:53_

---

_@Gankra approved on 2025-04-28 17:52_

---

_Label `breaking` added by @Gankra on 2025-04-28 18:45_

---

_Added to milestone `v0.7.0` by @Gankra on 2025-04-28 18:46_

---

_Comment by @Gankra on 2025-04-28 18:46_

ah 0.7.0 needs another rebase 😅 

---

_Comment by @codspeed-hq[bot] on 2025-04-28 20:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Ferr)

### Merging #13119 will **improve performances by 11.79%**

<sub>Comparing <code>charlie/err</code> (d84b0a3) with <code>release/070</code> (7529ff1)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_tag_compatibility[flyte-short-incompatible] `` | 829.7 ns | 742.2 ns | +11.79% |


---

_Merged by @charliermarsh on 2025-04-28 20:42_

---

_Closed by @charliermarsh on 2025-04-28 20:42_

---

_Branch deleted on 2025-04-28 20:42_

---
