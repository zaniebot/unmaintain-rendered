```yaml
number: 3867
title: "Remove unnecessary `::try_from`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/from
created_at: 2024-05-27T19:08:32Z
updated_at: 2024-05-27T19:20:29Z
url: https://github.com/astral-sh/uv/pull/3867
synced_at: 2026-01-12T16:05:54Z
```

# Remove unnecessary `::try_from`

---

_@charliermarsh_

## Summary

I didn't realize this even worked? We only define a `From` for this conversion.

---

_Label `internal` added by @charliermarsh on 2024-05-27 19:08_

---

_@charliermarsh reviewed on 2024-05-27 19:08_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/lib.rs`:73 on 2024-05-27 19:08_

Separate typo I noticed.

---

_Merged by @charliermarsh on 2024-05-27 19:18_

---

_Closed by @charliermarsh on 2024-05-27 19:18_

---

_Branch deleted on 2024-05-27 19:18_

---

_Comment by @codspeed-hq[bot] on 2024-05-27 19:20_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/from)

### Merging #3867 will **degrade performances by 6.1%**

<sub>Comparing <code>charlie/from</code> (558c374) with <code>main</code> (65b17f6)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/charlie/from)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/from` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 897.5 ns | 955.8 ns | -6.1% |


---
