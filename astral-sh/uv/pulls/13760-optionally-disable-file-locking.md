```yaml
number: 13760
title: optionally disable file locking
type: pull_request
state: closed
author: CodeMan62
labels: []
assignees: []
draft: true
base: main
head: disable-file-locking
created_at: 2025-06-01T12:30:53Z
updated_at: 2025-07-11T14:17:56Z
url: https://github.com/astral-sh/uv/pull/13760
synced_at: 2026-01-12T16:10:50Z
```

# optionally disable file locking

---

_@CodeMan62_

## Summary
Closes #13626 
this PR adds a `UV_UNSAFE_IGNORE_FLOCK` to disable file locking (flock) operations 

## Test Plan
manual testing 


---

_Comment by @codspeed-hq[bot] on 2025-06-01 12:46_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/CodeMan62%3Adisable-file-locking)

### Merging #13760 will **improve performances by 15.79%**

<sub>Comparing <code>CodeMan62:disable-file-locking</code> (184fbca) with <code>main</code> (13a86a2)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 110 ns | 95 ns | +15.79% |


---

_@zanieb reviewed on 2025-06-01 15:16_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:3601 on 2025-06-01 15:16_

This needs to be a global option if we're going to expose it in the CLI. I'd be tempted to just access the environment variable directly instead though.

---

_Review comment by @zanieb on `crates/uv-static/src/env_vars.rs`:153 on 2025-06-01 15:16_

Just wondering, why "IGNORE" instead of "DISABLE"?

---

_@zanieb reviewed on 2025-06-01 15:16_

---

_Converted to draft by @zanieb on 2025-06-01 15:16_

---

_Comment by @zanieb on 2025-06-01 15:17_

(Moving this to a draft because this option doesn't do anything yet)

---

_@CodeMan62 reviewed on 2025-06-01 15:47_

---

_Review comment by @CodeMan62 on `crates/uv-static/src/env_vars.rs`:153 on 2025-06-01 15:47_

this name was preffered in issue but let me change it 

---

_Comment by @CodeMan62 on 2025-07-11 14:17_

closing as it was stale and also issue is closed 

---

_Closed by @CodeMan62 on 2025-07-11 14:17_

---

_Branch deleted on 2025-07-11 14:17_

---
