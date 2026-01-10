```yaml
number: 12299
title: "Omit wheels from lockfile based on `--exclude-newer`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/exc
created_at: 2025-03-18T19:27:02Z
updated_at: 2025-03-22T16:27:12Z
url: https://github.com/astral-sh/uv/pull/12299
synced_at: 2026-01-10T11:10:39Z
```

# Omit wheels from lockfile based on `--exclude-newer`

---

_Pull request opened by @charliermarsh on 2025-03-18 19:27_

## Summary

We respect `--exclude-newer` during resolution, but we weren't applying it to individual _files_ when writing the lockfile. As a result, if wheels were added to a distribution after its initial release, we'd end up including them in the lockfile, even if they were uploaded after the `--exclude-newer` date.

Closes https://github.com/astral-sh/uv/issues/12296.


---

_Label `bug` added by @charliermarsh on 2025-03-18 19:27_

---

_Marked ready for review by @charliermarsh on 2025-03-18 19:28_

---

_Review requested from @Gankra by @charliermarsh on 2025-03-18 19:29_

---

_Review requested from @zanieb by @charliermarsh on 2025-03-18 19:29_

---

_Review requested from @konstin by @charliermarsh on 2025-03-18 19:29_

---

_Comment by @zanieb on 2025-03-18 19:30_

Is there going to be a user-facing consequence here for people with `--locked`?

---

_Comment by @charliermarsh on 2025-03-18 19:37_

No.

---

_Comment by @charliermarsh on 2025-03-18 20:13_

There's a panic in the benchmark script setup that looks real.

---

_@charliermarsh reviewed on 2025-03-18 20:21_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:519 on 2025-03-18 20:21_

This isn't valid because we need to adjust `best_wheel_index` above.

---

_Comment by @charliermarsh on 2025-03-18 20:30_

As a reminder (partly for myself), I think the reason we don't filter these out entirely (which would be way simpler) is because we want to be able to show them in error messages (i.e., we want to be able to say "There were wheels, but they were too new").

---

_Review comment by @charliermarsh on `crates/uv-bench/benches/uv.rs`:148 on 2025-03-18 20:45_

This depends on https://pypi.org/project/methodtools/0.4.7/#files, which added a wheel on August 23, 2024. So we can bump this to avoid building it from source.

---

_@charliermarsh reviewed on 2025-03-18 20:45_

---

_Comment by @codspeed-hq[bot] on 2025-03-18 20:52_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fexc)

### Merging #12299 will **improve performances by 20.51%**

<sub>Comparing <code>charlie/exc</code> (0964b22) with <code>main</code> (a95f4cf)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` resolve_warm_airflow `` | 702.2 ms | 582.7 ms | +20.51% |


---

_Comment by @zanieb on 2025-03-19 00:17_

The benchmark change is pretty funny. I'll review this tomorrow, thanks for the quick turnaround.

---

_Closed by @zanieb on 2025-03-19 00:17_

---

_Reopened by @zanieb on 2025-03-19 00:17_

---

_Comment by @zanieb on 2025-03-19 00:17_

(oops)

---

_Comment by @konstin on 2025-03-19 11:21_

I can reproduce a speedup in the right range by bumping the cutoff date:

```
Benchmark 1: uv pip compile --exclude-newer 2024-08-08 a.txt
  Time (mean ± σ):      92.2 ms ±   2.2 ms    [User: 100.6 ms, System: 108.0 ms]
  Range (min … max):    87.9 ms …  96.7 ms    32 runs
 
Benchmark 2: uv pip compile --exclude-newer 2024-09-01 a.txt
  Time (mean ± σ):      80.5 ms ±   4.5 ms    [User: 83.2 ms, System: 102.8 ms]
  Range (min … max):    74.2 ms …  97.3 ms    39 runs
```

---

_@konstin approved on 2025-03-19 11:21_

---

_Review comment by @Gankra on `crates/uv-distribution-types/src/prioritized_distribution.rs`:652 on 2025-03-19 12:39_

Is there a reason to not consider all `Incompatible()` instances "excluded"?

---

_@Gankra approved on 2025-03-19 12:43_

---

_@charliermarsh reviewed on 2025-03-22 15:58_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:652 on 2025-03-22 15:58_

Yeah...`ExcludeNewer` is kind of the odd one out here. "Incompatible" can also include things like "not compatible with the current platform". But for the `ExcludeNewer` distributions, we basically want to act like they don't exist at all. It would be easier if we could filter them out in the first place, but the problem is, we _do_ want to show dedicated error messages for them, so we have to propagate them throughout the system.

---

_Merged by @charliermarsh on 2025-03-22 16:27_

---

_Closed by @charliermarsh on 2025-03-22 16:27_

---

_Branch deleted on 2025-03-22 16:27_

---
