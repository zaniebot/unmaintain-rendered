```yaml
number: 19805
title: "[ty] support recursive type aliases"
type: pull_request
state: merged
author: carljm
labels:
  - bug
  - ty
assignees: []
merged: true
base: main
head: cjm/recta
created_at: 2025-08-07T12:39:01Z
updated_at: 2025-08-12T16:07:47Z
url: https://github.com/astral-sh/ruff/pull/19805
synced_at: 2026-01-12T15:56:47Z
```

# [ty] support recursive type aliases

---

_@carljm_

## Summary

Support recursive type aliases by adding a `Type::TypeAlias` type variant, which allows referring to a type alias directly as a type without eagerly unpacking it to its value.

We still unpack type aliases when they are added to intersections and unions, so that we can simplify the intersection/union appropriately based on the unpacked value of the type alias.

This introduces new possible recursive types, and so also requires expanding our usage of recursion-detecting visitors in Type methods. The use of these visitors is still not fully comprehensive in this PR, and will require further expansion to support recursion in more kinds of types (I already have further work on this locally), but I think it may be better to do this incrementally in multiple PRs.

## Test Plan

Added some recursive type-alias tests and made them pass.


---

_Label `ty` added by @carljm on 2025-08-07 12:39_

---

_Comment by @github-actions[bot] on 2025-08-07 12:41_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-12 00:50:16.219875316 +0000
+++ new-output.txt	2025-08-12 00:50:16.282875419 +0000
@@ -1,4 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/940f9c0/src/function/execute.rs:204:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(dc1d)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -67,23 +68,6 @@
 aliases_type_statement.py:48:23: error[invalid-type-form] Boolean operations are not allowed in type expressions
 aliases_type_statement.py:49:23: error[fstring-type-annotation] Type expressions cannot use f-strings
 aliases_type_statement.py:80:37: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:32:7: error[unresolved-attribute] Type `typing.TypeAliasType` has no attribute `other_attrib`
-aliases_typealiastype.py:39:26: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:52:40: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_typealiastype.py:53:40: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:54:42: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression
-aliases_typealiastype.py:54:43: error[invalid-type-form] Tuple literals are not allowed in this context in a type expression: Did you mean `tuple[int, str]`?
-aliases_typealiastype.py:55:42: error[invalid-type-form] List comprehensions are not allowed in type expressions
-aliases_typealiastype.py:56:42: error[invalid-type-form] Dict literals are not allowed in type expressions
-aliases_typealiastype.py:57:42: error[invalid-type-form] Function calls are not allowed in type expressions
-aliases_typealiastype.py:58:48: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_typealiastype.py:59:42: error[invalid-type-form] `if` expressions are not allowed in type expressions
-aliases_typealiastype.py:60:42: error[invalid-type-form] Variable of type `Literal[3]` is not allowed in a type expression
-aliases_typealiastype.py:61:42: error[invalid-type-form] Boolean literals are not allowed in this context in a type expression
-aliases_typealiastype.py:62:42: error[invalid-type-form] Int literals are not allowed in this context in a type expression
-aliases_typealiastype.py:63:42: error[invalid-type-form] Boolean operations are not allowed in type expressions
-aliases_typealiastype.py:64:42: error[invalid-type-form] F-strings are not allowed in type expressions
-aliases_typealiastype.py:66:47: error[unresolved-reference] Name `BadAlias21` used when not defined
 aliases_variance.py:18:24: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
 aliases_variance.py:28:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassA[typing.TypeVar("T_co")]'>` with no `__class_getitem__` method
 aliases_variance.py:44:16: error[non-subscriptable] Cannot subscript object of type `<class 'ClassB[typing.TypeVar("T_co"), typing.TypeVar("T_contra")]'>` with no `__class_getitem__` method
@@ -884,4 +868,5 @@
 typeddicts_operations.py:60:1: error[type-assertion-failure] Argument does not have asserted type `str | None`
 typeddicts_type_consistency.py:101:1: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `str`
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 885 diagnostics
+Found 869 diagnostics
+WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-07 12:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/contentviews/test__utils.py:23:38: error[invalid-argument-type] Argument to function `make_metadata` is incorrect: Expected `Message | TCPMessage | UDPMessage | WebSocketMessage | DNSMessage`, found `Response | None`
+ test/mitmproxy/contentviews/test__utils.py:23:38: error[invalid-argument-type] Argument to function `make_metadata` is incorrect: Expected `ContentviewMessage`, found `Response | None`

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @carljm on 2025-08-07 12:59_

