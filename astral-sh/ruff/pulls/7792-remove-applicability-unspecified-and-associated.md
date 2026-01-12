```yaml
number: 7792
title: "Remove `Applicability::Unspecified` and associated helpers"
type: pull_request
state: closed
author: zanieb
labels:
  - internal
assignees: []
base: zanie/applicability
head: zanie/app-unspecified
created_at: 2023-10-03T19:27:00Z
updated_at: 2023-10-05T18:09:38Z
url: https://github.com/astral-sh/ruff/pull/7792
synced_at: 2026-01-12T15:55:24Z
```

# Remove `Applicability::Unspecified` and associated helpers

---

_@zanieb_

Based on #7769

Removes the deprecated applicability level and helpers that were used to ease transitioning.

---

_Label `internal` added by @zanieb on 2023-10-03 19:27_

---

_Comment by @codspeed-hq[bot] on 2023-10-03 19:55_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/app-unspecified)

### Merging #7792 will **degrade performances by 2.43%**

<sub>Comparing <code>zanie/app-unspecified</code> (6eeadba) with <code>zanie/applicability</code> (ee903bf)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/zanie/app-unspecified)._

### Benchmarks breakdown

|     | Benchmark | `zanie/applicability` | `zanie/app-unspecified` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 38 ms | 38.9 ms | -2.43% |


---

_Comment by @zanieb on 2023-10-05 18:09_

Replaced by #7821 

---

_Closed by @zanieb on 2023-10-05 18:09_

---
