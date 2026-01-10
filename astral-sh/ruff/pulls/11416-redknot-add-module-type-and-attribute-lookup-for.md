```yaml
number: 11416
title: "[redknot] add module type and attribute lookup for some types"
type: pull_request
state: merged
author: plredmond
labels:
  - internal
assignees: []
merged: true
base: main
head: redknot.attrchain
created_at: 2024-05-14T00:56:28Z
updated_at: 2024-05-28T20:13:05Z
url: https://github.com/astral-sh/ruff/pull/11416
synced_at: 2026-01-10T21:55:59Z
```

# [redknot] add module type and attribute lookup for some types

---

_Pull request opened by @plredmond on 2024-05-14 00:56_

* Add a module type, `ModuleTypeId`
* Add an attribute lookup method `get_member` for `Type`
  * Only implemented for `ModuleTypeId` and `ClassTypeId`
  * [x] Should this be a trait?
    *Answer: no*
  * [x] Uses `unwrap`, but we should remove that. Maybe add a new variant to `QueryError`?
    *Answer: Return `Option<Type>` as is done elsewhere*
* Add `infer_definition_type` case for `Import`
* Add `infer_expr_type` case for `Attribute`
* Add a test to exercise these
* [x] remove all NOTE/FIXME/TODO after discussing with reviewers

---

_Comment by @github-actions[bot] on 2024-05-14 01:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[redknot] attr chain lookup for unspecified-encoding rule" to "[redknot] module attr chain lookup for unspecified-encoding rule" by @plredmond on 2024-05-14 01:12_

---

_Renamed from "[redknot] module attr chain lookup for unspecified-encoding rule" to "[redknot] module type, attribute type lookup" by @plredmond on 2024-05-24 21:41_

---

_Renamed from "[redknot] module type, attribute type lookup" to "[redknot] module type, attribute lookup" by @plredmond on 2024-05-24 21:42_

---

_Renamed from "[redknot] module type, attribute lookup" to "[redknot] add module type and attribute lookup for some types" by @plredmond on 2024-05-24 21:45_

---

_Marked ready for review by @plredmond on 2024-05-24 21:49_

---

_Review requested from @carljm by @plredmond on 2024-05-24 21:49_

---

_Review requested from @MichaReiser by @plredmond on 2024-05-24 21:49_

---

_Comment by @plredmond on 2024-05-24 21:51_

Since you folks were ooo, I added questions that I had as NOTE/TODO/FIXME comments in the diff.

---

_Label `internal` added by @MichaReiser on 2024-05-27 14:46_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:66 on 2024-05-27 14:50_

I think it would be okay to change the signature of `get_member` to return `QueryResult<Option<Type>>`.  I don't think we should panic because it's likely that a lint rule uses this API to test if a class has a specific member. 

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:57 on 2024-05-27 14:51_

I don't think we necessarily need a trait. Defining a `type_id.get_member` method seems sufficient because we don't need to dynamically call `get_member` on any `type_id` (we use static dispatch here)

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:376 on 2024-05-27 14:56_

Uh interesting. I assumed that `Module` would store the `FileId` but it indeed stores the `PathBuf`. I wonder if we should change `Module` to store a `FileId` instead of the `Path`. But I need to think this through more carefully before we do it. 

---

_@MichaReiser reviewed on 2024-05-27 14:58_

Looks good to me overall but I let @carljm approve who has more insight on what the actual inference logic should be.

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:55 on 2024-05-28 03:22_

I'm OK with simplified naming that leaves out `_type`; we understand that we are working with types here, everything is a type.

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:66 on 2024-05-28 03:25_

Yes, I agree that this method should return `QueryResult<Option<Type>>`. "No such member" and "query error (e.g. cancelled)" are distinct results that should be distinctly represented (and the "no such member" result is not an error.)

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:71 on 2024-05-28 03:27_

This is OK as is, but might make sense to wrap this up into `ClassTypeId.get_member()` method?

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:83 on 2024-05-28 03:32_

