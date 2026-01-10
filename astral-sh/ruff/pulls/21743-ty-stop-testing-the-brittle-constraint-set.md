```yaml
number: 21743
title: "[ty] Stop testing the (brittle) constraint set display implementation"
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/fire-display
created_at: 2025-12-01T22:50:59Z
updated_at: 2025-12-02T13:42:34Z
url: https://github.com/astral-sh/ruff/pull/21743
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Stop testing the (brittle) constraint set display implementation

---

_Pull request opened by @dcreager on 2025-12-01 22:50_

The `Display` implementation for constraint sets is brittle, and deserves a rethink. But later! It's perfectly fine for printf debugging; we just shouldn't be writing mdtests that depend on any particular rendering details. Most of these tests can be replaced with an equivalence check that actually validates that the _behavior_ of two constraint sets are identical.

---

_Review requested from @carljm by @dcreager on 2025-12-01 22:51_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-01 22:51_

---

_Review requested from @sharkdp by @dcreager on 2025-12-01 22:51_

---

_Label `internal` added by @dcreager on 2025-12-01 22:51_

---

_Label `ty` added by @dcreager on 2025-12-01 22:51_

---

_@dcreager reviewed on 2025-12-01 22:52_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:10583 on 2025-12-01 22:52_

This is an important bit that lets us replace all of the `reveal_type` tests with `static_assert`s.  The `ConstraintSet.__eq__` implementation now checks whether two constraint sets are _equivalent_, not whether they are _identical_.

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 22:52_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 22:54_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 498 diagnostics
+ Found 496 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@sharkdp approved on 2025-12-02 08:17_

Thank you!

I guess the major drawback of this is that `static_assert` will only give you a success/failure feedback, whereas a failing `reveal_type` tests gives you both display representations (one in the error message, one in the test assertion).

We could follow the route of many other test frameworks and introduce something like a `static_assert_eq(left, right)` function that would show the display representation of the types of both arguments in case of a failure. That might be even nicer than `reveal_type` tests because we could properly align the left and right hand side types in the terminal.

~~I'll merge this PR to unblock the typeshed update.~~ (no, that was the other PR)

---

_Merged by @sharkdp on 2025-12-02 08:17_

---

_Closed by @sharkdp on 2025-12-02 08:17_

---

_Branch deleted on 2025-12-02 08:17_

---

_Comment by @dcreager on 2025-12-02 13:42_

> ~I'll merge this PR to unblock the typeshed update.~ (no, that was the other PR)

The other PR was blocked on this one, though, so thank you regardless!

> We could follow the route of many other test frameworks and introduce something like a `static_assert_eq(left, right)` function that would show the display representation of the types of both arguments in case of a failure. That might be even nicer than `reveal_type` tests because we could properly align the left and right hand side types in the terminal.

That's a good idea, I opened https://github.com/astral-sh/ty/issues/1719 to track it.

---
