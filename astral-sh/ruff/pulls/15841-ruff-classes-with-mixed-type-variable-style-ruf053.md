```yaml
number: 15841
title: "[`ruff`] Classes with mixed type variable style (`RUF053`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: UP050
created_at: 2025-01-31T00:36:26Z
updated_at: 2025-02-06T18:37:14Z
url: https://github.com/astral-sh/ruff/pull/15841
synced_at: 2026-01-12T15:55:52Z
```

# [`ruff`] Classes with mixed type variable style (`RUF053`)

---

_@InSyncWithFoo_

## Summary

Resolves #15620.

`RUF053` reports the first `Generic[...]` base of any class with a PEP 695 type parameter list. It reuses much of the logic added in #15565, #15659 and other PRs.

Since the original code is fundamentally flawed, the fix takes some liberty in its rewrite. As such, it is always marked as unsafe.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-01-31 00:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-01-31 01:29_

---

_Label `preview` added by @ntBre on 2025-01-31 01:30_

---

_Review requested from @ntBre by @dhruvmanila on 2025-01-31 05:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/class_with_mixed_type_vars.rs`:40 on 2025-01-31 10:45_

```suggestion
/// class C[T](Generic[U, P, *Ts]): ...
```

(https://peps.python.org/pep-0646/#type-variable-tuples-must-always-be-unpacked)

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/class_with_mixed_type_vars.rs`:16 on 2025-01-31 10:47_

```suggestion
/// Checks for classes that have [PEP 695] [type parameter lists]
/// while also inheriting from `typing.Generic` or `typing_extensions.Generic`.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/class_with_mixed_type_vars.rs`:50 on 2025-01-31 10:51_

```suggestion
/// As the fix changes runtime behaviour, it is always marked as unsafe.
///
/// ## References
/// - [Python documentation: User-defined generic types](https://docs.python.org/3/library/typing.html#user-defined-generic-types)
/// - [Python documentation: type parameter lists](https://docs.python.org/3/reference/compound_stmts.html#type-params)
/// - [PEP 695 - Type Parameter Syntax](https://peps.python.org/pep-0695/)
///
/// [PEP 695]: https://peps.python.org/pep-0695/
/// [type parameter lists]: https://docs.python.org/3/reference/compound_stmts.html#type-params
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP050.py.snap`:40 on 2025-01-31 11:00_

there's actually two reasons why the original code would have raised an exception:
- It mixes a type parameter list with explicit inheritance from `Generic`
- Even if it didn't have a type parameter list, it would have raised an exception because `Generic` must be the last base class in the bases tuple

I think this is okay, however, since the rule is concerned with correctness and the fix is clearly documented to always change runtime behaviour.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP050.py.snap`:82 on 2025-01-31 11:04_

this is an incorrect fix because the `_D` `TypeVar` has a `default` argument. The proper fix here would be

```suggestion
26    |-class C[T](bytes, Generic[_D], bool): ...
   26 |+class C[T, _D = int](bytes, bool): ...
```

We don't currently handle `default` keywords at all in our PEP-695 autofixes; it's one of the outstanding items in https://github.com/astral-sh/ruff/issues/15642. For now, for this rule, I think we should just make sure that we don't try to autofix any classes that are generic over any `TypeVar`s with the `default` argument.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/class_with_mixed_type_vars.rs`:63 on 2025-01-31 11:05_

This doesn't feel very descriptive about what the autofix actually does... maybe this?

```suggestion
        Some("Remove `Generic` base class".to_string())
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP050.py.snap`:144 on 2025-01-31 11:09_

The correct way to make a class generic over a `TypeVarTuple` using the old-style syntax is to do either this:

```py
class C(Generic[*_As]): ...
```

or this:

```py
class C(Generic[Unpack[_As]]): ...
```

