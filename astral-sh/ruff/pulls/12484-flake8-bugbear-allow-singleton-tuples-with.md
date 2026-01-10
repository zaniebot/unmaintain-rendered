```yaml
number: 12484
title: "[flake8-bugbear] Allow singleton tuples with starred expressions in B013"
type: pull_request
state: merged
author: dylwil3
labels:
  - bug
assignees: []
merged: true
base: main
head: bug/unpacking-tuple-exceptions
created_at: 2024-07-24T04:29:20Z
updated_at: 2024-07-24T14:15:05Z
url: https://github.com/astral-sh/ruff/pull/12484
synced_at: 2026-01-10T21:47:02Z
```

# [flake8-bugbear] Allow singleton tuples with starred expressions in B013

---

_Pull request opened by @dylwil3 on 2024-07-24 04:29_

## Summary

Closes #12450. Reverts previous attempt at handling starred expressions introduced in #7080 - replacing this approach by simply allowing such expressions.

## Test Plan

- [x] Added bug case to test fixture
- [x] Cargo test 


---

_Comment by @codspeed-hq[bot] on 2024-07-24 04:34_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3:bug/unpacking-tuple-exceptions)

### Merging #12484 will **improve performances by 4.26%**

<sub>Comparing <code>dylwil3:bug/unpacking-tuple-exceptions</code> (2d4a9ab) with <code>main</code> (8659f2f)</sub>



### Summary

`⚡ 1` improvements
`✅ 32` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `dylwil3:bug/unpacking-tuple-exceptions` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `lexer[numpy/globals.py]` | 30.4 µs | 29.2 µs | +4.26% |


---

_Comment by @MichaReiser on 2024-07-24 13:16_

Thanks for your contribution. 

@AlexWaygood what's your take on this rule? I'm somewhat concerned of adding the rule to Ruff's rule set because it is a restriction lint rule, it forbids the use of specific syntax, and not a lint rule that improves the quality of the code overall. 

---

_Comment by @MichaReiser on 2024-07-24 13:17_

Whoopps. I think I mixed up the PRs.

---

_Label `bug` added by @MichaReiser on 2024-07-24 13:18_

---

_@MichaReiser approved on 2024-07-24 13:19_

---

_Comment by @MichaReiser on 2024-07-24 13:19_

Thanks. This looks good to me.

---

_Merged by @MichaReiser on 2024-07-24 13:19_

---

_Closed by @MichaReiser on 2024-07-24 13:19_

---

_Branch deleted on 2024-07-24 14:15_

---
