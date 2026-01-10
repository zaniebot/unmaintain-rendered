```yaml
number: 18901
title: "[ty] Add property test generators for variable-length tuples"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/proptest-tuple
created_at: 2025-06-23T17:38:53Z
updated_at: 2025-06-24T22:13:50Z
url: https://github.com/astral-sh/ruff/pull/18901
synced_at: 2026-01-10T18:39:09Z
```

# [ty] Add property test generators for variable-length tuples

---

_Pull request opened by @dcreager on 2025-06-23 17:38_

Add property test generators for the new variable-length tuples. This covers homogeneous tuples as well.

The property tests did their job! This identified several fixes we needed to make to various type property methods.

cf https://github.com/astral-sh/ruff/pull/18600#issuecomment-2993764471

---

_Review requested from @carljm by @dcreager on 2025-06-23 17:38_

---

_Review requested from @AlexWaygood by @dcreager on 2025-06-23 17:38_

---

_Review requested from @sharkdp by @dcreager on 2025-06-23 17:38_

---

_Label `internal` added by @dcreager on 2025-06-23 17:39_

---

_Label `ty` added by @dcreager on 2025-06-23 17:39_

---

_Comment by @github-actions[bot] on 2025-06-23 17:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
paasta (https://github.com/yelp/paasta)
+ error[unresolved-reference] paasta_tools/paastaapi/model/instance_status_adhoc.py:152:30: Name `_path_to_item` used when not defined
+ error[unresolved-reference] paasta_tools/paastaapi/model/instance_tasks.py:147:30: Name `_path_to_item` used when not defined
+ error[unresolved-reference] paasta_tools/paastaapi/model/resource.py:152:30: Name `_path_to_item` used when not defined
- Found 934 diagnostics
+ Found 937 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[unresolved-attribute] strawberry/tools/merge_types.py:26:11: Type `type` has no attribute `__strawberry_definition__`
- Found 372 diagnostics
+ Found 371 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ warning[possibly-unbound-attribute] src/hydra_zen/structured_configs/_implementations.py:2146:28: Attribute `get` on type `DataclassOptions | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/hydra_zen/structured_configs/_implementations.py:3153:21: Attribute `get` on type `DataclassOptions | None` is possibly unbound
- Found 595 diagnostics
+ Found 597 diagnostics

jinja (https://github.com/pallets/jinja)
+ warning[possibly-unresolved-reference] src/jinja2/filters.py:1740:17: Name `name` used when possibly not defined
- Found 205 diagnostics
+ Found 206 diagnostics

vision (https://github.com/pytorch/vision)
+ error[invalid-assignment] test/datasets_utils.py:1043:9: Object of type `LiteralString` is not assignable to `tuple[str, ...]`
- Found 1510 diagnostics
+ Found 1511 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ error[no-matching-overload] src/_pytest/raises.py:283:20: No overload of bound method `__init__` matches arguments
- Found 640 diagnostics
+ Found 641 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
+ warning[possibly-unbound-attribute] aiohttp/resolver.py:102:26: Attribute `get_resolver` on type `_DNSResolverManager | None` is possibly unbound
- Found 136 diagnostics
+ Found 137 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2100:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `EllipsisType`
+ error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2103:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `EllipsisType`
+ error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2106:43: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `EllipsisType`
+ error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2109:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `EllipsisType`
+ error[invalid-argument-type] static_frame/test/unit/test_type_clinic.py:2131:29: Argument to bound method `__init__` is incorrect: Expected `Sequence[Unknown]`, found `EllipsisType`
- Found 1833 diagnostics
+ Found 1838 diagnostics

sympy (https://github.com/sympy/sympy)
+ warning[possibly-unbound-attribute] sympy/calculus/euler.py:84:16: Attribute `args` on type `@Todo(Subscript expressions on intersections) | tuple[()]` is possibly unbound
+ warning[possibly-unbound-attribute] sympy/calculus/euler.py:92:24: Attribute `args` on type `@Todo(map_with_boundness: intersections with negative contributions) | tuple[()]` is possibly unbound
+ error[index-out-of-bounds] sympy/polys/polymatrix.py:79:42: Index 0 is out of bounds for tuple `tuple[()]` with length 0
+ error[index-out-of-bounds] sympy/polys/polymatrix.py:80:20: Index 0 is out of bounds for tuple `tuple[()]` with length 0
- error[not-iterable] sympy/simplify/radsimp.py:707:28: Object of type `Basic` is not iterable
+ error[not-iterable] sympy/simplify/radsimp.py:707:28: Object of type `set[Unknown] | Basic` may not be iterable
- error[invalid-argument-type] sympy/solvers/solveset.py:3128:25: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `tuple[Unknown, ...] | (Unknown & <Protocol with members '__iter__'>)`
+ error[invalid-argument-type] sympy/solvers/solveset.py:3128:25: Argument to function `next` is incorrect: Expected `SupportsNext[Unknown]`, found `tuple[Unknown, ...] | (@Todo(Subscript expressions on intersections) & <Protocol with members '__iter__'>)`
- Found 17915 diagnostics
+ Found 17919 diagnostics

```
</details>


---

_Label `internal` removed by @MichaReiser on 2025-06-23 18:43_

---

_Label `testing` added by @MichaReiser on 2025-06-23 18:43_

---

_@carljm reviewed on 2025-06-23 21:42_

Looks good! But I am seeing some property test failures here (see below). We need to either fix the underlying problems, or create issues for them and move those property tests into the "flaky" section (but that would be less preferable). I think some of these are not OK to move into "flaky" (subtyping transitivity in particular.)

Repro: `QUICKCHECK_TESTS=1000000 cargo test -p ty_python_semantic -- --ignored types::property_tests::stable`

(Running with a million iterations like this is pretty slow, but is the standard we need to hit in order to not get flaky CI failures on the property tests.)

Failures:

```
failures:

---- types::property_tests::stable::all_type_pairs_are_assignable_to_their_union stdout ----

thread 'types::property_tests::stable::all_type_pairs_are_assignable_to_their_union' panicked at /Users/carlmeyer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (VariableLengthTuple([], Intersection { pos: [None, SubclassOfAbcClass("ABC")], neg: [] }, []), Union([FixedLengthTuple([]), VariableLengthTuple([AlwaysTruthy], KnownClassInstance(Str), [])]))
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace

---- types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union stdout ----

thread 'types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union' panicked at /Users/carlmeyer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (VariableLengthTuple([], BytesLiteral("\0"), []), Union([FixedLengthTuple([]), AlwaysTruthy]))

---- types::property_tests::stable::subtype_of_is_transitive stdout ----

thread 'types::property_tests::stable::subtype_of_is_transitive' panicked at /Users/carlmeyer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (FixedLengthTuple([]), VariableLengthTuple([], BuiltinClassLiteral("bool"), []), VariableLengthTuple([BuiltinInstance("type")], Intersection { pos: [], neg: [] }, []))

---- types::property_tests::stable::subtype_of_implies_not_disjoint_from stdout ----

thread 'types::property_tests::stable::subtype_of_implies_not_disjoint_from' panicked at /Users/carlmeyer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (Intersection { pos: [FixedLengthTuple([])], neg: [VariableLengthTuple([], BytesLiteral(""), [SubclassOfAbcClass("ABC")])] }, VariableLengthTuple([], VariableLengthTuple([], KnownClassInstance(Int), [Never]), []))

---- types::property_tests::stable::subtype_of_is_antisymmetric stdout ----

thread 'types::property_tests::stable::subtype_of_is_antisymmetric' panicked at /Users/carlmeyer/.cargo/registry/src/index.crates.io-1949cf8c6b5b557f/quickcheck-1.0.3/src/tester.rs:165:28:
[quickcheck] TEST FAILED. Arguments: (VariableLengthTuple([], FixedLengthTuple([]), [FixedLengthTuple([])]), VariableLengthTuple([], FixedLengthTuple([]), []))


failures:
    types::property_tests::stable::all_fully_static_type_pairs_are_subtype_of_their_union
    types::property_tests::stable::all_type_pairs_are_assignable_to_their_union
    types::property_tests::stable::subtype_of_implies_not_disjoint_from
    types::property_tests::stable::subtype_of_is_antisymmetric
    types::property_tests::stable::subtype_of_is_transitive
```

---

_Comment by @carljm on 2025-06-23 21:45_

(Based on my experience debugging these property test failures recently, I recommend starting with the `subtype_of_is_transitive` failure, because failures of subtyping transitivity can lead to all kinds of other cascading problems where union simplification depends on the order elements are added, making other property test failures sensitive to ordering in ways that make them slippery to reproduce.)

---

_Comment by @dcreager on 2025-06-24 12:51_

> Running with a million iterations like this is pretty slow, but is the standard we need to hit in order to not get flaky CI failures on the property tests

Ha! I ran it yesterday with around 500,000 runs and didn't see any failures. I ran it this morning to reproduce your results and got a failure with the default of 100 :joy: 

---

_Comment by @dcreager on 2025-06-24 19:13_

Okay it's been running on a loop for around 15 minutes now and I'm not seeing any more proptest failures.

---

_Comment by @codspeed-hq[bot] on 2025-06-24 19:23_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fproptest-tuple?runnerMode=WallTime)

