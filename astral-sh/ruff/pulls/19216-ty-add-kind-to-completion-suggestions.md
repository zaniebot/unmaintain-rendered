```yaml
number: 19216
title: "[ty] Add \"kind\" to completion suggestions"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/completion-kind
created_at: 2025-07-08T19:54:53Z
updated_at: 2025-07-11T14:48:03Z
url: https://github.com/astral-sh/ruff/pull/19216
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Add "kind" to completion suggestions

---

_Pull request opened by @BurntSushi on 2025-07-08 19:54_

This PR does some refactoring to expose a `Type` for each completion
item. This is mostly unexciting, except for the case of determining the
type of an instance attribute. Once the `Type` information is exposed,
we define a mapping between it and [`CompletionItemKind`].

The mapping isn't perfect (it gives up on union/intersection types),
but we can improve it going forward. And it at least should give some
nice icons for many cases.

Closes astral-sh/ty#775

[`CompletionItemKind`]: https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#completionItemKind


---

_Review requested from @carljm by @BurntSushi on 2025-07-08 19:54_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-07-08 19:54_

---

_Review requested from @sharkdp by @BurntSushi on 2025-07-08 19:54_

---

_Review requested from @dcreager by @BurntSushi on 2025-07-08 19:54_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-07-08 19:54_

---

_Comment by @BurntSushi on 2025-07-08 19:55_

Demo in neovim:


https://github.com/user-attachments/assets/ab9368c4-af42-41d2-895d-88b2bce0a790

Demo in VS Code:


https://github.com/user-attachments/assets/783ccf95-39e6-4968-b41c-cc4cbde1edd0



---

_Review request for @dcreager removed by @BurntSushi on 2025-07-08 19:56_

---

_Review request for @carljm removed by @BurntSushi on 2025-07-08 19:56_

---

_Review requested from @dhruvmanila by @BurntSushi on 2025-07-08 19:56_

---

_Comment by @github-actions[bot] on 2025-07-08 19:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Label `server` added by @MichaReiser on 2025-07-09 06:06_

---

_Label `ty` added by @MichaReiser on 2025-07-09 06:06_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/ide_support.rs`:300 on 2025-07-09 06:15_

Can you document why the equality only takes the name into account but not its type

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/requests/completion.rs`:111 on 2025-07-09 06:17_

We'll need the exact same mapping for `ty_wasm` to show the same symbols in the playground. Because of that, it would be great if `Completion` had a `kind` method that returns our own mapping which the server/playground facades then map to VS code's kinds.

---

_@MichaReiser approved on 2025-07-09 06:21_

Nice. It seems there are now two failing mdtests. I would also like if @sharkdp could take a look at the changes to `all_declarations_and_bindings` 

---

_@sharkdp reviewed on 2025-07-09 06:59_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:1232 on 2025-07-09 06:59_

If you take the example above and use `reveal_type(quux.some_attribute)`, you'll notice that a lot of these types here do not correspond to the actual type of the argument. This is because taking the type from the class body declaration/binding directly does not invoke the descriptor protocol. For example, `__class__` has a type of `property` here, but if you were to invoke the descriptor protocol, you would call the `__get__` method of the `property` and get `type[Quux]` as the result.

Similar, all functions are descriptors. When accessed on a class instance (like `quux: Quux`), they turn into bound method objects. So the type of `__delattr__` should be `bound method Quux.__delattr__(name: str, /) -> None` (where the `self` parameter is already bound).

This probably means that we're incorrectly labeling methods as `CompletionItemKind::FUNCTION` instead of `CompletionItemKind::METHOD`, since we're not turning function literal types into bound method types.

Instead of repeating a lot of functionality from type inference here, I wonder if we can simply call `Type::member` for all attribute completions? No matter if it's a class member or an instance member, `Type::member` will do the right thing.

---

_Review comment by @sharkdp on `crates/ty_server/src/server/api/requests/completion.rs`:106 on 2025-07-09 07:05_

A lot of attributes may have dynamic types. Everything that is not annotated, for example. Maybe we should fall back to `VALUE`/`STRUCT` here for most of the variants in this match arm?

---

_@sharkdp reviewed on 2025-07-09 07:05_

---

_@sharkdp reviewed on 2025-07-09 07:08_

---

_Review comment by @sharkdp on `crates/ty_server/src/server/api/requests/completion.rs`:108 on 2025-07-09 07:08_

A decent heuristic might be to go through the elements in the union and to take the kind of the first element that is not `None`?

Similar for intersections, go through all positive elements..?

---

_Review comment by @sharkdp on `crates/ty_server/src/server/api/requests/completion.rs`:90 on 2025-07-09 07:09_

Maybe `INTERFACE` for protocols?

---

_@sharkdp reviewed on 2025-07-09 07:09_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/completion.rs`:91 on 2025-07-09 08:19_

