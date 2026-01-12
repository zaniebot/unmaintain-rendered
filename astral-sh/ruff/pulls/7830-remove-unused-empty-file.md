```yaml
number: 7830
title: Remove unused empty file
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zanie/unused-defaults
created_at: 2023-10-05T18:14:53Z
updated_at: 2023-10-05T18:38:01Z
url: https://github.com/astral-sh/ruff/pull/7830
synced_at: 2026-01-12T15:55:24Z
```

# Remove unused empty file

---

_@zanieb_

_No description provided._

---

_Label `internal` added by @zanieb on 2023-10-05 18:14_

---

_Comment by @codspeed-hq[bot] on 2023-10-05 18:31_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/unused-defaults)

### Merging #7830 will **degrade performances by 2.43%**

<sub>Comparing <code>zanie/unused-defaults</code> (1c3e87f) with <code>main</code> (709abd5)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/unused-defaults` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/default-rules[pydantic/types.py]` | 38 ms | 38.9 ms | -2.43% |


---

_@charliermarsh approved on 2023-10-05 18:33_

---

_Comment by @zanieb on 2023-10-05 18:35_

Dude how many times will I have to ack this "regression"

---

_Merged by @zanieb on 2023-10-05 18:35_

---

_Closed by @zanieb on 2023-10-05 18:35_

---

_Branch deleted on 2023-10-05 18:35_

---

_Comment by @charliermarsh on 2023-10-05 18:38_

They're non-blocking now, I just ignore them and merge when it's a single-file failure.

---