---

_Review requested from @AlexWaygood by @carljm on 2025-08-07 12:59_

---

_Review requested from @sharkdp by @carljm on 2025-08-07 12:59_

---

_Review requested from @dcreager by @carljm on 2025-08-07 12:59_

---

_Converted to draft by @carljm on 2025-08-07 13:04_

---

_Comment by @carljm on 2025-08-07 13:04_

Converting back to draft while I look into the new panic on the conformance suite.

---

_Comment by @carljm on 2025-08-07 22:22_

Ok, the new panic in the conformance suite is for an invalid type alias, so it shouldn't be a case that affects much if any real code (as seen in the fact that there aren't new ecosystem panics). I have a plan for how to fix it (as well as fixing recursive legacy/bare type aliases), but it will be significant enough that I'd rather do it in a separate PR. So putting this back up for review as-is.

---

_Marked ready for review by @carljm on 2025-08-07 22:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/builder.rs`:207 on 2025-08-08 09:27_

I think it would be beneficial to have documentation outlining the different strategies and their respective needs and when to use which one

---

_Comment by @MichaReiser on 2025-08-08 09:31_

Exciting!

I assume this change is because we no longer normalize the argument union:

```
mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/contentviews/test__utils.py:23:38: error[invalid-argument-type] Argument to function `make_metadata` is incorrect: Expected `Message | TCPMessage | UDPMessage | WebSocketMessage | DNSMessage`, found `Response | None`
+ test/mitmproxy/contentviews/test__utils.py:23:38: error[invalid-argument-type] Argument to function `make_metadata` is incorrect: Expected `ContentviewMessage`, found `Response | None`
```

I must say, I very much prefer the new output (but agree, that changing this in general is out of scope for this PR)

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/tuple.rs`:780 on 2025-08-08 09:50_

Looking at `CycleDetector`, making `visit` use a `&self` sounds fairly reasonable, given that the caching and `seen` aren't exposed (they're all handled inside `visit`). 
It also feels idiomatic that caching and seen would be "transparant" from the outside. 

The alternative is to change `Self::mixed` to take ` Vec<Type>` so that we at least can reuse the collection (because it always collects anyway)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:171 on 2025-08-08 09:50_

Ideally we'd complain about this at the point where it's defined, right? To warn the user that we might not interpret this in the way they expect, and that they've likely made a mistake in their type annotations?

Interpreting it as simplifying to `int` seems reasonable (though falling back to `Unknown` would also be fine here, I think), but the alias still doesn't really make any sense. Should we add a TODO about this?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:171 on 2025-08-08 09:51_

The name of this alias reminds me of a song from one of my favourite albums https://www.youtube.com/watch?v=cPbNpIG8x_s&ab_channel=Love-Topic

---

_@MichaReiser approved on 2025-08-08 09:52_

---

_Label `bug` added by @MichaReiser on 2025-08-08 09:52_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:8714 on 2025-08-08 10:06_

You could possibly simplify the `from_elements_impl` signature here a bit -- it would make it less ergonomic for callers, but there are only two callers, and having fewer signatures with complicated trait bounds seems like a nice win:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 763d153102..d72be366da 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -8687,23 +8687,22 @@ impl<'db> UnionType<'db> {
         I: IntoIterator<Item = T>,
         T: Into<Type<'db>>,
     {
-        Self::from_elements_impl(db, elements, UnionStrategy::EliminateSubtypes)
+        Self::from_elements_impl(
+            db,
+            elements.into_iter().map(Into::into),
+            UnionStrategy::EliminateSubtypes,
+        )
     }
 
-    pub(crate) fn from_elements_impl<I, T>(
+    pub(crate) fn from_elements_impl(
         db: &'db dyn Db,
-        elements: I,
+        elements: impl Iterator<Item = Type<'db>>,
         strategy: UnionStrategy,
-    ) -> Type<'db>
-    where
-        I: IntoIterator<Item = T>,
-        T: Into<Type<'db>>,
-    {
+    ) -> Type<'db> {
         elements
-            .into_iter()
             .fold(
                 UnionBuilder::new(db).strategy(strategy),
-                |builder, element| builder.add(element.into()),
+                builder::UnionBuilder::add,
             )
             .build()
     }
diff --git a/crates/ty_python_semantic/src/types/tuple.rs b/crates/ty_python_semantic/src/types/tuple.rs
index 8bcacbb5bb..f24699b96a 100644
--- a/crates/ty_python_semantic/src/types/tuple.rs
+++ b/crates/ty_python_semantic/src/types/tuple.rs
@@ -1134,7 +1134,7 @@ impl<'db> Tuple<Type<'db>> {
         db: &'db dyn Db,
         union_strategy: UnionStrategy,
     ) -> Type<'db> {
-        UnionType::from_elements_impl(db, self.all_elements(), union_strategy)
+        UnionType::from_elements_impl(db, self.all_elements().map(Type::from), union_strategy)
     }
 
     /// Concatenates another tuple to the end of this tuple, returning a new tuple.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:207 on 2025-08-08 10:07_

