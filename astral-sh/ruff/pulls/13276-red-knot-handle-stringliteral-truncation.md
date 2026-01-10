```yaml
number: 13276
title: "[red-knot] Handle StringLiteral truncation"
type: pull_request
state: merged
author: dylwil3
labels:
  - ty
assignees: []
merged: true
base: main
head: string-literals
created_at: 2024-09-07T06:16:36Z
updated_at: 2024-09-13T14:07:48Z
url: https://github.com/astral-sh/ruff/pull/13276
synced_at: 2026-01-10T21:30:32Z
```

# [red-knot] Handle StringLiteral truncation

---

_Pull request opened by @dylwil3 on 2024-09-07 06:16_

When a type of the form `Literal["..."]` would be constructed with too large of a string, this PR converts it to `LiteralString` instead.

We also extend inference for binary operations to include the case where one of the operands is `LiteralString`.

Closes #13224


---

_Review requested from @carljm by @dylwil3 on 2024-09-07 06:16_

---

_Review requested from @MichaReiser by @dylwil3 on 2024-09-07 06:16_

---

_Review requested from @AlexWaygood by @dylwil3 on 2024-09-07 06:16_

---

_Comment by @github-actions[bot] on 2024-09-07 06:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2061 on 2024-09-07 07:06_

Given that match arms are checked in order, and we already handled `(StringLiteral, StringLiteral, Add)` above, can we simplify this pattern to `((StringLiteral | LiteralString), (StringLiteral | LiteralString), Add)`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2086 on 2024-09-07 07:08_

Can we add tests for these two new branches? (I realize it's a little irritating to create a `LiteralString` in a test, since at the moment it requires creating a very long string literal -- but it's not _too_ bad.)

---

_@carljm approved on 2024-09-07 07:09_

This is excellent, thank you!!

---

_@dylwil3 reviewed on 2024-09-07 14:28_

---

_Review comment by @dylwil3 on `crates/red_knot_python_semantic/src/types/infer.rs`:2061 on 2024-09-07 14:28_

Good idea! I guess if someone changed around the order one of the unit tests would fail, so this seems pretty safe.

Done!

---

_@dylwil3 reviewed on 2024-09-07 14:28_

---

_Review comment by @dylwil3 on `crates/red_knot_python_semantic/src/types/infer.rs`:2086 on 2024-09-07 14:28_

Done!

---

_Comment by @codspeed-hq[bot] on 2024-09-07 14:33_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3:string-literals)

### Merging #13276 will **improve performances by 8.54%**

<sub>Comparing <code>dylwil3:string-literals</code> (c3c9ac7) with <code>main</code> (a7c9368)</sub>



### Summary

`⚡ 1` improvements
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dylwil3:string-literals` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 883.7 µs | 814.1 µs | +8.54% |


---

_@carljm reviewed on 2024-09-07 15:40_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2086 on 2024-09-07 15:40_

Thank you! I'm seeing a new test for the new match arm around adding Literal strings, but not for these two cases of multiplying by a literal int?

---

_@dylwil3 reviewed on 2024-09-07 16:09_

---

_Review comment by @dylwil3 on `crates/red_knot_python_semantic/src/types/infer.rs`:2086 on 2024-09-07 16:09_

Whoops, good catch! Added now

---

_@carljm approved on 2024-09-08 03:25_

---

_Merged by @carljm on 2024-09-08 03:25_

---

_Closed by @carljm on 2024-09-08 03:25_

---

_Branch deleted on 2024-09-08 04:27_

---

_Label `red-knot` added by @dhruvmanila on 2024-09-13 14:07_

---
