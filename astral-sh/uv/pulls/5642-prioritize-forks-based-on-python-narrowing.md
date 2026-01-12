```yaml
number: 5642
title: Prioritize forks based on Python narrowing
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/prioritiz
created_at: 2024-07-30T23:24:53Z
updated_at: 2024-07-31T14:29:17Z
url: https://github.com/astral-sh/uv/pull/5642
synced_at: 2026-01-12T16:06:56Z
```

# Prioritize forks based on Python narrowing

---

_@charliermarsh_

## Summary

First part of: https://github.com/astral-sh/uv/issues/4926. We should solve forks that _don't_ expand the world of supported versions (e.g., `python_version >= '3.11'` enables us to select new packages, since we narrow the supported version range).


---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-30 23:25_

---

_Review requested from @konstin by @charliermarsh on 2024-07-30 23:25_

---

_Label `enhancement` added by @charliermarsh on 2024-07-30 23:25_

---

_Label `preview` added by @charliermarsh on 2024-07-30 23:25_

---

_Marked ready for review by @charliermarsh on 2024-07-30 23:25_

---

_Comment by @codspeed-hq[bot] on 2024-07-30 23:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/prioritiz)

### Merging #5642 will **improve performances by 10.8%**

<sub>Comparing <code>charlie/prioritiz</code> (e268bed) with <code>main</code> (cbb13e6)</sub>



### Summary

`⚡ 1` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/prioritiz` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `resolve_warm_jupyter` | 100.1 ms | 90.3 ms | +10.8% |


---

_@konstin approved on 2024-07-31 08:14_

```
- Resolved 41 packages in [TIME]
+ Resolved 34 packages in [TIME]
```

nice

---

_@BurntSushi approved on 2024-07-31 14:08_

---

_Merged by @charliermarsh on 2024-07-31 14:29_

---

_Closed by @charliermarsh on 2024-07-31 14:29_

---

_Branch deleted on 2024-07-31 14:29_

---
