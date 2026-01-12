```yaml
number: 13120
title: Reject non-PEP 751 TOML files in install commands
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
  - breaking
assignees: []
merged: true
base: release/070
head: charlie/strict-file
created_at: 2025-04-26T14:45:39Z
updated_at: 2025-04-28T20:51:25Z
url: https://github.com/astral-sh/uv/pull/13120
synced_at: 2026-01-12T16:10:32Z
```

# Reject non-PEP 751 TOML files in install commands

---

_@charliermarsh_

## Summary

If you pass a TOML file to `uv pip install` that isn't recognized, we should just reject it instead of assuming `requirements.txt`. I just don't see a real case where it's better to let the command proceed.


---

_Label `error messages` added by @charliermarsh on 2025-04-26 14:45_

---

_Review requested from @Gankra by @charliermarsh on 2025-04-26 14:52_

---

_Review requested from @zanieb by @charliermarsh on 2025-04-26 14:52_

---

_@Gankra approved on 2025-04-28 17:55_

Will be interesting to hear if any users complain about this.

Strictly speaking it's breaking?

---

_Comment by @charliermarsh on 2025-04-28 18:26_

Yeah, I think we might as well put this in v0.7.

---

_Label `breaking` added by @charliermarsh on 2025-04-28 18:26_

---

_Added to milestone `v0.7.0` by @Gankra on 2025-04-28 18:45_

---

_Comment by @codspeed-hq[bot] on 2025-04-28 20:50_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fstrict-file)

### Merging #13120 will **improve performances by 11.79%**

<sub>Comparing <code>charlie/strict-file</code> (d9608ba) with <code>release/070</code> (7529ff1)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_tag_compatibility[flyte-short-incompatible] `` | 829.7 ns | 742.2 ns | +11.79% |


---

_Merged by @charliermarsh on 2025-04-28 20:51_

---

_Closed by @charliermarsh on 2025-04-28 20:51_

---

_Branch deleted on 2025-04-28 20:51_

---
