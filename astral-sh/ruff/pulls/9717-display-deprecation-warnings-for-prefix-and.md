```yaml
number: 9717
title: Display deprecation warnings for prefix and linter selections with all rules deprecated
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
draft: true
base: zb/pygrep
head: zb/deprecate-group
created_at: 2024-01-30T19:52:46Z
updated_at: 2024-01-31T16:59:45Z
url: https://github.com/astral-sh/ruff/pull/9717
synced_at: 2026-01-10T22:57:09Z
```

# Display deprecation warnings for prefix and linter selections with all rules deprecated

---

_Pull request opened by @zanieb on 2024-01-30 19:52_

If _all_ of the rules in a prefix or lint rule selector are deprecated then we warn. This means that if you select `PGH` we will throw a warning instead of silently deselecting the rules.

---

_Converted to draft by @zanieb on 2024-01-30 19:57_

---

_Comment by @codspeed-hq[bot] on 2024-01-30 20:04_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zb/deprecate-group)

### Merging #9717 will **improve performances by 11.13%**

<sub>Comparing <code>zb/deprecate-group</code> (fedca81) with <code>zb/pygrep</code> (7e763cf)</sub>



### Summary

`⚡ 4` improvements
`✅ 26` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `zb/pygrep` | `zb/deprecate-group` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-rules[large/dataset.py]` | 110.8 ms | 99.7 ms | +11.13% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 50.8 ms | 47.2 ms | +7.51% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 12.8 ms | 12.3 ms | +4.34% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 24.3 ms | 23.1 ms | +5.27% |


---

_Closed by @zanieb on 2024-01-31 16:59_

---
