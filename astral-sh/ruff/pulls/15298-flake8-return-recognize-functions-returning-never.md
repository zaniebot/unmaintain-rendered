```yaml
number: 15298
title: "[`flake8-return`] Recognize functions returning `Never` as non-returning (`RET503`)"
type: pull_request
state: merged
author: viccie30
labels:
  - rule
assignees: []
merged: true
base: main
head: ret503-never
created_at: 2025-01-06T09:45:27Z
updated_at: 2025-01-07T08:36:50Z
url: https://github.com/astral-sh/ruff/pull/15298
synced_at: 2026-01-12T15:55:50Z
```

# [`flake8-return`] Recognize functions returning `Never` as non-returning (`RET503`)

---

_@viccie30_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This pull request makes `flake8-return` recognize functions annotated to return [`typing.Never`](https://docs.python.org/3/library/typing.html#typing.Never) as non-returning.

## Test Plan

I have duplicated the existing tests for functions returning `NoReturn`.


---

_Comment by @github-actions[bot] on 2025-01-06 09:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser reviewed on 2025-01-06 11:40_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_return/snapshots/ruff_linter__rules__flake8_return__tests__RET503_RET503.py.snap`:475 on 2025-01-06 11:40_

Your code changes look correct, but they seem to have no effect. I'd expect that the rule wouldn't flag this specific line because there's no implicit return (it never returns). I added a ton of debug statements to `is_noreturn_func` and I suspect the problem is that the semantic model hasn't seen `bar` at the moment we try to resolve its name. My suspicious gets confirmed when changing the example to 

```py
import typing

def bar() -> typing.Never:
    abort()

def foo(x: int) -> int:
    if x == 5:
        return 5
    bar()
```

which raises a violation before your change but now doesn't. 

I suggest that you change your examples (including the existing `NoReturn`) to not use a nested function. We can add a dedicated test with a nested function with a doc comment explaining that this is currently not supported.

---

_Label `rule` added by @MichaReiser on 2025-01-06 11:40_

---

_@viccie30 reviewed on 2025-01-06 15:28_

---

_Review comment by @viccie30 on `crates/ruff_linter/src/rules/flake8_return/snapshots/ruff_linter__rules__flake8_return__tests__RET503_RET503.py.snap`:475 on 2025-01-06 15:28_

I have changed the current tests to no longer use nested functions and have added a currently ignored test checking nested functions.

---

_@MichaReiser approved on 2025-01-07 07:53_

---

_Merged by @MichaReiser on 2025-01-07 07:57_

---

_Closed by @MichaReiser on 2025-01-07 07:57_

---

_Branch deleted on 2025-01-07 08:36_

---
