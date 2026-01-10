```yaml
number: 15811
title: "[red-knot] Implicit instance attributes"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/implicit-instance-attributes
created_at: 2025-01-29T16:04:21Z
updated_at: 2025-02-03T19:40:59Z
url: https://github.com/astral-sh/ruff/pull/15811
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Implicit instance attributes

---

_Pull request opened by @sharkdp on 2025-01-29 16:04_

## Summary

Add support for implicitly-defined instance attributes, i.e. support type inference for cases like this:
```py
class C:
    def __init__(self) -> None:
        self.x: int = 1
        self.y = None

reveal_type(C().x)  # int
reveal_type(C().y)  # Unknown | None
```

A lot of things have been intentionally left out of this PR:
* Type inference for `self`
* Support attributes which are implicitly "declared" via their parameter types (`self.x = param`)
* Tuple-unpacking attribute assignments: `self.x, self.y = …`
* Diagnostic for conflicting declared types
* Attributes in statically-known-to-be-false branches are still visible:
   ```py
   class C:
       def __init__(self) -> None:
           if 2 + 3 < 4:
               self.x: str = "a"

   # # TODO: Ideally, this would result in a `unresolved-attribute` error. But mypy and pyright
   # do not support this either (for conditions that can only be resolved to `False` in type
   # inference), so it does not seem to be particularly important.
   reveal_type(C().x)  # revealed: str
   ```

Part of: https://github.com/astral-sh/ruff/issues/14164

## Benchmarks

Codspeed reports no change in a cold-cache benchmark, and a -1% regression in the incremental benchmark. On `black`'s `src` folder, I don't see a statistically significant difference between the branches (Reminder to self: always set the CPU frequency scaling governor to `performance` when benchmarking on a laptop):

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./red_knot_main check --project /home/shark/black/src` | 133.7 ± 9.5 | 126.7 | 164.7 | 1.01 ± 0.08 |
| `./red_knot_feature check --project /home/shark/black/src` | 132.2 ± 5.1 | 118.1 | 140.9 | 1.00 |

## Test Plan

Updated and new Markdown tests

---

_Label `red-knot` added by @sharkdp on 2025-01-29 16:04_

---

_@sharkdp reviewed on 2025-01-29 16:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:31 on 2025-01-29 16:41_

I think this doesn't work because we don't know what the type of `self` is.

---

_@sharkdp reviewed on 2025-01-29 16:41_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:182 on 2025-01-29 16:41_

See above: this does not work because we don't know what the type of `self` is.

---

_@sharkdp reviewed on 2025-01-29 16:43_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:449 on 2025-01-29 16:43_

Probably not something that we're going to address here. This would require narrowing on attribute expressions. `Unknown | Literal["value set in class method"]` is not wrong.

---

_Marked ready for review by @sharkdp on 2025-01-31 13:28_

---

_Review requested from @carljm by @sharkdp on 2025-01-31 13:28_

---

_Review requested from @MichaReiser by @sharkdp on 2025-01-31 13:28_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-01-31 13:28_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:49 on 2025-01-31 14:41_

Reading the docs on [`#[id]`](https://salsa-rs.netlify.app/overview.html?highlight=%23%5Bid%5D#id-fields), it's still not clear to me if this is correct or not. So please review this change of the `Expression` struct.

---

_@sharkdp reviewed on 2025-01-31 14:42_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:354 on 2025-01-31 19:03_

Perhaps there's no need here to make the reader think about precedence? We can either add parens or eliminate the addition. (I guess you were trying to make it "complex enough" that it's unlikely it could ever be special-cased without type inference?)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:239 on 2025-01-31 19:09_

it might also be interesting to add a version of this test where `a` and `b` are declared in the class body

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:293 on 2025-01-31 19:09_

we gotta add support for generics soon so we can write tests for `for` loops that don't involve all this tedious boilerplate 😆

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:77 on 2025-01-31 19:15_

nit: is it time to create a `ScopeInfo` struct so that we can access named fields on the items in this stack?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:61 on 2025-01-31 19:20_

Is there a lot of unpleasant fallout from just distinguishing functions and lambdas in `ScopeKind`? I wouldn't expect so, since we already have `ScopeId::is_function_like` when we want to check if a scope is "function-like". I don't think there's a principled reason `ScopeKind` _shouldn't_ distinguish them, we just haven't needed to yet. So unless there's a factor I'm missing, I'd just do that instead of introducing this new enum.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:470 on 2025-01-31 19:23_

