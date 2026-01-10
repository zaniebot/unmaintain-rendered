```yaml
number: 14178
title: "[red-knot] Property tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/property-tests
created_at: 2024-11-07T22:11:25Z
updated_at: 2024-12-03T13:31:25Z
url: https://github.com/astral-sh/ruff/pull/14178
synced_at: 2026-01-10T20:42:26Z
```

# [red-knot] Property tests

---

_Pull request opened by @sharkdp on 2024-11-07 22:11_

## Summary

This PR adds a new `property_tests` module with quickcheck-based tests that verify certain properties of types. The following properties are currently checked:

* `is_equivalent_to`:
  * is reflexive: `T` is equivalent to itself
* `is_subtype_of`:
  * is reflexive: `T` is a subtype of `T`
  * is antisymmetric: if `S <: T` and `T <: S`, then `S` is equivalent to `T`
  * is transitive: `S <: T` & `T <: U` => `S <: U`
* `is_disjoint_from`:
  * is irreflexive: `T` is not disjoint from `T`
  * is symmetric: `S` disjoint from `T` => `T` disjoint from `S`
* `is_assignable_to`:
  * is reflexive
* `negate`:
  * is an involution: `T.negate().negate()` is equivalent to `T`

There are also some tests that validate higher-level properties like:

* `S <: T` implies that `S` is not disjoint from `T`
* `S <: T` implies that `S` is assignable to `T`
* A singleton type must also be single-valued

These tests found a few bugs so far:

- #14177 
- #14195 
- #14196 
- #14210
- #14731

Some additional notes:

- Quickcheck-based property tests are non-deterministic and finding counter-examples might take an arbitrary long time. This makes them bad candidates for running in CI (for every PR). We can think of running them in a cron-job way from time to time, similar to fuzzing. But for now, it's only possible to run them locally (see instructions in source code).
- Some tests currently find false positive "counterexamples" because our understanding of equivalence of types is not yet complete. We do not understand that `int | str` is the same as `str | int`, for example. These tests are in a separate `property_tests::flaky` module.
- Properties can not be formulated in every way possible, due to the fact that `is_disjoint_from` and `is_subtype_of` can produce false negative answers.
- The current shrinking implementation is very naive, which leads to counterexamples that are very long (`str & Any & ~tuple[Any] & ~tuple[Unknown] & ~Literal[""] & ~Literal["a"] | str & int & ~tuple[Any] & ~tuple[Unknown]`), requiring the developer to simplify manually. It has not been a major issue so far, but there is a comment in the code how this can be improved.
- The tests are currently implemented using a macro. This is a single commit on top which can easily be reverted, if we prefer the plain code instead. With the macro:
  ```rs
  // `S <: T` implies that `S` can be assigned to `T`.
  type_property_test!(
      subtype_of_implies_assignable_to, db,
      forall types s, t. s.is_subtype_of(db, t) => s.is_assignable_to(db, t)
  );
  ```
  without the macro:
  ```rs
  /// `S <: T` implies that `S` can be assigned to `T`.
  #[quickcheck]
  fn subtype_of_implies_assignable_to(s: Ty, t: Ty) -> bool {
      let db = get_cached_db();
  
      let s = s.into_type(&db);
      let t = t.into_type(&db);
  
      !s.is_subtype_of(&*db, t) || s.is_assignable_to(&*db, t)
  }
  ```

## Test Plan

```bash
while cargo test --release -p red_knot_python_semantic --features property_tests types::property_tests; do :; done
```

---

_Label `red-knot` added by @AlexWaygood on 2024-11-07 22:12_

---

_@sharkdp reviewed on 2024-11-07 22:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3916 on 2024-11-07 22:19_

If we want to move forward with this, we could probably auto-generate this boilerplate in each test using a macro.

---

_Comment by @github-actions[bot] on 2024-11-07 22:37_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@sharkdp reviewed on 2024-11-07 23:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:3820 on 2024-11-07 23:11_

There must be a better way to do this. If only we had the author of this library on our team...

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:3820 on 2024-11-08 08:58_

CC: @BurntSushi 

---

_@MichaReiser reviewed on 2024-11-08 08:58_

---

_@BurntSushi reviewed on 2024-11-08 12:30_

---

_Review comment by @BurntSushi on `crates/red_knot_python_semantic/src/types.rs`:3820 on 2024-11-08 12:30_

I think this looks reasonable to me?

---

_Comment by @carljm on 2024-11-08 16:55_

