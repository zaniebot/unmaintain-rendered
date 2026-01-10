```yaml
number: 15353
title: "[red-knot] More comprehensive `is_assignable_to` tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/migrate-assignable_to-tests
created_at: 2025-01-08T13:52:11Z
updated_at: 2025-01-08T19:25:10Z
url: https://github.com/astral-sh/ruff/pull/15353
synced_at: 2026-01-10T20:34:00Z
```

# [red-knot] More comprehensive `is_assignable_to` tests

---

_Pull request opened by @sharkdp on 2025-01-08 13:52_

## Summary

This changeset migrates all existing `is_assignable_to` tests to a Markdown-based test. It also increases our test coverage in a hopefully meaningful way (not claiming to be complete in any sense). But at least I found and fixed one bug while doing so.

## Test Plan

Ran property tests to make sure the new test succeeds after fixing it.

---

_Label `red-knot` added by @sharkdp on 2025-01-08 13:52_

---

_Review requested from @carljm by @sharkdp on 2025-01-08 13:52_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-08 13:52_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-08 13:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4105 on 2025-01-08 13:54_

These two tests have no equivalent right now, but we have a `todo_types` Rust unit test that makes sure `int` is assignable to `@Todo` and vice versa.

---

_@sharkdp reviewed on 2025-01-08 13:54_

---

_@sharkdp reviewed on 2025-01-08 13:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:84 on 2025-01-08 13:56_

We could also remove/reduce these tests as they are covered by property tests, but I guess they don't really hurt? (and as we have learned yesterday and with the new bug uncovered here, it's easy to get some of them wrong)

---

_Comment by @github-actions[bot] on 2025-01-08 13:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:15 on 2025-01-08 13:58_

I thought about splitting this into multiple Markdown sections, but it didn't feel like this would improve the structure a lot, and I would have to repeat these imports multiple times.

---

_@sharkdp reviewed on 2025-01-08 13:58_

---

_@sharkdp reviewed on 2025-01-08 14:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:84 on 2025-01-08 14:02_

The last of these tests here (`is_assignable_to(Never, type[Any])`) is what's failing on main.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:153 on 2025-01-08 14:04_

the reason I used `ABCMeta` for these tests when they were a unit test was that it was an easily accessible stdlib metaclass. But now that we're rewriting these tests as mdtests, we can create our own custom metaclass as part of the test. This would make it easier for a casual reader to understand the purpose of the test, and would make the test more robust (it means the test won't break if typeshed unexpectedly changes the definition of `ABCMeta` -- I don't know _why_ we'd do that in typeshed, but it still seems best to keep our tests isolated from typeshed where possible).

The metaclass could just be as simple as:

```py
class Meta(type): ...
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:15 on 2025-01-08 14:06_

hmm, I do think splitting it into different Markdown sections would make the mdtest report easier to understand if the test failed. The nearest subheading is printed to the terminal when an mdtest fails, but in this file, it's just one huge test under the heading `# Assignable-to relation`, so it might be harder to figure out quickly exactly what about the assignable-to relation has gone wrong when a test fails

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/property_tests.rs`:307 on 2025-01-08 14:07_

Should we also have a subtyping version of this for fully static types?

```suggestion
    // Never should be assignable to every type
    type_property_test!(
        never_assignable_to_every_type, db,
        forall types t. Type::Never.is_assignable_to(db, t)
    );
    
    // and it should be a subtype of all fully static types
    type_property_test!(
        never_subtype_of_every_fully_static_type, db,
        forall types t. t.is_fully_static(db) => Type::Never.is_subtype_of(db, t)
    );
