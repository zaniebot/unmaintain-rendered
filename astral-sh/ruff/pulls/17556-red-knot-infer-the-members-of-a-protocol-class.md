```yaml
number: 17556
title: "[red-knot] Infer the members of a protocol class"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/protocol-member-inference
created_at: 2025-04-22T16:40:01Z
updated_at: 2025-04-23T21:37:29Z
url: https://github.com/astral-sh/ruff/pull/17556
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Infer the members of a protocol class

---

_@AlexWaygood_

## Summary

This PR adds the necessary machinery to infer the members of a protocol class, and uses that machinery to infer a precise return type for the `get_protocol_members` introspection function.

What this PR _doesn't_ do is infer what the _type_ of each protocol member is. We'll obviously have to do that in due course to properly implement protocol subtyping and assignability; when we add that functionality, I'm envisaging that `ProtocolClassLiteral::members()` would return a `HashMap` mapping from the member to its type rather than a boxed slice of `Name`s. I'm deliberately not doing that right now, however, as implementing all the subtyping rules around protocols is a somewhat large task, which I intend to approach incrementally over several PRs. The first stage will possibly not even consider the types of the protocol members at all.

## Test Plan

Existing mdtests updated.


---

_Label `red-knot` added by @AlexWaygood on 2025-04-22 16:40_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-22 16:40_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-22 16:40_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-22 16:40_

---

_Comment by @github-actions[bot] on 2025-04-22 16:44_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@sharkdp reviewed on 2025-04-22 21:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:1788 on 2025-04-22 21:13_

I don't think this will take visibility into account? I tried something similar for listing members of dataclasses at first, but then decided to iterate over declarations instead. You may want to do something similar here? See `ClassLiteralType::dataclass_fields`/`ClassLiteralType::own_dataclass_fields` and the `all_public_declarations` method I added on the use def map.

---

_@sharkdp reviewed on 2025-04-22 21:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:1788 on 2025-04-22 21:16_

The other advantage is that you'll get the type as well, using `symbol_from_declarations`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:568 on 2025-04-22 23:18_

TODO here also for the use of `Type::Tuple`? I think ideally for maximum clarity to future-us, we have TODOs both on tests that are known-wrong, and by the specific part of the implementation that is known wrong.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1736 on 2025-04-22 23:36_

Keep in mind (both in terms of method naming, and where you put what functionality) that autocomplete is definitely going to require the ability for `ClassLiteralType` itself to have a "list all members" method. And if  `ProtocolClassLiteral` will be used inside a future `Type::ProtocolInstance`, it will probably also need to expose that list-all-members method, without all the Protocol-specific filtering.  That could suggest some future refactoring to share some aspects of list-members functionality, but more concretely I think it suggests that we should reserve the method name `members` for the general "list all known members" that all types will need, and use something like `protocol_members` to distinguish this protocol-specific method.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1788 on 2025-04-22 23:52_

We should probably add a test for a protocol with a member defined under `if False` (or more realistically, `if sys.version_info >= ...` which evaluates to `False`).

---

_@carljm reviewed on 2025-04-22 23:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1736 on 2025-04-23 10:37_

> autocomplete

for some reason it took me a moment there to realise that you mean "the autocompletion feature we intend to build into the red-knot LSP for IDE users" rather than "autocompletion in the IDE now for red-knot developers" 😆

> That could suggest some future refactoring to share some aspects of list-members functionality, but more concretely I think it suggests that we should reserve the method name `members` for the general "list all known members" that all types will need, and use something like `protocol_members` to distinguish this protocol-specific method.

Interesting -- I think they will end up doing _nearly_ the same thing. I suppose the only difference between them really is that the `members` method I'm adding here (which I'll rename to `protocol_members`) should _not_ include implicit instance attributes (these are illegal in protocol classes, and do not constitute protocol members). But the autocomplete feature probably _should_ include those members:

```py
class Foo(Protocol):
    x: int

    def __init__(self):
        self.y = 42  # we should emit an error here,
                     # and not infer `Foo` as having a protocol member `y`
                     # (it should only have a single member, `x`)
```

---

_@AlexWaygood reviewed on 2025-04-23 10:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1788 on 2025-04-23 10:39_

> I don't think this will take visibility into account? I tried something similar for listing members of dataclasses at first, but then decided to iterate over declarations instead.

Great point, thank you!! I did look at the methods you added for dataclasses, but only briefly, as I realised that I would need to include methods, properties and bindings-without-declarations in the list of protocol members, so I realised I wouldn't be able to simply reuse the methods you added for dataclasses. I should have looked more closely!

---

_@AlexWaygood reviewed on 2025-04-23 10:39_

---

_@AlexWaygood reviewed on 2025-04-23 11:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1832 on 2025-04-23 11:52_

in due course this will need to be converted to a `HashMap` of some kind where the keys are the names of the protocol members and the values are the types of the protocol members. But I don't want to do that yet because I'm not yet sure whether e.g. an attribute member annotated with `Callable` should be treated the same as a method member (I need to write some more tests...)

---

_Review requested from @carljm by @AlexWaygood on 2025-04-23 11:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-23 11:54_

---

_@carljm reviewed on 2025-04-23 19:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1736 on 2025-04-23 19:01_

Right, makes sense. It seems like also the filtering-out of some "special" names is something we might not want to do for autocomplete?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1788 on 2025-04-23 19:22_

`cached_protocol_members`?

Not that it matters for namespacing since it's nested, but it may be clearer when it shows up in Salsa query traces.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1827 on 2025-04-23 19:29_

I guess this is necessary because you want to include undeclared bindings on the class, even though they are an error?

---

_@carljm approved on 2025-04-23 19:29_

Looks great!

---

_@AlexWaygood reviewed on 2025-04-23 19:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1827 on 2025-04-23 19:35_

Yes, exactly! Even though we might consider such members as being invalid, it's important that we nonetheless include them in the list of members or we could infer a class `X` as being a subtype of a protocol class `Y` even though `issubclass(X, Y)` evaluates to `False` at runtime. And that would obviously lead to incorrect inferences from us if a user was employing `issubclass()` or `isinstance()` to do type narrowing.

I'll add a comment!

---

_Closed by @AlexWaygood on 2025-04-23 21:32_

---

_Reopened by @AlexWaygood on 2025-04-23 21:32_

---

_Merged by @AlexWaygood on 2025-04-23 21:36_

---

_Closed by @AlexWaygood on 2025-04-23 21:36_

---

_Branch deleted on 2025-04-23 21:36_

---