Accessing a member of a union should really be a type error if any of the unioned types lack the member. In inference-only mode we won't want to issue this error, though -- I think we should add `Unknown` to the member type union instead.

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:88 on 2024-05-28 03:35_

In an intersection, the type must be all of the intersected types, so if any of those have the member, it has the member. So we can return a type if any of the types have it; if more than one has it, we should return the intersection of those types.

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:410 on 2024-05-28 03:38_

These all look good, can remove the NOTE

---

_@carljm reviewed on 2024-05-28 03:45_

This is excellent!

---

_@MichaReiser reviewed on 2024-05-28 07:40_

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:83 on 2024-05-28 07:40_

Long-term it would be interesting to still return a "guessed" type when only some variants implement the specific member. It could allow for better recovery in some situations (e.g. intelli sense). The API would need to be explicit that the type isn't certain. 

---

_@carljm reviewed on 2024-05-28 15:58_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:83 on 2024-05-28 15:58_

Yes, that's actually what my comment proposes, that e.g. if we have `x: A | B | C` and we access `x.foo`, and types `A` and `B` have a `foo` attribute but `C` does not, we would return `A.foo | B.foo | Unknown` instead of erroring. Effectively this is our best guess of the type, and the union with `Unknown` is the marker of our uncertainty.

---

_Review comment by @plredmond on `crates/red_knot/src/types.rs`:66 on 2024-05-28 16:41_

Yes, this is obvious in hindsight. :sweat_smile: ty

---

_@plredmond reviewed on 2024-05-28 16:41_

---

_@plredmond reviewed on 2024-05-28 16:42_

---

_Review comment by @plredmond on `crates/red_knot/src/types.rs`:376 on 2024-05-28 16:42_

Is there any change required for `ModuleTypeId` here? :)

---

_Review comment by @plredmond on `crates/red_knot/src/types.rs`:83 on 2024-05-28 17:10_

Ok! This approach (returning the union of `get_member` results for each type in the union, and adding `Unknown` once if any of result is `None`) makes more sense than what I was suggesting. I can do that in a followup PR if desired.

One question: Is there any case where `get_member` of a union type *doesn't* return a result?

For example, if you have `A | B` and neither has a `.foo` attribute, would `(A | B).get_member("foo")` be `Type::Unknown`? (sorry abusing syntax here)

---

_@plredmond reviewed on 2024-05-28 17:10_

---

_@plredmond reviewed on 2024-05-28 17:17_

---

_Review comment by @plredmond on `crates/red_knot/src/types.rs`:88 on 2024-05-28 17:17_

* No types have the member, return `None`
* One type has the member, return the member type (or a singleton intersection containing it?).
* Two types have the member, return the intersection of the member types.
* Two types have the member, but one type doesn't have the member...? It seems to me that the intersection ought to be empty.

---

_Review requested from @carljm by @plredmond on 2024-05-28 17:17_

---

_Comment by @plredmond on 2024-05-28 17:18_

Thanks for your review. I've made requested changes and asked some questions to describe the follow-up work in comments. Let me know when/if it's ok to merge.

---

_@carljm reviewed on 2024-05-28 17:24_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:83 on 2024-05-28 17:24_

Yeah this is a good question, and I don't have a clear answer yet. I need to think through more carefully when we should return None vs Unknown, and how that will interact with inference-only vs type-checking modes.

In principle a nice clean answer would to always use Unknown and never return None. But I worry that this doesn't allow us to distinguish "attribute doesn't exist" from "attribute exists but resolving its type gives us Unknown." But maybe it's ok to not distinguish these from the caller, if we're able to surface diagnostics wherever we create an Unknown. Which I think we should be able to. (It may require passing some kind of diagnostic-sink to all these methods.)

---

_@carljm reviewed on 2024-05-28 17:28_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:83 on 2024-05-28 17:28_

On the other hand I can imagine that in some lint rules we might want to be able to distinguish these cases, without necessarily creating a diagnostic for the missing attribute. 

---

