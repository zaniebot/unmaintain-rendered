```yaml
number: 21404
title: "[ty] Baseline for subscript assignment diagnostics"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/subscript-assignment-tests
created_at: 2025-11-12T13:48:43Z
updated_at: 2025-11-12T14:32:40Z
url: https://github.com/astral-sh/ruff/pull/21404
synced_at: 2026-01-12T15:57:23Z
```

# [ty] Baseline for subscript assignment diagnostics

---

_@sharkdp_

## Summary

Add (snapshot) tests for subscript assignment diagnostics. This is mainly intended to establish a baseline before I hope to improve some of these messages.



---

_Review requested from @carljm by @sharkdp on 2025-11-12 13:48_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-12 13:48_

---

_Review requested from @dcreager by @sharkdp on 2025-11-12 13:48_

---

_Label `testing` added by @sharkdp on 2025-11-12 13:48_

---

_Label `ty` added by @sharkdp on 2025-11-12 13:48_

---

_@sharkdp reviewed on 2025-11-12 13:52_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:622 on 2025-11-12 13:52_

I will fix this in a follow-up PR.

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:615 on 2025-11-12 13:52_

Those are pretty horrible. I am going to improve them soon.

---

_@sharkdp reviewed on 2025-11-12 13:52_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 13:53_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-12 13:53_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results

No ecosystem changes detected ✅

No memory usage changes detected ✅



---

_@sharkdp reviewed on 2025-11-12 13:54_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Basic_(25b3e0346f673d80).snap`:23 on 2025-11-12 13:54_

I think we may want to add a special diagnostic for subscript assignments to `dict` that doesn't mention `__setitem__`, which feels like a distraction here.

---

_@sharkdp reviewed on 2025-11-12 13:57_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_No_`__setitem__`_met…_(468f62a3bdd1d60c).snap`:26 on 2025-11-12 13:57_

This should also be improved. It should say something like "Cannot assign **to subscript on** object of type …"

---

_@sharkdp reviewed on 2025-11-12 13:58_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Possibly_missing_`__…_(efd3f0c02e9b89e9).snap`:22 on 2025-11-12 13:58_

This will probably also improve with my next PR.

---

_@AlexWaygood reviewed on 2025-11-12 14:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Basic_(25b3e0346f673d80).snap`:23 on 2025-11-12 14:04_

IMO the top-level summary for any subscript assignment error shouldn't mention `__setitem__`. It feels like we're immediately jumping into the implementation details of how subscript assignments are desugared by the interpreter at runtime. Even for non-dicts, I'd prefer something like

```
error[invalid-assignment]: Cannot assign a value of type `Literal["three"]` to a subscript key of type `Literal["retries"]` on an object of type `dict[str, int]`
note: Assigning to a subscript key implicitly calls `__setitem__`
note: `__setitem__` defined here <show source-code annotation>
```

For something like this case, where the type of the key (`Literal["retries"]` is assignable to the expected key type (`str`), we could go with something even shorter:


```
error[invalid-assignment]: Cannot assign a value of type `Literal["three"]` to a subscript key on an object of type `dict[str, int]`
note: A subscript assignment implicitly calls `__setitem__`
note: `__setitem__` defined here <show source-code annotation>
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Invalid_key_type_(d3d47de65fb3bad).snap`:27 on 2025-11-12 14:19_

the highlighted range here looks off. Shouldn't this be

```suggestion
2 | config[0] = 3  # error: [invalid-assignment]
  | ^^^^^^^^^^^^^
```

?

But maybe that's one of the things you hope to address in a followup 😄

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Invalid_key_type_for…_(815dae276e2fd2b7).snap`:27 on 2025-11-12 14:21_

I'd prefer something like

```suggestion
error[invalid-key]: Cannot assign a key of type `Literal[0]` on an object of type `Config`.
note: Only string literals are allowed as keys on TypedDicts.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Invalid_value_type_f…_(155d53762388f9ad).snap`:46 on 2025-11-12 14:22_

love this one already, so good

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Misspelled_key_for_`…_(7cf0fa634e2a2d59).snap`:32 on 2025-11-12 14:26_

❤️

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_No_`__setitem__`_met…_(468f62a3bdd1d60c).snap`:26 on 2025-11-12 14:27_

I'd prefer

```suggestion
error[invalid-assignment]: Cannot assign to a subscript on an object of type `ReadOnlyDict`
note: assigning to a subscript impliictly calls `__setitem__`
note: `ReadOnlyDict` has no `__setitem__` method
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/assignment_diagnosti…_-_Subscript_assignment…_-_Possibly_missing_`__…_(efd3f0c02e9b89e9).snap`:22 on 2025-11-12 14:28_

I'd prefer

```suggestion
error[invalid-assignment]: Subscript assignment may fail on object of type `dict[str, int] | None`
note: Method `__setitem__` of type `dict[str, int] | None` may be missing
```

---

_Comment by @sharkdp on 2025-11-12 14:29_

Merging this, we can still discuss it on my next PR. As we all know, diffs on snapshots are more valuable than snapshots themselves.

---

_Merged by @sharkdp on 2025-11-12 14:29_

---

_Closed by @sharkdp on 2025-11-12 14:29_

---

_Branch deleted on 2025-11-12 14:29_

---

_@AlexWaygood approved on 2025-11-12 14:29_

These are great. I left some ideas on what I'd personally prefer the diagnostic to be in a few places -- obviously not expecting you to fix any of them in this PR 😄

---

_Comment by @sharkdp on 2025-11-12 14:32_

Thanks @AlexWaygood. I will not fix all of these in the next PR, but I will keep the comments here as a note for a time after I have fixed some higher-priority issues.

---
