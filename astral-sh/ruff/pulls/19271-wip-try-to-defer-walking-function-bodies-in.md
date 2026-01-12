```yaml
number: 19271
title: "WIP: try to defer walking function bodies in SemanticIndexBuilder"
type: pull_request
state: closed
author: oconnor663
labels:
  - ty
assignees: []
draft: true
base: main
head: jack/nonlocal_deferred_function_bodies
created_at: 2025-07-10T23:55:39Z
updated_at: 2025-08-08T03:59:46Z
url: https://github.com/astral-sh/ruff/pull/19271
synced_at: 2026-01-12T15:56:35Z
```

# WIP: try to defer walking function bodies in SemanticIndexBuilder

---

_@oconnor663_

This PR is on top of https://github.com/astral-sh/ruff/pull/19112. @MichaReiser [suggested](https://github.com/astral-sh/ruff/pull/19112#pullrequestreview-3004188327) taking a shot at deferred walking of function bodies. This draft PR currently contains two commits. The first moves the `InvalidNonlocal` check into `SemanticIndexBuilder`. As expected, this makes one test case in `nonlocal.md` start failing, the one that relies on seeing bindings in outer scopes before checking `nonlocal` statements in inner scopes, basically this:
```py
def f():
    def g():
        nonlocal x  # allowed!
    x = 1
```

In the second commit I try to defer walking function bodies, to unbreak that test. In fact, _does_ unbreak that test! Hurray! Unfortunately it breaks a ton of other tests, and I'm having trouble figuring out why, because it seems to involve some (Salsa?) machinery I haven't seen yet. Here's a minimized repro:

```
$ cat test.py
class Foo:
    pass
foo = Foo()
$ ty check test.py
error[panic]: Panicked at crates/ty_python_semantic/src/types.rs:156:38 when checking `/tmp/test.py`: `Failed to retrieve the inferred type for an `ast::Expr` node passed to `TypeInference::expression_type()`. The `TypeInferenceBuilder` should infer and store types for all `ast::Expr` nodes in any `TypeInference` region it analyzes.`
info: This indicates a bug in ty.
info: If you could open an issue at https://github.com/astral-sh/ty/issues/new?title=%5Bpanic%5D, we'd be very appreciative!
info: Platform: linux x86_64
info: Args: ["/home/jacko/astral/ruff/target-mold/debug/ty", "check", "test.py"]
info: run with `RUST_BACKTRACE=1` environment variable to show the full backtrace information
info: query stacktrace:
   0: FunctionType < 'db >::signature_(Id(5007))
             at crates/ty_python_semantic/src/types/function.rs:595
             cycle heads: infer_scope_types(Id(c62)) -> IterationCount(0), FunctionType < 'db >::signature_(Id(5007)) -> IterationCount(0), FunctionType < 'db >::signature_(Id(5000)) -> IterationCount(0)
   1: infer_expression_types(Id(1463))
             at crates/ty_python_semantic/src/types/infer.rs:235
   2: infer_definition_types(Id(11ab))
             at crates/ty_python_semantic/src/types/infer.rs:159
   3: infer_scope_types(Id(c62))
             at crates/ty_python_semantic/src/types/infer.rs:130
             cycle heads: infer_scope_types(Id(c62)) -> IterationCount(0)
   4: FunctionType < 'db >::signature_(Id(5000))
             at crates/ty_python_semantic/src/types/function.rs:595
   5: infer_expression_types(Id(1400))
             at crates/ty_python_semantic/src/types/infer.rs:235
   6: infer_definition_types(Id(1001))
             at crates/ty_python_semantic/src/types/infer.rs:159
   7: infer_scope_types(Id(c00))
             at crates/ty_python_semantic/src/types/infer.rs:130
   8: check_file_impl(Id(800))
             at crates/ty_project/src/lib.rs:474
```

When I try to throw a lot of logging around, it seems like this is happening somewhere in the many, many built-in definitions we check implicitly. I'm hoping someone more experienced than me can take one look at this failure and intuit exactly what I broke? :sweat_smile: 

---

_Comment by @oconnor663 on 2025-07-11 00:10_

