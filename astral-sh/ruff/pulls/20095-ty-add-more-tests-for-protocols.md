```yaml
number: 20095
title: "[ty] Add more tests for protocols"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/proto-tests
created_at: 2025-08-26T10:07:56Z
updated_at: 2025-08-27T11:56:15Z
url: https://github.com/astral-sh/ruff/pull/20095
synced_at: 2026-01-12T15:56:54Z
```

# [ty] Add more tests for protocols

---

_@AlexWaygood_

## Summary

This PR adds more tests for protocols. It's split out from other PRs that provide functional changes to make those PRs easier to review.

Included here are:
- Bugfixes to tests that aren't actually testing the thing they're meant to be testing
- Regression tests for things that caused stack overflows in early versions of #19029 and #19866
- New equivalence tests for protocols with property members

## Test Plan

this pr is all tests

---

Co-authored-by: Shunsuke Shibayama <sbym1346@gmail.com>

---

_Label `testing` added by @AlexWaygood on 2025-08-26 10:07_

---

_Review requested from @carljm by @AlexWaygood on 2025-08-26 10:07_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-08-26 10:07_

---

_Label `ty` added by @AlexWaygood on 2025-08-26 10:07_

---

_Review requested from @dcreager by @AlexWaygood on 2025-08-26 10:07_

---

_@AlexWaygood reviewed on 2025-08-26 10:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/protocols.md`:1461 on 2025-08-26 10:09_

this was a bug in the test: we check assignability of `XSub` to `HasXProperty` a few lines above. Here we were meant to be asserting assignability of `XSub` vs `XReadWriteProperty`, but there was a copy/paste error in the test

---

_@carljm approved on 2025-08-26 23:36_

---

_Merged by @AlexWaygood on 2025-08-27 11:56_

---

_Closed by @AlexWaygood on 2025-08-27 11:56_

---

_Branch deleted on 2025-08-27 11:56_

---
