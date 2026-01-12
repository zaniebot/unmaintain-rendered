```yaml
number: 16900
title: "[red-knot] Do not emit `invalid-return-type` for abstract functions"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - ty
assignees: []
merged: true
base: main
head: rk-invalid-return-type
created_at: 2025-03-21T16:08:55Z
updated_at: 2025-03-23T20:53:02Z
url: https://github.com/astral-sh/ruff/pull/16900
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Do not emit `invalid-return-type` for abstract functions

---

_@InSyncWithFoo_

## Summary

Resolves #16895.

`abstractmethod` is now a `KnownFunction`. When a function is decorated by `abstractmethod` or when the parent class inherits directly from `Protocol`, `invalid-return-type` won't be emitted for that function.

## Test Plan

Markdown tests.


---

_Review requested from @carljm by @InSyncWithFoo on 2025-03-21 16:08_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-03-21 16:08_

---

_Review requested from @sharkdp by @InSyncWithFoo on 2025-03-21 16:08_

---

_Review requested from @dcreager by @InSyncWithFoo on 2025-03-21 16:08_

---

_Label `red-knot` added by @AlexWaygood on 2025-03-21 16:13_

---

_Comment by @github-actions[bot] on 2025-03-21 16:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:46:26: Function can implicitly return `None`, which is not assignable to return type `str`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:53:27: Function can implicitly return `None`, which is not assignable to return type `int`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:59:40: Function can implicitly return `None`, which is not assignable to return type `bool`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:84:71: Function can implicitly return `None`, which is not assignable to return type `bool`
- Found 141 diagnostics
+ Found 137 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/frame.py:47:42: Function can implicitly return `None`, which is not assignable to return type `str`
- Found 288 diagnostics
+ Found 287 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/console.py:264:10: Function can implicitly return `None`, which is not assignable to return type `ConsoleRenderable | RichCast | str`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/console.py:562:10: Function can implicitly return `None`, which is not assignable to return type `list`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/rich/rich/layout.py:86:32: Function can implicitly return `None`, which is not assignable to return type `str`
- Found 589 diagnostics
+ Found 586 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-03-21 16:22_

I don't know if we want to support `abstractproperty`, `abstractclassmethod` and `abstractstaticmethod`. They've all been deprecated for many Python releases, and are unsupported by both mypy and pyright. The support you're adding for them in this PR isn't too much added complexity, but I suspect that full support for these decorators might be a significant amount of complexity. It will be less confusing for users if we don't support them at all rather than partially supporting them.

---

_Comment by @InSyncWithFoo on 2025-03-21 16:47_

Glad to hear that! Reverted.

---

_@MichaReiser reviewed on 2025-03-21 16:52_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1230 on 2025-03-21 16:52_

Nit: We don't need to get the `ScopeId` here because we know that all scopes are from the same file (this is slightly faster)
```suggestion
                let current_scope_id = self.scope().file_scope_id(self.db());
                let current_scope = self.index.scope(current_scope_id);

                let Some(parent_scope_id) = current_scope.parent() else {
                    return false;
                };
                let parent_scope = self.index.scope(parent_scope_id);
```

---

_@MichaReiser reviewed on 2025-03-21 16:53_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1251 on 2025-03-21 16:53_

Can you add a test where the class uses a type parameters and, because of that, has one extra scope

---

_@MatthewMckee4 reviewed on 2025-03-21 17:08_

Should we add a test for "Function can implicitly return `None`, which is not assignable to return type `{}`" is emitted when you have a function with no body outside of class
```python
# error: [invalid-return-type] "Function can implicitly return `None`, which is not assignable to return type `int`"
def f(self) -> int: ...
```

---

_@AlexWaygood approved on 2025-03-21 18:48_

This looks great to me -- thank you!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1251 on 2025-03-21 19:14_

I think the test you added using a legacy `TypeVar` is useful because it shows that this code doesn't understand inheritance from `Protocol[T]` as creating a protocol class. But I think @MichaReiser's comment was aimed in a slightly different direction. New-style (PEP 695) generics create an extra nested type-params scope wrapping the function body or class body scope, which will confuse code like this that tries to walk up a fixed number of expected scopes. Thus I think we should test both of these cases:

```py
class GenericProto[T](Protocol[T]):
    def f(self) -> int: ...

