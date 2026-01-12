```yaml
number: 16873
title: "[red-knot] Add failing tests for `*` imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/star-imports-tests
created_at: 2025-03-20T16:24:01Z
updated_at: 2025-03-21T14:18:49Z
url: https://github.com/astral-sh/ruff/pull/16873
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Add failing tests for `*` imports

---

_@AlexWaygood_

## Summary

This PR adds a suite of tests for wildcard (`*`) imports. The tests nearly all fail for now, and those that don't, ahem, pass for the wrong reasons...

I've tried to add TODO comments in all instances for places where we are currently inferring the incorrect thing, incorrectly emitting a diagnostic, or emitting a diagnostic with a bad error message.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-03-20 16:24_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-20 16:24_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-20 16:24_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-20 16:24_

---

_@AlexWaygood reviewed on 2025-03-20 16:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:89 on 2025-03-20 16:25_

the reason why we do _not_ emit an error here is somewhat horrifying... we're currently inserting a global-scope symbol with the name `"*"` in the symbol table for `b.py`

---

_Comment by @github-actions[bot] on 2025-03-20 16:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:317 on 2025-03-21 00:30_

I think this could be done separately from the initial PR, but I'm guessing that's already your plan; having it in the initial set of tests doesn't preclude that, this test can just keep its TODOs longer.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:485 on 2025-03-21 00:33_

I would go for the first. If code is definitely wrong, even if it won't actually error at runtime until "used", I think it's generally better if we issue the error closer to the source, and not further from the source (which might be code with a different author who can't do anything about the problem.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:366 on 2025-03-21 00:36_

I think in type inference we can be pretty smart here (i.e. we could support dynamic members if they evaluate to a `Literal[str]` type). The question is how we handle it in semantic indexing. If we want to allow dynamic elements of `__all__`, I think such definitions of `__all__` would need to be treated as effectively the same as no `__all__` at all (for semantic indexing purposes), meaning any name that may be defined at global scope in the module is considered to be possibly wildcard imported.

---

_@carljm approved on 2025-03-21 00:37_

Looks great!

---

_@AlexWaygood reviewed on 2025-03-21 13:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:317 on 2025-03-21 13:29_

yup, exactly

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:485 on 2025-03-21 13:32_

I'm sort of inclined to go for the third: emit diagnostics at both locations?

- Having an invalid `__all__` and having an import statement that we can say for sure will fail both seem like distinct errors that I'd want to know about if writing a Python module (even if they both have the same root cause)
- If the module you're importing from is third-party and has an invalid `__all__` but we only emit an error where `__all__` is defined, the user won't get any warning that their `*` import from the third-party module will fail, since we don't display diagnostics relating to type errors in third-party code. This isn't that far-fetched: we've had several situations at typeshed where the library we're stubbing has an invalid `__all__` and this was never detected by the library's tests (though it was caught by typeshed's tests!)

---

_@AlexWaygood reviewed on 2025-03-21 13:32_

---

_@AlexWaygood reviewed on 2025-03-21 13:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:366 on 2025-03-21 13:32_

that all makes sense to me

---

_Comment by @AlexWaygood on 2025-03-21 14:16_

Added some more tests in https://github.com/astral-sh/ruff/pull/16873/commits/7cf8eabccc38410cd4d2a71b08e7c4555e80fc90 ... I don't _think_ they're contentious, but happy to address any comments in a followup! Merging for now.

---

_Merged by @AlexWaygood on 2025-03-21 14:17_

---

_Closed by @AlexWaygood on 2025-03-21 14:17_

---

_Branch deleted on 2025-03-21 14:17_

---
