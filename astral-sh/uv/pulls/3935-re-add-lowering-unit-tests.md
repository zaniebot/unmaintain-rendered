```yaml
number: 3935
title: Re-add lowering unit tests
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/readd-lowering-unit-tests
created_at: 2024-05-31T12:05:58Z
updated_at: 2024-05-31T12:17:50Z
url: https://github.com/astral-sh/uv/pull/3935
synced_at: 2026-01-12T16:05:56Z
```

# Re-add lowering unit tests

---

_@konstin_

Re-add the lowering unit tests removed in #3904. This also adds a `stop_discovery_at` feature to avoid running actual workspace discovery.

---

_Label `internal` added by @konstin on 2024-05-31 12:05_

---

_Comment by @codspeed-hq[bot] on 2024-05-31 12:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/konsti/readd-lowering-unit-tests)

### Merging #3935 will **degrade performances by 5.63%**

<sub>Comparing <code>konsti/readd-lowering-unit-tests</code> (b0f6be6) with <code>main</code> (9bb0679)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/konsti/readd-lowering-unit-tests)._

### Benchmarks breakdown

|     | Benchmark | `main` | `konsti/readd-lowering-unit-tests` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `resolve_warm_jupyter` | 86.1 ms | 91.3 ms | -5.63% |


---

_Merged by @konstin on 2024-05-31 12:17_

---

_Closed by @konstin on 2024-05-31 12:17_

---

_Branch deleted on 2024-05-31 12:17_

---
