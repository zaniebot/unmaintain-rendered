```yaml
number: 10437
title: "[`flake8-bugbear`] Allow tuples of exceptions (`B030`)"
type: pull_request
state: merged
author: ottaviohartman
labels:
  - bug
assignees: []
merged: true
base: main
head: thartman/B030-tuples
created_at: 2024-03-17T15:02:58Z
updated_at: 2024-03-18T00:37:27Z
url: https://github.com/astral-sh/ruff/pull/10437
synced_at: 2026-01-10T22:47:02Z
```

# [`flake8-bugbear`] Allow tuples of exceptions (`B030`)

---

_Pull request opened by @ottaviohartman on 2024-03-17 15:02_

Fixes #10426 

## Summary

Fix rule B030 giving a false positive with Tuple operations like `+`.

[Playground](https://play.ruff.rs/17b086bc-cc43-40a7-b5bf-76d7d5fce78a)
```python
try:
    ...
except (ValueError,TypeError) + (EOFError,ArithmeticError):
    ...
```

## Reviewer notes

This is a little more convoluted than I was expecting -- because we can have valid nested Tuples with operations done on them, the flattening logic has become a bit more complex.

Shall I guard this behind --preview?

## Test Plan

Unit tested.

---

_Comment by @github-actions[bot] on 2024-03-17 15:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @autinerd on 2024-03-17 15:25_

Oh, we seem to have worked on it at the same time, I just wanted to open a PR fixing this as well :sweat_smile: 

I checked out your PR and checked it against my tests, and it doesn't seem to be complete with the nesting.

```python
try:
    ...
except (ValueError, *(RuntimeError, TypeError), *((ArithmeticError,) + (EOFError,))):
                                                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ B030
    ...
```

This is flagged as error, although this is valid.


---

_Comment by @ottaviohartman on 2024-03-17 15:36_

Ok I can close this in favor of yours. 

---

_Comment by @ottaviohartman on 2024-03-17 16:45_

@autinerd actually let me know if you think I should fix that bug and continue, or just close this! Sorry about the confusion - I meant to say that I'd work on this issue.

---

_Comment by @autinerd on 2024-03-17 18:05_

I think that would be a good idea if you work on it, as my knowledge in the Ruff codebase (and Rust in general) is not really good. My solution was to handle it recursively, maybe it is possible to do it iteratively as well.

---

_@ottaviohartman reviewed on 2024-03-17 19:44_

---

_Review comment by @ottaviohartman on `crates/ruff_linter/resources/test/fixtures/flake8_bugbear/B030.py`:121 on 2024-03-17 19:44_

@autinerd added this test case

---

_@charliermarsh approved on 2024-03-18 00:24_

This looks good! Thanks.

---

_@charliermarsh reviewed on 2024-03-18 00:24_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_bugbear/rules/except_with_non_exception_classes.rs`:99 on 2024-03-18 00:24_

Gated these to `Operator::Add`.

---

_Label `bug` added by @charliermarsh on 2024-03-18 00:27_

---

_Renamed from "fix(B030): Allow adding tuples of exception classes" to "[`flake8-bugbear`] Allow tuples of exceptions (`B030`)" by @charliermarsh on 2024-03-18 00:28_

---

_Comment by @charliermarsh on 2024-03-18 00:28_

No need for preview here since it's a bug fix.

---

_Merged by @charliermarsh on 2024-03-18 00:31_

---

_Closed by @charliermarsh on 2024-03-18 00:31_

---
