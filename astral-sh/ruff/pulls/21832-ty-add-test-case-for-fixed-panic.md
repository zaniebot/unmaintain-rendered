```yaml
number: 21832
title: "[ty] Add test case for fixed panic"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/panic-test
created_at: 2025-12-07T15:53:19Z
updated_at: 2025-12-07T15:58:12Z
url: https://github.com/astral-sh/ruff/pull/21832
synced_at: 2026-01-12T15:57:34Z
```

# [ty] Add test case for fixed panic

---

_@AlexWaygood_

## Summary

We used to panic on this snippet, but we no longer do. The only thing left to do is to add a test case ensuring that we never do so again!

Closes https://github.com/astral-sh/ty/issues/899

## Test Plan

I ran `cargo run -p ty check --python-version=3.14` and `cargo run -p ty check --python-version=3.9` on this snippet. Neither panics anymore.


---

_Review requested from @carljm by @AlexWaygood on 2025-12-07 15:53_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-12-07 15:53_

---

_Label `testing` added by @AlexWaygood on 2025-12-07 15:53_

---

_Review requested from @dcreager by @AlexWaygood on 2025-12-07 15:53_

---

_Label `ty` added by @AlexWaygood on 2025-12-07 15:53_

---

_Comment by @astral-sh-bot[bot] on 2025-12-07 15:55_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-07 15:57_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_Merged by @AlexWaygood on 2025-12-07 15:58_

---

_Closed by @AlexWaygood on 2025-12-07 15:58_

---

_Branch deleted on 2025-12-07 15:58_

---
