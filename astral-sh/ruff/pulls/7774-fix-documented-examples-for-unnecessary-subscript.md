```yaml
number: 7774
title: "Fix documented examples for `unnecessary-subscript-reversal`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/C415
created_at: 2023-10-03T04:10:12Z
updated_at: 2023-10-03T04:25:08Z
url: https://github.com/astral-sh/ruff/pull/7774
synced_at: 2026-01-12T15:55:24Z
```

# Fix documented examples for `unnecessary-subscript-reversal`

---

_@charliermarsh_

## Summary

Two of the three listed examples were wrong: one was semantically incorrect, another was _correct_ but not actually within the scope of the rule.

Good motivation for us to start linting documentation examples :)

Closes https://github.com/astral-sh/ruff/issues/7773.


---

_Label `documentation` added by @charliermarsh on 2023-10-03 04:10_

---

_@charliermarsh reviewed on 2023-10-03 04:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_comprehensions/rules/unnecessary_subscript_reversal.rs`:43 on 2023-10-03 04:10_

(Unrelated refactor to take `ast::ExprCall` instead of destructured fields.)

---

_Merged by @charliermarsh on 2023-10-03 04:18_

---

_Closed by @charliermarsh on 2023-10-03 04:18_

---

_Branch deleted on 2023-10-03 04:18_

---

_Comment by @codspeed-hq[bot] on 2023-10-03 04:23_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/C415)

### Merging #7774 will **degrade performances by 2.35%**

<sub>Comparing <code>charlie/C415</code> (26ee2a2) with <code>main</code> (e129f77)</sub>



### Summary

`❌ 1` regressions
`✅ 24` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/charlie/C415)._

### Benchmarks breakdown

|     | Benchmark | `main` | `charlie/C415` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[pydantic/types.py]` | 38 ms | 38.9 ms | -2.35% |


---

_Comment by @github-actions[bot] on 2023-10-03 04:25_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---
