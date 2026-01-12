```yaml
number: 12934
title: Ignore blank line rules for docs formatting
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
assignees: []
merged: true
base: main
head: dhruv/docs
created_at: 2024-08-16T15:23:02Z
updated_at: 2024-08-16T15:28:33Z
url: https://github.com/astral-sh/ruff/pull/12934
synced_at: 2026-01-12T15:55:42Z
```

# Ignore blank line rules for docs formatting

---

_@dhruvmanila_

## Summary

fixes: #12933 

## Test Plan

`python scripts/check_docs_formatted.py --generate-docs`


---

_Label `documentation` added by @dhruvmanila on 2024-08-16 15:23_

---

_@MichaReiser approved on 2024-08-16 15:25_

---

_Merged by @dhruvmanila on 2024-08-16 15:27_

---

_Closed by @dhruvmanila on 2024-08-16 15:27_

---

_Branch deleted on 2024-08-16 15:27_

---

_Comment by @codspeed-hq[bot] on 2024-08-16 15:28_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/docs)

### Merging #12934 will **degrade performances by 5.45%**

<sub>Comparing <code>dhruv/docs</code> (223262f) with <code>main</code> (ef1f6d9)</sub>



### Summary

`❌ 1` regressions
`✅ 31` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/docs)._

### Benchmarks breakdown

|     | Benchmark | `main` | `dhruv/docs` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/all-rules[numpy/globals.py]` | 727.8 µs | 769.8 µs | -5.45% |


---
