```yaml
number: 1071
title: lsp query caching seems to constantly mixup functions with identical signatures
type: issue
state: closed
author: Gankra
labels:
  - bug
  - server
assignees: []
created_at: 2025-08-20T23:05:54Z
updated_at: 2025-08-21T20:36:42Z
url: https://github.com/astral-sh/ty/issues/1071
synced_at: 2026-01-10T02:06:24Z
```

# lsp query caching seems to constantly mixup functions with identical signatures

---

_Issue opened by @Gankra on 2025-08-20 23:05_

LSP functionality associated with looking up a function's signature is seemingly cached based on the raw type signature of a function. This affects goto-* on keyword arguments and docstrings for signature_help (notably *not* docstrings for hovering the function name!).

I'm fairly confident this is specifically a *caching* bug because the problem never appears in cursor tests, but does appear in an IDE. That is, if you check these queries in a cursor test it works perfectly, but if you try it in an IDE then all functions that share a signature (in a file?) suddenly have conflated info.

<img width="417" height="186" alt="Image" src="https://github.com/user-attachments/assets/171e950b-c316-434d-8f8f-95511967d7a4" />

```py
def my_func(x, y):
    '''my cool func'''
def my_other_func(x, y):
    '''some other func'''

my_func(x=0, y=1)
my_other_func(x=0, y=1)
```

https://play.ty.dev/b983c0f4-16cc-45c3-9269-6f96a6165e56


---

_Label `bug` added by @Gankra on 2025-08-20 23:05_

---

_Label `server` added by @Gankra on 2025-08-20 23:05_

---

_Comment by @Gankra on 2025-08-20 23:07_

https://github.com/astral-sh/ruff/pull/20013 has an example of me trying to show this in a cursor test and failing

---

_Comment by @MichaReiser on 2025-08-21 06:22_

This is odd. We don't do any salsa caching for hover (outside the semantic model)

---

_Comment by @MichaReiser on 2025-08-21 06:24_

I can't reproduce this in the playground or in VS Code

Edit: I can reproduce this for signature help but not for hover:

https://github.com/user-attachments/assets/ce6649c9-ca84-47ae-9cec-ede659efb908

---

_Comment by @MichaReiser on 2025-08-21 07:16_

Okay, I think I understand where things go wrong. Not sure what the proper fix is. The issue is the `into_callable` call here:

https://github.com/astral-sh/ruff/blob/4ac2b2c22283c9f8eb7f7fd945ab1c5f08098ab4/crates/ty_python_semantic/src/types/ide_support.rs#L821-L847

If we change this to call `bindings` directly on the `func_type`, then the "caching" issue goes away:

```rust
    if func_type.into_callable(db).is_none() {
        // Not a callable type
        return vec![];
    }

    let call_arguments =
        CallArguments::from_arguments(db, &call_expr.arguments, |_, splatted_value| {
            splatted_value.inferred_type(model)
        });

    let bindings = func_type.bindings(db).match_parameters(&call_arguments);

    // Extract signature details from all callable bindings
    bindings
        .into_iter()
        .flat_map(std::iter::IntoIterator::into_iter)
        .map(|binding| {
            let signature = &binding.signature;
            let display_details = signature.display(db).to_string_parts();
            let parameter_label_offsets = display_details.parameter_ranges.clone();
            let parameter_names = display_details.parameter_names.clone();

            CallSignatureDetails {
                signature: signature.clone(),
                label: display_details.label,
                parameter_label_offsets,
                parameter_names,
                definition: signature.definition(),
                argument_to_parameter_mapping: binding.argument_matches().to_vec(),
            }
        })
        .collect()
```

However, this breaks this test:

https://github.com/astral-sh/ruff/blob/d59282ebb5f5d60cda972a57d3b01fe7f1b02a29/crates/ty_ide/src/signature_help.rs#L487

Which I suspect is due to the comment here:

https://github.com/astral-sh/ruff/blob/ec3163781c645b2e72414e8b0c5686e815d34e0d/crates/ty_python_semantic/src/types.rs#L4536-L4549

The reason we can't reproduce this in cursor tests is that it requires two queries. What I see in the IDE is that VS Code requests inlay hints for `my_func` and then `my_other_func`, so that we end up with a callable type for `my_func` first.

Now, what we should fix in my view too is that we shouldn't inherit the `Definition` from the function literal for `CallableType`s. 

The part that I don't fully understand yet is how both `FunctionLiterals reduce to the same `CallableTypes even when their definitions are different.

