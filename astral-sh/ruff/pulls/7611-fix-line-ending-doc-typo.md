```yaml
number: 7611
title: Fix line ending doc typo
type: pull_request
state: merged
author: konstin
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: doc-type
created_at: 2023-09-22T20:07:49Z
updated_at: 2023-09-22T21:07:54Z
url: https://github.com/astral-sh/ruff/pull/7611
synced_at: 2026-01-12T02:39:10Z
```

# Fix line ending doc typo

---

_Pull request opened by @konstin on 2023-09-22 20:07_

Fixes https://docs.astral.sh/ruff/settings/#format-quote-style



---

_Label `documentation` added by @konstin on 2023-09-22 20:07_

---

_Label `internal` added by @konstin on 2023-09-22 20:07_

---

_@charliermarsh approved on 2023-09-22 20:08_

---

_Comment by @charliermarsh on 2023-09-22 20:08_

You might need to run `cargo dev generate-all`.

---

_Comment by @codspeed-hq[bot] on 2023-09-22 20:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/doc-type)

### Merging #7611 will **degrade performances by 3.15%**

<sub>Comparing <code>doc-type</code> (fc057fb) with <code>main</code> (5174e8c)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/doc-type)._

### Benchmarks breakdown

|     | Benchmark | `main` | `doc-type` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 91.1 ms | 94.1 ms | -3.15% |


---

_Merged by @charliermarsh on 2023-09-22 20:16_

---

_Closed by @charliermarsh on 2023-09-22 20:16_

---

_Branch deleted on 2023-09-22 20:16_

---

_Comment by @github-actions[bot] on 2023-09-22 20:22_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Comment by @MichaReiser on 2023-09-22 21:00_

Thank you

---

_Comment by @charliermarsh on 2023-09-22 21:07_

(I deployed this.)

---