There seems to be a `CompletionItemKind::PROPERTY`, should we use that instead here?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/completion.rs`:113 on 2025-07-09 08:23_

Should these be `FUNCTION` instead?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/server/api/requests/completion.rs`:109 on 2025-07-09 08:28_

Should we use `STRUCT` for these instead?

---

_@dhruvmanila approved on 2025-07-09 08:29_

Looks good! Maybe @AlexWaygood could take a look at the `Type` -> `CompletionItemKind` mapping?

---

_@sharkdp reviewed on 2025-07-09 08:48_

---

_Review comment by @sharkdp on `crates/ty_server/src/server/api/requests/completion.rs`:91 on 2025-07-09 08:48_

I guess `PROPERTY` was probably meant for something else. It's also extremely unlikely that someone will see something of type `PropertyInstance`, as you would normally see it's resolved `__get__` return type.

---

_Comment by @AlexWaygood on 2025-07-09 09:05_

> Looks good! Maybe @AlexWaygood could take a look at the `Type` -> `CompletionItemKind` mapping?

I see the list of possible kinds in the LSP spec, but is there a description of what each `Kind` is meant to represent somewhere? E.g. `Struct` is not a term that would normally apply to Python at all; I don't know that I fully understand what it means for a completion to have a `CompletionItemKind::STRUCT` in the context of ty

---

_Comment by @MichaReiser on 2025-07-09 09:12_

@AlexWaygood I don't think there is. My best guess is that a struct maps to a c# struct which has copy semantics by default, is passed by value, compares equal if its values are equal. This is unlike classes (in c#) that are passed by reference, compare equal if the reference is equal, ... 

---

_Review comment by @AlexWaygood on `crates/ty_server/src/server/api/requests/completion.rs`:99 on 2025-07-09 09:17_

I'm not sure I fully understand what `TEXT` is meant to represent, but my intuition would be something like when you're typing a string literal like this:

```py
"The Eiffel <CURSOR>"
```

and then we would (somehow) autocomplete `Tower` to finish the string, given arbitrary sophistication. I don't know if it makes sense to think of a completion like the following, where we would autocomplete `tribute` to finish the expression `Foo.attribute` as a `TEXT` completion rather than a `VALUE` completion (`Foo.attribute` will be inferred as having type `Literal["X"]` here, a.k.a. `Type::StringLiteral(_)`, so we'll be in this branch of the `match`):

```py
from typing import Final

class Foo:
    attribute: Final = "X"

Foo.at<CURSOR>
```