Notably the failure here is in `infer.rs`, which I think means the `SemanticIndex` is already built, so it shouldn't be sensitive to the _order_ of that build? It must mean I've started leaving something out in [the second commit](https://github.com/astral-sh/ruff/pull/19271/commits/3f22497bddc856c3557f66f6b1c5bd2ad65cc7ec)?

---

_@MichaReiser reviewed on 2025-07-11 08:16_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1173 on 2025-07-11 08:16_

I haven't fully dived into what's the issue but what I think is worth noting is that there are even some semantic index tests that are failing. 

What catches me as suspicious is that we don't use `visit_scoped_body` in the class body. Instead, class methods are visited as part of the enclosing module (or function) scope. However, this is problematic because the parent scope is now incorrect: It's the module scope's instead of the class scope. 

My naive fix of using `visit_scoped_body` for the class body leads to other interesting errors and not doing so is probably semantically correct(?). 

If methods need to be run in the module scope, then this probably requires changing how we store `Scope`s (or at least how we compute the range for the child scopes). The current representation is a tree flattened into a `Vec`. Each `Scope` stores a `Range<usize>` of its descendent scopes. The range is determined by capturing the length of the scopes vector when pushing a new Scope (that's where the next child will be inserted) and `pop_scope` sets the end to the new end of `scopes.len()`. 

https://github.com/astral-sh/ruff/blob/3f22497bddc856c3557f66f6b1c5bd2ad65cc7ec/crates/ty_python_semantic/src/semantic_index/builder.rs#L287-L290

---

_Label `ty` added by @AlexWaygood on 2025-07-11 11:17_

---

_Closed by @AlexWaygood on 2025-07-11 11:18_

---

_Reopened by @AlexWaygood on 2025-07-11 11:18_

---

_@oconnor663 reviewed on 2025-07-11 16:07_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1173 on 2025-07-11 16:07_

I think you're right. Here's the example I've come up with to try to crystallize this in my head:
```py
def f():
    class Foo:
        y = x  # NameError
        def g():
            nonlocal x  # allowed
    x = 1
```
So `y = x` in the class body is evaluated "eagerly", but `nonlocal x` in `g` is deferred (in some sense) to the end of `f`'s scope. However, as you point out, the scope stack at the end of `f` isn't correct for `g`; we need to put `Foo`'s class scope back on the stack when it's time to walk `g`? Something like that?

I'm actually enjoying this problem, but I promise not to sink too much time into it today :sweat_smile: I'll go ahead and land the [original PR](https://github.com/astral-sh/ruff/pull/19112) now, in any case.

---

_@MichaReiser reviewed on 2025-07-11 16:10_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1173 on 2025-07-11 16:10_

I think there's also a chance that scopes get out of order if you have something like

```
def foo():
	def bar(): ...
	
	class Foo: ...
	
	def baz(): ...
```

I think the scopes in `foo` would be `Foo`, `bar`, `baz` but they currently are `bar`, `Foo`, `baz`. I'm not sure if we rely on the ordering anywhere when traversing the descendent or child scopes.

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1173 on 2025-07-11 20:04_

> I'm not sure if we rely on the ordering anywhere when traversing the descendent or child scopes.

Yes, I'm starting to suspect this (in addition to the `scope_stack`) is a problem:

https://github.com/astral-sh/ruff/blob/b5c5f710fc12b5c512a2e5351684b8ffdf33761f/crates/ty_python_semantic/src/semantic_index/builder.rs#L285-L287

IIUC we assume that every scope covers a _range_ of `FileScopeId`s, and for example class scopes record the end of that range at the end of the class body. If we don't walk class methods before that point, I think it screws up how attributes bound in e.g. `__init__` are associated with the class.

That's enough digging for now. I think it'll be interesting to talk this over with @carljm after he gets back.

---

_@oconnor663 reviewed on 2025-07-11 20:04_

---

_Comment by @oconnor663 on 2025-08-08 03:58_

~~Closing in favor of https://github.com/astral-sh/ruff/pull/19820.~~ Woops I actually meant to comment the same on #19703. But I do think it makes sense to close this one too now that #19820 has avoided the need to defer function bodies.

---

_Closed by @oconnor663 on 2025-08-08 03:58_

---
