```yaml
number: 16916
title: "[red-knot] fix eager nested scopes handling"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: fix-eager-nested-scopes
created_at: 2025-03-22T15:51:30Z
updated_at: 2025-03-30T04:29:15Z
url: https://github.com/astral-sh/ruff/pull/16916
synced_at: 2026-01-10T19:40:36Z
```

# [red-knot] fix eager nested scopes handling

---

_Pull request opened by @mtshiba on 2025-03-22 15:51_

## Summary

From #16861, and the continuation of #16915.

This PR fixes the incorrect behavior of `TypeInferenceBuilder::infer_name_load` in eager nested scopes.

And this PR closes #16341.

## Test Plan

New test cases are added in `annotations/deferred.md`.

---

_Review requested from @carljm by @mtshiba on 2025-03-22 15:51_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-03-22 15:51_

---

_Review requested from @sharkdp by @mtshiba on 2025-03-22 15:51_

---

_Review requested from @dcreager by @mtshiba on 2025-03-22 15:51_

---

_Comment by @github-actions[bot] on 2025-03-22 15:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `red-knot` added by @MichaReiser on 2025-03-22 16:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/annotations/deferred.md`:71 on 2025-03-23 13:39_

I think the `Bar` reference here should be an unresolved-reference error, because the name `Bar` is defined in a class scope, which is not visible to nested scopes.

You make an explicit exception for this case in `infer_name_load`, but I don't think we should. It's not really clearly defined by the core runtime what the name resolution semantics should be for annotations when `from __future__ import annotations` is active (since the actual runtime effect is that the annotations remain strings and are not evaluated at all). But I don't think there is any reason to assume that the usual rules for names in class scopes should not apply. Pyright [still applies them](https://pyright-play.net/?strict=true&code=GYJw9gtgBA%2BjwFcAuCQFM5QJYQA5hCSgEMA7UsJYpLMUgZwChGBjAG2PvqgDEwwAXIygio7TtwBCxEENHyoAEzTAowABT00bYAEooAWgB8UabOELL6FCFJQtO5uK6niALzmjlqjQ72GTaQ8LKzQbOz9GIA). And if you use `typing.get_type_hints` at runtime to evaluate the stringified annotations, this throws a `NameError`.

(The `Foo` reference should not be an error. We should separate this into two different method definitions so it is clear which one errors and which does not.)
```suggestion
        def f(self, x: Foo):
            pass
            
        # error: [unresolved-reference]
        def g(self) -> Bar:
            return self
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4099 on 2025-03-23 13:47_

As discussed in the other comment in the test suite, the exception you make here for `self.is_deferred()` I think we should not make.

And I think the `scope.is_annotation` exception needs to be a bit more precise. Annotation scopes can see names defined in their immediately enclosing class scope, but not names defined in more distant enclosing class scopes. For example:

```py
 class A:
    a = 1
    class B:
        b = 2
        type S = a  # error!
        type T = b  # no error
```

We should add a test showing that the `type S = a` case is an `unresolved-reference` error, and adjust the implementation here accordingly.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4094 on 2025-03-23 13:50_

As discussed in other comments, I think only one of these exceptions is valid, and it's not related to lazy evaluation: in general, type parameter scopes can see names defined in their immediately-enclosing class scope.

```suggestion
                // There is one exception to this rule: type parameter scopes can see
                // names defined in an immediately-enclosing class scope.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4201 on 2025-03-23 13:57_

This is implemented as a special-case patch fix for one very specific case (references to classes inside the class definition), when what we need is a more general fix to the semantics implemented for eager nested scopes. Here is one example of a case that we currently handle wrongly, and is not fixed by this patch:

```py
# this line should be an unresolved-reference, but we don't error on it:
[x for _ in [1]]
x = 1
```

We should add this to the test suite. The fix should be implemented more like I described in my comment on your prior PR: when snapshotting visible bindings for names in eager nested scopes in semantic indexing, we also need to snapshot the fact that there are no visible bindings, and then respect that when we check `eager_bindings` above.

---

_@carljm requested changes on 2025-03-23 14:00_

Thanks for the PR! I think there are some adjustments needed.

---

_@mtshiba reviewed on 2025-03-24 12:33_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/types/infer.rs`:4201 on 2025-03-24 12:33_

