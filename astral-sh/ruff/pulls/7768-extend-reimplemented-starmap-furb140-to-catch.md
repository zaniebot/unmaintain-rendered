```yaml
number: 7768
title: "Extend `reimplemented-starmap` (`FURB140`) to catch calls with a single and starred argument"
type: pull_request
state: merged
author: tjkuson
labels:
  - rule
assignees: []
merged: true
base: main
head: star-starmap
created_at: 2023-10-02T21:58:57Z
updated_at: 2023-10-03T08:44:47Z
url: https://github.com/astral-sh/ruff/pull/7768
synced_at: 2026-01-12T02:39:10Z
```

# Extend `reimplemented-starmap` (`FURB140`) to catch calls with a single and starred argument

---

_Pull request opened by @tjkuson on 2023-10-02 21:58_

## Summary

Closes #7636.

## Test Plan

`cargo test`


---

_Comment by @codspeed-hq[bot] on 2023-10-02 22:07_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/tjkuson:star-starmap)

### Merging #7768 will **degrade performances by 2.37%**

<sub>Comparing <code>tjkuson:star-starmap</code> (b890639) with <code>main</code> (f872c3b)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/tjkuson:star-starmap)._

### Benchmarks breakdown

|     | Benchmark | `main` | `tjkuson:star-starmap` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 38 ms | 38.9 ms | -2.37% |


---

_Comment by @github-actions[bot] on 2023-10-02 22:14_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Marked ready for review by @tjkuson on 2023-10-02 22:21_

---

_Label `rule` added by @charliermarsh on 2023-10-03 01:07_

---

_Merged by @charliermarsh on 2023-10-03 01:38_

---

_Closed by @charliermarsh on 2023-10-03 01:38_

---

_Branch deleted on 2023-10-03 08:44_

---
