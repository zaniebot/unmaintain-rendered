```yaml
number: 13878
title: "[red-knot] Add internal documentation on kinds of types"
type: pull_request
state: closed
author: AlexWaygood
labels:
  - documentation
  - internal
  - ty
assignees: []
base: main
head: redknot-internal-docs
created_at: 2024-10-22T12:17:56Z
updated_at: 2026-01-07T13:55:08Z
url: https://github.com/astral-sh/ruff/pull/13878
synced_at: 2026-01-10T16:30:32Z
```

# [red-knot] Add internal documentation on kinds of types

---

_Pull request opened by @AlexWaygood on 2024-10-22 12:17_

## Summary

This PR adds purely internal documentation to assist us, as red-knot developers. By having a common reference with precise definitions for certain terms, there's a reduced risk of misunderstanding.

## Test Plan

`pre-commit run -a`


---

_Label `documentation` added by @AlexWaygood on 2024-10-22 12:17_

---

_Label `internal` added by @AlexWaygood on 2024-10-22 12:17_

---

_Label `red-knot` added by @AlexWaygood on 2024-10-22 12:17_

---

_Review requested from @sharkdp by @AlexWaygood on 2024-10-22 12:17_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-10-22 12:17_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-22 12:17_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-22 12:17_

---

_@sharkdp reviewed on 2024-10-22 12:24_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/docs/kind_of_types.md`:79 on 2024-10-22 12:24_

… and `Never`.

---

_@AlexWaygood reviewed on 2024-10-22 12:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:79 on 2024-10-22 12:25_

🙃

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/docs/kind_of_types.md`:109 on 2024-10-22 12:32_

One thing that could be clarified here: Is `Never` a single-value type or not? The way I implemented `is_single_valued()`, `Never` is excluded. So that definition would have to include something like "A single-value type is a non-empty type for which …"

---

_@sharkdp reviewed on 2024-10-22 12:32_

---

