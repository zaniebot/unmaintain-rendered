```yaml
number: 10856
title: Move remaining tests to new framework
type: pull_request
state: merged
author: dhruvmanila
labels:
  - parser
  - testing
assignees: []
merged: true
base: dhruv/parser
head: dhruv/move-tests
created_at: 2024-04-10T04:20:01Z
updated_at: 2024-04-10T04:39:24Z
url: https://github.com/astral-sh/ruff/pull/10856
synced_at: 2026-01-10T22:37:01Z
```

# Move remaining tests to new framework

---

_Pull request opened by @dhruvmanila on 2024-04-10 04:20_

## Summary

This PR moves the remaining tests to the resources directory. The reason this was remaining is because it's all mainly with item tests which I was working on at that moment. I've removed duplicate test cases and moved only the unique ones.

---

_Label `parser` added by @dhruvmanila on 2024-04-10 04:20_

---

_Label `testing` added by @dhruvmanila on 2024-04-10 04:20_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-10 04:20_

---

_Merged by @dhruvmanila on 2024-04-10 04:20_

---

_Closed by @dhruvmanila on 2024-04-10 04:20_

---

_Branch deleted on 2024-04-10 04:20_

---

_Comment by @codspeed-hq[bot] on 2024-04-10 04:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/move-tests)

### Merging #10856 will **degrade performances by 5.78%**

<sub>Comparing <code>dhruv/move-tests</code> (4cf084b) with <code>dhruv/parser</code> (38d649c)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/move-tests)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/move-tests` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.7 ms | 26.7 ms | +7.66% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.19% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.78% |


---

_Comment by @github-actions[bot] on 2024-04-10 04:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
