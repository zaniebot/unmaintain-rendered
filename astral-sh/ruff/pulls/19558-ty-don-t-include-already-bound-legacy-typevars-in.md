```yaml
number: 19558
title: "[ty] Don't include already-bound legacy typevars in function generic context"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/containing-legacy-typevar
created_at: 2025-07-25T14:28:51Z
updated_at: 2025-07-25T22:14:21Z
url: https://github.com/astral-sh/ruff/pull/19558
synced_at: 2026-01-12T15:56:42Z
```

# [ty] Don't include already-bound legacy typevars in function generic context

---

_@dcreager_

We now correctly exclude legacy typevars from enclosing scopes when constructing the generic context for a generic function.

more detail:

A function is generic if it refers to legacy typevars in its signature:

```py
from typing import TypeVar

T = TypeVar("T")

def f(t: T) -> T:
    return t
```

Generic functions are allowed to appear inside of other generic contexts. When they do, they can refer to the typevars of those enclosing generic contexts, and that should not rebind the typevar:

```py
from typing import TypeVar, Generic

T = TypeVar("T")
U = TypeVar("U")

class C(Generic[T]):
    @staticmethod
    def method(t: T, u: U) -> None: ...

# revealed: def method(t: int, u: U) -> None
reveal_type(C[int].method)
```

This substitution was already being performed correctly, but we were also still including the enclosing legacy typevars in the method's own generic context, which can be seen via `ty_extensions.generic_context` (which has been updated to work on generic functions and methods):

```py
from ty_extensions import generic_context

# before: tuple[T, U]
# after: tuple[U]
reveal_type(generic_context(C[int].method))
```



---

_Review requested from @carljm by @dcreager on 2025-07-25 14:28_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-25 14:28_

---

_Review requested from @sharkdp by @dcreager on 2025-07-25 14:28_

---

_Comment by @github-actions[bot] on 2025-07-25 14:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
psycopg (https://github.com/psycopg/psycopg)
- psycopg/psycopg/_py_transformer.py:329:16: error[invalid-return-type] Return type does not match returned value: expected `Row`, found `tuple[Any, ...]`
- psycopg_pool/psycopg_pool/pool.py:263:16: error[invalid-return-type] Return type does not match returned value: expected `CT`, found `(Unknown & ~AlwaysFalsy) | Connection[@Todo(Support for `typing.TypeAlias`)]`
- Found 658 diagnostics
+ Found 656 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/tests/test_auth_backends.py:8035:26: error[invalid-argument-type] Argument to bound method `set` is incorrect: Argument type `Unknown | int` does not satisfy upper bound of type variable `Self`
- Found 7421 diagnostics
+ Found 7420 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Label `ty` added by @MichaReiser on 2025-07-25 15:22_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:355 on 2025-07-25 19:29_

We sure it's worth passing these vs just re-querying them from Salsa when we need them? We are already passing in `definition` below which knows its `File`, and a `File` is all you need to query a semantic index or a parsed module. Querying a cached value from Salsa should be very cheap (we do this a _lot_).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:356 on 2025-07-25 19:30_

I don't think we need to thread this through. Immediately below we have the function `Definition`, which already knows which scope it occurs in. (This will mean that we skip the type parameters scope for a PEP 695 generic function, but as commented above I think that's fine, even preferable.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/function.rs`:344 on 2025-07-25 19:34_

For a PEP 695 generic function, this will be the type parameters scope. I think in this particular case it doesn't matter, because you walk up scopes from there, but it's a bit subtle and not clear from the naming. The type parameters scope can't ever bind a legacy typevar, so it would also be fine if we skipped it.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:388 on 2025-07-25 19:37_

```suggestion
/// Returns an iterator of any generic context introduced by the given scope or any enclosing
```

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:394 on 2025-07-25 19:41_

Related to above, I'd suggest instead of taking `module` or `index` we might just take a `File` here.

Or take only a `ScopeId`, which encompasses a `File` and a `FileScopeId`.

I guess the one argument in favor of taking `module` and `index` is that it kind of makes it explicit that this method has a dependency on the AST of the module containing `scope`. But I'm not sure how obvious this is; it's not the way we've typically handled this.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:388 on 2025-07-25 19:56_

`infer.rs` is already massive with `TypeInferenceBuilder` and the various `*Inference` structs. Is it the best place for these new free functions? Seems like they kind of would belong in `generics.rs`.

---

_@carljm approved on 2025-07-25 20:01_

Looks good!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:394 on 2025-07-25 20:31_

Done.

> I guess the one argument in favor of taking `module` and `index` is that it kind of makes it explicit that this method has a dependency on the AST of the module containing `scope`.

That wasn't my intent, and I think it's an open question how we want to model that better more generally, so I'm going with the simpler function signatures that you've suggested

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:344 on 2025-07-25 20:31_

See below; this is now only generated when creating a legacy generic context

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:356 on 2025-07-25 20:31_

Good idea, done!

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:355 on 2025-07-25 20:32_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:388 on 2025-07-25 20:37_

Moved. That also let me make them file-local instead of `pub(crate)`

---

_@dcreager reviewed on 2025-07-25 20:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/call/bind.rs`:648 on 2025-07-25 20:42_

nit:

```suggestion
                                    Type::ClassLiteral(class) => class.generic_context(db)
                                        .map(|generic_context| generic_context.as_tuple(db))
                                        .unwrap_or_else(|| Type::none(db)),
```

---

_@AlexWaygood reviewed on 2025-07-25 20:43_

---

_Merged by @dcreager on 2025-07-25 22:14_

---

_Closed by @dcreager on 2025-07-25 22:14_

---

_Branch deleted on 2025-07-25 22:14_

---