Does this mean that we should add undefined symbols to the symbol table of the enclosing scope and register  snapshots of empty bindings in `eager_bindings`?
Should we also register the builtin symbols as undefined ones?

---

_Review request for @sharkdp removed by @sharkdp on 2025-03-24 14:20_

---

_@carljm reviewed on 2025-03-24 17:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4201 on 2025-03-24 17:48_

I don't think we should add the symbol to any outer scope. It may be that we don't actually need any change in semantic index building? We already will store no bindings in `eager_bindings` if there are no bindings for a name in a scope. It may be that all we need is a bit more sophisticated logic here in `infer_name_load`, where we need to track as we iterate enclosing scopes if we are still in "eager" context (that is, every scope we've traversed is eager), and if we find no eager binding in a scope while still in eager context, we need to just continue to the next scope rather than falling back to normal non-eager symbol resolution. Once we are out of "eager" context (that is, we have hit a non-eager scope), then eager bindings are no longer relevant and we should check normally for non-eager bindings in each scope. (So I don't think we need any special handling for builtin symbols.)

We should have tests for a case like this, too:

```py
x = 1
def f():
    class C:
        y = [x for _ in [1]]
        x = 2
    reveal_type(C.y)  # should reveal [1]
```

And the same should be true if `x = 1` occurs after the definition of `f` instead of before it.


---

_@carljm reviewed on 2025-03-24 17:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4201 on 2025-03-24 17:49_

@dcreager would you be able to take a couple minutes to double-check my logic here and make sure I'm suggesting a sensible direction, since you wrote the initial eager-nested-scopes handling?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4201 on 2025-03-26 18:06_

Yep I think this approach makes sense!


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4184 on 2025-03-26 18:10_

This brings back some logic duplication that we discussed when first adding eager nested scope support (https://github.com/astral-sh/ruff/pull/16079#discussion_r1951742987).  Ideally we could have the return value of `SemanticIndex::eager_bindings` distinguish between "this enclosing scope is no longer part of an eager context" and "it is but there are no bindings for the symbol".  Then we'd have a three-armed `match` statement here (`return`, `continue`, and fall through), and we wouldn't have to track `in_eager_context` both here and in the semantic index builder.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4150 on 2025-03-26 18:11_

TIL about this exception.  Yuck!

---

_@dcreager reviewed on 2025-03-26 18:11_

---

_Review requested from @dcreager by @mtshiba on 2025-03-27 06:51_

---

_Review requested from @carljm by @mtshiba on 2025-03-27 06:51_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index.rs`:333 on 2025-03-27 18:17_

Just as a style nit I'm not a huge fan of this "immediately call a lambda" pattern.  I think it's a bit easier to follow with `let`/`else`:

```suggestion
        let Some(symbol_id) = self.symbol_tables[enclosing_scope].symbol_id_by_name(symbol)
        else {
            return EagerBindingsResult::NotFound;
        };
        let key = EagerBindingsKey {
            enclosing_scope,
            enclosing_symbol: symbol_id,
            nested_scope,
        };
        let Some(id) = self.eager_bindings.get(&key) else {
            return EagerBindingsResult::NotFound;
        };
        match self.use_def_maps[enclosing_scope].eager_bindings(*id) {
            Some(bindings) => EagerBindingsResult::Found(bindings),
            None => EagerBindingsResult::NotFound,
        }
```

---

_@dcreager approved on 2025-03-27 18:19_

One last style nit, otherwise I think this looks great.  Thanks!

---

_@carljm approved on 2025-03-28 15:11_

Thank you!

---

_Merged by @carljm on 2025-03-28 15:11_

---

_Closed by @carljm on 2025-03-28 15:11_

---

_Branch deleted on 2025-03-30 04:29_

---
