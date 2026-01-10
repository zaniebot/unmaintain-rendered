```yaml
number: 3969
title: "Use universal resolution in `uv lock`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/glob
created_at: 2024-06-03T00:07:42Z
updated_at: 2024-06-03T12:16:59Z
url: https://github.com/astral-sh/uv/pull/3969
synced_at: 2026-01-10T13:59:34Z
```

# Use universal resolution in `uv lock`

---

_Pull request opened by @charliermarsh on 2024-06-03 00:07_

## Summary

Wires up the optional markers in resolution, and adds respecting-the-markers to `Lock:: to_resolution`.

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-03 00:07_

---

_Label `preview` added by @charliermarsh on 2024-06-03 00:07_

---

_Marked ready for review by @charliermarsh on 2024-06-03 00:07_

---

_Comment by @codspeed-hq[bot] on 2024-06-03 00:13_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/glob)

### Merging #3969 will **improve performances by 6.72%**

<sub>Comparing <code>charlie/glob</code> (61c9d70) with <code>main</code> (c500b78)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/glob` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 926.7 ns | 868.3 ns | +6.72% |


---

_Merged by @charliermarsh on 2024-06-03 01:33_

---

_Closed by @charliermarsh on 2024-06-03 01:33_

---

_Branch deleted on 2024-06-03 01:33_

---

_@BurntSushi reviewed on 2024-06-03 12:16_

Nice, this all makes sense to me!

---
