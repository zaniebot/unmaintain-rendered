```yaml
number: 7822
title: Correct error in tuple example in ruff formatter docs
type: pull_request
state: merged
author: cosmojg
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-10-04T22:43:38Z
updated_at: 2023-10-04T22:57:03Z
url: https://github.com/astral-sh/ruff/pull/7822
synced_at: 2026-01-12T02:32:41Z
```

# Correct error in tuple example in ruff formatter docs

---

_Pull request opened by @cosmojg on 2023-10-04 22:43_

## Summary

The fourth element should be "d" instead of "c" in the tuple example in the ruff formatter docs.

## Test Plan

N/A

---

_@charliermarsh approved on 2023-10-04 22:44_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2023-10-04 22:44_

---

_Merged by @charliermarsh on 2023-10-04 22:51_

---

_Closed by @charliermarsh on 2023-10-04 22:51_

---

_Comment by @codspeed-hq[bot] on 2023-10-04 22:57_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cosmojg:patch-1)

### Merging #7822 will **improve performances by 2.41%**

<sub>Comparing <code>cosmojg:patch-1</code> (0aa1650) with <code>main</code> (90de108)</sub>



### Summary

`⚡ 1` improvements
`✅ 24` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `cosmojg:patch-1` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/default-rules[pydantic/types.py]` | 38.9 ms | 38 ms | +2.41% |


---
