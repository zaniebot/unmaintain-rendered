```yaml
number: 17396
title: "[red-knot] infer attribute assignments bound in comprehensions"
type: pull_request
state: merged
author: mtshiba
labels:
  - ty
assignees: []
merged: true
base: main
head: comprehension-attribute-assignment
created_at: 2025-04-14T17:05:47Z
updated_at: 2025-05-07T15:21:37Z
url: https://github.com/astral-sh/ruff/pull/17396
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] infer attribute assignments bound in comprehensions

---

_Pull request opened by @mtshiba on 2025-04-14 17:05_

## Summary

This PR is a follow-up to astral-sh/ruff#16852.

Instance variables bound in comprehensions are recorded, allowing type inference to work correctly.

This required adding support for unpacking in comprehension which resolves https://github.com/astral-sh/ruff/issues/15369.

## Test Plan

One TODO in `mdtest/attributes.md` is now resolved, and some new test cases are added.

---

_Review requested from @carljm by @mtshiba on 2025-04-14 17:05_

---

_Review requested from @AlexWaygood by @mtshiba on 2025-04-14 17:05_

---

_Review requested from @sharkdp by @mtshiba on 2025-04-14 17:05_

---

_Review requested from @dcreager by @mtshiba on 2025-04-14 17:05_

---

_Comment by @github-actions[bot] on 2025-04-14 17:07_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/async-utils/_misc/_queue_testing.py:50:30: Operator `+` is unsupported between objects of type `Literal["\u{1b}[30;1m%(asctime)s\u{1b}[0m "]` and `tuple[Unknown, Literal["\u{1b}[40;1m"]] | tuple[Unknown, Literal["\u{1b}[34;1m"]] | tuple[Unknown, Literal["\u{1b}[33;1m"]] | tuple[Unknown, Literal["\u{1b}[31m"]] | tuple[Unknown, Literal["\u{1b}[41m"]]`
- Found 30 diagnostics
+ Found 29 diagnostics

```
</details>


---

_Label `red-knot` added by @AlexWaygood on 2025-04-14 17:24_

---

_Comment by @dhruvmanila on 2025-04-14 22:33_

By the looks of it, this PR resolves https://github.com/astral-sh/ruff/issues/15369 ?

---

_@carljm reviewed on 2025-04-14 22:35_

This looks really good!

As @dhruvmanila mentions, it seems like this adds general support for unpacking in comprehension targets, not only for discovering instance attributes? If so, can we add some tests specifically for unpacking in comprehensions, to regular Name targets?

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/unpack.rs`:53 on 2025-04-14 22:41_

I think we should be clear in which cases it's going to be different as there's just one case - first generator in a generator expression or list / dict / set comprehension

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:3909 on 2025-04-14 22:45_

I think the support for inferring the `ExprName` targets in an unpacking is missing. Is that intentional?

I think it'd be useful to add full support for unpacking in comprehension in this PR. You can look at what kind of update is required in `infer_for_statement_definition`:

https://github.com/astral-sh/ruff/blob/b5d529e976d08e9cc03099d803db7561a4b65ebe/crates/red_knot_python_semantic/src/types/infer.rs#L3091-L3106

---

_@dhruvmanila reviewed on 2025-04-14 22:45_

---

_@carljm reviewed on 2025-04-14 23:15_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3909 on 2025-04-14 23:15_

I'd also be OK with doing that part (and adding tests for unpacking to Name nodes in comprehensions) as a follow-up PR. But in that case it would be good to add TODOs in this PR, just in case the follow-up doesn't happen soon.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-15 16:17_

---

_Review requested from @carljm by @mtshiba on 2025-04-15 16:18_

---

_Review requested from @dhruvmanila by @mtshiba on 2025-04-15 16:18_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:946 on 2025-04-15 16:53_

We could add a `debug_assert` here to verify that the semantic model is in the `ScopeKind::Comprehension` if the `unpackable` is a comprehension otherwise this could possibly panic. Also, add a "SAFETY" comment after the debug assert.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:2148 on 2025-04-15 16:56_

I think we should document why this `unwrap` is safe which as far as I understand is because the targets in comprehensions should always introduce a new scope so the `scope` variable points to the outer scope relative to the comprehension scope. We could possibly add a similar `debug_assert` similar to my previous comment.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:780 on 2025-04-15 17:04_

nit: I'd usually avoid adding a `new` constructor method if it's mirroring the exact fields that the struct contains similar to how it's being done for constructing `*DefinitionNodeRef`

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:760 on 2025-04-15 17:09_

Unrelated to this PR but I think the `target` here is either a `ExprName` or `ExprAttribute`, so it might be useful to have an enum similar to `TargetKind` which has two variants like `TargetExpr::Name(ast::ExprName)` and `TargetExpr::Attribute(ast::ExprAttribute)`. This should not be done in this PR as it requires updating other attribute unpacking as well.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/unpack.rs`:55 on 2025-04-15 17:15_

```suggestion
	/// Returns the scope in which the unpack value expression belongs.
	/// 
	/// The scope in which the target and value expression belongs to are usually the same
	/// except in generator expressions and comprehensions (list/dict/set), where the value 
	/// expression of the first generator is evaluated in the outer scope, while the ones in the subsequent 
	/// generators are evaluated in the comprehension scope.
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/unpack.rs`:60 on 2025-04-15 17:16_

```suggestion
    /// Returns the scope where the unpack target expression belongs to.
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/comprehensions/basic.md`:50 on 2025-04-15 17:18_

Thanks for adding the test case but I think we should add comprehensive unpacking test cases specific to comprehensions in https://github.com/astral-sh/ruff/blob/e2a38e4c0090cee5c20bde86a8ca9b30bffb2cfa/crates/red_knot_python_semantic/resources/mdtest/unpacking.md as well similar to how it's done for `for` and `with` statement.