class ProtoWithGenericMethod(Protocol):
    def f[T](self, x: T) -> T: ...
```

I suspect that the first case is already correctly finding the class node (because of the use of `parent_scope.node()` which already bypasses the nested type-params scope for the class), but will still not work yet for the same reason that the legacy-TypeVar test doesn't work (we don't understand inheriting `Protocol[T]` as creating a generic protocol.) I think it's fine for this to remain a TODO pending generic protocol support.

I suspect the latter case will not work because we will identify the wrong `parent_scope` (due to the nested type-params scope in between the class scope and the body scope of `f`). I think this is important to fix before we land this code.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1259 on 2025-03-21 19:43_

In terms of API, rather than doing all this right here in type inference, I think we should have methods `Class::is_protocol() -> bool` and `FunctionType::enclosing_class() -> Option<Class<'db>>`.

In terms of how those methods are implemented, I did wonder about an alternate strategy that would record the enclosing-class information for each function in semantic indexing. This would imply that `DefinitionKind::Function` would include an `Option<FileScopeId>` which is the body scope of the lexically enclosing class.

But I'm not sure this is really better; it would require extra work in semantic indexing to maintain the "enclosing class" state and clear it appropriately (that is, nested functions inside methods are not themselves methods and should have no enclosing class), and I think the scope-walking code you have here can be made fully correct without too much difficulty.

---

_@carljm reviewed on 2025-03-21 19:43_

Thank you, this is a nice improvement!

---

_Comment by @carljm on 2025-03-21 19:47_

> Should we add a test for "Function can implicitly return `None`, which is not assignable to return type `{}`" is emitted when you have a function with no body outside of class

I think we should test this, but it seems orthogonal to this PR. If we are lacking a test for this and you want to submit a PR to add it, that would be great!

---

_@carljm reviewed on 2025-03-23 14:08_

Added tests look good! I would still like to see us break up the mass of the new added code by extracting some reusable and meaningfully-named functions.

---

_Comment by @InSyncWithFoo on 2025-03-23 17:00_

@carljm `FunctionType::enclosing_class()` seems unhelpful here, since `infer_function_body()` is trying to figure out that type in the first place. Am I missing something?

---

_Comment by @carljm on 2025-03-23 17:46_

> `FunctionType::enclosing_class()` seems unhelpful here, since `infer_function_body()` is trying to figure out that type in the first place. Am I missing something?

`infer_function_body` is not part of figuring out the `FunctionType`; checking the function body is separate and only done on demand. So it would be possible to get the `FunctionType` here by looking up the function `Definition` and querying its type. But you're right that it may not be the simplest / most ergonomic thing.

My other concern is that we should let `Class` keep responsibility for interpreting the types of its bases, rather than having ad-hoc code in various places looking directly at the base class elements in the AST. But I'm fine with just adding a TODO for that for now, since it will be natural to do it when actually adding `Protocol` support.

---

_@carljm reviewed on 2025-03-23 17:47_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1254 on 2025-03-23 17:47_

```suggestion
                // TODO move this to `Class` once we add proper `Protocol` support
                node_ref.bases().iter().any(|base| {
```

---

_Comment by @carljm on 2025-03-23 17:48_

Still halfway tempted to move "find the enclosing class" logic to `Scope`, but I think we can wait on that until/unless we have a second or third use case for the same logic. (I think it initially struck me as duplication because we had this same code previously, but I think the previous occurrence of it was removed in favor of storing the "defining node" of a scope in the scope itself.)

---

_Comment by @carljm on 2025-03-23 17:49_

Thanks for the PR!

---

_Merged by @carljm on 2025-03-23 17:51_

---

_Closed by @carljm on 2025-03-23 17:51_

---

_Branch deleted on 2025-03-23 17:54_

---

_Comment by @carljm on 2025-03-23 20:53_

Ok, I found where I had seen this is-method-of-class logic before, and we still have it (and it's currently wrong). See https://github.com/astral-sh/ruff/issues/16928

---
