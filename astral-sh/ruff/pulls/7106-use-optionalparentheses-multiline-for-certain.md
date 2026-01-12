```yaml
number: 7106
title: "Use `OptionalParentheses::Multiline` for certain constants"
type: pull_request
state: closed
author: cnpryer
labels: []
assignees: []
draft: true
base: main
head: stmt-assign
created_at: 2023-09-03T19:34:19Z
updated_at: 2023-09-04T01:45:20Z
url: https://github.com/astral-sh/ruff/pull/7106
synced_at: 2026-01-12T02:45:38Z
```

# Use `OptionalParentheses::Multiline` for certain constants

---

_Pull request opened by @cnpryer on 2023-09-03 19:34_

Closes #7067

## Summary

‼️ This solution is incomplete (see [comment](https://github.com/astral-sh/ruff/pull/7106#discussion_r1314359674)).

This PR updates the `NeedsParentheses` implementation for `ExprConstant` so that booleans, `None`, and ellipses use multiline to resolve the following difference with `black`:

```py
# Input
def f():
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa = (
        True
    )

# Black (23.7.0)
def f():
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa = (
        True
    )

# Ruff (0.0.287)
def f():
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa = True
```

## Test Plan

Current and new fixtures.



---

_Comment by @codspeed-hq[bot] on 2023-09-03 19:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cnpryer:stmt-assign)

### Merging #7106 will **degrade performances by 3.61%**

<sub>Comparing <code>cnpryer:stmt-assign</code> (ca90098) with <code>main</code> (834566f)</sub>



### Summary

`❌ 1` regressions
`✅ 19` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/cnpryer:stmt-assign)._

### Benchmarks breakdown

|     | Benchmark | `main` | `cnpryer:stmt-assign` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `formatter[numpy/globals.py]` | 1.3 ms | 1.3 ms | -3.61% |


---

_Comment by @cnpryer on 2023-09-03 19:44_

3.61%. Damn...

---

_Converted to draft by @cnpryer on 2023-09-03 19:45_

---

_@cnpryer reviewed on 2023-09-04 01:35_

---

_Review comment by @cnpryer on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/assign.py`:72 on 2023-09-04 01:35_

This is a counter example to the current `Multiline` solution.

---

_Comment by @cnpryer on 2023-09-04 01:38_

I'll reopen when I can look at this again.

---

_Closed by @cnpryer on 2023-09-04 01:38_

---

_Branch deleted on 2023-09-04 01:38_

---
