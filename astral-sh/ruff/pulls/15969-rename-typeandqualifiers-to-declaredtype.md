```yaml
number: 15969
title: "Rename `TypeAndQualifiers` to `DeclaredType`"
type: pull_request
state: closed
author: dhruvmanila
labels:
  - ty
assignees: []
draft: true
base: main
head: dhruv/rename-type-and-qualifiers
created_at: 2025-02-05T13:36:45Z
updated_at: 2025-02-14T09:58:54Z
url: https://github.com/astral-sh/ruff/pull/15969
synced_at: 2026-01-10T19:57:22Z
```

# Rename `TypeAndQualifiers` to `DeclaredType`

---

_Pull request opened by @dhruvmanila on 2025-02-05 13:36_

## Summary

This PR renames `TypeAndQualifiers` to `DeclaredType`.

The main motivation for this rename is that in #15848 I need to pass the `ReExport` information around and `TypeAndQualifiers` as a name is limited to only include the `TypeQualifiers`. The re-export information is relevant only for a declared type.

Another potential name that I thought of was `TypeWithMetadata` which doesn't tie this with using it only in the declaration sites.


---

_Label `red-knot` added by @dhruvmanila on 2025-02-05 13:36_

---

_Review requested from @carljm by @dhruvmanila on 2025-02-05 13:36_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-05 13:36_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-02-05 13:36_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-02-05 13:36_

---

_Comment by @AlexWaygood on 2025-02-05 14:00_

Hmm -- I'm not sure that type qualifiers will always _only_ be relevant for declared types. We hope to at some point support "implicit class variables" -- there's some TODOs in our tests, such as here:

https://github.com/astral-sh/ruff/blob/c69b19fe1ddcd3cc57a5fa7f0541037465c277b7/crates/red_knot_python_semantic/resources/mdtest/attributes.md?plain=1#L499-L514

@sharkdp might have some more context on whether the plan is to support these in a similar way to our current support for explicit `ClassVar`s or not

---

_Comment by @sharkdp on 2025-02-05 16:03_

> @sharkdp might have some more context on whether the plan is to support these in a similar way to our current support for explicit `ClassVar`s or not

Hm, yes. I'm also not sure if we'll use (a potentially renamed) `TypeAndQualifiers` for this.

I don't have strong feelings about the name of this struct, but `DeclaredType` seems a bit too narrow for me. The declared *type* is pretty much what is stored in the `inner: Type<…>` field. Maybe `AnnotatedType`? Following the distinction between type expressions and annotations expressions in the [syntax](https://typing.readthedocs.io/en/latest/spec/annotations.html#type-and-annotation-expressions)?

---

_Comment by @dhruvmanila on 2025-02-06 04:46_

> I don't have strong feelings about the name of this struct, but `DeclaredType` seems a bit too narrow for me. The declared _type_ is pretty much what is stored in the `inner: Type<…>` field. Maybe `AnnotatedType`? Following the distinction between type expressions and annotations expressions in the [syntax](https://typing.readthedocs.io/en/latest/spec/annotations.html#type-and-annotation-expressions)?

Yeah, I think that makes sense. Although, I've realized that I might need to pass the `ReExport` information via the bindings as well i.e., augment the binding `Type` with additional information. I'm going to put this on hold until that is finalized.

---

_Converted to draft by @dhruvmanila on 2025-02-06 04:46_

---

_Comment by @dhruvmanila on 2025-02-14 09:58_

I'm going to close this for now although I do like `AnnotatedType` but I think we should wait until we have some more clarity on what would be a good name there.

---

_Closed by @dhruvmanila on 2025-02-14 09:58_

---
