```yaml
number: 11253
title: Add decorator types to function type
type: pull_request
state: merged
author: plredmond
labels: []
assignees: []
merged: true
base: main
head: redknot-decorators
created_at: 2024-05-02T23:08:53Z
updated_at: 2024-05-02T23:58:57Z
url: https://github.com/astral-sh/ruff/pull/11253
synced_at: 2026-01-10T22:37:02Z
```

# Add decorator types to function type

---

_Pull request opened by @plredmond on 2024-05-02 23:08_

* Add `decorators: Vec<Type>` to `FunctionType` struct
* Thread decorators through two `add_function` definitions
* Populate decorators at the callsite in `infer_symbol_type`
* Small test

---

_Review requested from @carljm by @plredmond on 2024-05-02 23:08_

---

_@plredmond reviewed on 2024-05-02 23:11_

---

_Review comment by @plredmond on `crates/red_knot/src/types/infer.rs`:88 on 2024-05-02 23:11_

I'm doing an `.expect(..)` which could panic. Pretty sure we shouldn't do that, but not sure what pattern to follow there. Drop failures silently? Log into some kind of warnings-structure?

---

_@plredmond reviewed on 2024-05-02 23:11_

---

_Review comment by @plredmond on `crates/red_knot/src/types/infer.rs`:87 on 2024-05-02 23:11_

I'm currently using the given `file_id` for the call to `infer_expr_type` but that might be wrong. I'm not sure if that's meant to be the `file_id` where the `expr` exists (if so, then this is correct?) or something else.

---

_@plredmond reviewed on 2024-05-02 23:12_

---

_Review comment by @plredmond on `crates/red_knot/src/types.rs`:519 on 2024-05-02 23:12_

Is use of a random placeholder type in this test ok?

---

_Comment by @github-actions[bot] on 2024-05-02 23:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:519 on 2024-05-02 23:23_

Yep for sure, in a test it's fine.

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:87 on 2024-05-02 23:24_

It's meant to be the file_id in which the expr exists (it's used for calling back into `infer_symbol_type` to resolve names in the expression), so this is correct!

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:88 on 2024-05-02 23:26_

This function (`infer_symbol_type`) also returns a `QueryResult<Type>`, so you can just use the `?` operator here to bubble up the error condition, if we get one, and unwrap the type.

I think currently the only error condition is `QueryCancelled`, meaning conditions (i.e. files) have changed and this query is already obsolete so we should stop working on it.

---

_@carljm approved on 2024-05-02 23:27_

Just make the expect -> ? change and then I think this is good to merge!

EDIT: also look into the pre-commit failure?

---

_Comment by @carljm on 2024-05-02 23:33_

> Should I add more cases to infer_expr_type which is currently a todo?

Not in this PR! We will want to flesh that out eventually, but it's separable from this PR.

---

_@plredmond reviewed on 2024-05-02 23:37_

---

_Review comment by @plredmond on `crates/red_knot/src/types/infer.rs`:87 on 2024-05-02 23:37_

Great. Thanks.

---

_@plredmond reviewed on 2024-05-02 23:38_

---

_Review comment by @plredmond on `crates/red_knot/src/types/infer.rs`:88 on 2024-05-02 23:38_

Whoops, I forgot rust has maybe/either monads. Thanks! Willfix :)

---

_Review requested from @carljm by @plredmond on 2024-05-02 23:42_

---

_@carljm approved on 2024-05-02 23:49_

Looks great, thank you!

---

_Marked ready for review by @plredmond on 2024-05-02 23:58_

---

_Merged by @plredmond on 2024-05-02 23:58_

---

_Closed by @plredmond on 2024-05-02 23:58_

---

_Branch deleted on 2024-05-02 23:58_

---
