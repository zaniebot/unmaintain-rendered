```yaml
number: 7626
title: "Treat form feed as whitespace in `SimpleTokenizer`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/ff
created_at: 2023-09-23T22:50:45Z
updated_at: 2023-09-25T14:43:20Z
url: https://github.com/astral-sh/ruff/pull/7626
synced_at: 2026-01-12T15:55:24Z
```

# Treat form feed as whitespace in `SimpleTokenizer`

---

_@charliermarsh_

## Summary

This is whitespace as per `is_python_whitespace`, and right now it tends to lead to panics in the formatter. Seems reasonable to treat it as whitespace in the `SimpleTokenizer` too.

Closes .https://github.com/astral-sh/ruff/issues/7624.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-23 22:50_

---

_Review requested from @konstin by @charliermarsh on 2023-09-23 22:50_

---

_Label `formatter` added by @charliermarsh on 2023-09-23 22:50_

---

_Comment by @codspeed-hq[bot] on 2023-09-23 22:59_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/ff)

### Merging #7626 will **degrade performances by 2.29%**

<sub>Comparing <code>charlie/ff</code> (33f0413) with <code>main</code> (17ceb5d)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/ff)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/ff` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 92 ms | 94.1 ms | -2.29% |


---

_Comment by @github-actions[bot] on 2023-09-23 23:04_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@MichaReiser approved on 2023-09-25 07:16_

---

_Review comment by @konstin on `crates/ruff_python_formatter/resources/test/fixtures/ruff/form_feed.py`:2 on 2023-09-25 07:22_

Could you trim that example down?

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:548 on 2023-09-25 07:22_

Please add a comment explaining that we ignore the true semantics of formfeed here ...

---

_Review comment by @konstin on `crates/ruff_python_trivia/src/tokenizer.rs`:823 on 2023-09-25 07:23_

... and here

---

_@konstin approved on 2023-09-25 07:23_

---

_Merged by @charliermarsh on 2023-09-25 14:35_

---

_Closed by @charliermarsh on 2023-09-25 14:35_

---

_Branch deleted on 2023-09-25 14:35_

---
