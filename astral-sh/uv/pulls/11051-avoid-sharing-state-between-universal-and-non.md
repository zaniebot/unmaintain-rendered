```yaml
number: 11051
title: Avoid sharing state between universal and non-universal resolves
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/shared-state
created_at: 2025-01-29T02:56:08Z
updated_at: 2025-01-29T03:27:33Z
url: https://github.com/astral-sh/uv/pull/11051
synced_at: 2026-01-12T16:09:39Z
```

# Avoid sharing state between universal and non-universal resolves

---

_@charliermarsh_

## Summary

This is a really subtle issue. I'm actually having trouble writing a test for it, though the problem makes sense. In short, we're sharing the `SharedState` between the `BuildContext` and the universal resolver. The `SharedState` includes `VersionMap`, which tracks incompatibilities... The incompatibilities use the platform tags, which are only present when resolving from the `BuildContext` (i.e., when resolving build dependencies). The universal resolver then fails because it sees a bunch of "incompatible" wheels that are incompatible with the current platform (i.e., the current Python interpreter).

In short, we _cannot_ share a `SharedState` across two operations that perform a universal and then a platform-specific resolution. So this PR adds separate types and fixes up any overlapping usages.

A better setup, for the future, would be to somehow share the underlying simple metadata, and only track separate `VersionMap` -- since there _is_ a bunch of data we can share. But that's a larger change.

Closes https://github.com/astral-sh/uv/issues/10977.


---

_Label `bug` added by @charliermarsh on 2025-01-29 02:56_

---

_Comment by @codspeed-hq[bot] on 2025-01-29 03:26_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fshared-state)

### Merging #11051 will **improve performances by 10.5%**

<sub>Comparing <code>charlie/shared-state</code> (720298c) with <code>main</code> (3878c00)</sub>



### Summary

`⚡ 1` improvements  
`✅ 13` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` resolve_warm_jupyter `` | 66 ms | 59.7 ms | +10.5% |


---

_Merged by @charliermarsh on 2025-01-29 03:27_

---

_Closed by @charliermarsh on 2025-01-29 03:27_

---

_Branch deleted on 2025-01-29 03:27_

---

_Comment by @charliermarsh on 2025-01-29 03:27_

What the heck? Why did this improve performance?

---
