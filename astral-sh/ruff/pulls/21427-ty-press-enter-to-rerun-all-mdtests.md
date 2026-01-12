```yaml
number: 21427
title: "[ty] Press 'enter' to rerun all mdtests"
type: pull_request
state: merged
author: sharkdp
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-runner-interactive
created_at: 2025-11-13T13:41:52Z
updated_at: 2025-11-13T16:48:36Z
url: https://github.com/astral-sh/ruff/pull/21427
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Press 'enter' to rerun all mdtests

---

_@sharkdp_

## Summary

Allow users of `mdtest.py` to press enter to rerun all mdtests without recompiling (thanks @AlexWaygood).

I swear I tried three other approaches (including a fully async version) before I settled on this solution. It is indeed silly, but works just fine.

## Test Plan

Interactive playing around

---

_Review requested from @carljm by @sharkdp on 2025-11-13 13:41_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-13 13:41_

---

_Review requested from @dcreager by @sharkdp on 2025-11-13 13:41_

---

_Label `internal` added by @sharkdp on 2025-11-13 13:41_

---

_Label `ty` added by @sharkdp on 2025-11-13 13:41_

---

_Comment by @astral-sh-bot[bot] on 2025-11-13 13:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-13 13:45_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@AlexWaygood approved on 2025-11-13 14:07_

Yes!!

---

_Merged by @sharkdp on 2025-11-13 14:34_

---

_Closed by @sharkdp on 2025-11-13 14:34_

---

_Branch deleted on 2025-11-13 14:34_

---

_Comment by @carljm on 2025-11-13 16:48_

Thank you!! This solves my biggest irritation with `mdtest.py`.

---
