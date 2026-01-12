```yaml
number: 7208
title: Enable preview mode during benchmarks
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zanie/preview-benchmark
created_at: 2023-09-06T19:07:44Z
updated_at: 2023-09-11T19:09:34Z
url: https://github.com/astral-sh/ruff/pull/7208
synced_at: 2026-01-12T02:45:38Z
```

# Enable preview mode during benchmarks

---

_Pull request opened by @zanieb on 2023-09-06 19:07_

Split out of https://github.com/astral-sh/ruff/pull/7195 so benchmark changes from enabling additional rules can be reviewed separately.

---

_Converted to draft by @zanieb on 2023-09-06 19:07_

---

_Comment by @codspeed-hq[bot] on 2023-09-06 19:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/preview-benchmark)

### Merging #7208 will **degrade performances by 5.6%**

<sub>Comparing <code>zanie/preview-benchmark</code> (32e5cab) with <code>main</code> (6566d00)</sub>



### Summary

`❌ 5 (👁 5)` regressions
`✅ 20` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `zanie/preview-benchmark` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `linter/all-rules[numpy/globals.py]` | 4 ms | 4.1 ms | -2.58% |
| 👁 | `linter/all-rules[numpy/ctypeslib.py]` | 33.2 ms | 34.5 ms | -3.93% |
| 👁 | `linter/all-rules[unicode/pypinyin.py]` | 15.4 ms | 15.7 ms | -2.29% |
| 👁 | `linter/all-rules[pydantic/types.py]` | 69.1 ms | 72.4 ms | -4.59% |
| 👁 | `linter/all-rules[large/dataset.py]` | 154.5 ms | 163.7 ms | -5.6% |


---

_Comment by @github-actions[bot] on 2023-09-06 22:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @zanieb on 2023-09-06 23:42_

Another option is to add new benchmarks for preview mode. Probably a good way to separate performance changes in preview and stable?

---

_Comment by @MichaReiser on 2023-09-09 08:34_

> Another option is to add new benchmarks for preview mode. Probably a good way to separate performance changes in preview and stable?

Sounds good to me. Separating preview and stable benchmarks can help narrow down a regression or catch unintentional performance regressions when supposedly only changing preview code. It could then be taken off lightly as: this is preview only when in fact, we just regressed stable. 



---

_Marked ready for review by @zanieb on 2023-09-11 17:30_

---

_Label `preview` added by @zanieb on 2023-09-11 18:38_

---

_Comment by @zanieb on 2023-09-11 18:51_

I think I'd like to include this as a general improvement and we can look for a contributor for splitting them into separate benchmarks?

---

_Merged by @zanieb on 2023-09-11 19:09_

---

_Closed by @zanieb on 2023-09-11 19:09_

---

_Branch deleted on 2023-09-11 19:09_

---
