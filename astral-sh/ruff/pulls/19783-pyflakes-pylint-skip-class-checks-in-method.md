```yaml
number: 19783
title: "[`pyflakes`/`pylint`] Skip `__class__` checks in method definitions (…"
type: pull_request
state: closed
author: mikeleppane
labels:
  - bug
assignees: []
base: main
head: fix(#18442)/class-binding-is-not-defined
created_at: 2025-08-06T12:53:06Z
updated_at: 2025-08-26T13:50:22Z
url: https://github.com/astral-sh/ruff/pull/19783
synced_at: 2026-01-12T15:56:47Z
```

# [`pyflakes`/`pylint`] Skip `__class__` checks in method definitions (…

---

_@mikeleppane_

## Summary

### Problem
Rules [F841](https://docs.astral.sh/ruff/rules/unused-variable/) (`unused-variable`) and [PLE0117](https://docs.astral.sh/ruff/rules/nonlocal-without-binding/) (`nonlocal-without-binding`) incorrectly flagged usage within method definitions, causing false positives.
Root Cause: Both rules failed to recognize that it is a special variable automatically available in Python method definitions. In methods, it is implicitly bound by the Python runtime, making assignments to it legitimate and declarations valid. `__class__``__class__``nonlocal __class__`

### Solution
- Added special handling for when detected within method definitions: `__class__`
- **F841**: Skip unused variable warnings for assignments in methods `__class__`
- **PLE0117**: Allow statements in methods without requiring explicit outer bindings `nonlocal __class__`

```python
class C:
    def set_class(self, cls):
        __class__ = cls  # No longer warns about unused variable
        
    def get_class(self):
        nonlocal __class__  # No longer warns about missing binding
        return __class__
```


## Test Plan

```bash
cargo test
```


---

_Comment by @github-actions[bot] on 2025-08-06 13:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-08-06 15:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pyflakes/rules/unused_variable.rs`:289 on 2025-08-06 15:25_

We might be able to use something like this here for traversing the parent scopes:

https://github.com/astral-sh/ruff/blob/4d57fcd5a488e628aa26aaacd0a61728609ff310/crates/ruff_linter/src/checkers/ast/mod.rs#L702-L708

---

_@ntBre reviewed on 2025-08-06 15:41_

Thanks for working on this!

This looks like it fixes the issue for these two rules. Did you look into my suggestion about synthesizing a `__class__` binding for class scopes (https://github.com/astral-sh/ruff/issues/18442#issuecomment-2935976782)? I'm still not sure if that's a good idea or not, but it could be nice to resolve this more generally instead of adding special cases to individual rules as needed, and that seemed like one approach.

In grepping around, it looks like we might already have handling for this, if we can call this enclosing `resolve_load` function:

https://github.com/astral-sh/ruff/blob/4d57fcd5a488e628aa26aaacd0a61728609ff310/crates/ruff_python_semantic/src/model.rs#L412-L422

If neither of these solutions looks fruitful, I'm happy with what you have here too :)

---

_Comment by @mikeleppane on 2025-08-07 13:57_

@ntBre thanks for the good comments! What would you think about this approach, in which we add a method to the semantic model that would expose implicit global functionality? Something like this:

```rust
impl<'a> SemanticModel<'a> {
    /// Check if a name would resolve as an implicit global from a given scope
    /// without actually creating references or modifying state.
    pub fn would_resolve_as_implicit_global(&self, name: &str, scope_id: ScopeId) -> bool {
        let scope = &self.scopes[scope_id];
        
        match name {
            "__class__" => {
                if !scope.kind.is_function() {
                    return false;
                }
                
                let mut seen_function = false;
                for ancestor_scope_id in self.ancestor_scopes(scope_id) {
                    let ancestor_scope = &self.scopes[ancestor_scope_id];
                    if ancestor_scope.kind.is_class() && seen_function {
                        return true;
                    }
                    seen_function |= ancestor_scope.kind.is_function();
                }
                false
            }
            "__module__" | "__qualname__" => {
                scope.kind.is_class()
            }
            _ => false,
        }
    }
}

// RULE

pub(crate) fn unused_variable(checker: &Checker, name: &str, binding: &Binding) {
    if binding.is_unpacked_assignment() {
        return;
    }

    if checker
        .semantic()
        .would_resolve_as_implicit_global(name, binding.scope)
    {
        return;
    }

    /*// Skip __class__ in method definitions - it's automatically bound by Python
    if name == "__class__" && is_binding_in_method_definition(checker, binding) {
        return;
    }*/

    let mut diagnostic = checker.report_diagnostic(
        UnusedVariable {
            name: name.to_string(),
        },
        binding.range(),
    );
    if let Some(fix) = remove_unused_variable(binding, checker) {
        diagnostic.set_fix(fix);
    }
}


```
With this, the implementation of both rules becomes trivial, and we have something more general. I think now it solves this beautifully.






---

_Comment by @dscorbett on 2025-08-07 17:51_

I haven’t reviewed the code but I have three responses to the opening comment.

```python
    def set_class(self, cls):
        __class__ = cls  # No longer warns about unused variable
```
There actually should be an F841 warning because `__class__` is a local variable here, not a nonlocal variable, and is therefore unused.

```python
    def get_class(self):
        nonlocal __class__  # No longer warns about missing binding
        return __class__
```
There should be a warning because the `nonlocal` statement is redundant, though I agree it is out of scope for PLE0117. This could be a new rule analogous to [`global-variable-not-assigned` (PLW0602)](https://docs.astral.sh/ruff/rules/global-variable-not-assigned/).

Finally, the original issue used F841 and PLE0117 as examples. The fix should probably be at a more fundamental level in the code about bindings and scopes, affecting all rules.

---

_Comment by @ntBre on 2025-08-13 18:12_

Thanks @dscorbett, it sounds like the fix here is not quite right.

@mikeleppane I'm not sure adding this method to the semantic model would be very helpful, at least as used in this snippet. We'd still have to be careful to call it in each affected rule, so it's not much of a step up from the type of special-casing in this PR. That's why I'm still intrigued by the synthetic binding approach. It would be a small special case when creating new scopes, and then all of our other infrastructure should continue working as usual. That may still not work, though, because `__class__` isn't bound in the class scope, but it's also not exactly bound in the method scope because you can't usually do something like this:

```pycon
>>> class C:
...     def f():
...         x = 1
...         nonlocal x
...         return x
...
  File "<python-input-5>", line 4
    nonlocal x
    ^^^^^^^^^^
SyntaxError: name 'x' is assigned to before nonlocal declaration
```

which seems analogous to the `nonlocal` case for `__class__` if you synthesize it as a binding in the method.

I think I've talked myself out of the synthetic binding, so maybe a new semantic model method _is_ called for, but ideally we'd call it within existing semantic model methods so that every rule benefits from it automatically. It may also be worth checking how ty handles this. I didn't turn up anything obviously helpful in a very quick grep, but I suspect that they may have an answer somewhere.

We _could_ push a synthetic scope between the class and the method, which seems closest to how this is implemented in CPython, but then we have to skip that scope kind everywhere else, which doesn't seem worth it.

---

_Comment by @ntBre on 2025-08-26 13:50_

Thanks again for your work here! I think we can close this now in favor of https://github.com/astral-sh/ruff/pull/20048. I pulled in your tests over there and added you as a co-author :)

---

_Closed by @ntBre on 2025-08-26 13:50_

---