_@sharkdp reviewed on 2024-10-22 12:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/docs/kind_of_types.md`:151 on 2024-10-22 12:33_

Sorry for being pedantic: "except for the type itself and the empty type".

---

_@AlexWaygood reviewed on 2024-10-22 12:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:151 on 2024-10-22 12:34_

Already fixed this in my latest push, I think :-)

---

_@AlexWaygood reviewed on 2024-10-22 12:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:151 on 2024-10-22 12:34_

Also this is exactly the kind of PR where pedantry is most important haha

---

_Review requested from @sharkdp by @AlexWaygood on 2024-10-22 12:52_

---

_Comment by @AlexWaygood on 2024-10-22 13:10_

I wondered whether these docs would be better placed somewhere like https://typing.readthedocs.io/en/latest/. But I'm not sure it would be a great fit for that site, because:
- Some of the concepts for our internal model of Python's types are not standardised as part of the spec. For example, our concepts of "class-literal" and "function-literal" singleton types are not something that all Python type checkers have
- It's not clear that these terms are relevant to either the typing spec or the user-facing documentation parts of https://typing.readthedocs.io/en/latest/

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:53 on 2024-10-22 14:58_

I get what you're aiming to say here, but I don't think this is true. If we have, for instance, a sealed enum type `E`, with members `E.A`, `E.B`, and `E.C`, then the type `Literal[E.A] | Literal[E.B]` is a proper subtype of `E`, and so is the type `Literal[E.B] | Literal[E.C]`, but those two proper subtypes are not disjoint.

I'm not sure how to say what you're trying to say here, other than to say that the sealed type consists of the union of the literal types for all its known inhabitants, which kind of feels like just repeating the definition of a sealed type?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:65 on 2024-10-22 15:01_

This is true for `bool`, because it only has two inhabitants, so we can't construct any non-disjoint union types of its inhabitants.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:82 on 2024-10-22 15:24_

This is not true, because it fails to consider unions of inhabitants, which are also proper subtypes of `Foo`. But I think if you just replace "proper subtype" with "inhabitant", leave out Never, and don't mention disjointness (since individual objects are by definition "disjoint" from each other), you can reach the same conclusion below (regarding how a negated intersection with the overall type is equivalent to a union of literal types for its inhabitants), which is the important part.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:79 on 2024-10-22 15:24_

Here you mention only `X` and `Y`, but below you also mention `Z`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:155 on 2024-10-22 15:48_

I think this wording has a similar problem with arbitrary intersection and union types as the above wording on sealed types.

I think we have to present closed types in a way that's more tied to "instance" types -- as in, it's a type defined as instances of a class, which cannot be subclassed, therefore all inhabitants of the type must be instances of the final class.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 15:50_

I don't think this paragraph is right. I don't think "has no proper subtypes other than itself and Never" is a workable or useful definition of what it means to be a closed type, and I think both `bool` and `Eggs` should be considered closed types.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:109 on 2024-10-22 15:52_

It might be considered implied by "equivalence", but I think we should be explicit from the beginning that we are talking here about equivalence with respect to Python equality as defined by `__eq__` and `__ne__` implementations. "Equivalent with respect to their runtime value" is less clear what exactly we are talking about.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:123 on 2024-10-22 15:55_

The definition of a single-value type is specific to the Python equality relation, so it seems odd to say "there must exist a ... relation" here.

I would say instead that a single-value type must have a reflexive, symmetric, and transitive definition of Python equality.

But really I kind of think this entire paragraph might not be necessary; I think the literal-type examples you give above are probably sufficient to clarify what a real single-value type looks like. In practice we are never going to try to define any single-value type on a non-builtin class, or any class that might challenge the requirements.

---

_@carljm requested changes on 2024-10-22 15:56_

Thanks for writing this up! The core ideas look great, I think there are a few issues in some wordings.

---

_@AlexWaygood reviewed on 2024-10-22 16:00_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:53 on 2024-10-22 16:00_

Great point, thanks. I think the best solution is just to remove this paragraph.

---

_@AlexWaygood reviewed on 2024-10-22 16:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:109 on 2024-10-22 16:16_

The introductory paragraph at the top of the document states:

> This document provides definitions for various kinds of types, with examples of Python types that satisfy these definitions.

I'm _trying_ to present generalised definitions in the first 1-2 sentences of each section, that would apply outside of the specific context of Python, then in later sentences expand on the initial definition and link it back to the concept of Python types. So I would _prefer_ not to mention `__eq__` or `__ne__` in the first sentence here, as this is very specific to the way equivalence between arbitrary objects is generally implemented in Python.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:123 on 2024-10-22 16:18_

Well, you could theorise that there could exist other single-value types in Python other than the ones we know about; they just wouldn't be recognised as such by red-knot.

> In practice we are never going to try to define any single-value type on a non-builtin class

I don't think this is true. `Literal[E.A]`, for an enum `E` with a member `A`, is a single-value type (as a consequence of being a singleton type).

---

_@AlexWaygood reviewed on 2024-10-22 16:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 16:25_

What _is_ the term you would use for a type where it is known that no proper subtypes exist other than `Never`?

---

_@AlexWaygood reviewed on 2024-10-22 16:25_

---

_@carljm reviewed on 2024-10-22 16:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:123 on 2024-10-22 16:28_

> `Literal[E.A]`, for an enum `E` with a member `A`, is a single-value type.

Fair! And if we observe a custom `__eq__` implementation on an `Enum` subclass, we probably have to stop treating literal types of those enum members as single-valued types, because we no longer can claim to understand the equality relation for instances of that enum type.

---

_@carljm reviewed on 2024-10-22 16:35_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:109 on 2024-10-22 16:35_

I didn't realize you were aiming to initially present a definition of each type abstracted from the Python context. I'm not sure how concretely valuable that is, since this is documentation for red-knot, which is a Python type-checker. And I think it's a bit challenging for "single-valued type" specifically, because what we mean by the term in red-knot _is_ specific to the relation defined by `__eq__` and `__ne__`. It's certainly possible, though.

---

_@carljm reviewed on 2024-10-22 16:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:109 on 2024-10-22 16:38_

I think the core issue here is that the phrase "equivalent with respect to their runtime value" doesn't have any clear meaning in an abstract type system sense. So if you want to present an abstract definition, you'll have to start by saying "Assuming some equivalence relation is defined on type inhabitants..." and go from there.

(You can probably also skip discussion of reflexivity, transitivity, and symmetry, and relegate that to a footnoted link to e.g. https://en.wikipedia.org/wiki/Equivalence_relation, if you are using the term "equivalence relation", since an equivalence relation is defined to require all of those properties.)

---

_@AlexWaygood reviewed on 2024-10-22 16:40_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:109 on 2024-10-22 16:40_

> I didn't realize you were aiming to initially present a definition of each type abstracted from the Python context. I'm not sure how concretely valuable that is, since this is documentation for red-knot, which is a Python type-checker.

It's not documentation for _users_ of red-knot though; it's documentation for red-knot developers to help them (us) think about type-system concepts. I personally find it helpful to consider these concepts in a more abstract way initially, before then linking the concepts back to the specific context of Python :-)

---

_@carljm reviewed on 2024-10-22 16:41_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 16:41_

I think that would have to be a singleton type. Any type with more than one inhabitant in principle has proper subtypes that are not `Never`.

---

_@AlexWaygood reviewed on 2024-10-22 16:43_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 16:43_

`Literal[50000000000000000]` is not a singleton type (multiple inhabitants exist at runtime), but has no non-`Never` proper subtypes

---

_@carljm reviewed on 2024-10-22 16:49_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:114 on 2024-10-22 16:49_

I would also suggest avoiding a description like "entirely fungible", because it is not well-defined. Whether the inhabitants are fungible or not is a matter of perspective and what you're doing with them! If you are doing `is` comparisons, they are not fungible. It's better if we state things precisely in terms of some equivalence relation: rather than "entirely fungible and equal", the type inhabitants are "all in the same equivalence class according to some equivalence relation" (the Python equality relation, in our specific case.)

---

_@AlexWaygood reviewed on 2024-10-22 16:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:114 on 2024-10-22 16:51_

Sad -- there are so few situations in which you get to use the word "fungible"; I really thought this was one of them

---

_@AlexWaygood reviewed on 2024-10-22 16:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:114 on 2024-10-22 16:53_

(While I concede that you're pedantically correct, I _do_ struggle to see what you might be doing with numbers such that one `50000000000000000` `int` object would not be fungible with another `50000000000000000` `int` object 😄)

---

_@carljm reviewed on 2024-10-22 16:55_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:114 on 2024-10-22 16:55_

Only things you shouldn't be doing with them, for sure! The intent of the language is absolutely that they should be fungible. 

---

_@carljm reviewed on 2024-10-22 16:57_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 16:57_

None that we have (or ever would) define or attempt to name, but sure, in principle you can have the singleton type consisting only of the integer object with value `50000000000000000` that exists at a certain memory address, which is a proper subtype of `Literal[50000000000000000]` that is not `Never`.

---

_@AlexWaygood reviewed on 2024-10-22 16:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 16:59_

grrr, fine!

---

_@AlexWaygood reviewed on 2024-10-22 17:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 17:07_

In some ways I find this a very useful clarification: it's good to remember that the set of Python types recognised by red-knot is a subset of the set of possible types that exist in Python. From this perspective, there really exists an infinite number of possible singleton types in Python.

In other ways, I find this not so useful, because I still feel like we're missing a term to describe types which _in red-knot's model_ can have no possible non-`Never` proper subtypes _that would be understood as such by red-knot_. You've successfully convinced me that this is a concept that cannot be expressed in terms of an abstract framework divorced from the concept of Python types and our internal model of Python types. But I still think it's a useful concept nonetheless and something we should have a term for.

---

_@carljm reviewed on 2024-10-22 18:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 18:23_

I think I'm not understanding _why_ this is a useful or important concept.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:77 on 2024-10-22 18:26_

This phrasing feels slightly off to me. It's not about whether red-knot "recognizes them as singleton types" -- it's not as if red-knot recognizes them as some other kind of type instead! It's just that they are types for which red-knot has no internal representation (nor is there any way to spell them in a type expression.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:124 on 2024-10-22 18:27_

```suggestion
for any given sealed type `X` where the only inhabitants of `X` are `A`, `B` and `C`,
```

---

_@AlexWaygood reviewed on 2024-10-22 18:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 18:28_

It is what I was _trying_ to describe... but perhaps you're right, maybe it just isn't actually a useful thing to describe. Not sure...

Anyway, I'm reworking the section titled closed types to more accurately describe the actual type-theory concept described by the term "closed types" :-) will probably finish off that rewrite tomorrow morning and push those changes.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:153 on 2024-10-22 18:29_

This seems to leave out the key third requirement: that the inhabitants of the type are all equal to each other according to the `__eq__` and `__ne__` implementations of that class. It was stated above in the abstract form, but for clarity it seems worth saying it again in the specific form.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-22 18:36_

Ok, looking forward to it!

---

_@carljm reviewed on 2024-10-22 18:36_

---

_@AlexWaygood reviewed on 2024-10-22 18:58_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:124 on 2024-10-22 18:58_

No, I think that would make the sentence inaccurate. Your phrasing is true for the specific case of enum members, which I use as an example in the sentences following this, but is not true in the abstract, and this sentence describes an abstract relation. Here I theorise a hypothetical sealed type where there are exactly three proper subtypes and describe how such a sealed type might work.

I think it's possibly confusing that I'm using the same letters in my abstract description of the relation as I then use in my enum example immediately below, since the enum example does not in fact correspond exactly to the abstract relation I'm describing here. (The enum type in the example has more than three subtypes.)

---

_@carljm reviewed on 2024-10-22 19:01_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:123 on 2024-10-22 19:01_

It's not clear to me what the importance is here of the description "equivalent to the union of all of its proper subtypes" -- that describes every type! Not every type has a finite and enumerable set of proper subtypes, but _every_ type is equivalent to the union of all its proper subtypes.

---

_@carljm reviewed on 2024-10-22 19:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:124 on 2024-10-22 19:04_

Ah, I see, makes sense.

I still have the feeling (commented above) that the emphasis here isn't quite on the right characteristics. I'm not sure if there's any value added by discussing or enumerating proper subtypes in this section at all. The important thing is that the type's _inhabitants_ are finite and enumerable, and therefore if by intersection we eliminate some subset of inhabitants, we are able to simplify the representation to a type consisting of the remainder of the inhabitants

---

_@AlexWaygood reviewed on 2024-10-22 19:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:124 on 2024-10-22 19:09_

Hmm, I agree that the enumerability of the inhabitants is the more important characteristic, but I find both characteristics to be useful things to bear in mind. Still, I'll take a look tomorrow and see if I can improve the emphasis

---

_@sharkdp reviewed on 2024-10-23 08:12_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-23 08:12_

> > `Literal[50000000000000000]` is not a singleton type (multiple inhabitants exist at runtime), but has no non-`Never` proper subtypes
>
> None that we have (or ever would) define or attempt to name, but sure, in principle you can have the singleton type consisting only of the integer object with value `50000000000000000` that exists at a certain memory address, which is a proper subtype of `Literal[50000000000000000]` that is not `Never`.

This is purely a discussion about nomenclature, so I'm not sure how useful it is to go deeper, but I'm not sure I completely agree with this *"every possible set of runtime values corresponds to *some* type, even if we can't name/construct it"* viewpoint. I think the following definition is more useful: A type is a syntactic form that we assign to terms of the programming language. In this definition, it's still true that every type represents a set of possible runtime values. But it's not true that every possible set of runtime values corresponds to a type. *Which* particular sets of runtime values can be represented as a type depends on the type system and its expressiveness. In C's type system, you can not assign a type to the set `{true}`. In Python, you can; In Python, you can not assign a type to objects that are used exactly once. In languages with linear types, you can.

In Python's type system, there is no way to construct a type that represents all objects at memory location 0xf7…, and I don't think it's useful to think of types in terms of sets of runtimes values that could *potentially* be represented by a more expressive type system. I would not claim that C has intersection types, just because I can think of a set of runtime values that would be represented by `int8_t & uint8_t = {0, 1, …, 127}`. And I would not claim that Python has linear types, just because I can think of the set of runtime objects that are used exactly once.

---

_@dhruvmanila reviewed on 2024-10-23 08:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/docs/kind_of_types.md`:13 on 2024-10-23 08:48_

