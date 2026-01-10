```yaml
number: 12367
title: "Skip repeated directories in `PATH` when searching for Python interpreters"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/python-seen
created_at: 2025-03-21T14:12:51Z
updated_at: 2025-04-03T16:13:09Z
url: https://github.com/astral-sh/uv/pull/12367
synced_at: 2026-01-10T11:10:39Z
```

# Skip repeated directories in `PATH` when searching for Python interpreters

---

_Pull request opened by @zanieb on 2025-03-21 14:12_

Closes https://github.com/astral-sh/uv/issues/12302

The change is visible in [this commit](https://github.com/astral-sh/uv/pull/12367/commits/49be22dad9b091a39e1c013032fb740dba652abf).

---

_Label `enhancement` added by @zanieb on 2025-03-21 14:12_

---

_@zanieb reviewed on 2025-03-21 14:23_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:522 on 2025-03-21 14:23_

Indented, best viewed with https://github.com/astral-sh/uv/pull/12367/files?diff=unified&w=1

---

_Review comment by @charliermarsh on `crates/uv-python/src/discovery.rs`:4 on 2025-03-21 22:35_

Let's default to `FxHashSet`.

---

_@charliermarsh approved on 2025-03-22 00:29_

---

_Comment by @charliermarsh on 2025-03-22 00:30_

Should this be implemented by de-duping the _interpreters_ rather than the parent directories? I assume not, but it'd be good to note why.

---

_Comment by @zanieb on 2025-04-03 13:44_

> Should this be implemented by de-duping the interpreters rather than the parent directories? 

We probably _could_ but I'm not sure what it gets us. We'd spend more time collecting interpreters we've already seen. We could dedupe interpreter _paths_ instead of parent directories but that also just adds redundant directory item enumeration.

---

_Comment by @charliermarsh on 2025-04-03 14:00_

> We could dedupe interpreter paths instead of parent directories but that also just adds redundant directory item enumeration.

I think that cost of that will be really low compared to the cost of querying the interpreters. But is it even better? Do we _want_ to show symlinks of adjacent Python interpreters (like `python` vs `python3` in the same directory)? I can't rememeber.


---

_Comment by @zanieb on 2025-04-03 14:23_

> I think that cost of that will be really low compared to the cost of querying the interpreters.

I agree. I don't see why it'd be preferable though.

> But is it even better? Do we want to show symlinks of adjacent Python interpreters (like python vs python3 in the same directory)?

Yes, I think it's important we can represent this. There are a couple reasons off the top of my head...

- It's meaningful to show that `python` and `python3` are both available on the `PATH` and invoke the same interpreter
- The name of the executable is relevant for some downstream logic around "default" versions, i.e., have you opted into free-threaded Python by installing it as `python3`?

We could deduplicate those later, e.g., during the `uv python list` display (we already do some deduplication there). However, during discovery it's important to avoid filtering too aggressively.

---

_Marked ready for review by @zanieb on 2025-04-03 14:44_

---

_Comment by @codspeed-hq[bot] on 2025-04-03 14:55_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Fpython-seen)

### Merging #12367 will **degrade performances by 10.19%**

<sub>Comparing <code>zb/python-seen</code> (ef6a569) with <code>main</code> (be3d5df)</sub>



### Summary

`❌ 1` regressions  
`✅ 13` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/zb%2Fpython-seen)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` wheelname_tag_compatibility[flyte-short-incompatible] `` | 771.4 ns | 858.9 ns | -10.19% |


---

_Merged by @zanieb on 2025-04-03 16:13_

---

_Closed by @zanieb on 2025-04-03 16:13_

---

_Branch deleted on 2025-04-03 16:13_

---
