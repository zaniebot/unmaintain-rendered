```yaml
number: 17525
title: "[red-knot] Move `InstanceType` to its own submodule"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/move-instancetype
created_at: 2025-04-21T16:09:25Z
updated_at: 2025-04-22T11:34:47Z
url: https://github.com/astral-sh/ruff/pull/17525
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Move `InstanceType` to its own submodule

---

_@AlexWaygood_

## Summary

This PR moves red-knot's `InstanceType` struct out of `class.rs` and into its own submodule. The `InstanceType::class` field is made private; the _only_ way of creating `InstanceType`s is now through the `Type::instance()` method (which is also moved to the new `instance.rs` submodule).

The motivation for this change is that it helps pave the way for adding a new `Type::ProtocolInstance` variant. When we have such a variant, I'm envisaging that the `Type::instance()` method would change to look something like this:

```rs
impl<'db> Type<'db> {
    pub fn instance(db: &'db dyn Db, class: ClassType<'db>) -> Self {
        if class.is_protocol(db) {
            Self::ProtocolInstance(ProtocolInstanceType { class })
        } else {
            Self::Instance(InstanceType { class })
        }
    }
}
```

Since the `Type::instance()` method would be the only public constructor for both `InstanceType` and `ProtocolInstanceType`, this would guarantee the invariant that nominal instance types would always be represented in our model using `Type::Instance` variants, and `Protocol` instance types would always be represented using `Type::ProtocolInstance` variants.

I also moved the `KnownInstanceType` enum to its own submodule in this PR, as it has very little to do with the other types in `class.rs`, and has equally little to do with either `InstanceType` or the `ProtocolInstanceType` that live (/will live) in `instance.rs`.

This PR is best reviewed commit by commit! I tried to include helpful commit messages explaining what each commit does.

## Test Plan

`cargo test -p red_knot_python_semantic`


---

_Label `red-knot` added by @AlexWaygood on 2025-04-21 16:09_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-21 16:09_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-21 16:09_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-21 16:09_

---

_Comment by @github-actions[bot] on 2025-04-21 16:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-04-21 16:27_

Changes here look fine to me either way. I'm curious to learn more about the pros and cons of a separate `Type::ProtocolInstance` vs reusing `Type::Instance`.

---

_Comment by @dcreager on 2025-04-21 17:16_

> Changes here look fine to me either way. I'm curious to learn more about the pros and cons of a separate `Type::ProtocolInstance` vs reusing `Type::Instance`.

To make sure I understand the alternative you're suggesting: `InstanceType` would become an enum, and `ProtocolInstance` would live there instead of in `Type`?

---

_Comment by @carljm on 2025-04-21 17:29_

> To make sure I understand the alternative you're suggesting: `InstanceType` would become an enum, and `ProtocolInstance` would live there instead of in `Type`?

No, I was asking why we need an enum or `ProtocolInstance` at any level. It seems possible in principle for protocols to live entirely within `Type::Instance` and `InstanceType`, and differentiate internally based on `class.is_protocol()` of the wrapped class.

I do think there are likely advantages in clarity (and maybe even in performance) to having a separate variant, because it means we don't have to check `is_protocol()` as often, and because the behavior of many methods will be entirely different for protocols vs other instances, and because semantically protocol types are a very different thing from instance types. So I'm not trying to say it would be better not to have a separate variant. I'm just interested in whether there are reasons I'm not seeing why it is _necessary_.

---

_Review request for @sharkdp removed by @sharkdp on 2025-04-22 07:03_

---

_Comment by @AlexWaygood on 2025-04-22 11:30_

> I'm curious to learn more about the pros and cons of a separate `Type::ProtocolInstance` vs reusing `Type::Instance`.

Ah, sorry. Perhaps I should have written up a short design doc for this. I remember discussing this plan with you in a 1:1, but that was probably a couple of weeks ago by now, and that anyway wasn't a conversation that was visible to the rest of the team :-(

The first reason why I think it makes sense for `ProtocolInstance` to be a separate variant is just that it will be much less bug-prone than trying to combine nominal and structural subtyping in one variant. Protocol instances will have fundamentally different logic to nominal instances in many of our core type-relational methods such as `Type::is_subtype_of`, `Type::is_assignable_to`, `Type::is_equivalent_to` and `Type::is_gradual_equivalent_to`. We saw in https://github.com/astral-sh/ruff/pull/17126 that there were several bugs in our core type-relational methods to do with the four types nested under `Type::Callable`, mostly to do with the fact that we were often treating the four types as though they were the same when they were not. Nesting the types in the same variant made these mistakes easy to make and hard to spot: some of the bugs I fixed in that PR I knew about before I started working on the PR, but most of them I only discovered through the process of flattening out the nested type into four `Type` variants. I think having a separate `ProtocolInstance` variant from the start will be a sounder foundation on which to build structural subtyping.

A second reason is that, as we've discussed in the past, I think we will have to support synthesized protocols as well as protocols that point to a specific class definition. This will be important for normalizing protocols into types that share the same Salsa ID if they are in fact equivalent to each other, which is necessary for our implementation of `Type::is_equivalent_to` for union types. I.e., we need to ensure that this test eventually passes:

https://github.com/astral-sh/ruff/blob/d2b20f736789ab954445604546e25562e3906a19/crates/red_knot_python_semantic/resources/mdtest/protocols.md?plain=1#L650-L664

So I'm imagining that the `Type::ProtocolInstance` variant will wrap a type that looks something like this:

```rs
enum ProtocolInstanceType<'db> {
    FromClass(ClassType<'db>),
    Synthesized(SynthesizedProtocolType<'db>),
}

#[salsa::tracked]
struct SynthesizedProtocolType<'db> {
    members: Box<[ProtocolMember<'db>]>,
}
```

As we've also discussed in the past, this also has the advantage that we will in the future be able to consider removing the `Type::Callable` variant and instead represent `Callable` types as synthesized protocols with a single member (`__call__`).

---

_Merged by @AlexWaygood on 2025-04-22 11:34_

---

_Closed by @AlexWaygood on 2025-04-22 11:34_

---

_Branch deleted on 2025-04-22 11:34_

---