Relatedly: if a `PlaceAndQualifiers` with _any_ type has the `Final` qualifier, it actually probably make sense to use `CompletionItemKind::CONSTANT`, since the `Place` cannot be reassigned to if it has that qualifier. (`Qualifiers` are not stored on a `Type`, but they are stored on a `PlaceAndQualifiers`, and `Type::member()` returns a `PlaceAndQualifiers`

---

_Review comment by @AlexWaygood on `crates/ty_server/src/server/api/requests/completion.rs`:113 on 2025-07-09 09:21_

`WrapperDescriptor` should be `FUNCTION`, `MethodWrapper` should be `METHOD`

---

_@AlexWaygood reviewed on 2025-07-09 09:21_

---

_@BurntSushi reviewed on 2025-07-09 14:49_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/completion.rs`:106 on 2025-07-09 14:49_

I don't think I have a strong opinion here. I kind of feel like we _shouldn't_ affix a kind if we don't actually know what it is. I guess I would rather the kind be unknown in the common case rather than occasionally wrong? Maybe this is something we leave for now.

EDIT: Changed a "should" to a "shouldn't"

---

_@BurntSushi reviewed on 2025-07-09 14:56_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/completion.rs`:108 on 2025-07-09 14:56_

Aye, done.

---

_@BurntSushi reviewed on 2025-07-09 14:59_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:1232 on 2025-07-09 14:59_

Thank you, this was super helpful.

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/completion.rs`:111 on 2025-07-09 15:00_

Blech. Yeah.

---

_@BurntSushi reviewed on 2025-07-09 15:00_

---

_@AlexWaygood reviewed on 2025-07-09 15:12_

---

_Review comment by @AlexWaygood on `crates/ty_server/src/server/api/requests/completion.rs`:109 on 2025-07-09 15:12_

`SpecialForm` could arguably be `CONSTANT` -- these are special symbols in the `typing` module that are all heavily special-cased by the typing system. They are type constructors, special forms, and type qualifiers that all have unique meaning when it comes to constructing types or annotating variables. You should never reassign them

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/completion.rs`:109 on 2025-07-09 15:16_

I don't know. It's not obvious to me that we should. Like, `KnownInstance` can be a `TypeVar`? Maybe these need to be introspected more.

---

_@BurntSushi reviewed on 2025-07-09 15:16_

---

_@AlexWaygood reviewed on 2025-07-09 15:18_

---

_Review comment by @AlexWaygood on `crates/ty_server/src/server/api/requests/completion.rs`:109 on 2025-07-09 15:18_

Yeah, if it's a `KnownInstanceType::TypeVar` it should probably be `TypeParameter`

---

_@BurntSushi reviewed on 2025-07-09 15:20_

---

_Review comment by @BurntSushi on `crates/ty_server/src/server/api/requests/completion.rs`:99 on 2025-07-09 15:20_

I change this to `VALUE`. I'm partially convinced by your argument.

As for `CONSTANT`, I added your idea to my notes for a future improvement. :-)

---

_Comment by @BurntSushi on 2025-07-09 15:23_

w.r.t. to `STRUCT`, my interpretation of that is basically maximally permissive: "product type." That's how Rust uses it at least, so there's some precedent. We can certainly change this in response to user feedback.

---

_@AlexWaygood approved on 2025-07-09 15:27_

---

_Review requested from @sharkdp by @BurntSushi on 2025-07-09 15:39_

---

_Merged by @BurntSushi on 2025-07-09 16:03_

---

_Closed by @BurntSushi on 2025-07-09 16:03_

---

_Branch deleted on 2025-07-09 16:03_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:271 on 2025-07-10 09:04_

I think this is fine for instances now, but note that something similar happens for accessing attributes on the class directly as well. Whenever an attribute is available on the *metaclass*, the descriptor protocol is invoked.

Attribute access always works like this: if the attribute is available on the *meta type* (for instances: the class, for classes: the metaclass), the descriptor protocol is invoked. If the attribute is available on the type itself, the descriptor protocol is not invoked.

To illustrate: https://play.ty.dev/2f98105c-1f02-4e16-89dd-79fa8b82b462

There's an additional complication if the attribute is accessible on both the meta type and the type itself. Which of these takes precedence depends on the nature of the descriptor (data descriptor vs non-data descriptor).

This is why I suggested elsewhere that we might be best off by calling `Type::member` for all attribute candidates? This would not just guarantee that we invoke the descriptor protocol if necessary. It would also take care of precedence, and handle unions/intersections, as well as possible-unboundness of attributes correctly (by building union types of result types from different stages of the descriptor protocol).

If you're looking for a test case that would show that something is not yet handled correctly, I guess the following metaclass example would work:

```py
class Meta(type):
    @property
    def meta_attr(self) -> int:
        return 0

class C(metaclass=Meta): ...

# accessing `meta_attr` on `C` should be `int`
```

---

_@sharkdp reviewed on 2025-07-10 09:04_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/src/types/ide_support.rs`:271 on 2025-07-11 14:48_

PR up with an attempt to fix this: https://github.com/astral-sh/ruff/pull/19286

---

_@BurntSushi reviewed on 2025-07-11 14:48_

---