I think this is where I have to bail out because of time :( 

@dhruvmanila you worked on callable types and overloads. Can you help us understand how definitions are propagated through here?


---

_Comment by @dhruvmanila on 2025-08-21 08:16_

I think I see the problem which is that the `CallableSignature` has a custom implementation of `PartialEq` that excludes the `Definition` and it's only the `Definition` that separates the two `CallableSignature`:

https://github.com/astral-sh/ruff/blob/a5cbca156ccba18900f95d5597db9ed2935a31d6/crates/ty_python_semantic/src/types/signatures.rs#L973-L982

This means that when `CallableType::new` is invoked on the two `CallableSignature`, the definition isn't taken into account by salsa to see whether to create a new `CallableType` or return the interned one.

This is also based on the logs below:

```
2025-08-21 13:26:44.774870000 DEBUG request{id=6 method="textDocument/signatureHelp"}: func_type: def my_func(x, y) -> Unknown
2025-08-21 13:26:44.775171000 DEBUG request{id=6 method="textDocument/signatureHelp"}: FunctionType: FunctionLiteral(Id(2000))
2025-08-21 13:26:44.775211000 DEBUG request{id=6 method="textDocument/signatureHelp"}: signature: CallableSignature { overloads: [Signature { generic_context: None, inherited_generic_context: None, definition: Some(Definition { [salsa id]: Id(1802) }), parameters: Parameters { value: [Parameter { annotated_type: None, kind: PositionalOrKeyword { name: Name("x"), default_type: None }, form: Value }, Parameter { annotated_type: None, kind: PositionalOrKeyword { name: Name("y"), default_type: None }, form: Value }], is_gradual: false }, return_ty: None }] }
2025-08-21 13:26:44.775404000 DEBUG request{id=6 method="textDocument/signatureHelp"}: FunctionType::into_callable_type = CallableType(Id(2c00))
2025-08-21 13:26:44.775435000 DEBUG request{id=6 method="textDocument/signatureHelp"}: callable_type: (x, y) -> Unknown

2025-08-21 13:26:47.184187000 DEBUG request{id=9 method="textDocument/signatureHelp"}: func_type: def my_other_func(x, y) -> Unknown
2025-08-21 13:26:47.184587000 DEBUG request{id=9 method="textDocument/signatureHelp"}: FunctionType: FunctionLiteral(Id(2001))
2025-08-21 13:26:47.184651000 DEBUG request{id=9 method="textDocument/signatureHelp"}: signature: CallableSignature { overloads: [Signature { generic_context: None, inherited_generic_context: None, definition: Some(Definition { [salsa id]: Id(1805) }), parameters: Parameters { value: [Parameter { annotated_type: None, kind: PositionalOrKeyword { name: Name("x"), default_type: None }, form: Value }, Parameter { annotated_type: None, kind: PositionalOrKeyword { name: Name("y"), default_type: None }, form: Value }], is_gradual: false }, return_ty: None }] }
2025-08-21 13:26:47.184737000 DEBUG request{id=9 method="textDocument/signatureHelp"}: FunctionType::into_callable_type = CallableType(Id(2c00))
2025-08-21 13:26:47.184786000 DEBUG request{id=9 method="textDocument/signatureHelp"}: callable_type: (x, y) -> Unknown
```

---

_Comment by @dhruvmanila on 2025-08-21 08:17_

So, as oppose to the comment mentioned, the definition seems to be important in this scenario.

---

_Comment by @MichaReiser on 2025-08-21 08:42_

@dhruvmanila and I discussed this further. We see two possible solutions but it isn't clear to us which provides the best tradeoffs. We'd appreciate some input from @carljm  or @dcreager.


The main issue is that `CallableType` (an interned value) doesn't consider `Definition` in its equality. This is a big footgun that we should resolve and there are two options:

1. Remove `Definition` from `CallableType`: We don't need the `Definition` for typing. This has the advantage that we only have a single interned `CallableType` for all types with the same signature (where the only difference would be the `Definition`). The only challenge with this, calling `bindings()` on a `ClassLiteral` doesn't work for constructor calls. But maybe it should and `signature_help` should call `bindings` directly on the `function` type instead of going through `into_callable` that loses the `Definition`

2. Remove the custom `Eq` and `PartialEq` and `Hash` implementations from `Signature` so that two callable types are only equal if the definitions are the same (or Salsa will "merge" callable types with different definitions). Ignore the `Definition` in the `Signatures` `is_equivalent` implementation ([here](https://github.com/astral-sh/ruff/blob/33030b34cd3266a70f0d75e83d886dd8ac0b49ed/crates/ty_python_semantic/src/types/signatures.rs#L197-L214)). 




---

_Comment by @AlexWaygood on 2025-08-21 11:09_

From a typing perspective, I would say it's conceptually quite weird that a `CallableType` can have a `Definition` attached to it 😄 A `CallableType` is an abstract structural type that can be inhabited by many different function objects at runtime, so it's strange for it to carry around data that points back to a specific, concrete function object as its `Definition`.

However, I do see that from an LSP perspective it's quite convenient for it to "hold onto" the original function from which it was derived. And we also hold onto other information that's irrelevant if you're only interested in the question of the set of possible objects that could inhabit the type at runtime. For example, `CallableType`s hold onto the types of the default values of parameters -- but from a typing perspective, the types of the default values are irrelevant. The only thing that matters from a typing perspective is _whether_ there is a default value for a given parameter or not.

I think either approach that you outline could work here. Note that we already deliberately throw away the types of default values and the definition when we normalize `CallableType`s, in order to ensure that equivalent `CallableType`s share the same Salsa ID after normalization. (I wasn't actually aware of the custom `PartialEq` and `Hash` implementations; it's possible that https://github.com/astral-sh/ruff/pull/19615 was unnecessary since we have them.)

---

_Comment by @MichaReiser on 2025-08-21 11:29_

> (I wasn't actually aware of the custom PartialEq and Hash implementations; it's possible that https://github.com/astral-sh/ruff/pull/19615 was unnecessary since we have them.)

Or that your PR allows us to remove the custom equal implementation :)

---

_Comment by @carljm on 2025-08-21 14:29_

I think there's a lot going on on the LSP side here that I'm not really familiar with, but

> maybe it should and signature_help should call bindings directly on the function type instead of going through into_callable that loses the Definition

is what sounds most right to me. I think `CallableType` shouldn't have a `Definition` at all, and the LSP should not be eager to transform a function type into a general callable type.

But then, I'm probably missing some important things because I don't understand the relevance of this:

> The only challenge with this, calling bindings() on a ClassLiteral doesn't work for constructor calls.

---

_Comment by @MichaReiser on 2025-08-21 14:54_

> But then, I'm probably missing some important things because I don't understand the relevance of this:

The issue with calling `bindings` directly on a class literal (without going through `into_callable`) is that `a = Class()` no longer resolves to `Class.__init__`, which I assume is due to the TODO here:

https://github.com/astral-sh/ruff/blob/ec3163781c645b2e72414e8b0c5686e815d34e0d/crates/ty_python_semantic/src/types.rs#L4536-L4549

---

_Comment by @Gankra on 2025-08-21 15:25_

I certainly agree that it would be better if the LSP used Type-y APIs less, as it really wants to reason in terms of Definitions basically everywhere. The impression I have is that we end up using Types in more places because so much of ty_python_semantic is focused on Solve Types and ends up discarding Definition-y info that it intermediately computes while trying to solve the types -- info that the LSP is ultimately wanting in directly queryable form.

So while I agree it's supremely weird to grab a definition off of a type, I think it ends up being a practical compromise for now.

---

_Comment by @carljm on 2025-08-21 16:13_

I think there's probably some truth to that, particularly for features like find-references and goto-definition? But I'm not sure that dichotomy really applies here. For hover info, we really do have to resolve types -- we have to know what the possible runtime values of the thing you are hovering are, in order to display accurate information about them. Then the question is just, once we've resolved types, how precise is the type we have, and what do we know about that type that we can display? And knowing where the inhabitant(s) of that type is/are defined is one thing we may or may not know about the type.

I don't agree that it's inherently weird to pull a definition off of a type. If the type is "the singleton type of the function `foo` created from a specific `def foo` statement" (which is a type we have, its `Type::FunctionLiteral`), then the information we have about that type includes "which `def foo` statement", and it's perfectly reasonable to be able to get a Definition from that type.

If the type we have is "the type of all callables that take a single parameter named `x` of type `int` and return a `str`" (this is `Type::CallableType`), there are many, many possible runtime inhabitants of that type, and we have no idea where they might be defined. So it doesn't make sense for that type to store or be able to provide a `Definition`.

The problem here as I see it is that the LSP is transforming a `FunctionLiteral` type (which properly knows where its defined) into a more abstract `CallableType` (which _shouldn't_ have any idea where its defined), for relatively obscure reasons involving limitations in how we currently handle class constructors, and then that decision is pressing us to keep information on `CallableType` that doesn't properly belong there.

---

_Comment by @MichaReiser on 2025-08-21 16:26_

> The problem here as I see it is that the LSP is transforming a FunctionLiteral type (which properly knows where its defined) into a more abstract CallableType (which shouldn't have any idea where its defined), for relatively obscure reasons involving limitations in how we currently handle class constructors, and then that decision is pressing us to keep information on CallableType that doesn't properly belong there.

I agree with this. I think the fix is still the best move now but I'll create a follow up issue to consider removing `Definition` from `CallableType` and move signature help to use `type.bindings` directly (and resolving the TODO)

---

_Closed by @Gankra on 2025-08-21 20:36_

---