```suggestion
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
pub(crate) enum UnionStrategy {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:213 on 2025-08-08 10:07_

```suggestion
    pub(crate) const fn should_eliminate_subtypes(self) -> bool {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:454 on 2025-08-08 10:10_

I'm surprised the equivalence check here is safe if the subtype check isn't. Doesn't the `is_equivalent_to` implementation also involve evaluating the inner value?

It seems like it should be fine to also avoid simplifying equivalent types out of unions -- we could probably replace this with a naive `==` check and be fine?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:249 on 2025-08-08 10:11_

```suggestion
                f.write_str(&alias.name(self.db))
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:7157 on 2025-08-08 10:16_

nit: I think our pattern elsewhere has generally been to use the `_type` prefix for methods like this rather than `_impl`. E.g. `infer_binary_expression` calls `infer_binary_expression_type` which is an inner method that recursively calls itself; `infer_compare_expression` calls `infer_binary_type_comparison` (not a prefix, but a similar-ish principle!)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/visitor.rs`:228 on 2025-08-08 10:20_

Should we add a `visit_type_alias_type` to the trait so that concrete implementations of the trait have it available as a hook?

---

_@AlexWaygood approved on 2025-08-08 10:20_

Looks great!!

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:1 on 2025-08-08 12:36_

Awesome

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1761 on 2025-08-08 12:50_

Do the other recursive calls to `is_equivalent_to` in this method need to be updated to use `is_equivalent_to_impl` instead?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:1336 on 2025-08-08 12:53_

Is there a rule of thumb (ideally documented) for when you can make the recursive call directly, and when it needs to be guarded by `visitor.visit`? My guess would be that you only need the visitor guard on the variants where a cycle would connect back to itself?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/tuple.rs`:780 on 2025-08-08 13:03_

> The alternative is to change `Self::mixed` to take ` Vec<Type>` so that we at least can reuse the collection (because it always collects anyway)

+1 to this — `Self::mixed` is purely internal so you can make the callers responsible for collecting, and then you avoid this issue entirely.

---

_@dcreager approved on 2025-08-08 13:05_

---

_@carljm reviewed on 2025-08-11 20:54_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/tuple.rs`:780 on 2025-08-11 20:54_

Going with the change to `Self::mixed` for now, as that's more localized; we can consider interior mutability for `TypeTransformer` / `CycleDetector` as a separate change.

---

_@carljm reviewed on 2025-08-11 22:14_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/tuple.rs`:780 on 2025-08-11 22:14_

I lied, I decided that making visitors use interior mutability is a better long-term answer, and if we're doing that anyway, then the change to `mixed` doesn't really make sense (just makes all the call sites more verbose). https://github.com/astral-sh/ruff/pull/19871 looks like an improvement to me.

---

_@carljm reviewed on 2025-08-11 22:15_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/pep695_type_aliases.md`:171 on 2025-08-11 22:15_

Yes, I think ideally we'd complain about this here, but I don't think it's high-priority; I'll add a TODO

---

_@carljm reviewed on 2025-08-12 00:04_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:8714 on 2025-08-12 00:04_

I removed this altogether in the latest version of the PR.

---

_@carljm reviewed on 2025-08-12 00:05_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:454 on 2025-08-12 00:05_

In the latest version of the PR, I no longer need the new UnionStrategy, so I've removed it; we can add it back in and consider it separately for UX or perf (not cycle) reasons.

It's no longer needed because now `TupleType::into_class_type` is a Salsa query, and cycle handling on that query means we no longer need to avoid the union simplification.

---

_@carljm reviewed on 2025-08-12 00:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:213 on 2025-08-12 00:06_

No longer present.

---

_@carljm reviewed on 2025-08-12 00:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:207 on 2025-08-12 00:06_

No longer present.

---

_@carljm reviewed on 2025-08-12 00:26_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/tuple.rs`:780 on 2025-08-12 00:26_

This is now rebased on top of that change.

---

_Comment by @codspeed-hq[bot] on 2025-08-12 00:39_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Frecta?runnerMode=WallTime)

### Merging #19805 will **not alter performance**

<sub>Comparing <code>cjm/recta</code> (0ca632a) with <code>main</code> (d76fd10)</sub>



### Summary

`✅ 8` untouched benchmarks  





---

_@carljm reviewed on 2025-08-12 00:49_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1336 on 2025-08-12 00:49_

I added some documentation on this to the top of `types/cyclic.rs`.

---

_Comment by @carljm on 2025-08-12 14:55_

> I assume this change is because we no longer normalize the argument union

No, this change is because we no longer eagerly unpack type aliases.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:1761 on 2025-08-12 15:29_

Yes, at some point. There are also some places in `has_relation_to_impl` that need this update. I'd been debating whether to fully thread through the visitors everywhere in this PR, or do it as a follow-up PR, with added regression tests.

---

_@carljm reviewed on 2025-08-12 15:29_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/tuple.rs`:226 on 2025-08-12 15:44_

the fact that static-frame is the most significant benchmark regression makes me suspect that this has something to do with the performance regressions. We [know](https://github.com/astral-sh/ruff/pull/19844) that this method is extremely hot when checking static-frame for some reason (https://github.com/astral-sh/ruff/pull/19811#issuecomment-3168844143, https://github.com/astral-sh/ruff/pull/19811#issuecomment-3169041607), so if there's now a chance that we'll sometimes have to recover from cycles when calling this function, that seems likely to slow down static-frame quite a bit. This also means that the static-frame repo isn't necessarily representative of most real-world code, however; it seems like a pretty extreme case.

---

_@AlexWaygood reviewed on 2025-08-12 15:45_

---

_@carljm reviewed on 2025-08-12 15:55_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/builder.rs`:207 on 2025-08-12 15:55_

`UnionStrategy` is no longer part of this PR

---

_@carljm reviewed on 2025-08-12 15:59_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/tuple.rs`:226 on 2025-08-12 15:59_

Yeah, this was one thing I've looked into a bit. I've confirmed (via Salsa tracing) that in checking static frame, we don't ever actually hit a cycle on this query (which is not surprising; if we did, then we likely would have panicked on static-frame before this PR). It's possible that there is still some effect simply from having the cycle handlers in place (Salsa does go into a different path for executing such queries), though I don't recall seeing a visible effect from that when we've added cycle handling to queries in the past. Even if it's very hot, presumably most of those accesses are cached; in that case, cycle handling shouldn't cause any overhead at all.

If this is the cause, then I think we just have to eat it, pending generalized improvements to the efficiency of Salsa cycle handling.

---

_Comment by @carljm on 2025-08-12 16:01_

Perf regression seems reduced to the 1-3% range. Possible causes are either overhead from adding cycle handling to a hot query, or overhead from passing around CycleDetector visitors through many more hot methods (`has_relation_to` in particular) -- I suspect it's more the latter. Either way, though, I think this is an acceptable level of regression, and one we will have to accept in order to gain recursive type support. So I'm ready to land this.

PR has changed somewhat, but mostly by removing `UnionStrategy`, so I'm going to go with the approvals I've already got -- post-land review of the more recent changes is welcome!

---

_Merged by @carljm on 2025-08-12 16:03_

---

_Closed by @carljm on 2025-08-12 16:03_

---

_Branch deleted on 2025-08-12 16:03_

---

_Comment by @charliermarsh on 2025-08-12 16:07_

Let's go!

---