This is awesome! Thank you for doing this, I've been wanting to explore this. I think property testing is very well suited to testing type relation invariants, and I do think we should move forward with actually landing this.

---

_@sharkdp reviewed on 2024-12-02 14:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/Cargo.toml`:53 on 2024-12-02 14:47_

Dev-dependencies can't be optional, otherwise I would have made these dependent on the `property_tests` feature.

---

_Marked ready for review by @sharkdp on 2024-12-02 15:43_

---

_Review requested from @carljm by @sharkdp on 2024-12-02 15:43_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-02 15:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:37 on 2024-12-02 19:01_

Including `i64::MIN` and `i64::MAX` here also seems likely to uncover interesting edge cases?

---

_Comment by @sharkdp on 2024-12-02 19:08_

Hm, we run tests with `--all-features`, so my current approach doesn't work. I'm working on it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:134 on 2024-12-02 19:15_

It's not said clearly here, but I think the implication is that we should add this simplification of types containing `Never` (which doesn't seem too complicated? I guess it would require mostly that we introduce a builder pattern for constructing tuple types also?), and once we've done that, this could go away?

Maybe this should be a TODO and/or a new GH issue?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:142 on 2024-12-02 19:17_

Is this correct? Based on the above comment, I'm assuming the semantics of `contains_never` are really supposed to be "is equivalent to Never" -- but an intersection that contains `Never` as a negative element is not equivalent to `Never`. In fact, `Never` should just be eliminated from negative elements of an intersection.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:249 on 2024-12-02 19:21_

Another useful property we could test here when we fix the issue about `is_subtype_of` and non-fully-static types is that a non-fully-static type never participates in subtyping. (This assumes that we add `Type::is_fully_static` so we can check this predicate.)

---

_@carljm approved on 2024-12-02 19:21_

This is great, thank you!

---

_Comment by @MichaReiser on 2024-12-02 20:09_

I think you can just ignore them and they can then be run by name

---

_Comment by @sharkdp on 2024-12-02 20:20_

> I think you can just ignore them and they can then be run by name

That's an option, but I wanted to mark some of them as `#[ignore]`d  for another reason as well (because they yield many false positives right now). I guess I can also just comment them out for now.. but I don't like commented out committed code :-/

---

_@sharkdp reviewed on 2024-12-02 20:38_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:37 on 2024-12-02 20:38_

Ok. I wasn't sure about these. They seem more like red knot internal limitations. But if we add property tests for things like the type level `Literal[…]` math, adding those makes sense.

---

_@carljm reviewed on 2024-12-02 21:04_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/property_tests.rs`:37 on 2024-12-02 21:04_

Yeah, that's fair, it may not make sense to add these unless we add property tests for literal arithmetic.

---

_Comment by @MichaReiser on 2024-12-02 21:20_

I'd prefer ignoring them with different reasons. Commented out code defeats the reason why we're merging the tests in the first place

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:134 on 2024-12-02 21:53_

https://github.com/astral-sh/ruff/pull/14744

---

_@sharkdp reviewed on 2024-12-02 21:53_

---

_@sharkdp reviewed on 2024-12-02 21:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:142 on 2024-12-02 21:53_

You're right, thanks. This will be resolved/removed via #14744.

---

_@sharkdp reviewed on 2024-12-03 08:02_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:249 on 2024-12-03 08:02_

Added as a task in #14524

---

_Review comment by @MichaReiser on `Cargo.lock`:2195 on 2024-12-03 08:03_

Do we make use of the `env_logger` and `regex` feature or could we disable all default features? 

https://docs.rs/crate/quickcheck/1.0.3/features

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/property_tests.rs`:144 on 2024-12-03 08:16_

I find the macro fairly hard to read. What I understand is that their primary benefit is to reduce the repetition of creating the cached db and dereferencing it. 

