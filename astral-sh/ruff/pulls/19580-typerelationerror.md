```yaml
number: 19580
title: TypeRelationError
type: pull_request
state: closed
author: MatthewMckee4
labels:
  - ty
assignees: []
draft: true
base: main
head: type-relation-error
created_at: 2025-07-27T22:44:40Z
updated_at: 2026-01-13T21:43:28Z
url: https://github.com/astral-sh/ruff/pull/19580
synced_at: 2026-01-13T22:36:07Z
```

# TypeRelationError

---

_@MatthewMckee4_

## Summary

Towards https://github.com/astral-sh/ty/issues/163

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @AlexWaygood on 2025-07-27 22:54_

---

_Comment by @MatthewMckee4 on 2025-07-27 23:57_

One thing I just can't catch is messing everything up, will go over it tmr evening. If anyone has a time for a quick scan it should be in types.rs, but im not too sure

---

_@MichaReiser reviewed on 2025-07-28 07:56_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1253 on 2025-07-28 07:56_

Nit: It feels a bit off to me to use a `Result` here. I would expect an `Err` only if two types can't be compared but that's not how `Result` is used here. 

I think I'd use a custom enum over `Result` with a `Yes` and `No(reason)` variants. We can mark the type as `#[must_use]` if we're worried about callers not handling the error but it seems not handling the error actually seems to be the default? 


What's unclear to me is how we plan on aggregating `TypeRelationError`s. But maybe that's just because it's not yet clear to me what variants `TypeRelationError` will have

---

_@AlexWaygood reviewed on 2025-07-28 10:17_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1253 on 2025-07-28 10:17_

I think using `Result` might have some advantages further down the line: methods like `map_err` could be useful for bubbling up a low-level assignability failure of a wrapped type into a high-level assignability failure for the wrapping type. But I agree that we need a more ergonomic solution for the default case: I was going to suggest having low-level methods `try_type_relation`, `try_subtype_relation` and `try_assignability_relation` (which you can use if you need to know _why_ the relation doesn't apply between two types), but to also have high-level `has_relation_to`, `is_subtype_of` and `is_assignable_to` methods like we have now:

```rs
impl<'db> Type<'db> {
    fn has_relation_to(db: &'db dyn Db, other: Type<'db>, relation: TypeRelation) -> bool {
        self.try_type_relation(db, other, relation).is_ok()
    }

    fn is_subtype_of(db: &'db dyn Db, other: Type<'db>) -> bool {
        self.has_relation_to(db, other, TypeRelation::Subtyping)
    }

    fn is_assignable_to(db: &'db dyn Db, other: Type<'db>) -> bool {
        self.has_relation_to(db, other, TypeRelation::Assignability)
    }
}
```

it's a bit hard to tell what the best design is, however, until we try to start trying to use (in diagnostics) the extra information now available to us.

---

_@MichaReiser reviewed on 2025-07-28 10:19_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:1253 on 2025-07-28 10:19_

We can always implement `map_err` etc. on the custom enum. I just find `is_ok` to be too implicit and `has_relation_to(db, other, relation) == Ok(())` seems to verbose (and also confusing). 



---

_@AlexWaygood reviewed on 2025-07-28 10:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:1253 on 2025-07-28 10:22_

I don't have a strong opinion

---

_@MatthewMckee4 reviewed on 2025-07-28 10:26_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1253 on 2025-07-28 10:26_

try_... seems like a good structure and simplifies this PR a lot

---

_@MatthewMckee4 reviewed on 2025-07-28 10:32_

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:1253 on 2025-07-28 10:32_

Creating our own enum also means we can implement from bool. Which also simplifies a lot of code

---

_Comment by @github-actions[bot] on 2025-07-28 17:06_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-28 17:52_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @codspeed-hq[bot] on 2025-07-28 18:00_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/MatthewMckee4%3Atype-relation-error?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #19580 will **not alter performance**

<sub>Comparing <code>MatthewMckee4:type-relation-error</code> (e26ec50) with <code>main</code> (81867ea)</sub>



### Summary

`✅ 8` untouched  





---

_Comment by @codspeed-hq[bot] on 2025-07-28 18:01_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/MatthewMckee4%3Atype-relation-error?runnerMode=WallTime)

### Merging #19580 will **not alter performance**

<sub>Comparing <code>MatthewMckee4:type-relation-error</code> (e26ec50) with <code>main</code> (81867ea)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Comment by @MatthewMckee4 on 2025-07-28 19:32_

Will mark as ready for some initial feedback

---

_Comment by @MatthewMckee4 on 2025-07-28 19:55_

Not sure as of now what to do about the regressions