it looks to me like you might be able to get away without the clone here? All tests pass if I apply this diff to your branch:

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index.rs b/crates/red_knot_python_semantic/src/semantic_index.rs
index 71abd2dfe..d8e79d4b6 100644
--- a/crates/red_knot_python_semantic/src/semantic_index.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index.rs
@@ -144,7 +144,8 @@ pub(crate) struct SemanticIndex<'db> {
 
     /// Maps from class body scopes to attribute assignments that were found
     /// in methods of that class.
-    attribute_assignments: FxHashMap<FileScopeId, FxHashMap<String, Vec<AttributeAssignment<'db>>>>,
+    attribute_assignments:
+        FxHashMap<FileScopeId, FxHashMap<&'db str, Vec<AttributeAssignment<'db>>>>,
 }
 
 impl<'db> SemanticIndex<'db> {
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
index 659e17e68..55e661ba6 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -102,7 +102,8 @@ pub(super) struct SemanticIndexBuilder<'db> {
     definitions_by_node: FxHashMap<DefinitionNodeKey, Definition<'db>>,
     expressions_by_node: FxHashMap<ExpressionNodeKey, Expression<'db>>,
     imported_modules: FxHashSet<ModuleName>,
-    attribute_assignments: FxHashMap<FileScopeId, FxHashMap<String, Vec<AttributeAssignment<'db>>>>,
+    attribute_assignments:
+        FxHashMap<FileScopeId, FxHashMap<&'db str, Vec<AttributeAssignment<'db>>>>,
 }
 
 impl<'db> SemanticIndexBuilder<'db> {
@@ -453,7 +454,7 @@ impl<'db> SemanticIndexBuilder<'db> {
     fn register_attribute_assignment(
         &mut self,
         object: &ast::Expr,
-        attr: &ast::Identifier,
+        attr: &'db str,
         attribute_assignment: AttributeAssignment<'db>,
     ) {
         if let Some(class_body_scope) = self.is_method_of_class() {
@@ -467,7 +468,7 @@ impl<'db> SemanticIndexBuilder<'db> {
                 self.attribute_assignments
                     .entry(class_body_scope)
                     .or_default()
-                    .entry(attr.id().as_str().to_owned())
+                    .entry(attr)
                     .or_default()
                     .push(attribute_assignment);
             }
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:77 on 2025-01-31 19:24_

If we don't introduce `BuilderScopeKind`, then I think the scope kind is derivable from the `FileScopeId`, and we don't need to add a third element to this tuple?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:828 on 2025-01-31 19:25_

What about assignments to attributes of the first argument of a classmethod or staticmethod?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:84 on 2025-01-31 19:27_

I think here again you can avoid some clones by using `&'db str` rather than `Name`:

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
index 659e17e68..abf51f956 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -81,7 +81,7 @@ pub(super) struct SemanticIndexBuilder<'db> {
     /// The match case we're currently visiting.
     current_match_case: Option<CurrentMatchCase<'db>>,
     /// The name of the first function parameter of the innermost function that we're currently visiting.
-    current_first_parameter_name: Option<Name>,
+    current_first_parameter_name: Option<&'db str>,
 
     /// Flow states at each `break` in the current loop.
     loop_break_states: Vec<FlowSnapshot>,
@@ -460,8 +460,8 @@ impl<'db> SemanticIndexBuilder<'db> {
             // We only care about attribute assignments to the first parameter of a method,
             // i.e. typically `self` or `cls`.
             let accessed_object_refers_to_first_parameter =
-                object.as_name_expr().map(|name| &name.id)
-                    == self.current_first_parameter_name.as_ref();
+                object.as_name_expr().map(|name| name.id.as_str())
+                    == self.current_first_parameter_name;
 
             if accessed_object_refers_to_first_parameter {
                 self.attribute_assignments
@@ -795,7 +795,7 @@ where
                         let mut first_parameter_name = parameters
                             .iter_non_variadic_params()
                             .next()
-                            .map(|first_param| first_param.parameter.name.id().clone());
+                            .map(|first_param| first_param.parameter.name.id.as_str());
                         std::mem::swap(
                             &mut builder.current_first_parameter_name,
                             &mut first_parameter_name,
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:522 on 2025-01-31 19:30_

I wonder if we could use this enum here (or do something like it) for the second parameter of `add_standalone_expression_impl`, rather than using a `bool`? We could move it (and rename it -- `ValueOrTypeExpression`?).

https://github.com/astral-sh/ruff/blob/fab86de3ef91460e58496f74ba1ce3edc4b9d69c/crates/red_knot_python_semantic/src/types.rs#L3701-L3711

I find it hard to tell why a `true` or `false` argument is being passed into a function in Rust without jumping to the definition of the function in question (which is hard if I'm reading a diff on GitHub).

---

_@AlexWaygood reviewed on 2025-01-31 19:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:49 on 2025-01-31 19:34_

I think it's fine. This will get cleared up in a future rev of Salsa.

---

_@sharkdp reviewed on 2025-01-31 19:35_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:293 on 2025-01-31 19:35_

I tried `[1, 2]` multiple times over the past months and it *still* doesn't work! 😀

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4138 on 2025-01-31 19:37_

This TODO confused me for a bit. I think it is now obsolete and should be removed in this PR?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4144 on 2025-01-31 19:38_

I guess at this point we do attribute lookup fully lazily, and without any Salsa caching. I wonder if there's enough work repeated here to make some Salsa caching of attribute types worth it? But we can evaluate this separately.

---

_@sharkdp reviewed on 2025-01-31 19:39_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:522 on 2025-01-31 19:39_

> I find it hard to tell why a true or false argument is being passed into a function in Rust without jumping to the definition of the function in question

... which is why I created two variants of this function that call ..._impl, so you don't have to see it in the "actual" code :-).

But I'm also fine with an enum, if you think that's clearer.

---

_@carljm reviewed on 2025-01-31 19:41_

Review not complete yet, just submitting partway through

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:522 on 2025-01-31 19:42_

heh, yeah. To give the full story here: while reading through the diff, I persuaded myself that it wasn't worth making this nit comment (for the reason you give), carried on reading the diff... and then saw that you were also adding an `infer_as_type_expression: bool` field to the `Expression` struct, which made me wonder if an enum like this could be more generally useful :-)

---

_@AlexWaygood reviewed on 2025-01-31 19:42_

---

_@carljm reviewed on 2025-01-31 19:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:77 on 2025-01-31 19:43_

My suggestion is that we may not need to add a third item to this tuple in this PR? But making it a struct could be a good idea regardless.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4164 on 2025-01-31 19:46_

I suppose we could add a comment here about why `Unknown` is in this union? It probably repeats something you already have in the tests, but it might be useful for a reader. Something short like "When an attribute type is only inferred, not declared, we add `Unknown` to the union to reflect that assigning any type to this attribute is allowed."

---

_@carljm approved on 2025-01-31 20:03_

Ok, finished review, only had one more comment.

This is, as usual, awesome work.

---

_@sharkdp reviewed on 2025-01-31 21:06_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:470 on 2025-01-31 21:06_

Thanks!

---

_@sharkdp reviewed on 2025-01-31 22:09_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:61 on 2025-01-31 22:09_

To be honest, I wasn't completely sure if that would have confused two different meanings of "scope kind", and if that merging of function+lambda was important for some reason.

I now tried to split the `ScopeKind::Function` case in to `Function`+`Lambda` and then used `ScopeKind` instead of `BuilderScopeKind`, but that leads to salsa cycles. I think it's because `ScopeId::scope` calls `semantic_index`, but I can look into that again next week.

---

_@sharkdp reviewed on 2025-01-31 22:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:828 on 2025-01-31 22:15_

Those are not (properly) supported yet. We do have one pre-existing test for attribute assignments in `@classmethod`s in this file, but we do not yet find these implicitly-declared attributes when calling `.member` on the class object. With this PR, we're potentially in a weird state where we find those attributes on instances, but not on the class object. If that's acceptable, I'll open an issue and add that to my goals for next week. Otherwise, I can also include it in this PR.

---

_@carljm reviewed on 2025-01-31 22:59_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:828 on 2025-01-31 22:59_

I don't think the functionality scope of this PR needs to be expanded, this can definitely be a follow-up. I was more just thinking that some TODO tests around it would clarify the intention and fit in well with the existing tests in this PR, which are already a mix of "complete" and "TODO". But there's certainly no need for that, the tests can come with the feature.

---

_@carljm reviewed on 2025-01-31 23:12_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:61 on 2025-01-31 23:12_

Yeah, makes sense. Semantically I think there's no distinction (and no reason lambdas and functions need to be merged), so it's nicer if we don't proliferate enums for the same concept.

In terms of the cycles, we should avoid using `ScopeId` API in semantic index building. It's not necessary to use it, since semantic index building is already inherently per-file, and the only _information_ added by `ScopeId` over `FileScopeId` is the file. Beyond that it's just a matter of some minor API rearrangements or additions for ergonomics, we have the needed information. Given a `FileScopeId`, `SemanticIndexBuilder::scopes` map can get us a `Scope`, which has a `NodeScopeWithKind`, from which we can get a `ScopeKind`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:153 on 2025-02-02 17:13_

What about cases where the method is conditionally defined by using an if statement? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4144 on 2025-02-02 17:16_

We may want to have salsa caching here to avoid invalidating types from module a if the class is defined in module b. Or is it guaranteed that this method is only ever called from within the same module (or if it is called across-modules, that it goes through a salsa query?)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:147 on 2025-02-02 17:19_

This is a fairly heavy data structure with a nested map that stores a list. 

It may be worth considering if there are any constraints that allow us to simplify the nesting or e.g use an IndexVec by ScopeId and accept that many entries are empty 

---

_@MichaReiser reviewed on 2025-02-02 17:19_

---

_@sharkdp reviewed on 2025-02-02 19:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:153 on 2025-02-02 19:40_

`if` statements do not introduce new scopes, so this works fine (see also the "Conditionally declared / bound attributes" test)

---

_@sharkdp reviewed on 2025-02-03 10:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index.rs`:147 on 2025-02-03 10:56_

I tried to investigate this, so I auto-generated a [pathological example](https://gist.github.com/sharkdp/87e3454d765c3927e4243d40fbcb42dd) with many attribute assignments. This file intentionally does *not access* those attributes, in order not trigger type inference for these attributes (in which case this branch *is* quite a bit slower than main). But we still build the full semantic index when checking this file.

Even for this file, with thousands of attribute accesses, `register_attribute_assignment` only makes up 1.5% of the whole profile:

![image](https://github.com/user-attachments/assets/92dc65ef-ef91-4ae9-93a5-918ced7cef4f)

And I hardly see any difference in overall performance between *main* and this feature branch (which not just registers attribute accesses but also creates many more standalone expressions):

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `main` | 141.9 ± 6.1 | 136.2 | 159.1 | 1.00 |
| `feature` | 144.6 ± 3.2 | 138.5 | 153.6 | 1.02 ± 0.05 |

One optimization that seems sensible to me would be to use a `SmallVec` instead of a `Vec`, since most real-world classes probably just assign once or twice to the same attribute. So I generated another version of the file where there are exactly two methods assigning to the same attribute, expecting to see a difference between an inline-capacity of 1 and 2. But it's all just noise:

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `Vec<…>` | 99.6 ± 1.3 | 97.3 | 102.4 | 1.00 |
| `SmallVec<[…, 1]>` | 100.1 ± 2.8 | 97.2 | 112.9 | 1.01 ± 0.03 |
| `SmallVec<[…, 2]>` | 99.8 ± 1.4 | 97.2 | 103.1 | 1.00 ± 0.02 |

I personally think it's something worth revisiting when we introduce micro-benchmarks, but until then, it seems hard to measure, and I'd rather not introduce any complications here without being confident that they actually make the code faster. What do you think?

---

_@sharkdp reviewed on 2025-02-03 11:13_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4144 on 2025-02-03 11:13_

I think I need some help answering these questions.


> Or is it guaranteed that this method is only ever called from within the same module

I don't think so.

> or if it is called across-modules, that it goes through a salsa query?

This function is only called from `Class::instance_member` (via `own_instance_member`), which in turn can be called from:
* `Type::member`
* `TypeInferenceBuilder::infer_attribute_expression`

The former (`Type::member`) is called from all sorts of different places.

---

_@MichaReiser reviewed on 2025-02-03 11:24_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4144 on 2025-02-03 11:24_

Thanks. Yeah, in that case, I think `implicit_instance_attribute` or `own_instance_member` or `member` should be a salsa query because: `semantic_index` changes whenever the file where the class is declared changes, and nothing is constraining `member` calls to be from the same file. 

An alternative is to introduce a `attribute_assignments(db, class_body_scope)` query that returns the assignments just for that scope. That might be cheaper but it still has the benefit that `implicit_instance_attribute` only gets invalidated when the attribute assignments in that specific scope changed.

We use a similar pattern for `use_def_map` and `symbol_table` where those queries return a specific "slice" of the `SemanticIndex` which is less likely to change.

Let me know if that helps or if you need some more input (also happy to discuss on Discord/sync)


---

_@MichaReiser reviewed on 2025-02-03 11:37_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:148 on 2025-02-03 11:37_

I'm unsure if using a `&'db str` here is actually safe... I thought that references is generally disallowed (by having to implement the `Update` trait) but it Salsa seems to allow it here. 

The issue I see here is that the AST might get collected between two revisions (because of LRU caching), in which case the map keys would point to invalid memory addresses. 

I posted in [salsa's zullip](https://salsa.zulipchat.com/#narrow/channel/145099-Using-Salsa/topic/Query.20result.20with.20.26'db.20lifetime.20field) but maybe @carljm remembers

---

_@sharkdp reviewed on 2025-02-03 14:16_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index.rs`:148 on 2025-02-03 14:16_

Thanks, Micha. I think both Alex and I had hesitations when replacing `String` with `&'db str` here, but we assumed it would be fine when it compiled.

Not this exact `&'db` reference, but I managed to make red_knot segfault today when I returned a `&'db` reference from a query. As noted in the zulip thread, this seems to be a known salsa bug: https://github.com/salsa-rs/salsa/issues/610.

Will revert 59d112f6cb537907e4dd4e10605a881c1254f401

---

_@MichaReiser reviewed on 2025-02-03 14:25_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index.rs`:148 on 2025-02-03 14:25_

You may want to use `Name` instead of `String` to avoid allocations in most cases. 

We may want to explore interning `Name`s... now that we're storing them in many tables.  Or could we store a `FileSymbolId` instead? Ah no, that doesn't work because `x` in `self.x` is not a symbol, only `self` is

---

_@AlexWaygood reviewed on 2025-02-03 14:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index.rs`:148 on 2025-02-03 14:25_

> I think both Alex and I had hesitations when replacing `String` with `&'db str` here, but we assumed it would be fine when it compiled.

heh, yes!

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4158 on 2025-02-03 15:48_

I wonder if this would be useful to have as a general query and if it should be defined next to `infer_expression_types` instead. 

---

_@MichaReiser reviewed on 2025-02-03 15:48_

---

_@sharkdp reviewed on 2025-02-03 15:52_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4158 on 2025-02-03 15:52_

I had a similar thought. It could certainly be used in more places, but I wasn't sure if we want the additional query in those places? I'll note it down as a task and open a small PR after this has been merged.

---

_@sharkdp reviewed on 2025-02-03 15:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:4144 on 2025-02-03 15:56_

For other reviewers: I implemented something that I discussed with Micha.

Removing `salsa::tracked` from either `attribute_assignments` or the inline `infer_expression_type` query makes the newly added test fail.

---

_@MichaReiser approved on 2025-02-03 18:26_

---

_@sharkdp reviewed on 2025-02-03 18:33_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:828 on 2025-02-03 18:33_

I'll note this down as a task for a follow-up.

---

_Merged by @sharkdp on 2025-02-03 18:34_

---

_Closed by @sharkdp on 2025-02-03 18:34_

---

_Branch deleted on 2025-02-03 18:34_

---

_@carljm reviewed on 2025-02-03 19:38_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4158 on 2025-02-03 19:38_

It is intuitive to me why `attribute_assignments` should be a query, but it's less intuitive to me why this should be a query. I don't see what additional dependencies are introduced here above those that the underlying `infer_expression_types` query would have anyway. So I guess this is not about dependencies, but rather about a smaller returned value so backdating can be more effective?

It feels intuitively to me that we would get more caching value with fewer cached memos if we placed the salsa query caching at the level of `Class::own_instance_member` or `Type::member`, rather than at such a fine-grained level that does such little work over the underlying `infer_expression_types`. But we should of course validate any such changes with observed performance differences.

---

_@MichaReiser reviewed on 2025-02-03 19:40_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:4158 on 2025-02-03 19:40_

The problem is the `expression.node_ref`. It accesses the AST node from the module where the class is declared, meaning the enclosing query has to re-run whenever the declaring module changes -- which we don't want.

Doing it here is mainly about avoiding calling a query (and caching the value which is expensive) in cases where we don't need to. Although I admit, I don't have any numbers on whether caching here is a significantly lower number than caching at the `own_instance_member` level.

---