### Merging #18901 will **not alter performance**

<sub>Comparing <code>dcreager/proptest-tuple</code> (35a675f) with <code>main</code> (919af96)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_Comment by @AlexWaygood on 2025-06-24 19:30_

Are the new primer diagnostics here expected?

---

_Comment by @dcreager on 2025-06-24 19:48_

> Are the new primer diagnostics here expected?

My hope is they're related to the test failure that I just fixed

---

_Comment by @AlexWaygood on 2025-06-24 19:55_

> > Are the new primer diagnostics here expected?
> 
> My hope is they're related to the test failure that I just fixed

looks like we still have a few new ones

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3475 on 2025-06-24 20:41_

```suggestion
                // The tuple type is AlwaysTrue if its inhabitants must always have length >=1
                (minimum, _) if minimum > 0 => Truthiness::AlwaysTrue,
                // The tuple type is Ambiguous if its inhabitants could be of any length
                _ => Truthiness::Ambiguous
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:366 on 2025-06-24 20:43_

this is a great observation! 😃

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:414 on 2025-06-24 20:43_

nit: this is quite a complicated type to repeat twice in the same file. Maybe introduce a type alias?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:404 on 2025-06-24 20:46_

I feel like I understand all the words here but don't understand how they relate to the method, or what this method would be practically used for :-(

Why do we need to "prenormalize" tuples? What's the meaning of the `skip_while` call in the method body?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:864 on 2025-06-24 21:04_

```suggestion
        if let ((minimum, _), (_, Some(maximum))) | ((_, Some(maximum)), (minimum, _)) =
            (self.size_hint(), other.size_hint())
        {
            if maximum < minimum {
                return true;
            }
        }
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:193 on 2025-06-24 21:12_

