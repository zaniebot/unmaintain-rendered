```yaml
number: 10655
title: Avoid failing when deserializing unknown tags
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/unknown
created_at: 2025-01-15T22:19:59Z
updated_at: 2025-01-15T23:03:30Z
url: https://github.com/astral-sh/uv/pull/10655
synced_at: 2026-01-10T11:45:02Z
```

# Avoid failing when deserializing unknown tags

---

_Pull request opened by @charliermarsh on 2025-01-15 22:19_

## Summary

Closes https://github.com/astral-sh/uv/issues/10654#issuecomment-2594022975.


---

_Review requested from @zanieb by @charliermarsh on 2025-01-15 22:22_

---

_@zanieb reviewed on 2025-01-15 22:23_

---

_Review comment by @zanieb on `crates/uv/tests/it/lock.rs`:3440 on 2025-01-15 22:23_

This comment not relevant right? (and similar)

---

_@zanieb reviewed on 2025-01-15 22:24_

---

_Review comment by @zanieb on `crates/uv-platform-tags/src/abi_tag.rs`:274 on 2025-01-15 22:24_

Is there a more graceful way we should be handling them? This sort of looks "handled".

---

_@charliermarsh reviewed on 2025-01-15 22:25_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/abi_tag.rs`:274 on 2025-01-15 22:25_

I think ideally we'd keep what we have today (so we have good error messages and keep the struct size small), but we'd filter out unknown tags when deserializing.

---

_Marked ready for review by @charliermarsh on 2025-01-15 22:27_

---

_@charliermarsh reviewed on 2025-01-15 22:27_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/abi_tag.rs`:274 on 2025-01-15 22:27_

What I have here is also acceptable though.

---

_Label `bug` added by @charliermarsh on 2025-01-15 22:27_

---

_@zanieb approved on 2025-01-15 22:32_

---

_Comment by @codspeed-hq[bot] on 2025-01-15 22:37_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Funknown)

### Merging #10655 will **degrade performances by 13.9%**

<sub>Comparing <code>charlie/unknown</code> (866caef) with <code>main</code> (37e31c3)</sub>



### Summary

`❌ 1` regressions  
`✅ 13` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie%2Funknown)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/unknown` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` build_platform_tags[burntsushi-archlinux] `` | 279.1 µs | 324.2 µs | -13.9% |


---

_@Drewroen approved on 2025-01-15 22:48_

---

_@messerli-wallace approved on 2025-01-15 22:48_

---

_Merged by @charliermarsh on 2025-01-15 23:03_

---

_Closed by @charliermarsh on 2025-01-15 23:03_

---

_Branch deleted on 2025-01-15 23:03_

---
