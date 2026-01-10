```yaml
number: 13538
title: Red Knot - Infer the return value of bool() 
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/bool-function
created_at: 2024-09-27T14:36:33Z
updated_at: 2024-09-27T19:18:58Z
url: https://github.com/astral-sh/ruff/pull/13538
synced_at: 2026-01-10T20:59:36Z
```

# Red Knot - Infer the return value of bool() 

---

_Pull request opened by @TomerBin on 2024-09-27 14:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Following #13449, this PR adds custom handling for the bool contractor, so when the input type has statically known truthiness value, it will be used as the return value of the bool function.
For example, in the following snippet x will now be resolved to `Literal[True]` instead of `bool`.
```python
x = bool(1)
```
 
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Some cargo tests were added. 
<!-- How was it tested? -->


---

_Review requested from @carljm by @TomerBin on 2024-09-27 14:36_

---

_Review requested from @MichaReiser by @TomerBin on 2024-09-27 14:36_

---

_Review requested from @AlexWaygood by @TomerBin on 2024-09-27 14:36_

---

_Comment by @codspeed-hq[bot] on 2024-09-27 14:42_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/TomerBin:tomer/bool-function)

### Merging #13538 will **not alter performance**

<sub>Comparing <code>TomerBin:tomer/bool-function</code> (4aabb57) with <code>main</code> (1639488)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_Comment by @github-actions[bot] on 2024-09-27 15:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `red-knot` added by @MichaReiser on 2024-09-27 15:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2034 on 2024-09-27 17:04_

This should happen inside `Type::call` instead of here, and it should use `FunctionType::is_stdlib_symbol` instead of querying the builtin type and comparing for equality.

---

_@carljm requested changes on 2024-09-27 17:06_

Thank you!! This looks like the right behavior, but the implementation should go inside `Type::call`.

---

_@Slyces reviewed on 2024-09-27 18:52_

---

_Review comment by @Slyces on `crates/red_knot_python_semantic/src/types.rs`:598 on 2024-09-27 18:52_

This might not fit into the scope, but I've recently added some functions similar to `bool` in #13511, namely `repr` and `str`.

I think there's a pattern where we'll eventually want to extend this piece of code to deal with all currently implemented magic methods (`__bool__`, `__str__`, `__repr__`, ...) that have an associated builtin calling them.

Maybe we could introduce an enumeration for builtins with a method on `ClassType` that returns an `Option<Builtin>` to have something easily extendable like
```rust
impl<'db> ClassType<'db> {
    pub(crate) fn to_maybe_builtin(self, db: &'db dyn Db) -> Option<Builtin> {
        if self.is_stdlib_symbol(db, "builtins", "bool") {
            Some(Builtin::Bool)
        } else if self.is_stdlib_symbol(db, "builtins", "str") {
            Some(Builtin::Str)
        } else if self.is_stdlib_symbol(db, "builtins", "repr") {
            Some(Builtin::Str)
        } else {
            None
        }
    }
 }
```
And in the selected code something along the lines of
```rust
                let maybe_builtin = class.to_maybe_builtin(db);
                CallOutcome::callable(match maybe_builtin {
                    Some(Builtin::Bool) => arg_types
                        .first()
                        .unwrap_or(&Type::Unknown)
                        .bool(db)
                        .into_type(db),
                    Some(Builtin::Str) => arg_types.first().unwrap_or(&Type::Unknown).str(db),
                    Some(Builtin::Repr) => arg_types.first().unwrap_or(&Type::Unknown).repr(db),
                    None => Type::Instance(class),
                })
```
Doing this for `repr` and `str` is obviously outside of scope.

What do you think @carljm ?

---

_@carljm reviewed on 2024-09-27 18:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:598 on 2024-09-27 18:57_

Yes, I think we will want to do something like this, though I think we may also want to store the `Builtin` enum value in the `ClassType` (and also in `FunctionType` -- this could simplify our support for `reveal_type`) so that we can just call `is_stdlib_symbol` when the class or function is defined, rather than at every call-site.

But I think that can/should be done as a separate PR, rather than as part of this PR; it's easier to get these generalizations right once there are a few cases in place to generalize.

---

_@carljm approved on 2024-09-27 19:11_

---

_Merged by @carljm on 2024-09-27 19:11_

---

_Closed by @carljm on 2024-09-27 19:11_

---

_Branch deleted on 2024-09-27 19:18_

---