aaaaaah, this is what the "prenormalization" stuff in `tuple.rs` is for? Serves me right for reading the code before the tests, I guess 😆 though I still feel like we could use more (and clearer) comments alongside the code.

Another really cool observation that `tuple[int, *tuple[int, ...]]` describes exactly the same set of possible runtime objects as `tuple[*tuple[int, ...], int]` 😃

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:207 on 2025-06-24 21:13_

I think `type[object] == type` is not the _best_ example of equivalent types here, since we eagerly normalize `type[object]` to `type`. `(int | str) == (str | int)` might be a better example, since we only lazily normalize unions when we check equivalence?

---

_@AlexWaygood reviewed on 2025-06-24 21:14_

Very cool work

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:348 on 2025-06-24 21:18_

What about testing truthiness of mixed tuples with a prefix/suffix and a variable-length portion? They should be always truthy, but I don't see any tests for it here.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:304 on 2025-06-24 21:19_

I think it would be worth a note here like this:

```suggestion
The truthiness of the empty tuple is `False`.

(TODO: this is only true as long as we assume that a subclass of `tuple` doesn't override `__bool__` or `__len__` -- we should enforce this.)
```

---

_Comment by @dcreager on 2025-06-24 21:19_

> looks like we still have a few new ones

Most of them look like new true positives!

This one is a false positive:

```
error[index-out-of-bounds] sympy/polys/polymatrix.py:79:42: Index 0 is out of bounds for tuple `tuple[()]` with length 0
```

