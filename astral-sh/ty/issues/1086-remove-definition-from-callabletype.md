```yaml
number: 1086
title: "Remove `Definition` from `CallableType`"
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-08-22T06:26:56Z
updated_at: 2025-11-17T17:58:30Z
url: https://github.com/astral-sh/ty/issues/1086
synced_at: 2026-01-12T15:54:24Z
```

# Remove `Definition` from `CallableType`

---

_@MichaReiser_

Today, `CallableType` stores a `Definition` so that `signature_help` and `inlay_hints` can extract a docstring. However, it seems semantically wrong that two `CallableType` that are only different in their definition don't compare equal (and are interned twice):

> If the type we have is "the type of all callables that take a single parameter named `x` of type `int` and return a `str`" (this is `Type::CallableType`), there are many, many possible runtime inhabitants of that type, and we have no idea where they might be defined. So it doesn't make sense for that type to store or be able to provide a `Definition`.

As far as typing's concerned, two `CallableType`s are equivalent even if their `Definition` differs. I suspect that this could lead to subtle bugs where we show an incorrect docstring if the `Definition` isn't removed in all type operations and we then propagate.

There's also no strong reason for the LSP to use `CallableTypes` in the first place. The only reason the LSP server depends on `CallableType` to have a `Definition` is because [`call_signature_details`](https://github.com/astral-sh/ruff/blob/4ac2b2c22283c9f8eb7f7fd945ab1c5f08098ab4/crates/ty_python_semantic/src/types/ide_support.rs#L819) calls `into_callable()` on the callee (`CallExpression::func`) which resolves `a = Class()` to `Class.__init__`, something that calling `bindings()` directly on `Type` doesn't, I suspect due to this TODO:

https://github.com/astral-sh/ruff/blob/14fe1228e72dca90db6b3dbb7e07f973d175ea0e/crates/ty_python_semantic/src/types.rs#L4683-L4696

We should rewrite the LSP so that `call_signature_details` doesn't call `into_callable` and instead resolves the bindings directly on the callee's type but without loosing support for resolving constructor calls. This will then allow us to remove `Definition` from `CallableType`.

You can find some more details in the conversation in https://github.com/astral-sh/ty/issues/1071#issuecomment-3209578522


---

_Label `server` added by @MichaReiser on 2025-08-22 06:26_

---

_Comment by @carljm on 2025-11-17 17:55_

This is complicated by the issues discussed in #770. We currently wrongly infer a method literal type when accessing a method off a non-final class. Since a subclass could override the method, this is wrong; we should infer just the general callable type instead. But if we do that, and remove definition from callable types, we will lose all detailed hover information and goto for methods; this is a bad outcome.

So I'm not sure if we can remove tracking an optional definition in callable types. 

---

_Comment by @carljm on 2025-11-17 17:58_

Or else we need a deeper refactor so that attribute lookups etc return more than just a type, they return a type plus additional information needed for the LSP. 

---