```

---

_@AlexWaygood reviewed on 2025-01-08 14:08_

Fantastic!

---

_@sharkdp reviewed on 2025-01-08 14:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:15 on 2025-01-08 14:11_

I did have quite a few failing assertions while writing those tests, and since we report proper spans for those static assertion failures, it's actually quite easy to jump to the exact line that is failing. If you still think it makes sense to split them anyway, I'm happy to do that. I agree that it would probably look nicer (in rendered form).

---

_@sharkdp reviewed on 2025-01-08 14:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:153 on 2025-01-08 14:13_

> This would make it easier for a casual reader to understand the purpose of the test

Indeed! I was actually wondering what was special about `ABCMeta`.

---

_@AlexWaygood reviewed on 2025-01-08 14:14_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:15 on 2025-01-08 14:14_

Good point about the spans... but yeah, I still think I'd weakly prefer to have it split up, for improved readability when rendered :-)

---

_@AlexWaygood reviewed on 2025-01-08 14:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:84 on 2025-01-08 14:17_

The unit tests generally use simpler types than what the property tests come up with, so I do like having both, for ease of debugging test failures 😄 though maybe that's improved now that https://github.com/astral-sh/ruff/pull/15297 has landed, and we have more shrinking -- I haven't experienced any property test failures in the last day, so I don't know what the new experience is like!

---

_Label `testing` added by @AlexWaygood on 2025-01-08 14:21_

---

_@sharkdp reviewed on 2025-01-08 14:34_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:84 on 2025-01-08 14:34_

![image](https://github.com/user-attachments/assets/04afaa91-fbb4-45fd-9b56-7f9154b8d229)


---

_@sharkdp reviewed on 2025-01-08 14:59_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:15 on 2025-01-08 14:59_

Done. You may now print your personal hardcover version.

---

_@carljm approved on 2025-01-08 17:33_

Love it, thank you!!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:10 on 2025-01-08 17:40_

```suggestion
### Fully static

Fully static types participate in subtyping. If a type `S` is a subtype of `T`,
`S` will also be assignable to `T`. Two equivalent types are subtypes of each other:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:37 on 2025-01-08 17:41_

```suggestion
### Gradual types

Gradual types do not participate in subtyping, but can still be assignable to other 
types (and static types can be assignable to gradual types):
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:54 on 2025-01-08 17:41_

```suggestion
### Boolean literals

`Literal[True]` and `Literal[False]` are both subtypes of (and therefore
assignable to) `bool`, which is in turn a subtype of `int`:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:85 on 2025-01-08 17:44_

```suggestion
### String literals and `LiteralString`

All string-literal types are subtypes of (and therefore assignable to) `LiteralString`,
which is in turn a subtype of `str`:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:178 on 2025-01-08 17:45_

We could also add

```suggestion
static_assert(is_assignable_to(type[Any], Meta))
static_assert(is_assignable_to(type[Unknown], Meta))
static_assert(is_assignable_to(Meta, type[Any]))
static_assert(is_assignable_to(Meta, type[Unknown]))
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:183 on 2025-01-08 17:46_

we don't have any gradual tuples in this seciton, and I wonder if we should (e.g. `tuple[Any, Literal[2]]` should be assignable to `tuple[int, int]` but not to `tuple[int, str]`). Doesn't _need_ to be done in this PR, though

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:283 on 2025-01-08 17:48_

```suggestion
### Everything is assignable to `object`

`object` is Python's top type; the set of all possible objects at runtime:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:302 on 2025-01-08 17:49_

```suggestion
### Every type is assignable to `Any` / `Unknown`

`Any` and `Unknown` are gradual types. They could materialize to any given type at
runtime, and so any type is assignable to them:
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:332 on 2025-01-08 17:49_

```suggestion
### `Never` is assignable to every type

`Never` is Python's bottom type: the empty set, a type with no inhabitants.
It is therefore assignable to any arbitrary type.
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:123 on 2025-01-08 17:54_

```suggestion
In the following tests, `TypeOf[str]` is a singleton type with a single inhabitant, the class `str`.
This contrasts with `type[str]`, which represents "all possible subclasses of `str`".

Both `TypeOf[str]` and `type[str]` are subtypes of `type` and `type[object`], which both 
represents "all possible instances of `type`"; therefore both `type[str]` and 
`TypeOf[str]` are assignable to `type`. `type[Any]`, on the other hand, represents a
type of unknown size or inhabitants, but which is known to be no larger than the set of
possible objects represented by `type`.
```

---

_@AlexWaygood approved on 2025-01-08 17:55_

Some suggested commentary. But I don't mind if you want to leave it to a followup! (And I'm happy to do the followup.) Feel free to merge!

---

_@AlexWaygood reviewed on 2025-01-08 18:37_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:126 on 2025-01-08 18:37_

```suggestion
Both `TypeOf[str]` and `type[str]` are subtypes of `type` and `type[object]`, which both represent
```

---

_@sharkdp reviewed on 2025-01-08 19:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:183 on 2025-01-08 19:19_

I did that now, and also added tests with gradual types in the union and intersection types sections.

---

_Merged by @sharkdp on 2025-01-08 19:25_

---

_Closed by @sharkdp on 2025-01-08 19:25_

---

_Branch deleted on 2025-01-08 19:25_

---