(We don't handle `Unpack` yet in our PEP-695 rules -- that's also an oustanding task in https://github.com/astral-sh/ruff/issues/15642)

I'm not sure we should try to autofix classes that are invalidly generic over `TypeVarTuple`s like this.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/snapshots/ruff_linter__rules__pyupgrade__tests__UP050.py.snap`:298 on 2025-01-31 11:11_

...fair enough 😆

I guess if you were actually running Ruff from the command line with `--unsafe-fixes --fix`, it would get rid of both the `Generic` base classes, because it runs all selected rules in a loop until there are no more autofixes left to apply? If so, could you maybe add a comment to that effect in the fixture file?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/class_with_mixed_type_vars.rs`:50 on 2025-01-31 11:12_

We should also mention in the `Fix safety` section that the autofix can delete comments

---

_@AlexWaygood had review dismissed on 2025-01-31 11:16_

Thanks! Some comments below. I haven't looked in detail at the implementation yet, just at docs and the behaviour of the rule as indicated by the snapshots.

I don't think this should be a `pyupgrade` rule, since all our existing `pyupgrade` rules are modernisation/stylistic rules. This rule is different, since it detects (and fixes) behaviour that would cause the code to raise an exception at runtime. I understand that you want to share the code being used in the existing `pyugprade` PEP-695 rules, but we could always move those utilities to somewhere else in the `ruff_linter` crate, where it could be accessed by both the pyupgrade rules and this rule

---

_@ntBre reviewed on 2025-01-31 13:35_

I haven't reviewed the code in detail yet either, but it looks a little longer than I expected. I might be oversimplifying things, but can we pretty much reuse the logic from the UP046 class rule and just perform an `Edit::insertion` at the end of type parameter list instead of writing a full list? That would save you the effort of duplicating all the checks from UP046 (like the `default` kwarg that Alex pointed out) and converting existing `TypeParam`s to `TypeVar`s only to write them back into `TypeParam`s. This would also lose you any deduplication it looks like you're doing with `is_existing_param_of_same_class`, but the incoming code was a real mess if it had mixed type variables *with* duplicates :laughing: 

---

_Comment by @InSyncWithFoo on 2025-01-31 16:51_

> [...] can we [...] just perform an `Edit::insertion` at the end of type parameter list instead of writing a full list?

I added a testcase for this:

```python
# Original
class C[
	T  # Comment about `T`
](Generic[_A]): ...

# Wrong fix
class C[
	T  # Comment about `T`, _A
]: ...

# Also wrong fix (slightly)
class C[
	T, _A  # Comment about `T`
]: ...

# Correct fix, but looks irritating
class C[
	T  # Comment about `T`
, _A]: ...
```

I wasn't sure if relying on the formatter is the way to go.

---

_Comment by @ntBre on 2025-01-31 16:59_

Ah interesting, I did not catch that about comments, thanks! I'll try to give this a fuller review today.

---

_Comment by @AlexWaygood on 2025-01-31 23:31_

Could you possibly retitle the PR and edit the description to make clear that this is no longer proposed as a pyupgrade rule?

---

_Renamed from "[`pyupgrade`] Classes with mixed type variable style (`UP050`)" to "[`ruff`] Classes with mixed type variable style (`RUF060`)" by @InSyncWithFoo on 2025-01-31 23:38_

---

_Review requested from @AlexWaygood by @InSyncWithFoo on 2025-01-31 23:40_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:85 on 2025-02-02 18:02_

nit: this is only used once on line 101, so I'd probably inline it, or at least move the binding closer to where it's used.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:92 on 2025-02-02 18:04_

```suggestion
    let Some(type_params) = type_params else {
```

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF060.py`:13 on 2025-02-02 18:21_

@AlexWaygood would know better than me, but is this a valid `TypeVarTuple` call? From the [docs](https://docs.python.org/3/library/typing.html#typing.TypeVarTuple), it looks like it can only take a `default`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-02 18:29_

Did you consider writing this function more like UP046 and UP047 by using the existing `TypeVarReferenceVisitor`?

```rust

fn convert_type_vars(
    generic_base: &ExprSubscript,
    old_style_type_vars: &Expr,
    type_params: &TypeParams,
    class_arguments: &Arguments,
    checker: &Checker,
) -> Option<Fix> {
    let mut type_vars = type_params
        .type_params
        .iter()
        .map(TypeVar::from)
        .collect::<Vec<_>>();

    let mut visitor = TypeVarReferenceVisitor {
        vars: vec![],
        semantic: checker.semantic(),
        any_skipped: false,
    };
    visitor.visit_expr(old_style_type_vars);

    let Some(mut converted_type_vars) = check_type_vars(visitor.vars) else {
        return None;
    };

    let remove_generic_base = remove_argument(
        generic_base,
        class_arguments,
        Parentheses::Remove,
        checker.source(),
    )
    .ok()?;

    type_vars.append(&mut converted_type_vars);

    let source = checker.source();
    let new_type_params = DisplayTypeVars {
        type_vars: &type_vars,
        source,
    };

    let replace_type_params =
        Edit::range_replacement(new_type_params.to_string(), type_params.range);

    Some(Fix::unsafe_edits(
        remove_generic_base,
        [replace_type_params],
    ))
}
```

When I made this change locally, it revealed that I did not properly handle the `bound` kwarg to `ParamSpec` (or `TypeVarTuple` but at least that one isn't in the docs as noted above), which I think might be why you wrote it differently. I think it would be preferable to fix the bug in the shared infrastructure if possible, though.

For the specifics of the bug fix, @AlexWaygood  again would know better, but the syntax error I get from Python for code like

```python
def f[**P: [int, float]](p: **P): ...
```

is `SyntaxError: cannot use bound with ParamSpec`, so I'd probably go for either ignoring `bound`s for non-`TypeVar`s or at least bailing out before offering a fix if they are detected.

---

_@ntBre requested changes on 2025-02-02 18:36_

Thanks for this! It looks good to me overall besides a couple of nits and a bigger question about reusing the `TypeVarReferenceVisitor`.

---

_@AlexWaygood reviewed on 2025-02-02 18:41_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/ruff/RUF060.py`:13 on 2025-02-02 18:41_

Good catch! That's correct, yes

---

_@AlexWaygood reviewed on 2025-02-02 18:43_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-02 18:43_

Yes, providing a `bound` is not valid for a `ParamSpec`. The [PEP that introduced `ParamSpec`](https://peps.python.org/pep-0612/#declaration) stated:

> The runtime should accept `bound`s and `covariant` and `contravariant` arguments in the declaration just as `typing.TypeVar` does, but for now we will defer the standardization of the semantics of those options to a later PEP.

(The later PEP has yet to come!)

---

_@InSyncWithFoo reviewed on 2025-02-02 20:33_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-02 20:33_

`TypeVarReferenceVisitor` has several problems.

1. It does not differentiate valid and invalid arguments to `Generic`:

	```python
	class C(Generic[Unpack[_A]):  ...
	#                      ^^ Not `TypeVarTuple`
	class C(Generic[Unpack[*_As]):  ...
	#                      ^^^^ Double unpacked
	```

	Originally I intended to fix these as well, but @AlexWaygood [said](https://github.com/astral-sh/ruff/pull/15841#discussion_r1937061965) such cases should not be autofixed. As stated, the fix can be as free as necessary, so I'm fine with this change as long as Alex confirms it.

2. It ignores "unknown" type variables:

	```python
	from somewhere import APublicTypeVar
	
	# Before
	class C[T](Generic[APublicTypeVar, _A]): ...

	# After
	class C[T, _A: str]: ...
	```

	This too is a case where it would do more harm to provide a fix. A patch will cause a rather big change that also affects other rules relying on `TypeVarReferenceVisitor`, in which case I will have to change those as well. To be honest, that's not something I want to do in this PR.


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-02 22:57_

For point (2), this is what I added the[ `TypeVarReferenceVisitor::any_skipped`](https://github.com/astral-sh/ruff/blob/b08ce5fb18c89792a4744c8642d1a82726e58f5b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs#L196-L198) field for. If `any_skipped` you can bail out without a fix. I left that out of my suggestion above, but that's what [`non_pep695_generic_class`](https://github.com/astral-sh/ruff/blob/b08ce5fb18c89792a4744c8642d1a82726e58f5b/crates/ruff_linter/src/rules/pyupgrade/rules/pep695/non_pep695_generic_class.rs#L179) does.

---

_@ntBre reviewed on 2025-02-02 22:57_

---

_@InSyncWithFoo reviewed on 2025-02-03 02:50_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-03 02:50_

Admittedly, I didn't see `any_skipped` (I must have been [blind](https://en.wikipedia.org/wiki/Inattentional_blindness)), but that doesn't solve the problem either. It doesn't account for existing PEP 695 type parameters, which is, I think, understandable and fixable:

```python
class C[T](Generic[T, _A]): ...
```

---

_@InSyncWithFoo reviewed on 2025-02-03 03:15_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-03 03:15_

@ntBre I [added](https://github.com/astral-sh/ruff/pull/15841/commits/2ed42ae9f5add677033b976480e8b66210822c03) a few more edge cases that you might be interested in.

---

_@AlexWaygood reviewed on 2025-02-03 14:22_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-03 14:22_

> 1. Originally I intended to fix these as well, but @AlexWaygood [said](https://github.com/astral-sh/ruff/pull/15841#discussion_r1937061965) such cases should not be autofixed. As stated, the fix can be as free as necessary, so I'm fine with this change as long as Alex confirms it.

It's all a little subjective -- as you say, the code we're starting off with is broken, and the fix attempts to fix at least one way in which it is broken, so the whole point of the fix is to change the behaviour of the code in some way. I still do lean towards saying that we should try to keep the fix narrowly focussed on one specific kind of brokenness, though, if that makes sense? 😄 If the code is broken in two different -- and actually quite distinct -- ways, it feels wrong for us to fix one reason for brokenness as a side effect of fixing the other reason for brokenness. There's a few reasons for this:
- The user only explicitly opted into the rule about classes that have type parameters and also explicitly inherit from `Generic`. It might be confusing or outright unwelcome if the fix for that rule also ends up fixing other issues.
- Fixing other issues as a side effect of this fix might conceal issues that the user wasn't even aware about, and might have preferred to fix manually
- Are we _confident_ that what the user intended here is that they meant to have `Generic[*_As]` or `Generic[Unpack[_As]]`? Maybe they actually meant `_As` to be a TypeVar rather than a TypeVarTuple? I don't think we should be guessing for something like that

---

_@InSyncWithFoo reviewed on 2025-02-05 18:17_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-05 18:17_

I take it that the current state of affairs should be kept intact, then?

---

_@ntBre reviewed on 2025-02-05 19:48_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:54 on 2025-02-05 19:48_

Do we want to be slightly more specific here? I guess it will definitely remove comments inside the `Generic[...]`. Is there anywhere else?

I guess I read this as "it might or might not remove comments" rather than "it will remove comments depending on where they are." UP046 and UP047 

---

_@ntBre reviewed on 2025-02-05 19:50_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:130 on 2025-02-05 19:50_

Oops, I clicked "single comment" instead of starting a review, but I'm reviewing this again now, expecting to approve. Alex may want to have another look too, though.

---

_@InSyncWithFoo reviewed on 2025-02-05 19:56_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:54 on 2025-02-05 19:56_

How about this?

> Additionally, comments within the fix range will not be preserved.

---

_@ntBre reviewed on 2025-02-05 19:58_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:54 on 2025-02-05 19:58_

Sounds good to me!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:186 on 2025-02-05 20:01_

nit: I think `checker` is only used for this call, so you could just pass a `&SemanticModel`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:184 on 2025-02-05 20:06_

I think you can get rid of this check completely by initializing `encountered` with the names of the `existing_type_params`:

```diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs b/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
index b2ac21005..f9c7f718e 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
@@ -5,7 +5,7 @@ use ruff_macros::{derive_message_formats, ViolationMetadata};
 use ruff_python_ast::{
     Arguments, Expr, ExprStarred, ExprSubscript, ExprTuple, StmtClassDef, TypeParams,
 };
-use ruff_python_semantic::{Binding, BindingKind, SemanticModel};
+use ruff_python_semantic::SemanticModel;
 
 use crate::checkers::ast::Checker;
 use crate::fix::edits::{remove_argument, Parentheses};
@@ -139,7 +139,7 @@ fn convert_type_vars(
         .map(TypeVar::from)
         .collect::<Vec<_>>();
 
-    let mut converted_type_vars = match old_style_type_vars {
+    let mut converted_type_vars = match dbg!(old_style_type_vars) {
         Expr::Tuple(ExprTuple { elts, .. }) => {
             generic_arguments_to_type_vars(elts.iter(), type_params, checker)?
         }
@@ -173,20 +173,13 @@ fn generic_arguments_to_type_vars<'a>(
     existing_type_params: &TypeParams,
     checker: &'a Checker,
 ) -> Option<Vec<TypeVar<'a>>> {
-    let is_existing_param_of_same_class = |binding: &Binding| {
-        // This first check should have been unnecessary,
-        // as a type parameter list can only contain type-parameter bindings.
-        // Named expressions, for example, are syntax errors.
-        // However, Ruff doesn't know that yet (#11118),
-        // so here it shall remain.
-        matches!(binding.kind, BindingKind::TypeParam)
-            && existing_type_params.range.contains_range(binding.range)
-    };
-
     let semantic = checker.semantic();
 
     let mut type_vars = vec![];
-    let mut encountered: Vec<&str> = vec![];
+    let mut encountered: Vec<&str> = existing_type_params
+        .iter()
+        .map(|tp| tp.name().as_str())
+        .collect();
 
     for expr in exprs {
         let (name, unpacked) = match expr {
@@ -204,10 +197,9 @@ fn generic_arguments_to_type_vars<'a>(
             _ => return None,
         };
 
-        let binding = semantic.only_binding(name).map(|id| semantic.binding(id))?;
         let name_as_str = name.id.as_str();
 
-        if is_existing_param_of_same_class(binding) || encountered.contains(&name_as_str) {
+        if encountered.contains(&name_as_str) {
             continue;
         }
 
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:214 on 2025-02-05 20:23_

nit: This suggestion is stacked on the last one, but if you use a `HashSet` for `encountered`, you can do this in one line:

```diff

diff --git a/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs b/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
index f9c7f718e..348365543 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
@@ -6,6 +6,7 @@ use ruff_python_ast::{
     Arguments, Expr, ExprStarred, ExprSubscript, ExprTuple, StmtClassDef, TypeParams,
 };
 use ruff_python_semantic::SemanticModel;
+use rustc_hash::FxHashSet;
 
 use crate::checkers::ast::Checker;
 use crate::fix::edits::{remove_argument, Parentheses};
@@ -176,7 +177,7 @@ fn generic_arguments_to_type_vars<'a>(
     let semantic = checker.semantic();
 
     let mut type_vars = vec![];
-    let mut encountered: Vec<&str> = existing_type_params
+    let mut encountered: FxHashSet<&str> = existing_type_params
         .iter()
         .map(|tp| tp.name().as_str())
         .collect();
@@ -197,14 +198,10 @@ fn generic_arguments_to_type_vars<'a>(
             _ => return None,
         };
 
-        let name_as_str = name.id.as_str();
-
-        if encountered.contains(&name_as_str) {
+        if !encountered.insert(name.id.as_str()) {
             continue;
         }
 
-        encountered.push(name_as_str);
-
         let type_var = expr_name_to_type_var(semantic, name)?;
 
         match (&type_var.kind, unpacked, &type_var.restriction) {
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:224 on 2025-02-05 20:45_

I found this part pretty confusing because it's not really related to the rule at hand. If I understand it now, you're checking for invalid combinations of `TypeVar::kind`, unpacking, and `TypeVar::restriction`? I guess that's somewhat obvious from the arguments to `match`, but it wasn't clear to me what it was doing here. Just a short comment like

```rust
// check for invalid combinations of type variable kinds, unpacking, and bounds/constraints
```

would probably be plenty, optionally with some examples of invalid cases.

Ideally I think this logic would be in `expr_name_to_type_var`, but that probably needs more work than belongs in this PR because `expr_name_to_type_var` will also need the `unpacked` value. That might have the additional benefit of handling `Unpack` in the other rules, though.

---

_@ntBre approved on 2025-02-05 21:06_

Thanks for your work on this. I especially appreciate the thorough test fixtures. I think I'll need to steal some of those for UP046 and UP047, if you don't get to them first :slightly_smiling_face: 

I'd like to see my latest comments addressed, but they're mostly nits so I'm still happy to approve.

---

_@ntBre reviewed on 2025-02-05 23:37_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:235 on 2025-02-05 23:37_

Nice, this is even more clear than the comment I proposed!

We might want some `is_*` methods for `TypeParamKind` at some point, but you don't need to do that here, in my opinion.

Similarly, I guess `restriction` should just be a field on `TypeParamKind::TypeVar` instead of on `TypeVar` itself too if the other kinds can't have restrictions. Again, we can leave that for a future PR, though.

---

_@MichaReiser reviewed on 2025-02-06 08:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/codes.rs`:1012 on 2025-02-06 08:05_

Any reason this rule isn't 059 or fills any of the other "holes" in our numbering?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/pep695/mod.rs`:1 on 2025-02-06 14:07_

Ideally I feel like we would now move these utilities to somewhere like `crates/ruff_linter/src/pep695_utils.rs` (or somewhere in `crates/ruff_python_semantic`), if they're now being used by non-`pyupgrade` rules. But that doesn't need to be done in this PR.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:163 on 2025-02-06 14:14_

`remove_argument` returns a `Result`, and if it's an `Err` variant, that `Err` variant contains some valuable information about why the function wasn't able to remove the argument. By using `.ok()?` here, you're throwing away that information. It would be better to have this function return `Result<Option<Fix>>`, and then use `try_set_optional_fix` to add the fix to the diagnostic. The advantage of this is that if you run Ruff with enough `-v` flag, it prints the error message to the terminal to explain why it wasn't able to create an autofix.

TL;DR: I'd do this:

<details>
<summary>Patch</summary>

```diff
diff --git a/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs b/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
index 6750d774d..764681ef3 100644
--- a/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
+++ b/crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs
@@ -104,16 +104,15 @@ pub(crate) fn class_with_mixed_type_vars(checker: &mut Checker, class_def: &Stmt
     };
 
     let mut diagnostic = Diagnostic::new(ClassWithMixedTypeVars, generic_base.range);
-
-    if let Some(fix) = convert_type_vars(
-        generic_base,
-        old_style_type_vars,
-        type_params,
-        arguments,
-        checker,
-    ) {
-        diagnostic.set_fix(fix);
-    }
+    diagnostic.try_set_optional_fix(|| {
+        convert_type_vars(
+            generic_base,
+            old_style_type_vars,
+            type_params,
+            arguments,
+            checker,
+        )
+    });
 
     checker.diagnostics.push(diagnostic);
 }
@@ -133,7 +132,7 @@ fn convert_type_vars(
     type_params: &TypeParams,
     class_arguments: &Arguments,
     checker: &Checker,
-) -> Option<Fix> {
+) -> anyhow::Result<Option<Fix>> {
     let mut type_vars = type_params
         .type_params
         .iter()
@@ -141,17 +140,21 @@ fn convert_type_vars(
         .collect::<Vec<_>>();
 
     let semantic = checker.semantic();
-    let mut converted_type_vars = match old_style_type_vars {
+    let converted_type_vars = match old_style_type_vars {
         Expr::Tuple(ExprTuple { elts, .. }) => {
-            generic_arguments_to_type_vars(elts.iter(), type_params, semantic)?
+            generic_arguments_to_type_vars(elts.iter(), type_params, semantic)
         }
         expr @ (Expr::Subscript(_) | Expr::Name(_)) => {
-            generic_arguments_to_type_vars(iter::once(expr), type_params, semantic)?
+            generic_arguments_to_type_vars(iter::once(expr), type_params, semantic)
         }
-        _ => return None,
+        _ => None,
+    };
+
+    let Some(converted_type_vars) = converted_type_vars else {
+        return Ok(None);
     };
 
-    type_vars.append(&mut converted_type_vars);
+    type_vars.extend(converted_type_vars);
 
     let source = checker.source();
     let new_type_params = DisplayTypeVars {
@@ -160,14 +163,14 @@ fn convert_type_vars(
     };
 
     let remove_generic_base =
-        remove_argument(generic_base, class_arguments, Parentheses::Remove, source).ok()?;
+        remove_argument(generic_base, class_arguments, Parentheses::Remove, source)?;
     let replace_type_params =
         Edit::range_replacement(new_type_params.to_string(), type_params.range);
 
-    Some(Fix::unsafe_edits(
+    Ok(Some(Fix::unsafe_edits(
         remove_generic_base,
         [replace_type_params],
-    ))
+    )))
 }
```

</details>

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:154 on 2025-02-06 14:14_

There's no need to declare the variable as `mut` if you use `extend()` rather than `append`:

```suggestion
    let converted_type_vars = match old_style_type_vars {
        Expr::Tuple(ExprTuple { elts, .. }) => {
            generic_arguments_to_type_vars(elts.iter(), type_params, semantic)?
        }
        expr @ (Expr::Subscript(_) | Expr::Name(_)) => {
            generic_arguments_to_type_vars(iter::once(expr), type_params, semantic)?
        }
        _ => return None,
    };

    type_vars.extend(converted_type_vars);
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:181 on 2025-02-06 14:16_

Could you add some documentation to this function to explain why it sometimes returns `None`? I always do a double take when I see a function that returns `Option<Vec<_>>`, because I always wonder why it doesn't just return an empty `Vec` instead of returning `None`. What does returning `None` signify here that wouldn't be expressed by returning an empty `Vec`?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:141 on 2025-02-06 14:17_

nit: It's nice to avoid the "turbofish operator" where possible

```suggestion
    let mut type_vars: Vec<_> = type_params
        .type_params
        .iter()
        .map(TypeVar::from)
        .collect();
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__preview__RUF060_RUF060.py.snap`:134 on 2025-02-06 14:19_

very nice!

---

_@AlexWaygood reviewed on 2025-02-06 14:22_

Thank you -- this overall looks great now! And I'll second @ntBre that your tests are excellent. I just have a few coding nits:

---

_Review comment by @ntBre on `crates/ruff_linter/src/codes.rs`:1012 on 2025-02-06 16:20_

There's a [draft PR](https://github.com/astral-sh/ruff/pull/15903) for 059 last time I checked but not sure about other holes

---

_@ntBre reviewed on 2025-02-06 16:20_

---

_@InSyncWithFoo reviewed on 2025-02-06 18:15_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:141 on 2025-02-06 18:15_

Actually, I prefer turbofishes. As chaining is all about visual flow, I think it makes more sense to place it next to `.collect()` and not at the top of the chain.

Then again, methods like `.into()` doesn't even allow that, as they are not generic themselves but the wrapping `impl`s are. Consistency first, I guess?

```rust
let a: Type = b.into();    // Works
let a = b.into::<Type>();  // Doesn't work, even though it looks nicer
```

---

_@AlexWaygood approved on 2025-02-06 18:22_

Nice!

---

_@AlexWaygood reviewed on 2025-02-06 18:23_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/class_with_mixed_type_vars.rs`:141 on 2025-02-06 18:23_

Well, like all stylistic things, it's subjective, and I certainly wouldn't have blocked the PR being merged over this :-)

Personally I prefer annotating the variable as it's just slightly more concise, and to me it makes sense to put the type of the variable alongside its declaration. Again, though, it's just my subjective opinion!

---

_Renamed from "[`ruff`] Classes with mixed type variable style (`RUF060`)" to "[`ruff`] Classes with mixed type variable style (`RUF053`)" by @InSyncWithFoo on 2025-02-06 18:30_

---

_Merged by @AlexWaygood on 2025-02-06 18:35_

---

_Closed by @AlexWaygood on 2025-02-06 18:35_

---

_Branch deleted on 2025-02-06 18:37_

---
