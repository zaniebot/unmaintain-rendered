```yaml
number: 14982
title: Use a unified variant cache across resolver and installer
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: charlie/wheel-variant
head: charlie/wheel-variant-cache
created_at: 2025-07-30T21:04:03Z
updated_at: 2025-07-30T23:56:12Z
url: https://github.com/astral-sh/uv/pull/14982
synced_at: 2026-01-10T06:53:02Z
```

# Use a unified variant cache across resolver and installer

---

_Pull request opened by @charliermarsh on 2025-07-30 21:04_

## Summary

Resolves a couple different TODOs around variant caching. We don't yet cache to disk, though.


---

_Review requested from @konstin by @charliermarsh on 2025-07-30 21:04_

---

_@charliermarsh reviewed on 2025-07-30 21:04_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1324 on 2025-07-30 21:04_

Resolved this TODO.

---

_@charliermarsh reviewed on 2025-07-30 21:05_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:1331 on 2025-07-30 21:05_

Resolved this TODO.

---

_@charliermarsh reviewed on 2025-07-30 21:05_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/index.rs`:28 on 2025-07-30 21:05_

Resolved this TODO.

---

_Comment by @codspeed-hq[bot] on 2025-07-30 21:13_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fwheel-variant-cache?runnerMode=Instrumentation)

### Merging #14982 will **degrade performances by 18.53%**

<sub>Comparing <code>charlie/wheel-variant-cache</code> (6fccf51) with <code>main</code> (3df972f)[^unexpected-base]</sub>
[^unexpected-base]: No successful run was found on <code>charlie/wheel-variant</code> (863517e) during the generation of this report, so <code>main</code> (3df972f) was used instead as the comparison base. There might be some changes unrelated to this pull request in this report.



### Summary

`❌ 1` regressions  
`✅ 2` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Fwheel-variant-cache?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` resolve_warm_jupyter_universal `` | 202.2 ms | 248.1 ms | -18.53% |


---

_@charliermarsh reviewed on 2025-07-30 23:32_

---

_Review comment by @charliermarsh on `crates/uv-distribution/src/distribution_database.rs`:624 on 2025-07-30 23:32_

We now cache the provider calls in-memory (not to-disk), and run them in parallel.

---

_Marked ready for review by @charliermarsh on 2025-07-30 23:51_

---

_Merged by @charliermarsh on 2025-07-30 23:56_

---

_Closed by @charliermarsh on 2025-07-30 23:56_

---

_Branch deleted on 2025-07-30 23:56_

---
