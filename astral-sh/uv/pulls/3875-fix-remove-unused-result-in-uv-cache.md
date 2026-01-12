```yaml
number: 3875
title: "fix: remove unused `Result` in uv-cache"
type: pull_request
state: merged
author: tdejager
labels: []
assignees: []
merged: true
base: main
head: fix/remove-unused-result
created_at: 2024-05-28T08:50:28Z
updated_at: 2024-05-28T09:19:09Z
url: https://github.com/astral-sh/uv/pull/3875
synced_at: 2026-01-12T16:05:54Z
```

# fix: remove unused `Result` in uv-cache

---

_@tdejager_


## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
It removes the unused result when creating the Cache with the `from_path` constructor. I don't believe it does any io operations any more at least.



---

_Comment by @codspeed-hq[bot] on 2024-05-28 09:04_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/tdejager:fix/remove-unused-result)

### Merging #3875 will **degrade performances by 6.1%**

<sub>Comparing <code>tdejager:fix/remove-unused-result</code> (c3f0c99) with <code>main</code> (a89e146)</sub>



### Summary

`❌ 1` regressions
`✅ 12` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/uv/branches/tdejager:fix/remove-unused-result)._

### Benchmarks breakdown

|     | Benchmark | `main` | `tdejager:fix/remove-unused-result` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `wheelname_tag_compatibility[flyte-short-incompatible]` | 897.5 ns | 955.8 ns | -6.1% |


---

_@konstin approved on 2024-05-28 09:13_

---

_Merged by @konstin on 2024-05-28 09:19_

---

_Closed by @konstin on 2024-05-28 09:19_

---
