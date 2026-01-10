```yaml
number: 11256
title: "[red-knot] resolve class members"
type: pull_request
state: merged
author: carljm
labels:
  - internal
assignees: []
merged: true
base: main
head: cjm/class-methods
created_at: 2024-05-03T01:09:47Z
updated_at: 2024-05-03T17:34:14Z
url: https://github.com/astral-sh/ruff/pull/11256
synced_at: 2026-01-10T22:37:02Z
```

# [red-knot] resolve class members

---

_Pull request opened by @carljm on 2024-05-03 01:09_

## Summary

Add the ability to (lazily) resolve members of a class type.

## Test Plan

cargo test


---

_Review requested from @MichaReiser by @carljm on 2024-05-03 01:09_

---

_Review requested from @plredmond by @carljm on 2024-05-03 01:09_

---

_Review requested from @AlexWaygood by @carljm on 2024-05-03 01:09_

---

_Comment by @github-actions[bot] on 2024-05-03 01:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @MichaReiser on 2024-05-03 06:51_

---

_Review comment by @MichaReiser on `crates/red_knot/src/symbols.rs`:542 on 2024-05-03 06:52_

Should we remove the first argument. It always seems to be `builder.cur_scope`

---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:418 on 2024-05-03 06:55_

Including `FileId` here is interesting because the symbol table itself is all scoped by the `FileId`. That means, having the `FileId` kind of feels redundant. But I assume it is necessary when we start referring to this type from other modules. 


---

_Review comment by @MichaReiser on `crates/red_knot/src/types.rs`:417 on 2024-05-03 06:56_

```suggestion
    /// File in which the class was defined
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/types/infer.rs`:123 on 2024-05-03 06:57_

This query wouldn't work with salsa because `name` isn't an ingredient. Or at least, it couldn't be a cached query which I think it also isn't here. 

I think the way this would be implemented in salsa is that this is a method on `ClassTypeId`.

`class.get_member(db, name: &Name)` 

---

_@MichaReiser approved on 2024-05-03 07:07_

Looks good. 

What's unclear to me is whether we use the `FileId` on `ClassType`, considering that it is already part of `ClassTypeId`.

---

_Review comment by @AlexWaygood on `crates/red_knot/src/types/infer.rs`:123 on 2024-05-03 10:35_

Two questions:

1. Should this recurse up into superclasses to search for the member in those class bodies, if it can't find them in the class itself? I'm anticipating there are three possible answers to this: "yes", "no (we'll do the proper recursive searching through the mro somewhere else)", and "yes, but not now". Whichever answer you choose, I think it would be good to add a comment noting the behaviour we choose and explaining why we've chosen it ;)
2. How are we distinguishing between class variables and instance variables here? I can think of two possible solutions (though there may be more):
   1. Rather than returning a `QueryResult<Option<Type>>`, the function could return `QueryResult<Option<ClassMember>>`, where `ClassMember` is an enum:
      ```rs
      enum ClassMember {
          /// e.g. `x: int` at the class scope,
          /// or `self.${var}` assignments within methods
          InstanceMember(Type),
          /// e.g. `x: ClassVar[int]` at the class scope,
          /// method definitions, or `x = 42` in the class scope,
          /// or `cls.${var}` assignments within classmethods
          ClassMember(Type),
      }
      ```
   2. We could have two different functions, one for retrieving class variables and the other for retrieving instance variables.
   
   Again, it's okay if this question is deferred for now to keep things simple, but some kind of TODO comment might be in order.

---

_@AlexWaygood approved on 2024-05-03 10:36_

You deleted all the code I wrote yesterday with you 😭

j/k, it makes sense to do this lazily! Left some questions below, but basically looks good

---

_@carljm reviewed on 2024-05-03 14:00_

---

_Review comment by @carljm on `crates/red_knot/src/types.rs`:418 on 2024-05-03 14:00_

I think it is not redundant due to symbol table being scoped by `FileId`, because the type will travel across modules, and doesn't itself hold a reference to its symbol table.

But I think it is redundant for the reason you noted above, that `ClassTypeId` already carries the `FileId`! So I will remove it.

---

_@carljm reviewed on 2024-05-03 14:07_

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:123 on 2024-05-03 14:07_

This isn't a query at all (i.e. it's not exposed as part of any Db trait), it's just a free function that happens to take a db as an argument. (The same is currently true of `infer_expr_type`, though that one almost certainly will become a real query.)

This is not directly cached because it's just a thin convenience wrapper over `infer_symbol_type`, which is cached.

Whether this is a free function or a method on `ClassTypeId` is just a question of which API we prefer, it's not related to whether or not it's a query (a free function can just be a regular function, not a query), so it's not related to salsa either.

But I do think having it be a method on `ClassTypeId` is nice, in particular because it means we don't need a `FileId` on `ClassType` anymore, nor an extra `file_id` argument. I'll make that change.

---

_Review comment by @carljm on `crates/red_knot/src/types/infer.rs`:123 on 2024-05-03 14:21_

Great points!

> Should this recurse up into superclasses

I think we will need both a method that does, and a method that doesn't (which the method that does will need to use). So I think it's fine to first add the method that doesn't, though when I actually add the override-check lint rule, it may make sense to add the recursive lookup method then.

I'll add a comment and/or rename this method for better clarity.

> How are we distinguishing between class variables and instance variables here?

Currently we aren't, and we will need to at some point. It's good to at least consider now how we will want to.

I think one tricky thing here is that there are really three variants: instance member, class member, and instance-or-class member. (`x = 2` in class body without a `ClassVar` annotation is the latter, in general, though we may want to do additional analysis of whether any method includes `self.x = ...` to possibly narrow it down to "class member" if not.)

Because of that, I think I'm inclined to do this as two methods, where for some names, both methods would return the result? But I'm not sure, and I think it can be deferred. I'll add a TODO.

---

_@carljm reviewed on 2024-05-03 14:21_

---

_Merged by @carljm on 2024-05-03 17:34_

---

_Closed by @carljm on 2024-05-03 17:34_

---

_Branch deleted on 2024-05-03 17:34_

---
