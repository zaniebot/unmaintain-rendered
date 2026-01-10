```yaml
number: 11434
title: Be compatible with non-PEP440 version under env var UV_NON_PEP440_COMPAT
type: pull_request
state: closed
author: JakkuSakura
labels: []
assignees: []
base: main
head: version-compat
created_at: 2025-02-12T04:38:08Z
updated_at: 2025-02-27T00:36:19Z
url: https://github.com/astral-sh/uv/pull/11434
synced_at: 2026-01-10T11:10:38Z
```

# Be compatible with non-PEP440 version under env var UV_NON_PEP440_COMPAT

---

_Pull request opened by @JakkuSakura on 2025-02-12 04:38_

## Summary
Under env var UV_NON_PEP440_COMPAT, the version parser will tolerate non-PEP440 versions like the legacy pip. This will probably fix https://github.com/astral-sh/uv/issues/5478

## Test Plan
Tests included



---

_Comment by @codspeed-hq[bot] on 2025-02-12 04:44_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/JakkuSakura%3Aversion-compat)

### Merging #11434 will **degrade performances by 13.14%**

<sub>Comparing <code>JakkuSakura:version-compat</code> (5dac27b) with <code>main</code> (1cd9c37)</sub>



### Summary

`❌ 1` regressions  
`✅ 13` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/JakkuSakura%3Aversion-compat)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` wheelname_tag_compatibility[flyte-short-incompatible] `` | 771.4 ns | 888.1 ns | -13.14% |


---

_Comment by @JakkuSakura on 2025-02-12 09:17_

Seems the CI for windows is random and unrelated

---

_Review requested from @konstin by @zanieb on 2025-02-12 14:49_

---

_Review request for @konstin removed by @konstin on 2025-02-12 16:04_

---

_Review requested from @charliermarsh by @konstin on 2025-02-12 16:04_

---

_Comment by @charliermarsh on 2025-02-12 16:31_

I appreciate the PR but I'd prefer not to support this. It seems to be a violation of standards. What's the concrete problem you're trying to solve?

---

_Comment by @JakkuSakura on 2025-02-20 07:41_

Understood. We are migrating our legacy python codebase to latest python version and ecosystem. Our versioning is non-standard because our codebase predates the standard. Having this feature help us to migrate our projects to uv. Without this, it's not possible to use tools other than legacy pip and setup.py. Fixing them one by one would take a long time.

I hoped to help other users with this PR, but fine just living in a fork


---

_Comment by @charliermarsh on 2025-02-27 00:36_

Okay, got it. I do appreciate the PR but I think I'd prefer to pass on this change given that it hasn't been highly requested by users.

---

_Closed by @charliermarsh on 2025-02-27 00:36_

---