---

_@dhruvmanila reviewed on 2025-04-15 17:19_

This looks great. My comments are mainly around adding / updating comments and increasing the test coverage for unpacking in comprehensions.

---

_Review request for @sharkdp removed by @sharkdp on 2025-04-15 18:19_

---

_@mtshiba reviewed on 2025-04-16 15:00_

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:760 on 2025-04-16 15:00_

OK, I'll work on this in a separate PR (this would be a topic related to #16894).

We also need to support `ExprSubscript` as a target expression.

---

_@dhruvmanila reviewed on 2025-04-16 16:20_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:760 on 2025-04-16 16:20_

Just as a note it's not a priority to make this refactor, just something nice to have and is not necessarily required.

---

_Review requested from @MichaReiser by @mtshiba on 2025-04-16 16:22_

---

_Review requested from @dhruvmanila by @mtshiba on 2025-04-16 16:29_

---

_@carljm approved on 2025-04-16 23:54_

This looks good to me, but will let @dhruvmanila have another look and merge if happy with it.

---

_@dhruvmanila reviewed on 2025-04-17 18:25_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:3919 on 2025-04-17 18:25_

Shouldn't this just use the existing `infer_target` method? Or, why is a separate method required in this case?

---

_@dhruvmanila reviewed on 2025-04-17 18:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:3919 on 2025-04-17 18:26_

That method (`infer_target`) isn't correct currently as it has a bug (https://github.com/astral-sh/ruff/issues/16514) but we're aware of it.

---

_Review comment by @mtshiba on `crates/red_knot_python_semantic/src/types/infer.rs`:3919 on 2025-04-18 17:18_

I think we cannot simply replace `infer_comprehension_target` with `infer_target`.

Specifically, if we change the code as follows, the tests will fail as follows:

```diff
# infer.rs#L3897
-         self.infer_comprehension_target(target);
+         self.infer_target(target, iter, |db, iter_ty| {
+             iter_ty.iterate(db)
+         });
```

```txt
invalid.md - Tests for invalid types in type expressions - Invalid Collection based AST nodes

  crates\red_knot_python_semantic\resources\mdtest\annotations\invalid.md:98 panicked at crates\red_knot_python_semantic\src\types\infer.rs:561:9
  crates\red_knot_python_semantic\resources\mdtest\annotations\invalid.md:98 assertion `left == right` failed
  left: ScopeId { [salsa id]: Id(801), file: File(System("\\src\\mdtest_snippet.py")), file_scope_id: FileScopeId(1), count: Count { ghost: PhantomData<fn(red_knot_python_semantic::semantic_index::symbol::ScopeId)> } }
 right: ScopeId { [salsa id]: Id(800), file: File(System("\\src\\mdtest_snippet.py")), file_scope_id: FileScopeId(0), count: Count { ghost: PhantomData<fn(red_knot_python_semantic::semantic_index::symbol::ScopeId)> } }

---

attributes.md - Attributes - Class and instance variables - Pure instance variables - Attributes defined in comprehensions

  crates\red_knot_python_semantic\resources\mdtest\attributes.md:392 panicked at crates\red_knot_python_semantic\src\types\infer.rs:561:9
  crates\red_knot_python_semantic\resources\mdtest\attributes.md:392 assertion `left == right` failed
  left: ScopeId { [salsa id]: Id(37ac), file: File(System("\\src\\mdtest_snippet.py")), file_scope_id: FileScopeId(11), count: Count { ghost: PhantomData<fn(red_knot_python_semantic::semantic_index::symbol::ScopeId)> } }
 right: ScopeId { [salsa id]: Id(37a9), file: File(System("\\src\\mdtest_snippet.py")), file_scope_id: FileScopeId(10), count: Count { ghost: PhantomData<fn(red_knot_python_semantic::semantic_index::symbol::ScopeId)> } }
```

`TypeInferenceBuilder::extend` fails because the scope of the comprehension and the scope of the value=iter are different.

~~However, since the implementations of `infer_comprehension_target` and `infer_target_impl` are almost the same, I will use this instead.~~

It would be better if `infer_target` supported comprehension.

---

_@mtshiba reviewed on 2025-04-18 17:18_

---

_@dhruvmanila reviewed on 2025-04-19 01:05_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:3919 on 2025-04-19 01:05_

> It would be better if `infer_target` supported comprehension.

Agreed. Thanks for making the change. I think the logic to infer the value expression using `infer_same_file_expression_type` is only to be done for the first comprehension, right? I think we could update the `infer_target` specifically the `to_assigned_ty` to instead be `infer_value_expr` so that the closure is responsible to infer the (non `Name` node) `value` expression so that we can check whether `is_first` is true inside the `infer_comprehension`.

I do have to change the logic of `infer_target` a bit to fix the bug as mentioned in https://github.com/astral-sh/ruff/issues/16514 but that's something for later.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:3919 on 2025-04-19 01:07_

I've pushed [`329a0f2` (#17396)](https://github.com/astral-sh/ruff/pull/17396/commits/329a0f278bfe5cc319a35c18e3315c76f7ae3cde) to reflect what I propose.

---

_@dhruvmanila reviewed on 2025-04-19 01:07_

---

_Comment by @dhruvmanila on 2025-04-19 01:11_

The mypy_primer change is removing a false positive, it popped up in the overload PR.

---

_@dhruvmanila approved on 2025-04-19 01:11_

Thank you!

---

_Merged by @dhruvmanila on 2025-04-19 01:12_

---

_Closed by @dhruvmanila on 2025-04-19 01:12_

---

_Branch deleted on 2025-04-27 09:22_

---
