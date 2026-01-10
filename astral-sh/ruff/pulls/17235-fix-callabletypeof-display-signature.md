```yaml
number: 17235
title: "Fix `CallableTypeOf` display signature"
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-callable-type-of-display
created_at: 2025-04-06T16:54:13Z
updated_at: 2025-04-06T17:13:39Z
url: https://github.com/astral-sh/ruff/pull/17235
synced_at: 2026-01-10T19:40:37Z
```

# Fix `CallableTypeOf` display signature

---

_Pull request opened by @MatthewMckee4 on 2025-04-06 16:54_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

If you reveal type of a variable annotated with `CallableTypeOf[<bound method>]` it includes the self parameter, it shouldnt do this

## Test Plan

update type_api.md


---

_Marked ready for review by @MatthewMckee4 on 2025-04-06 16:54_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-04-06 16:54_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-04-06 16:54_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-04-06 16:54_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-04-06 16:54_

---

_Comment by @github-actions[bot] on 2025-04-06 16:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@AlexWaygood reviewed on 2025-04-06 17:04_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:6932 on 2025-04-06 17:04_

this should be slightly more performant, and looks a little cleaner :-)

```suggestion
                    let revealed_signature = if argument_type.is_bound_method() {
                        signature.bind_self()
                    else {
                        signature.clone()
                    };

                    Type::Callable(CallableType::new(db, revealed_signature))
```

It requires adding a `Type::is_bound_method()` method, similar to this one:

https://github.com/astral-sh/ruff/blob/4571c5f56f660d04c89f2ac00bfb8c0159597561/crates/red_knot_python_semantic/src/types.rs#L578-L580

---

_Label `red-knot` added by @AlexWaygood on 2025-04-06 17:07_

---

_@AlexWaygood approved on 2025-04-06 17:12_

I'm not totally sure that lazily binding the signature is the best design -- it seems quite error-prone that you have to remember to do things like this at all callsites of `Type::signatures()`. A more robust design (but possibly less performant?) might be to eagerly bind `self` when inferring the signature of the method in `Type::signatures()`. But with the current design, I think this is the correct bugfix (and makes things much less confusing for developers in the short term!).

Curious if @dcreager has thoughts on the design, since he introduced `Type::signatures()` a few weeks ago in https://github.com/astral-sh/ruff/commit/23ccb52fa6f5b9e9bdfa5afe2adc0b65ff15b229!

---

_Merged by @AlexWaygood on 2025-04-06 17:12_

---

_Closed by @AlexWaygood on 2025-04-06 17:12_

---

_Branch deleted on 2025-04-06 17:13_

---
