```yaml
number: 13894
title: "[red-knot] Type narrowing for `isinstance` checks"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/isinstance-narrowing
created_at: 2024-10-23T14:11:33Z
updated_at: 2024-10-23T18:51:36Z
url: https://github.com/astral-sh/ruff/pull/13894
synced_at: 2026-01-12T15:55:46Z
```

# [red-knot] Type narrowing for `isinstance` checks

---

_@sharkdp_

## Summary

Add type narrowing for [`isinstance(object, classinfo)`](https://docs.python.org/3/library/functions.html#isinstance) checks:
```py
x = 1 if flag else "a"

if isinstance(x, int):
    reveal_type(x)  # revealed: Literal[1]
```

To do:
- [x] Should we emit a diagnostic if we detect that arguments to the `isinstance` call are incorrect? Or would that happen anyway in type inference, because we would check `isinstance` calls against [this typeshed signature](https://github.com/python/typeshed/blob/ca65e087f1f9dfc28e89192b60ce7cfc2e92c674/stdlib/builtins.pyi#L1468):
    ```pyi
    def isinstance(obj: object, class_or_tuple: _ClassInfo, /) -> bool: ...
    ```
    => would happen in type inference/checking.
- [x] ~~Add support for [PEP 604](https://peps.python.org/pep-0604/) union types on the right-hand side. Supporting `str | int` is different from supporting `(str, int)`: For the latter, we can simply use the inferred type (`tuple[type[str], type[int]]`) and generate a constraint from it. However, if we attempt to do the same for the former, we would get `builtin.UnionType` (or `@Todo`, currently). So we probably need to walk the AST and extract the necessary information from there, similar to how we would generate a type annotation from the AST of `str | int`.~~ => deferred for now

closes #13893

## Test Plan

New Markdown-based tests in `narrow/isinstance.md`.


---

_Label `red-knot` added by @sharkdp on 2024-10-23 14:11_

---

_Comment by @github-actions[bot] on 2024-10-23 14:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:1 on 2024-10-23 14:30_

Could you also add some tests for cases which should pass type-checking, but for which we can't infer a narrowing constrant? E.g.

```py
t = type("t", (), {})  # we should infer this as an instance of type, I think?
x = 1 if flag else "foo"

if isinstance(x, t):
    reveal_type(x)
```

---

_Marked ready for review by @sharkdp on 2024-10-23 14:35_

---

_Review requested from @carljm by @sharkdp on 2024-10-23 14:35_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-23 14:35_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-23 14:35_

---

_@sharkdp reviewed on 2024-10-23 14:36_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:26 on 2024-10-23 14:36_

I can't add a test for `isinstance(…, tuple[(int, str)])` because we don't support generics yet.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:227 on 2024-10-23 14:37_

This is a little naive ;) You need to make sure that it's `builtins.isinstance` from the standard library that's being called, not just any user-defined function that happens to be called `isinstance()`. (It would be great to add a test for this!)

I think what you want to do is add an `IsInstance` variant to this enum:

https://github.com/astral-sh/ruff/blob/72c18c8225166f1f08a826c622b9cbcb48331162/crates/red_knot_python_semantic/src/types.rs#L1600-L1606

And then add code here so that we infer `KnownFunction::IsInstance` for `builtins.isinstance()`: https://github.com/astral-sh/ruff/blob/72c18c8225166f1f08a826c622b9cbcb48331162/crates/red_knot_python_semantic/src/types/infer.rs#L778-L783

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:245 on 2024-10-23 14:42_

```suggestion
            if let [ast::Expr::Name(ast::ExprName { id, .. }), rhs] = &*expr_call.arguments.args {
                let symbol = self.symbols().symbol_id_by_name(id).unwrap();

                let rhs_type = inference.expression_ty(rhs.scoped_ast_id(self.db, scope));

                // TODO: add support for PEP 604 union types on the right hand side:
                // isinstance(x, str | (int | float))
                if let Some(constraint) = self.to_isinstance_constraint(&rhs_type) {
                    self.constraints.insert(symbol, constraint);
                }
            }
        }
    }
```

---

_@AlexWaygood reviewed on 2024-10-23 14:43_

---

_@sharkdp reviewed on 2024-10-23 14:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:227 on 2024-10-23 14:49_

Oops :smile: 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:26 on 2024-10-23 15:05_

`isinstance(..., tuple[(int, str)])` doesn't work at runtime:

```
>>> x = (1, "a")
>>> isinstance(x, tuple)
True
>>> isinstance(x, tuple[(int, str)])
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: isinstance() argument 2 cannot be a parameterized generic
```

So we should (eventually, not in this PR) emit a diagnostic for this in type inference, and we don't need to support it in narrowing.

(Also, FWIW, although we don't support generalized generics yet, we do have a representation for heterogeneous tuple types specifically, since they are a special case, and it wouldn't be very hard to add support in type-expression inference for understanding `tuple[int, str]` as a type annotation, if that were blocking something else.)

---

_@carljm reviewed on 2024-10-23 15:13_

---

_@AlexWaygood reviewed on 2024-10-23 15:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:228 on 2024-10-23 15:14_

We might also want to check that there are no keyword arguments here (`expr_call.arguments.keywords`). Neither mypy nor pyright does any type narrowing for this (which will obviously fail at runtime; `isinstance()` has no `foo` argument):

```py
class A: ...
class B: ...

def f(x: object):
    if isinstance(x, A, foo="bar"):
        reveal_type(x)
```

---

_@carljm reviewed on 2024-10-23 15:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/narrow.rs`:227 on 2024-10-23 15:15_

Another related case that should be handled automatically by Alex's suggested change, and IMO we should add a test for (though perhaps one could argue it's not necessary to support? not sure if mypy/pyright do) is the case where isinstance has been aliased to a different name:

```
>>> myfunc = isinstance
>>> myfunc(1, int)
True
```

---

_@AlexWaygood reviewed on 2024-10-23 15:15_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:26 on 2024-10-23 15:15_

I _think_ we should also already accurately infer the _runtime_ value of `tuple[int, str]` as being a `types.GenericAlias` instance:

```pycon
>>> tuple[int, str]
tuple[int, str]
>>> type(_)
<class 'types.GenericAlias'>
```

---

_@AlexWaygood reviewed on 2024-10-23 15:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:227 on 2024-10-23 15:18_

Mypy supports aliasing `isinstance` by import but not by assignment: https://mypy-play.net/?mypy=latest&python=3.12&gist=14114360a8603492b2f0b992fac6c0c7

Pyright supports both: https://pyright-play.net/?enableExperimentalFeatures=true&code=GYJw9gtgBARgrgSwDYBcEDsDOUEQA5ggo6YaYoCG6AxgKZQXbBhgCwAUBzBSFALwkylGrQ4cAJrWBRgACgAeALihgYAK1rUUASkUcoBnNOZgFAGhzode9obtQQtAG60KSAPooAnnloLt%2BvYGgQYI0twg5lDkILoh9o4ubp4%2BfvLaQA

We should support both, and add tests for both, IMO

---

_@sharkdp reviewed on 2024-10-23 15:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:1 on 2024-10-23 15:32_

Done.

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:227 on 2024-10-23 15:34_

We now support both (and have tests for both). Thanks!

---

_@sharkdp reviewed on 2024-10-23 15:34_

---

_@sharkdp reviewed on 2024-10-23 15:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:26 on 2024-10-23 15:35_

Yes, I saw `types.GenericAlias` being inferred for `tuple[int, str]` and then wrote my comment above, unfortunately without thinking one step futher. Thank you for the clarification.

---

_@sharkdp reviewed on 2024-10-23 15:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:228 on 2024-10-23 15:39_

Done.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:153 on 2024-10-23 15:40_

```suggestion

# TODO: this should cause us to emit a diagnostic
# (`isinstance` has no `foo` parameter)
if isinstance(x, int, foo="bar"):
```

It would also be great to add a test for other invalid inputs like `isinstance(x, "not a type")` IMO

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/narrow/isinstance.md`:88 on 2024-10-23 15:41_

nit: let's make sure this test continues to test what we want it to test

```suggestion
t = type("t", (), {})

# This isn't testing what we want it to test if we infer anything more precise here:
reveal_type(t)  # revealed: type
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:879 on 2024-10-23 15:42_

Given that `IsInstance` doesn't have special behavior when called in type inference, I think it would be better to defer to typeshed rather than hardcoding its return type. So I would use `function_type.return_type(db)` here, or even better collapse this with the `None` arm.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:231 on 2024-10-23 15:43_

nit: let's add a `FunctionType::is_known` method, for symmetry with `ClassType::is_known`:

https://github.com/astral-sh/ruff/blob/72c18c8225166f1f08a826c622b9cbcb48331162/crates/red_knot_python_semantic/src/types.rs#L1622-L1627

---

_@AlexWaygood reviewed on 2024-10-23 15:44_

---

_@carljm approved on 2024-10-23 15:44_

Looks great! Just one nit.

---

_@carljm reviewed on 2024-10-23 15:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:879 on 2024-10-23 15:50_

With Alex's suggestion above of a `FunctionType::is_known` method, this could probably just become an `if function_type.is_known(KnownFunction::RevealType) { ... } else { ... }` instead of a match, at least until we have another known function with special inference-time behavior.

---

_@AlexWaygood reviewed on 2024-10-23 15:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:209 on 2024-10-23 15:51_

nit: not sure about the name of this method. I think the usual convention is that `to_*` methods convert `self` into another type -- but here we're converting a passed-in argument to another type.

This routine actually looks like it could either be a method on `Type` or a freestanding function that accepts `db` and `classinfo`

---

_@sharkdp reviewed on 2024-10-23 17:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/narrow.rs`:209 on 2024-10-23 17:50_

> This routine actually looks like it could either be a method on `Type` 

I don't know. `Type` seems too prominent a place for this helper function.

> or a freestanding function that accepts `db` and `classinfo`

Yeah, okay.

> nit: not sure about the name of this method. I think the usual convention is that `to_*` methods convert `self` into another type -- but here we're converting a passed-in argument to another type.

It's now a free function, but I renamed it to `generate_isinstance_constraint` anyway.

---

_@AlexWaygood reviewed on 2024-10-23 17:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/narrow.rs`:209 on 2024-10-23 17:51_

Yeah, I like what you've done best, I think. Having it in `narrow.rs` improves encapsulation -- we should only need this functionality for narrowing, not for anything else -- so it's better not to have it as a method on `Type`

---

_@AlexWaygood approved on 2024-10-23 18:00_

Looks great!

---

_@carljm approved on 2024-10-23 18:30_

🎉 

---

_Merged by @sharkdp on 2024-10-23 18:51_

---

_Closed by @sharkdp on 2024-10-23 18:51_

---

_Branch deleted on 2024-10-23 18:51_

---