There are a bunch of unannotated assignments to a variable in an `if/elif/...` chain. The first branch assigns it `()`, but other branches assign it a list slice, so I'm not sure why only the empty tuple is making it to this point. (There's also a length guard which is something we don't support yet, https://github.com/astral-sh/ty/issues/560)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:414 on 2025-06-24 21:22_

Unfortunately it's not a type, it's an `impl` trait(s), so I don't think I can make an alias for it.  

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3475 on 2025-06-24 21:22_

I don't think we should use the word "inhabitants" in this way; we usually use that word to refer to runtime values that "inhabit" types. If we need to refer to the parts of a tuple, we could use "elements". But I'm not sure we need to; why can't we just say "The tuple type is AlwaysFalse if it can only have a length of 0", "The tuple type is AlwaysTrue if it must have a length >= 1", etc.?

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:207 on 2025-06-24 21:26_

Done

---

_@carljm approved on 2025-06-24 21:28_

Nice!

---

_@AlexWaygood reviewed on 2025-06-24 21:31_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:3475 on 2025-06-24 21:31_

> I don't think we should use the word "inhabitants" in this way; we usually use that word to refer to runtime values that "inhabit" types.

I don't understand this objection -- isn't that exactly how I'm using the word? The tuple type is `AlwaysTrue` if all concrete inhabitants of that tuple type -- e.g. `(1, 2)`, `(1, 2, 3)` -- all have length >=1. The length of that first tuple has 2; the length of that second tuple is 3; both tuples are runtime values that "inhabit" certain tuple types?

---

_@carljm reviewed on 2025-06-24 21:33_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:3475 on 2025-06-24 21:33_

Sorry, I misunderstood your wording! Yes, I think it's accurate

---

_Label `testing` removed by @AlexWaygood on 2025-06-24 21:34_

---

_@AlexWaygood reviewed on 2025-06-24 21:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:863 on 2025-06-24 21:36_

I don't totally understand why this doesn't work when your previous `match` statement did (they seem like they should produce equivalent code!) -- but I think this should fix the regression caused by my previous suggestion, and it seems more obviously correct as well!

```suggestion
        let (self_min, self_max) = self.size_hint();
        let (other_min, other_max) = other.size_hint();
        if self_max.is_some_and(|max|max < other_min) {
            return true;
        }
        if other_max.is_some_and(|max|max < self_min) {
            return true;
        }
```

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:404 on 2025-06-24 21:44_

Added a (hopefully) better comment

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:193 on 2025-06-24 21:45_

see above — hopefully better comment up at the code

---

_@dcreager reviewed on 2025-06-24 21:45_

---

_@AlexWaygood reviewed on 2025-06-24 21:46_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:404 on 2025-06-24 21:46_

it's *much* better, thank you!!

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:348 on 2025-06-24 21:49_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:304 on 2025-06-24 21:51_

There is a note at the bottom of the section mentioning this (along with a test that shows we currently allow a subclass that overrides `__bool__`).  Is that sufficient, or do you think it needs a note here too?

---

_@dcreager reviewed on 2025-06-24 21:51_

---

_@AlexWaygood reviewed on 2025-06-24 21:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:414 on 2025-06-24 21:53_

you could maybe just use the concrete type instead of the trait, and then you can alias it? This seems to compile fine:

```diff
diff --git a/crates/ty_python_semantic/src/types/tuple.rs b/crates/ty_python_semantic/src/types/tuple.rs
index d19666fcd2..5d264a35f5 100644
--- a/crates/ty_python_semantic/src/types/tuple.rs
+++ b/crates/ty_python_semantic/src/types/tuple.rs
@@ -154,6 +154,8 @@ impl<'db> TupleType<'db> {
     }
 }
 
+type TupleElementIterator<'a, 'db> = std::iter::Copied<std::slice::Iter<'a, Type<'db>>>;
+
 /// A fixed-length tuple spec.
 ///
 /// Tuple specs are used for more than just `tuple` instances, so they allow `Never` to appear as a
@@ -179,9 +181,7 @@ impl<'db> FixedLengthTupleSpec<'db> {
         &self.0
     }
 
-    pub(crate) fn elements(
-        &self,
-    ) -> impl DoubleEndedIterator<Item = Type<'db>> + ExactSizeIterator + '_ {
+    pub(crate) fn elements(&self) -> TupleElementIterator<'_, 'db> {
         self.0.iter().copied()
     }
 
@@ -372,9 +372,7 @@ impl<'db> VariableLengthTupleSpec<'db> {
         })
     }
 
-    fn prefix_elements(
-        &self,
-    ) -> impl DoubleEndedIterator<Item = Type<'db>> + ExactSizeIterator + '_ {
+    fn prefix_elements(&self) -> TupleElementIterator<'_, 'db> {
         self.prefix.iter().copied()
     }
 
@@ -409,9 +407,7 @@ impl<'db> VariableLengthTupleSpec<'db> {
         )
     }
 
-    fn suffix_elements(
-        &self,
-    ) -> impl DoubleEndedIterator<Item = Type<'db>> + ExactSizeIterator + '_ {
+    fn suffix_elements(&self) -> TupleElementIterator<'_, 'db> {
         self.suffix.iter().copied()
     }
```