_@carljm reviewed on 2024-05-28 17:37_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:88 on 2024-05-28 17:37_

There never a need for singleton intersection (or union), it should  always simplify to just the single type. 

In the final case, the type that is missing the attribute should just contribute nothing to the intersection, it shouldn't contribute the bottom type (thus making the intersection empty). I would have to think a bit more about the right way to think about this formally, but I'm sure it's the right behavior. A & B & C must be an A and a B and a C, so if only A and B have the attribute, we know the attribute is present and its type is A.foo & B.foo; C contributes no information about its type. 

---

_@carljm reviewed on 2024-05-28 17:39_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:376 on 2024-05-28 17:39_

I think it's fine as is for now. We can always change later. 

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:452 on 2024-05-28 17:42_

Just realized we should actually call this `get_class_member` because ClassTypeId is used by both Class and Instance types, and the set of visible attributes can vary in each case.

(Not 100% sure this is the best way to handle it, but it's the naming convention currently used otherwise here, so let's stick to it until/unless we switch to a different approach.)

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:457 on 2024-05-28 17:43_

And we can remove `get_class_member` from this TODO list. 

---

_@carljm approved on 2024-05-28 17:45_

I think with the one method name change I suggested, this is ready to merge. The union and intersection TODOs don't specify all the details we discussed in those comment threads, but they don't need to, especially since we're not totally clear on all those details yet. 

---

_Comment by @carljm on 2024-05-28 17:47_

Oh there are a couple clippy issues to fix, too. 

Looks like you could easily support AnnotatedAssignment the same as Assignment for now (with a TODO to actually look at the annotation) and our Definition handling would cover all variants?

---

_Comment by @plredmond on 2024-05-28 19:28_

* Clippy: rename an unused variable which we'll use after completing the `get_member` case for unions
* Added case to `infer_definition_type` for `AnnotatedAssignment` variant which returns `Type::Unknown` when the value isn't present; includes a TODO to look at the annotation (which presumably will have a type expression)

---

_@plredmond reviewed on 2024-05-28 19:31_

---

_Review comment by @plredmond on `crates/red_knot/src/types.rs`:599 on 2024-05-28 19:31_

I left this as a TODO for now, but could implement it if you tell me what you'd like. It's the *type* of a module, which doesn't have much info when python prints it out. Probably a bikeshed.
```python
>>> import x
>>> x
<module 'x' from '/home/asteroid/code/redknot.attrchain/x.py'>
>>> type(x)
<class 'module'>
```

---

_@carljm reviewed on 2024-05-28 19:33_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:599 on 2024-05-28 19:33_

I think copying the runtime output (for the module object, not the type, since this is not the general module type but the literal type for a particular module) is reasonable here; totally fine with you either doing it in this PR or leaving it as a todo for later, up to you. 

---

_@plredmond reviewed on 2024-05-28 19:33_

---

_Review comment by @plredmond on `crates/red_knot/src/types.rs`:83 on 2024-05-28 19:33_

I think that keeping those cases distinct is important at this level, but perhaps it should wait until we have a use case to drive their clarification.

---

_@carljm approved on 2024-05-28 19:36_

Clippy still not happy with that variable name, but once you appease clippy this looks good. 

---

_Comment by @plredmond on 2024-05-28 19:37_

Oh weird. ~~Maybe the CI-clippy is getting some stricter options than my local one.~~[Edit: No, I just forgot to run it] Ty

---

_Review comment by @plredmond on `crates/red_knot/src/types.rs`:599 on 2024-05-28 19:57_

Well, `DisplayType` only has access to `TypeStore`. We'd need a `SemanticDb` to call `ModuleTypeId::name` and/or a `Files` to call `Files::path` on `FileId`. I don't think I can access either of these in this context. I'll leave it for now.

---

_@plredmond reviewed on 2024-05-28 19:57_

---

_Merged by @plredmond on 2024-05-28 20:13_

---

_Closed by @plredmond on 2024-05-28 20:13_

---

_Branch deleted on 2024-05-28 20:13_

---
