```yaml
number: 16091
title: "Show variants in `uv pip list`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: charlie/wheel-variant
head: charlie/wheel-variant-pip-list
created_at: 2025-10-01T19:55:04Z
updated_at: 2025-10-01T20:06:24Z
url: https://github.com/astral-sh/uv/pull/16091
synced_at: 2026-01-10T06:36:15Z
```

# Show variants in `uv pip list`

---

_Pull request opened by @charliermarsh on 2025-10-01 19:55_

## Summary

Looks like:

```
Package Version Variant
------- ------- ----------
numpy   2.3.3   accelerate
scipy   1.16.2  accelerate
```

Closes #16086.


---

_Review requested from @konstin by @charliermarsh on 2025-10-01 19:55_

---

_Label `enhancement` added by @charliermarsh on 2025-10-01 19:55_

---

_Label `preview` added by @charliermarsh on 2025-10-01 19:55_

---

_Marked ready for review by @charliermarsh on 2025-10-01 19:55_

---

_Merged by @charliermarsh on 2025-10-01 20:05_

---

_Closed by @charliermarsh on 2025-10-01 20:05_

---

_Branch deleted on 2025-10-01 20:05_

---

_Comment by @codspeed-hq[bot] on 2025-10-01 20:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fwheel-variant-pip-list?runnerMode=Instrumentation)

### Merging #16091 will **degrade performances by 23.49%**

<sub>Comparing <code>charlie/wheel-variant-pip-list</code> (bec7fc7) with <code>main</code> (d51a1e7)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>charlie/wheel-variant</code> (072866b) during the generation of this report, so <code>main</code> (d51a1e7) was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`❌ 2` regressions  
`✅ 1` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Fwheel-variant-pip-list?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` resolve_warm_airflow `` | 760.3 ms | 867 ms | -12.3% |
| ❌ | `` resolve_warm_jupyter_universal `` | 204 ms | 266.6 ms | -23.49% |


---