---

_@AlexWaygood reviewed on 2025-06-24 21:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:304 on 2025-06-24 21:54_

(I'm also planning on tackling this this week or next as part of my tuple subclasses/NamedTuples work)

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:414 on 2025-06-24 21:55_

I mildly prefer the `impl` trait, since I don't like encoding concrete types into the function signature if we don't need to name them for some other reason.  But I can make this change if you feel strongly.

---

_@dcreager reviewed on 2025-06-24 21:55_

---

_@AlexWaygood reviewed on 2025-06-24 22:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:414 on 2025-06-24 22:04_

I guess you could do something ✨fancy✨ like this, but it probably isn't worth it just to make some type signatures less complicated 😆

```diff
diff --git a/crates/ty_python_semantic/src/types/tuple.rs b/crates/ty_python_semantic/src/types/tuple.rs
index d19666fcd2..5c1c7b09e1 100644
--- a/crates/ty_python_semantic/src/types/tuple.rs
+++ b/crates/ty_python_semantic/src/types/tuple.rs
@@ -154,6 +154,16 @@ impl<'db> TupleType<'db> {
     }
 }
 
+pub(crate) trait TupleElementIterator<'db>:
+    ExactSizeIterator<Item = Type<'db>> + DoubleEndedIterator
+{
+}
+
+impl<'db, T> TupleElementIterator<'db> for T where
+    T: ExactSizeIterator<Item = Type<'db>> + DoubleEndedIterator
+{
+}
+
 /// A fixed-length tuple spec.
 ///
 /// Tuple specs are used for more than just `tuple` instances, so they allow `Never` to appear as a
@@ -179,9 +189,7 @@ impl<'db> FixedLengthTupleSpec<'db> {
         &self.0
     }
 
-    pub(crate) fn elements(
-        &self,
-    ) -> impl DoubleEndedIterator<Item = Type<'db>> + ExactSizeIterator + '_ {
+    pub(crate) fn elements(&self) -> impl TupleElementIterator<'db> + '_ {
         self.0.iter().copied()
     }
 
@@ -372,9 +380,7 @@ impl<'db> VariableLengthTupleSpec<'db> {
         })
     }
 
-    fn prefix_elements(
-        &self,
-    ) -> impl DoubleEndedIterator<Item = Type<'db>> + ExactSizeIterator + '_ {
+    fn prefix_elements(&self) -> impl TupleElementIterator<'db> + '_ {
         self.prefix.iter().copied()
     }
 
@@ -409,9 +415,7 @@ impl<'db> VariableLengthTupleSpec<'db> {
         )
     }
 
-    fn suffix_elements(
-        &self,
-    ) -> impl DoubleEndedIterator<Item = Type<'db>> + ExactSizeIterator + '_ {
+    fn suffix_elements(&self) -> impl TupleElementIterator<'db> + '_ {
         self.suffix.iter().copied()
     }
```

Feel free to leave things as they are, I don't feel strongly!

---

_@carljm reviewed on 2025-06-24 22:05_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/type_compendium/tuple.md`:304 on 2025-06-24 22:05_

Oh that's probably sufficient if we have it mentioned somewhere

---

_@AlexWaygood approved on 2025-06-24 22:07_

nice!

---

_Merged by @dcreager on 2025-06-24 22:13_

---

_Closed by @dcreager on 2025-06-24 22:13_

---

_Branch deleted on 2025-06-24 22:13_

---