---

_Comment by @MichaReiser on 2025-07-29 06:56_

One thing you could try is ot use `thin_vec`. I suspect that the performance regression is mainly because the result of the type operations no longer fit into a single register. But it might also be that we return `false` in many places and tracking the information is anything but free. 

But I think I'll need a better understanding for what we want to use this new information and if it's even worth tracking all possible errors. 

It might be good to start with: What exact information do we need and, from there, backtrack on what information we need to propagate in these relation methods. 

It might also be that we want separate relation methods to avoid the overhead in the most common path.

---

_Comment by @MatthewMckee4 on 2025-07-29 08:05_

It's probably also useful if I turn TypeRelationError into an enum? All of the todo fails right now will create a new vec, but for the most part most vec will have one item

---

_Comment by @AlexWaygood on 2025-07-29 11:01_

> But I think I'll need a better understanding for what we want to use this new information and if it's even worth tracking all possible errors.
> 
> It might be good to start with: What exact information do we need and, from there, backtrack on what information we need to propagate in these relation methods.

https://github.com/astral-sh/ty/issues/163#issuecomment-3131926504 and https://github.com/astral-sh/ty/issues/163#issuecomment-3127554546 show examples of why we need to track this information

---

_@AlexWaygood reviewed on 2025-07-29 11:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:472 on 2025-07-29 11:04_

this is a lot more expensive than what we do on `main` because it means that we now have to iterate through the entire MRO (and allocate a `Vec`) whereas previously we could short-circuit as soon as we spotted that one class was a subclass of the other. We should avoid `.collect()` calls in this and similar cases wherever possible, and continue to short-circuit. I think in most cases, it's probably okay if we only provide a single reason why one type is not a subtype of another, even if there are multiple underlying reasons why the relation cannot be satisfied.

`Protocol`s might be an exception to this, but we can deal with that later

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types/class.rs`:472 on 2025-07-29 11:38_

Yeah I realised this, will address later

---

_@MatthewMckee4 reviewed on 2025-07-29 11:38_

---

_Comment by @MichaReiser on 2025-07-29 11:58_

> https://github.com/astral-sh/ty/issues/163#issuecomment-3131926504 and https://github.com/astral-sh/ty/issues/163#issuecomment-3127554546 show examples of why we need to track this information

What's not clear to me from this if this is something we need to integrate into the `is_` methods directly or if we should have a separate method that visits a type again and then diagnoses why it isn't assignable (by re-testing each union element to find the one that isn't assignable). 



---

_Comment by @AlexWaygood on 2025-07-29 12:09_

> What's not clear to me from this if this is something we need to integrate into the `is_` methods directly or if we should have a separate method that visits a type again and then diagnoses why it isn't assignable (by re-testing each union element to find the one that isn't assignable).

Yeah, that's a good question. It would be interesting to experiment with some different designs here.

The nice thing about the current approach in this PR is that it does not increase the maintenance burden of the type-relational methods. I would definitely not want to go back to a situation similar to what we had before https://github.com/astral-sh/ruff/pull/18430: having separate `Type::is_assignable_to` and `Type::is_subtype_of` methods was a maintenance nightmare that led to many subtle bugs across our code. I'm concerned that your suggestion _might_ similarly lead to us trying to have duplicated implementations of `Type::has_relation_to` which would be very hard to keep in sync (this method is very subtle, and very complex!).

There might also be ways of implementing your suggestion that would not significantly increase the maintenance burden. On the other hand, I also think there are some obvious ways of optimising this PR branch, as I commented above

---

_Comment by @MatthewMckee4 on 2025-07-29 13:38_

Nice, thats a bit better

---

_Comment by @MatthewMckee4 on 2025-07-29 14:17_

nice!!

---

_Comment by @MatthewMckee4 on 2025-07-29 16:05_

There's maybe an argument to rename `TypeRelationErrorKind` to `TypeRelationErrorReason`?

I'm also not sure if we should have a base (`NoReason`) case, it seems like most of those cases would be a simple `DoesNotInheritFrom` or similar.

---

_Comment by @MatthewMckee4 on 2025-08-12 14:06_

Is there any interest in taking this further?

---

_Comment by @AlexWaygood on 2025-08-12 14:21_

> Is there any interest in taking this further?

Yes, definitely! I think we absolutely need something like this.

Sorry we haven't got back to you, that's on me :-( There's a lot of things to juggle right now.

---

_Comment by @MatthewMckee4 on 2025-08-18 22:55_

All good, when you want to revisit let me know and I can fix the merge conflicts 

---

_Closed by @MatthewMckee4 on 2026-01-13 21:43_

---
