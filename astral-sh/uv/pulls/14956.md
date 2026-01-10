```yaml
number: 14956
title: "Make the `BuildDispatch` interpreter method async"
type: pull_request
state: merged
author: tdejager
labels:
  - internal
assignees: []
merged: true
base: main
head: feat/async-interpreter
created_at: 2025-07-29T14:40:11Z
updated_at: 2025-07-31T11:42:31Z
url: https://github.com/astral-sh/uv/pull/14956
synced_at: 2026-01-10T06:53:02Z
```

# Make the `BuildDispatch` interpreter method async

---

_Pull request opened by @tdejager on 2025-07-29 14:40_

This is a bit of a weird request, but in [pixi](https://pixi.sh) we are making use of this function to lazily instantiate a conda environment. Well, in actuality we are using a shim to the `BuildDispatch` to actually to only create a conda prefix, if some package needs to be built during the resolution phase. Otherwise we can resolve everything without an enviroment containing a python intepreter.

We are using a method now - that uses the runtime to run async code inside this function, as `interpreter` is the first method called on a `BuildContext` when running a source build - using `tokio::Handle::block_on`.
However was causing a deadlock in very specific situations, me and @baszalmstra + @wolfv have investigated this thoroughly, but have not been able to find the root cause. It would hang in a part of the uv code that hits the index, but that is **after** all of our initialization *and the blocking call* was completed.
Changing this to be fully async fixes the problem, this requires this method to be async though.

We get that this is not necessarily required, and we might find a workaround, but I wanted to try it this way first.

Thanks!

---

_Comment by @zanieb on 2025-07-30 13:01_

It perturbs me a little, but I don't really have the heart to say no :)

Will you fix CI?

---

_Comment by @codspeed-hq[bot] on 2025-07-31 07:24_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/tdejager%3Afeat%2Fasync-interpreter?runnerMode=Instrumentation)

### Merging #14956 will **improve performances by 10.72%**

<sub>Comparing <code>tdejager:feat/async-interpreter</code> (2b4112a) with <code>main</code> (3df972f)</sub>



### Summary

`⚡ 1` improvements  
`✅ 2` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` resolve_warm_jupyter `` | 70.5 ms | 63.7 ms | +10.72% |


---

_Comment by @tdejager on 2025-07-31 07:32_

@zanieb I understand that feeling completely :) I fixed the CI

---

_Merged by @zanieb on 2025-07-31 11:42_

---

_Closed by @zanieb on 2025-07-31 11:42_

---

_Label `internal` added by @zanieb on 2025-07-31 11:42_

---
