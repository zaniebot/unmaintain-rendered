```yaml
number: 19400
title: "[ty] Show the raw argument type in `reveal_type` "
type: pull_request
state: merged
author: dcreager
labels:
  - internal
  - ty
assignees: []
merged: true
base: main
head: dcreager/reveal-argument
created_at: 2025-07-17T14:48:50Z
updated_at: 2025-07-17T20:50:31Z
url: https://github.com/astral-sh/ruff/pull/19400
synced_at: 2026-01-12T15:56:38Z
```

# [ty] Show the raw argument type in `reveal_type` 

---

_@dcreager_

This PR is changes how `reveal_type` determines what type to reveal, in a way that should be a no-op to most callers.

Previously, we would reveal the type of the first parameter, _after_ all of the call binding machinery had done its work. This includes inferring the specialization of a generic function, and then applying that specialization to all parameter and argument types, which is relevant since the typeshed definition of `reveal_type` is generic:

```pyi
def reveal_type(obj: _T, /) -> _T: ...
```

Normally this does not matter, since we infer `_T = [arg type]` and apply that to the parameter type, yielding `[arg type]`. But applying that specialization also simplifies the argument type, which makes `reveal_type` less useful as a debugging aid when we want to see the actual, raw, unsimplified argument type.

With this patch, we now grab the original unmodified argument type and reveal that instead.

In addition to making the debugging aid example work, this also makes our `reveal_type` implementation more robust to custom typeshed definitions, such as

```py
def reveal_type(obj: Any) -> Any: ...
```

(That custom definition is probably not what anyone would want, since you wouldn't be able to depend on the return type being equivalent to the argument type, but still)

---

_Review requested from @carljm by @dcreager on 2025-07-17 14:48_

---

_Review requested from @AlexWaygood by @dcreager on 2025-07-17 14:48_

---

_Review requested from @sharkdp by @dcreager on 2025-07-17 14:48_

---

_Label `internal` added by @dcreager on 2025-07-17 14:48_

---

_Label `ty` added by @dcreager on 2025-07-17 14:48_

---

_@dcreager reviewed on 2025-07-17 14:50_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:1056 on 2025-07-17 14:50_

This is the meat of the change; the rest is just refactoring to enable this + cleanup.

We need the union builder because technically multiple arguments could be matched to the first parameter, though that would require a custom typeshed definition of `reveal_type` that uses a variadic parameter:

```pyi
def reveal_type(*args) -> Any: ...
```

---

_Comment by @github-actions[bot] on 2025-07-17 14:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp approved on 2025-07-17 17:29_

Thank you very much!

I was thinking about how to write a test for this. One could probably create an unsimplified `int | Literal[1]` union type via `UnionType::new(db, […])` (which we would ideally make private if we could, but for this test, it would be helpfully available).

But then how do we feed that type into `reveal_type`? We could expose some unsafe functionality via `ty_extensions` maybe, but that seems like a very bad idea.

All that is to say: I'm fine with there not being a test. It will help everyone who's working on the {Union/Intersection}Builder in the future.

---

_Comment by @dcreager on 2025-07-17 17:51_

I can try writing a Rust test instead of an mdtest

---

_Comment by @sharkdp on 2025-07-17 18:28_

> I can try writing a Rust test instead of an mdtest

I was thinking about Rust tests as well. But I assume it's not trivial to invoke the whole call inference machinery without having an AST. And if you construct an AST by parsing some example, then it's unclear how to inject that "wrong" type? Maybe you figure something out, it's certainly not impossible. But I'm not sure if it's worth spending a lot of time on. That's what I wanted to say originally.

---

_Merged by @dcreager on 2025-07-17 20:50_

---

_Closed by @dcreager on 2025-07-17 20:50_

---

_Branch deleted on 2025-07-17 20:50_

---
