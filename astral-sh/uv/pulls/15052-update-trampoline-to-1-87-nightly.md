```yaml
number: 15052
title: Update trampoline to ~1.87 nightly
type: pull_request
state: merged
author: samypr100
labels:
  - windows
assignees: []
merged: true
base: main
head: trampoline-187-update
created_at: 2025-08-04T03:42:29Z
updated_at: 2025-09-27T22:38:52Z
url: https://github.com/astral-sh/uv/pull/15052
synced_at: 2026-01-12T16:11:33Z
```

# Update trampoline to ~1.87 nightly

---

_@samypr100_

## Summary

1. Given the upcoming 1.89 update, this bumps uv-trampoline to "~1.87" (closest nightly) from "~1.86" (closest nightly).
2. Adds additional CI check for arm builds now that runners are available.

I wasn't sure the MSRV policy applies to uv-trampoline, so I didn't go for higher than ~1.87 nightly.
This PR also fixes a build issue starting after 1.87 where fma and fmaf symbols were missing.
Temporarily dded `#[allow(clippy::ptr_eq)]` to `close_handles` as this lint should not trigger anymore in 1.88 and above.

## Test Plan

Existing tests and local build process. I did not commit the built binaries for security purposes.


---

_Label `windows` added by @samypr100 on 2025-08-04 03:42_

---

_Marked ready for review by @samypr100 on 2025-08-05 00:15_

---

_@konstin approved on 2025-08-06 12:04_

---

_Merged by @konstin on 2025-08-06 12:45_

---

_Closed by @konstin on 2025-08-06 12:45_

---

_Comment by @codspeed-hq[bot] on 2025-08-06 12:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/samypr100%3Atrampoline-187-update?runnerMode=Instrumentation)

### Merging #15052 will **degrade performances by 13.28%**

<sub>Comparing <code>samypr100:trampoline-187-update</code> (ec7f24b) with <code>main</code> (91653f5)</sub>



### Summary

`❌ 1` regressions  
`✅ 2` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/samypr100%3Atrampoline-187-update?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` resolve_warm_airflow `` | 687.3 ms | 792.6 ms | -13.28% |


---

_Branch deleted on 2025-09-27 22:38_

---