nit: it might be useful to have some definition of the word "inhabitant" here

---

_@AlexWaygood reviewed on 2024-10-23 09:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-23 09:32_

> This is purely a discussion about nomenclature

Hey, the whole PR is about nomenclature 😆

One complication here is that red-knot's model of Python's types _is_ more expressive than the model outlined by the typing spec. (In fact, you could argue that all Python type checkers necessarily have a more expressive model than the one outlined in the typing spec -- but I think red-knot goes further than most here.) One obvious example is our concept of function-literal and class-literal types -- you _could_ argue that the concept of class-literal types in particular is strongly implied by much of the rest of the typing spec, but nowhere is it explicitly specified. Similarly, we treat `IntersectionType`s as first-class types and infer them in many places -- but an `Intersection` type is still unspecified in the typing spec. (Mypy, for example, [does narrowing in many places that clearly _implies_ the existence of an intersection type](https://github.com/python/mypy/blob/60d1b3776229d3e3c332413ef49ad89be469a5e4/mypy/checker.py#L5416-L5440), but it has no first-class notion of intersection types in its model.)

So I could put forward at least three possible definitions of "type" here:
- A maximally broad definition: "Any theoretical set of runtime objects constitutes a 'type'"
- A maximally narrow definition: "Only a type explicitly specified by the typing spec can be described as a type"
- A maximally useful definition: "Any set of runtime objects constitutes a type if it would plausibly be useful to represent it as a type in red-knot in order to accurately and precisely model Python's runtime semantics"

I think the maximally useful definition is probably the most, well... useful... but it's also the least precise! It's hard to know which sets of runtime objects we might plausibly find useful to represent as types _at some point_.

---

_@AlexWaygood reviewed on 2024-10-23 09:44_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:155 on 2024-10-23 09:44_

> I think we have to present closed types in a way that's more tied to "instance" types -- as in, it's a type defined as instances of a class, which cannot be subclassed, therefore all inhabitants of the type must be instances of the final class.

I'm not sure we want the concept of "closed" types to be limited to _just_ `@final` nominal types, though. I think we would at least want `TypedDict`s with `closed=True` to qualify (assuming [PEP 728](https://peps.python.org/pep-0728/) is accepted)?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-23 14:49_

Once a type system has full set-theoretic types (arbitrary unions, intersections, and negations), it becomes quite hard to draw a clear line between which possible sets of objects are a type and which are not; many quite complex and precise types can be constructed ad-hoc. Because of this, and because of what Alex says (which sets of objects we choose to represent as a type internally is not set in stone by either the language or the typing spec, but is essentially up to our own judgment of what is useful for representing Python semantics, and may change over time), I think the clearest approach in our terminology is to avoid defining types based on an assumption that no proper subtype of the type could ever exist, despite the type having multiple distinguishable inhabitants.

(But perhaps more important to the original intent of this thread is that I also don't find that definition _otherwise_ useful or correct in this case: I do think that `bool` and `Eggs` are closed types.)

---

_@carljm reviewed on 2024-10-23 14:49_

---

_@AlexWaygood reviewed on 2024-10-23 14:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:182 on 2024-10-23 14:54_

> I do think that `bool` and `Eggs` are closed types

Yeah, you're right that I was trying to use the word "closed" to describe a different concept to what it means everywhere else in type theory; that was obviously wrong and silly. No disagreements on _that_ point.

---

_Comment by @AlexWaygood on 2024-10-24 15:07_

I updated the PR: I abandoned the attempt to describe a generalised concept of "closed types"; instead, I now just attempt to describe final nominal types (classes explicitly decorated wtih `@final`, and enum classes).

---

_Review requested from @carljm by @AlexWaygood on 2024-10-24 15:07_

---

_Comment by @MichaReiser on 2024-10-24 16:00_

I'm looking forward to read this but don't wait on my review :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:27 on 2024-10-24 16:25_

Although I think it's fine to include this paragraph, I also wonder if we should link the "core concepts" section of the typing spec more prominently at the top of this doc (rather than just in a footnote), and explicitly call it out as a pre-requisite to this document. Because a) I think all red-knot developers should read and understand the core concepts section of the typing spec anyway, and b) it's not a great use of our time to rewrite definitions here that are already given in the core concepts section, and/or in the glossary, of the typing spec.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:31 on 2024-10-24 16:31_

While true, this statement feels a bit random here; it's not clear why it matters to mention it here.

As phrased here, it's trivially true given that `T <: T` for all types `T`, and `T | U == T` if `U <: T`.

It's also true (but somewhat less trivially so) if we're talking about all proper subtypes of `T`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:175 on 2024-10-24 16:57_

```suggestion
Many single-value types exist that are not singleton types: examples include
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:219 on 2024-10-24 17:02_

It may be confusing to readers that technically `Literal[Ham.A]` still is a single-value type, given this definition of `Ham`, because all `Ham` instances compare equal to everything! And this is technically still a valid equivalence relation, in which all inhabitants of `Literal[Ham.A]` do compare equal.

I think in  _practice_ though, we will not want to give red-knot that level of intelligence about the specific implementation of `__eq__`; we will rather want to just say that if `__eq__` is defined, we don't understand the equality relation, and can no longer conclude that `Literal[Ham.A]` is a single-valued type.

It may be useful to clarify this distinction, or it may be fine to just change this `return True` to `return False` and avoid the issue altogether here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:258 on 2024-10-24 17:04_

Is there a missing code example following this? Or should the colon be a full stop instead?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:235 on 2024-10-24 17:07_

Nit: are you talking about proper subtypes here? Every final type has one nominal subtype: itself!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:254 on 2024-10-24 17:12_

I think this section is a bit debatable.

If we are talking about a conceptual `Truthy` type consisting of "all objects whose `__bool__` actually returns `True`", that might work for this example, but it is probably not a good example to include since it's not clear if we even want to define such a type, given that individual objects can easily change their `__bool__` behavior over time.

If we are talking about the Protocol type shown here, then given a final type X, `X & Truthy` will always be either `X` or `Never`, it will never be another proper subtype of `X`, because `X.__bool__` has a known signature, and that signature is either a subtype of `(self) -> Literal[True]` or it is not.

A better example of proper subtypes of final types might be a final generic type with a covariant type parameter? In that case `X[B] <: X[A]` if `B <: A`, even though `X` is final.

At a higher level, though, it remains unclear to me why it is important to discuss whether a final type has any proper subtypes here?

---

_@carljm approved on 2024-10-24 17:15_

This looks good! Most of my comments are minor nits.

---

_@AlexWaygood reviewed on 2024-10-24 17:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:258 on 2024-10-24 17:30_

Oh oops, I added a code example and then deleted it again because it didn't really add anything. I'll get rid of the colon.

---

_@AlexWaygood reviewed on 2024-10-24 17:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:235 on 2024-10-24 17:31_

Argh, yes! And `Never` is a subtype of everything, again, of course

---

_@AlexWaygood reviewed on 2024-10-24 17:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:254 on 2024-10-24 17:35_

I find it pretty useful to remember that runtime subclassing and static subtyping are linked but not the same thing. Just because something is `@final` does not mean it cannot be subtyped, only that it cannot be subclassed; all types except singleton types can be subtyped.

---

_@AlexWaygood reviewed on 2024-10-24 17:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:27 on 2024-10-24 17:36_

Yes. Though I'm not sure all this information exists in the typing spec exactly as written. But I was also worried about overlap. I'll take another look. 

---

_@carljm reviewed on 2024-10-24 18:03_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:27 on 2024-10-24 18:03_

https://typing.readthedocs.io/en/latest/spec/concepts.html#fully-static-types says

> If an object v is a member of the set of objects denoted by a fully static type T, we can say that v is a “member of” the type T, or v “inhabits” T.

---

_@AlexWaygood reviewed on 2024-10-28 12:51_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:235 on 2024-10-28 12:51_

> And `Never` is a subtype of everything, again, of course

...Except `Never` is not a _nominal_ subtype.

---

_Comment by @AlexWaygood on 2024-10-28 19:11_

I've once again reworked the final section of this substantially, because I realised my last draft still had some inaccuracies. For example, I wrote that:

> a final type in Python still has subtypes, just no *nominal* subtypes.

which is pretty obviously wrong when you think about enums: `Literal[E.A]` is a nominal type (well, it's certainly not a _structural_ type, anyway), and it's obviously a subtype of the enum `E`.

My conclusion from all this conversation is that while the concept of final _classes_ might be useful, the concept of final _types_ actually isn't that useful at all (and the name is somewhat misleading in some ways). So I scrapped the section defining a concept of "final _types_" and replaced it with a section describing "final _classes_".

I'd appreciate it if you could give the last section another once-over @carljm!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:33 on 2024-10-28 19:12_

nit: the use of the qualifier "known" in this sentence invites uncertainty about whether you are saying "there is definitely exactly one inhabitant, and it is known" or "there is exactly one known inhabitant, there may be other unknown ones."

You could be more explicit that it is the former, but I'm not sure it's worth it, vs just removing the word "known" -- I think it's obvious that if we can be sure there's exactly one inhabitant of type, that means we know what it is :)
```suggestion
(and can only ever be) exactly one inhabitant of the type at runtime:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:43 on 2024-10-28 19:13_

ooh TIL that this is a usable code block language, neat!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:93 on 2024-10-28 19:14_

There's still a reference to "final type" here, which may no longer make sense given later changes.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:168 on 2024-10-28 19:17_

Not important, but may help readers tie together some mentioned concepts.

```suggestion
    is reflexive, symmetric, and transitive (satisfying the definition of
    an equivalence relation.)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:175 on 2024-10-28 19:18_

nit: I think "in terms of identity" is redundant here, especially when you immediately go on to specifically discuss memory addresses
```suggestion
However, they are not necessarily the *same* object;
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:178 on 2024-10-28 19:19_

We said above that we can also narrow sealed types by identity, not just singleton types:
```suggestion
that the type is a sealed type.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:206 on 2024-10-28 19:22_

`Ham` is not a singleton type, but `Literal[Ham.A]` is

```suggestion
(but we *can* still narrow the `Literal[Ham.A]` type using identity, as with all singleton types):
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:229 on 2024-10-28 19:23_

nit: this applies to all final classes, whether or not they are made final by the use of `@final` (and saying "is `@final`" reads oddly to me, even as a way to say "is decorated with `typing.final`")
```suggestion
For any two classes `X` and `Y`, if `X` is final and `X` is not a subclass of `Y`,
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:241 on 2024-10-28 19:25_

minor nit, not related to the core point: I think using plurals is generally not great naming practice for either enums or classes generally, suggest `Animal` instead

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:233 on 2024-10-28 19:26_

I think "types associated with final classes" is a vague and verbose way to word this; "final class types" I think is a clearer unambiguous reference to the "a type defined as ..." in the previous sentence.
```suggestion
However, final class types can still have non-`Never` proper subtypes.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:233 on 2024-10-28 19:29_

I still feel like this section buries the lede in a major way.

From red-knot's perspective, by far the most important thing about final class types is that we know exactly what methods and attributes they define, with what signatures (including especially dunder methods), and we know there can't be infinite possible subclasses overriding those methods and attributes. But we don't even mention that here, beyond a very vague reference in this phrase to "fewer ways of subtypign"; instead we go on to discuss (at relative length, with an example) the edge case of how some final class types can still have proper subtypes, although it remains unclear in the text why the existence or non-existence of proper subtypes should matter to red-knot or the reader.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/docs/kind_of_types.md`:253 on 2024-10-28 19:37_

I would probably eliminate this entire paragraph. It's confusing, and really not clear what important point it makes.

---

_@carljm approved on 2024-10-28 19:39_

The first several sections of this are really excellent, I have only the most minor of wording nits.

The last section is improved, but I still feel it suffers from a significant "so what?" factor, in that it spends IMO too many words discussing things that I don't think are particularly relevant to red-knot behavior. But this isn't a blocking concern.

---

_@AlexWaygood reviewed on 2024-10-28 19:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/docs/kind_of_types.md`:233 on 2024-10-28 19:52_

I guess I was going out of my way to avoid using the term "final types" here, because I was getting less and less convinced that it was actually a useful term that didn't just invite confusion 😄 But you're probably right that my roundabout language here doesn't really add any clarity.

---

_Comment by @T-256 on 2025-01-03 15:33_

Do you still want to land this? We now have _mdtests_ which also have good docs inside themselves.

---

_Comment by @AlexWaygood on 2025-01-03 15:34_

> Do you still want to land this?

yes

---

_Comment by @AlexWaygood on 2026-01-07 13:55_

doesn't seem like I'll get back to this any time soon, and our priority should probably be user-facing docs at this point

---

_Closed by @AlexWaygood on 2026-01-07 13:55_

---

_Branch deleted on 2026-01-07 13:55_

---
