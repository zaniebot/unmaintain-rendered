```yaml
number: 3996
title: Remove Python from available versions
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/avail
created_at: 2024-06-03T19:57:20Z
updated_at: 2024-06-03T20:11:46Z
url: https://github.com/astral-sh/uv/pull/3996
synced_at: 2026-01-10T13:54:02Z
```

# Remove Python from available versions

---

_Pull request opened by @charliermarsh on 2024-06-03 19:57_

## Summary

I believe this is no longer necessary. Part of the problem here is that we can't _know_ the full set of available Python versions, especially once we start resolving against a `Requires-Python` rather than a fixed set of two versions.


---

_Marked ready for review by @charliermarsh on 2024-06-03 19:57_

---

_Label `internal` added by @charliermarsh on 2024-06-03 19:57_

---

_Comment by @codspeed-hq[bot] on 2024-06-03 20:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/avail)

### Merging #3996 will **improve performances by 6.5%**

<sub>Comparing <code>charlie/avail</code> (1ad38fa) with <code>main</code> (10cd6b9)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/avail` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 955.8 ns | 897.5 ns | +6.5% |


---

_Merged by @charliermarsh on 2024-06-03 20:11_

---

_Closed by @charliermarsh on 2024-06-03 20:11_

---

_Branch deleted on 2024-06-03 20:11_

---
