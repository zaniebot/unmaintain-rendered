```yaml
number: 13888
title: "Add a test case and update the error message for `required-environments` hint"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: konsti/hint-at-required-environments
head: zb/hint
created_at: 2025-06-06T18:23:58Z
updated_at: 2025-06-06T19:13:05Z
url: https://github.com/astral-sh/uv/pull/13888
synced_at: 2026-01-10T11:10:42Z
```

# Add a test case and update the error message for `required-environments` hint

---

_Pull request opened by @zanieb on 2025-06-06 18:23_

Review of #13575 

---

_@zanieb reviewed on 2025-06-06 18:25_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9499 on 2025-06-06 18:25_

This is just a dumb wheel renamed to have a Windows platform tag.

---

_@zanieb reviewed on 2025-06-06 18:26_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:26_

Hm, the "you're on" part will be platform specific still. I guess we can filter that.

---

_@zanieb reviewed on 2025-06-06 18:28_

---

_Review comment by @zanieb on `crates/uv-resolver/src/lock/mod.rs`:5056 on 2025-06-06 18:28_

I'm willing to have two messages still, but if we do I think it should be in this order, i.e.,

hint: There are only wheels for the following platforms ..
hint: Consider adding your platform to ...

I interested in seeing how a single hint feels too though

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:28_

Won't this fail on linux?

---

_@konstin reviewed on 2025-06-06 18:28_

---

_@konstin reviewed on 2025-06-06 18:36_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/mod.rs`:5056 on 2025-06-06 18:36_

The single path is def nicer.

---

_@konstin reviewed on 2025-06-06 18:37_

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:37_

Can we put a wheel a fantasy platform on packse and be able to run this test on windows too?

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:38_

We can just rename that tag to an arbitrary value, what fantasy platforms will we accept?

---

_@zanieb reviewed on 2025-06-06 18:38_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:38_

Yeah, that's my comment at https://github.com/astral-sh/uv/pull/13888#discussion_r2132665972

I think it's fine to filter this out

---

_@zanieb reviewed on 2025-06-06 18:38_

---

_@zanieb reviewed on 2025-06-06 18:42_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:42_

A fantasy platform like `foo` doesn't work ofc

---

_@zanieb reviewed on 2025-06-06 18:42_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:42_

(Added)

---

_@konstin reviewed on 2025-06-06 18:46_

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:46_

`macos_1000_0` maybe? Or similarly ridiculous high glibc.

---

_@zanieb reviewed on 2025-06-06 18:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:52_

I tried that as well as like `macos_11_0_fat` but the hint doesn't display then. Sort of hesitant to debug it at the moment, maybe we should just open a follow-up?

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:57_

(also tried `android_27_arm64_v8a`)

---

_@zanieb reviewed on 2025-06-06 18:57_

---

_@konstin reviewed on 2025-06-06 18:59_

---

_Review comment by @konstin on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 18:59_

I mean I can take that test case over, I'm confident I can find something

---

_@konstin approved on 2025-06-06 19:01_

r+ for the main changes, figuring out the wheel tag is non-blocking for me.

---

_Comment by @codspeed-hq[bot] on 2025-06-06 19:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/uv/branches/zb%2Fhint)

### Merging #13888 will **degrade performances by 11.02%**

<sub>Comparing <code>zb/hint</code> (6fa485c) with <code>konsti/hint-at-required-environments</code> (70db5cb)</sub>



### Summary

`❌ 1` regressions  
`✅ 11` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/zb%2Fhint)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` resolve_warm_jupyter `` | 60.4 ms | 67.8 ms | -11.02% |


---

_Comment by @zanieb on 2025-06-06 19:03_

I'll squash into your branch

---

_Merged by @zanieb on 2025-06-06 19:03_

---

_Closed by @zanieb on 2025-06-06 19:03_

---

_Branch deleted on 2025-06-06 19:03_

---

_@zanieb reviewed on 2025-06-06 19:13_

---

_Review comment by @zanieb on `crates/uv/tests/it/sync.rs`:9527 on 2025-06-06 19:13_

Opened #13890 to track

---