* How much faster is reusing the db over creating a new database?
* Have you considered adding a snapshot method to the database instead of locking it? Databases are thread safe and can be shared (for as long as you don't update the db with a `&mut Db` reference

```
        pub fn snapshot(&self) -> Self {
            Self {
                storage: self.storage.clone(),
                files: self.files.snapshot(),
                system: self.system.clone(),
                vendored: self.vendored.clone(),
                events: Arc::clone(&self.events),
            }
        }
```

A test then becomes 

```
    #[test]
    #[quickcheck_macros::quickcheck]
    fn equivalent_to_is_reflexive2(t: crate::types::tests::Ty) -> bool {
        let db = get_cached_db();
        let t = t.into_type(&db);
        t.is_equivalent_to(&db, t)
    }
```

Which I don't find too bad

---

_@MichaReiser reviewed on 2024-12-03 08:16_

---

_@sharkdp reviewed on 2024-12-03 08:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:144 on 2024-12-03 08:40_

> How much faster is reusing the db over creating a new database?

Three orders of magnitude. It's essential. I found some bugs only after I added this optimization.

> * Have you considered adding a snapshot method to the database instead of locking it

I have not. Would the benefit be to get rid of the `let db = &*db;`, or do you think that would be faster?

> I find the macro fairly hard to read

Because macros are hard to read, or because this particular macro is hard to read? :smile:

I don't disagree, but it's also not like we're hiding lots of complexity in there. It's really just a boilerplate-reduction macro. I personally care much more about the readability of the individual tests. The macro is ugly, yes. But I won't have to read the macro code again when writing new tests.

> A test then becomes [...] Which I don't find too bad

Well, you picked the easiest example there ;-). With just one type and no logical implication. Did you see the macro-vs-non-macro comparison in the PR description? Or this example, which has three types:

```rs
#[quickcheck]
fn subtype_of_is_transitive(s: Ty, t: Ty, u: Ty) -> bool {
    let db = get_cached_db();
    let db = &*db;
    let s = s.into_type(db);
    let t = t.into_type(db);
    let u = u.into_type(db);

    !(s.is_subtype_of(db, t) && t.is_subtype_of(db, u)) || s.is_subtype_of(db, u)
}
```

```rs
type_property_test!(
    subtype_of_is_transitive,
    db,
    (s, t, u),
    s.is_subtype_of(db, t) && t.is_subtype_of(db, u) => s.is_subtype_of(db, u)
);
```

> their primary benefit is to reduce the repetition of creating the cached db and dereferencing it

It also removes the necessity for lots of `let t = t.into_type(db);` lines, and allows us to write logical implications using `premise => conclusion` instead of `!(premise) || (conclusion)`.

Let me know what you think. I'm going to be okay if we remove it again :smile:.

---

_@MichaReiser reviewed on 2024-12-03 09:04_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/property_tests.rs`:144 on 2024-12-03 09:04_

> I have not. Would the benefit be to get rid of the let db = &*db;, or do you think that would be faster?

It would allow you to remove the mutex guard and the deref (you still need to use `&db` when calling any function)


> Because macros are hard to read, or because this particular macro is hard to read? 😄

It's somewhat both. I had to expand the macro to understand what the different parts map to. Like what's up with the `(s, t, u)`? which part is even the assertion? Some extra documentation might help. 


> Did you see the macro-vs-non-macro comparison in the PR description? Or this example, which has three types:

I admit, I did not! I see how it simplifies the code a bit, but it doesn't significantly reduce the number of lines. 

Could we implement `into_type` for tuples so that you can keep writing `(s, t, u) = (s, t, u).into_types(db)`?

> It also removes the necessity for lots of let t = t.into_type(db); lines, and allows us to write logical implications using premise => conclusion instead of !(premise) || (conclusion).

I do like that. Maybe we can limit the macro part to just that? 

I'm not opposed to keeping the macros. I just had to expand them, and I still spent about 5 minutes trying to figure out what was going on. But I'm also not sure if it's worth it if you spend more time on it.


---

_@MichaReiser approved on 2024-12-03 09:40_

---

_@sharkdp reviewed on 2024-12-03 12:50_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/property_tests.rs`:144 on 2024-12-03 12:50_

> Like what's up with the `(s, t, u)`? which part is even the assertion?

These are valid concerns. I now pushed another commit where I change the syntax to the following:

```rs
type_property_test!(
    subtype_of_implies_assignable_to, db,
    forall types s, t. s.is_subtype_of(db, t) => s.is_assignable_to(db, t)
);
```

The intention is that it reads like a mathematical property `∀ S, T. …`.

I also simplified the macro definition (only two clauses instead of four), clarified the variable names and added some documentation.

I kind of like how this looks, and would merge it for now. I'm happy to revisit it though if I get more feedback on it (from you or others).



> It would allow you to remove the mutex guard and the deref (you still need to use `&db` when calling any function)

I'll note it down as a task for me (low priority), thanks.

---

_Merged by @sharkdp on 2024-12-03 12:54_

---

_Closed by @sharkdp on 2024-12-03 12:54_

---

_Branch deleted on 2024-12-03 12:54_

---

_@MichaReiser reviewed on 2024-12-03 13:31_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/property_tests.rs`:144 on 2024-12-03 13:31_

Thanks. I like this much more!

---
