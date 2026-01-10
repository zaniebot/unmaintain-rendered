```yaml
number: 19758
title: "[ty] Fix attribute access on `TypedDict`s"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/typeddict-attributes
created_at: 2025-08-05T10:10:52Z
updated_at: 2025-08-05T11:59:11Z
url: https://github.com/astral-sh/ruff/pull/19758
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Fix attribute access on `TypedDict`s

---

_Pull request opened by @sharkdp on 2025-08-05 10:10_

## Summary

This PR fixes a few inaccuracies in attribute access on `TypedDict`s. It also changes the return type of `type(person)` to `type[dict[str, object]]` if `person: Person` is an inhabitant of a `TypedDict`  `Person`. We still use `type[Person]` as the *meta type* of Person, however (see reasoning [here](https://github.com/astral-sh/ruff/pull/19733#discussion_r2253297926)).

## Test Plan

Updated Markdown tests.

---

_Label `ty` added by @sharkdp on 2025-08-05 10:10_

---

_Comment by @github-actions[bot] on 2025-08-05 10:12_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-05 10:14_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_@sharkdp reviewed on 2025-08-05 10:32_

---

_Review comment by @sharkdp on `crates/ty_ide/src/completion.rs`:1257 on 2025-08-05 10:32_

These were just (slightly) wrong before. FYI @BurntSushi.

---

_@sharkdp reviewed on 2025-08-05 10:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/ide_support.rs`:329 on 2025-08-05 10:36_

This is a small bugfix. Instead of trying to access the member on instances of the parent class, just access them on the type that we're adding completions for — similar to how it's done in the method above. If we don't do this, the descriptor protocol will be invoked with the wrong instance type (which is why we see the changes in bound methods).

It was also possible to change this for `TypedDict`, because "instances"/inhabitants of `TypedDict`-based classes are not represented by `Type::NominalInstance`, but `Type::TypedDict`.

---

_Marked ready for review by @sharkdp on 2025-08-05 10:37_

---

_Review requested from @carljm by @sharkdp on 2025-08-05 10:37_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-08-05 10:37_

---

_Review requested from @dcreager by @sharkdp on 2025-08-05 10:37_

---

_Review requested from @MichaReiser by @sharkdp on 2025-08-05 10:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:220 on 2025-08-05 11:10_

what you have is accurate as-is, but this might be clearer for a first-time reader:

```suggestion
But they *can* be accessed on `type[Person]`, because this function would accept the class object
`Person` as an argument:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/typed_dict.md`:228 on 2025-08-05 11:10_

```suggestion
def accepts_typed_dict_class(t_person: type[Person]) -> None:
    reveal_type(t_person.__total__)  # revealed: bool
    reveal_type(t_person.__required_keys__)  # revealed: frozenset[str]
    reveal_type(t_person.__optional_keys__)  # revealed: frozenset[str]

accepts_typed_dict_class(Person)
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:672 on 2025-08-05 11:12_

nit: I think we should avoid making things `pub` wherever possible, as Clippy doesn't warn us about unused code if something is `pub`. (I know there are lots of `Type` methods that violate this general rule, and ideally we'd clean them up IMO, but it's also probably not that important to do so.)

```suggestion
    pub(crate) const fn is_typed_dict(&self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5623 on 2025-08-05 11:15_

Shouldn't this be a class-literal type rather than a subclass-of type? Similar to the way we know that an int-literal must be an instance of exactly `int` (not a subclass of `int`), it's invalid for a `TypedDict` inhabitant to be an instance of a `dict` subclass -- it must be an instance of exactly `dict`, I think? This is different to `NominalInstance` types, which is why we use `SubclassOf` types as the meta-types for most instance types.

---

_@AlexWaygood reviewed on 2025-08-05 11:16_

---

_@sharkdp reviewed on 2025-08-05 11:33_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:5623 on 2025-08-05 11:33_

> Shouldn't this be a class-literal type rather than a subclass-of type?

This is what I had at first. Then I noticed that `type({})` is `type[dict[Unknown, Unknown]]` and I tried to model that similarly.

> it's invalid for a `TypedDict` inhabitant to be an instance of a `dict` subclass -- it must be an instance of exactly `dict`

Yes, I think that's right. Will change back to `<class 'dict[str, object]'>`.

---

_@AlexWaygood reviewed on 2025-08-05 11:40_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5623 on 2025-08-05 11:40_

> This is what I had at first. Then I noticed that `type({})` is `type[dict[Unknown, Unknown]]` and I tried to model that similarly.

yeah, the apparent inconsistency here is due to the fact that with `x = {}`, we infer the type of `x` as `dict[Unknown, Unknown]`, which is "lossy" -- looking at the source code, we know that it is (for now) an instance of _exactly_ `dict`, but this is not recorded in the type we infer: the type we infer allows for the possibility that `x` could be an instance of a subclass of `dict`. That lossiness in the type then means that the only safe type we can infer for `type(x)` here is `type[dict[Unknown, Unknown]]` rather than `<class 'dict[Unknown, Unknown]'>`

And the "lossiness" of the type we infer for `x` there is sort-of deliberate: you usually want to be able to substitute a subclass of `dict` wherever a `dict` is expected! But for `TypedDict` types, that's not allowed, so we can use a more precise type for `type()` calls on `TypedDict` inhabitants.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5621 on 2025-08-05 11:43_

this `.expect()` call feels like it could fail if somebody used a custom typeshed with a silly `dict` definition -- `.unwrap_or_else(Type::unknown)` might be safer?

---

_@AlexWaygood approved on 2025-08-05 11:43_

Thank you!

---

_Merged by @sharkdp on 2025-08-05 11:59_

---

_Closed by @sharkdp on 2025-08-05 11:59_

---

_Branch deleted on 2025-08-05 11:59_

---
