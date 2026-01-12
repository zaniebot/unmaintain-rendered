```yaml
number: 16409
title: "[red-knot] treat annotated assignments without RHS in stubs as bindings"
type: pull_request
state: merged
author: mishamsk
labels:
  - ty
assignees: []
merged: true
base: main
head: 16264-treat-declarations-in-stubs-as-bindings
created_at: 2025-02-27T02:12:34Z
updated_at: 2025-02-28T19:47:20Z
url: https://github.com/astral-sh/ruff/pull/16409
synced_at: 2026-01-12T15:55:54Z
```

# [red-knot] treat annotated assignments without RHS in stubs as bindings

---

_@mishamsk_

## Summary

Make a small-ish change, marking all declarations in stubs from annotated assignments as bound, even if there is no RHS. This is done by changing the implementation of `category`, that now takes whether it is in a stub context as an argument. I think this reflects logic encapsulation better then checking for stub context in downstream code, combining the category return with the stub flag.

However, current state of red knot would only reveal one behavior change: avoid a very rare (maybe even non-existent) false negative when a stub uses a global variable defined without a default value in the same stub.

With the current symbol boundness inference logic is not changed, unbound class and instance attributes do not emit diagnostics => there is no apparent difference between stubs & regular modules.

Fixes #16264 

## Test Plan

A couple of new mdtest sections.

---

_Comment by @mishamsk on 2025-02-27 04:10_

@carljm need your help here before I mark this for review.

I've implemented the change, however I wanted to make sure that I fully understand what you meant in the original issue.

**1. The mention of Protocol case**

You mentioned `Protocol` as an instance of a special form in non-annotation position that will not be properly handled by knot. I was very surprised to see how it is typed in the [typeshed](https://github.com/python/typeshed/blob/main/stdlib/typing.pyi). It doesn't match the [actual implementation](https://github.com/python/cpython/blob/fda056e64bdfcac3dd3d13eebda0a24994d83cb8/Lib/typing.py#L2114). In the implementation `SpecialForm` is not used as a base class, moreover it explicitly defines `__mro_entires__` that will raise if one attempts to subclass it. It is used to mark special forms used only in annotation position. `Protocol` on the other hand is a proper subclass of `Generic` and can be used as a base class.

Running mypy on a subset of `typing.pyi` yields something along these lines:
```
ttt.pyi:7: error: Variable "ttt.Protocol" is not valid as a type  [valid-type]
ttt.pyi:7: note: See https://mypy.readthedocs.io/en/stable/common_issues.html#variables-vs-type-aliases
ttt.pyi:7: error: Invalid base class "Protocol"  [misc]
```

and knot reports roughly equivalent diagnostic. Which seems legit to me.

What am I missing? What was the case you were thinking about? Check out the new test I've added, that's the only thing I could come up with.

**2. Class/Instance variables**

The result of this change will be invisible unless/until access to unbound class/instance variables will start emitting diagnostics. I assume it wasn't the goal of this issue/PR to tackle the re-design of symbol boundness classification. Am I right?

---

_Comment by @codspeed-hq[bot] on 2025-02-27 04:25_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mishamsk%3A16264-treat-declarations-in-stubs-as-bindings)

### Merging #16409 will **improve performances by 5.09%**

<sub>Comparing <code>mishamsk:16264-treat-declarations-in-stubs-as-bindings</code> (f8e7136) with <code>main</code> (980faff)</sub>



### Summary

`⚡ 1` improvements  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` red_knot_check_file[incremental] `` | 5.5 ms | 5.2 ms | +5.09% |


---

_Label `red-knot` added by @AlexWaygood on 2025-02-27 09:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1053 on 2025-02-27 18:55_

This class seems unused? I'd expect a) that this attribute should be named `class_or_instance_var` instead of `instance_var` (to emphasize the point that in a stub we consider it possibly a class-and-instance var, not necessarily a pure instance var, despite the lack of initialization) and b) to then have the `py` file below do a `reveal_type(C.class_or_instance_var)` to demonstrate that we don't error on accessing it on the class. (This would be explicitly testing precisely the same case that causes this PR to eliminate the `__module__` false positive in the tomllib benchmark.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1061 on 2025-02-27 18:56_

This test doesn't really seem relevant to this PR at all; when importing a symbol we have always preferred declarations, I'm pretty sure we already test this for both stubs and non-stubs over in the import tests. AFAICS this test would have passed the same before this PR.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1050 on 2025-02-27 18:59_

This test does look relevant to this PR, but it doesn't seem like it fits here in `attributes.md`, as it's not related to attributes. I think it would fit better in something like `stubs/locals.md`. The test I suggest below regarding `C.class_or_instance_var` would reasonably belong in this file.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:19 on 2025-02-27 19:00_

```suggestion
## Access to attributes declarated in stubs
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:32 on 2025-02-27 19:41_

Similar to above, I kinda think we should name this `class_or_instance_var`, because the impact of the change in this PR is that we now will consider it potentially a class var (because we will consider it bound on the class).

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:57 on 2025-02-27 19:43_

It's not clear to me what particular edge cases we are aiming to test by having a subclass of `C` defined in a non-stub file; it feels like a lot of these assertions aren't really related to the behavior that declarations in stubs are considered bindings? I feel like the main test we need here is one we don't yet have (a test of `C.class_or_instance_var`, assuming it is renamed). Though that's the same test I recommended above, we maybe don't need it in both test files.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:546 on 2025-02-27 19:48_

I think we should update this comment to discuss the stub case too

---

_@carljm reviewed on 2025-02-27 20:15_

> @carljm need your help here before I mark this for review.

Oops, once I was here I kinda forgot it wasn't marked ready for review, and I went ahead and looked it over and commented; sorry if some of my comments were things you already knew about and planned to change.

FWIW the code changes here look great to me! My inline comments are only about the tests. So all in all I think this PR is very close to ready for merge.

> **1. The mention of Protocol case**
> 
> You mentioned `Protocol` as an instance of a special form in non-annotation position that will not be properly handled by knot. I was very surprised to see how it is typed in the [typeshed](https://github.com/python/typeshed/blob/main/stdlib/typing.pyi). It doesn't match the [actual implementation](https://github.com/python/cpython/blob/fda056e64bdfcac3dd3d13eebda0a24994d83cb8/Lib/typing.py#L2114). In the implementation `SpecialForm` is not used as a base class, moreover it explicitly defines `__mro_entires__` that will raise if one attempts to subclass it. It is used to mark special forms used only in annotation position. `Protocol` on the other hand is a proper subclass of `Generic` and can be used as a base class.

`_SpecialForm` is used to annotate symbols in typeshed that type checkers cannot handle according to the "normal" rules, and will need to special case. `Protocol` is one of these symbols. The annotation of `_SpecialForm` is not intended to suggest that type-checkers should handle `Protocol` as if it were literally an instance of `_SpecialForm` at runtime. Instead it just means "you're on your own, this is going to need special-casing directly in the type checker."

You can look at our `KnownClass` struct (and its usage) for how we handle special-casing of many of these special forms (as well as other builtin types) already. We don't yet recognize or support `typing.Protocol` in `KnownClass`, so we currently just infer it as an instance of `_SpecialForm`, but this is something that will have to change for us to add any support for protocols.

It's kind of confusing that I mentioned `Protocol` in the issue description even though we don't have support for it yet; sorry about that. I think what happened is @sharkdp tried adding minimal recognition of `Protocol` in an earlier PR, and ran into the bug you are fixing in this PR, and because of that bug we never landed the `Protocol` recognition.

You can already see this bug with `Protocol`:

```py
from typing import SupportsInt
reveal_type(SupportsInt.__mro__)
```

On main this reveals `tuple[Literal[SupportsInt], Unknown, Literal[object]]`. The `Unknown` in the middle of that MRO is the bug you are fixing in this PR. It's currently `Unknown` because local uses of a name in the same scope consider only bindings, not declarations, and (until this PR) `Protocol: _SpecialForm` is not considered a binding, even in a stub file, so we just consider uses of `Protocol` in the global scope of `typing.pyi` as uses of an unbound name, resulting in the type `Unknown`. Of course, if we fix that bug without also adding recognition of `Protocol` and support for using it as a base class, then we'd instead just have an error about an instance of `_SpecialForm` not being a valid base class, and we'd still end up with `Unknown` in the MRO.

All that said, I don't actually think we should add support for `Protocol` and a test for this in this PR. Your simpler test with `uses_constant = CONSTANT` is testing the same behavior fix in a simpler way, and is IMO sufficient for this PR.

> Running mypy on a subset of `typing.pyi` yields something along these lines:
> 
> ```
> ttt.pyi:7: error: Variable "ttt.Protocol" is not valid as a type  [valid-type]
> ttt.pyi:7: note: See https://mypy.readthedocs.io/en/stable/common_issues.html#variables-vs-type-aliases
> ttt.pyi:7: error: Invalid base class "Protocol"  [misc]
> ```
> 
> and knot reports roughly equivalent diagnostic. Which seems legit to me.

This kind of an experiment is I think perhaps more confusing than useful, because type checkers (including both us and mypy) special-case many names in `typing.pyi`, and do so based on those names occurring in a module named `typing`. So the behavior seen when you check that same code in a module not named `typing` bears little resemblance to how the same type checker treats the actual `typing` module. (I guess this is relevant if you want to know how a type checker would treat the same code without any special-casing, but I don't think that tells us much here.)

> **2. Class/Instance variables**
> 
> The result of this change will be invisible unless/until access to unbound class/instance variables will start emitting diagnostics.

No, I think the change is already visible in how we interpret `x: int` in a class body in a stub file. Before this PR, we would consider `x` to be an instance-only variable (because no binding in class body) and would issue an error if its accessed directly on the class. After this PR, we should consider it a class-or-instance var, and allow access on the class. This change is visible in the removed `__module__` false positive in the benchmark, and I've also suggested in my inline comments that we test it explicitly.

>  I assume it wasn't the goal of this issue/PR to tackle the re-design of symbol boundness classification. Am I right?

Correct, that should be a separate PR.


---

_Comment by @carljm on 2025-02-27 20:30_

The CodSpeed perf results here are really interesting. It's nice to see the win on the incremental benchmark, but I'm not sure I understand where it's coming from! It looks like we also see a 3% regression on the cold-check benchmark. I suspect this is just because we now understand many more name references inside stub files, leading to more complex work than just "consider it unbound and fall back to Unknown". So I don't think we need to do anything about that.

Regarding the "missing key" panic, it looks like[ the file it panics on](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/resources/test/fixtures/pycodestyle/E501_4.py) has just a single line of code, which we end up parsing as [an annotated assignment with no RHS and a BinOp annotation, followed by a number of bare Name expressions](https://play.ruff.rs/3f294d91-1d79-4421-8d9c-94729197bcb6). One possibility is that somehow this PR is triggering a query cycle analyzing this code. Per the TODO comment in `infer_definition_types_cycle_recovery`, a cycle can currently lead to missing expression types. If you can verify that we are newly hitting `infer_definition_types_cycle_recovery` when checking this example, then I think we can just add this example to the `KNOWN_FAILURES` list in `crates/red_knot_project/tests/check.rs` and move on -- I'm working on cycle issues already.

---

_Comment by @carljm on 2025-02-27 20:33_

On second look, given that the name assigned to is also referenced in the annotation, I'm quite confident that the problem there is a cycle. When looking up bindings for that name before this PR, we'd have found none; after this PR, we find the same binding we are already trying to analyze (because annotations are also "lazy" in stub files, so they don't only look at previous bindings but also subsequent ones.)

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1053 on 2025-02-28 02:22_

restored [crates/red_knot_python_semantic/resources/mdtest/attributes.md](https://github.com/astral-sh/ruff/pull/16409/files/8b465292febb00df30ba1c403e39aa33d73818e7#diff-614f44a420ee418948672dbd893e8a713ea03fe24d5e4550ce8386baf6d9ad9d) and consolidated all tests in stub folder. See new changes there

---

_@mishamsk reviewed on 2025-02-28 02:22_

---

_@mishamsk reviewed on 2025-02-28 02:22_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/attributes.md`:1050 on 2025-02-28 02:22_

moved to a new test file as suggested

---

_@mishamsk reviewed on 2025-02-28 02:24_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:57 on 2025-02-28 02:24_

Agree. When I wrote this I haven't realized the class_or_instance impact. I only noticed that the bench has been fixed after initial commit.

I remove irrelevant tests as you've suggested

---

_Marked ready for review by @mishamsk on 2025-02-28 02:24_

---

_Review requested from @MichaReiser by @mishamsk on 2025-02-28 02:24_

---

_Review requested from @AlexWaygood by @mishamsk on 2025-02-28 02:24_

---

_Review requested from @sharkdp by @mishamsk on 2025-02-28 02:24_

---

_Comment by @mishamsk on 2025-02-28 02:34_

@carljm I've applied all suggestions and consolidated tests in stub subfolder.

I also did further investigation into the cycle related panic, and looks like I'll need your and maybe @MichaReiser help. What I've learned:

- I did confirm that it is the cycle recovery by temporarily injecting some debugging code
- After adding the first panic to known errors, the second popped up in https://github.com/mishamsk/ruff/blob/16264-treat-declarations-in-stubs-as-bindings/crates/ruff_linter/resources/test/fixtures/pyflakes/F401_0.py#L43

For convenience here is the snippet that causes a cycle:
```py
class X:
    datetime: datetime
```

what I do not understand is why it happens in this test that uses `SemanticModel` API's, but if I run compiled knot on this file OR add similar code to mdtests - everything is fine, no panic/cycle. I haven't traced the differences between [HasType::inferred_type](https://github.com/mishamsk/ruff/blob/16264-treat-declarations-in-stubs-as-bindings/crates/red_knot_python_semantic/src/semantic_model.rs#L52) that's is used in the failing test vs. regular tests/cli. But there has to be something that causes the cycle.

I tried ignoring the `F401_0`, but then another one panics. I didn't check how many, but this PR seems to be triggering a lot of panics. So I thought it may be more prudent to fix the root cause. Any suggestions?


---

_Comment by @mishamsk on 2025-02-28 02:39_

> The CodSpeed perf results here are really interesting. It's nice to see the win on the incremental benchmark, but I'm not sure I understand where it's coming from!

yes, I've also noticed this. Magic?;-) A random guess - before that a lack of binding caused [back-off logic](https://github.com/mishamsk/ruff/blob/16264-treat-declarations-in-stubs-as-bindings/crates/red_knot_python_semantic/src/types/infer.rs#L3577) during symbol search (in `infer_name_load`), which is now unnecessary. If so, that would mean that this back-off mechanism is complicated enough to cause such a negative impact when symbol is not bound locally and needs to be looked up further up the chain.

---

_Comment by @carljm on 2025-02-28 02:44_

> what I do not understand is why it happens in this test that uses `SemanticModel` API's

It's because only these tests assert (as a bug-catching mechanism, via the PullTypes visitor) that every expression has a type recorded for it. In normal type inference we do not spend the time to do a full AST walk and assert this. The cycle itself does not fail (due to the cycle recovery function) but that cycle recovery function doesn't insert a type for every expression, so it causes these coverage tests to panic due to missing types for some expressions.

This will be fixed by https://github.com/astral-sh/ruff/pull/14029, so it's OK to just add a lot of new `KNOWN_FAILURES` here. The examples you've shown so far look like legitimate cycles that would be expected with the change of semantics in this PR, so I'm OK with just silencing them for now.

The other option would be to extract just the "fallback type" part of https://github.com/astral-sh/ruff/pull/14029 (not the fixpoint iteration part), where it gives `TypeInference` the possibility to have a "fallback type" used for all expressions that don't have a specific type inserted. Then we can create this `TypeInference` with fallback (probably just to `Type::Todo(cycle handling)`) in the cycle recovery function, and these panics should go away.

If we do the latter, it would probably be better to do it as a separate PR, so we can see the perf impact of just the "fallback type" feature alone. There may be some ways we can improve it if it's a regression.

---

_Comment by @carljm on 2025-02-28 02:47_

> A random guess - before that a lack of binding caused [back-off logic](https://github.com/mishamsk/ruff/blob/16264-treat-declarations-in-stubs-as-bindings/crates/red_knot_python_semantic/src/types/infer.rs#L3577) during symbol search (in `infer_name_load`), which is now unnecessary. If so, that would mean that this back-off mechanism is complicated enough to cause such a negative impact when symbol is not bound locally and needs to be looked up further up the chain.

It could be related to this. The improvement shows up in the "incremental" benchmark, not the "cold" benchmark. In the incremental benchmark we make a no-op change to one file (add a comment), so we are not really testing the performance of type inference, we are testing how fast Salsa can re-validate that nothing has changed. So usually improvements here are because we call fewer Salsa queries, or have fewer dependencies between Salsa queries. But it's true that this change could mean many fewer inference queries on scopes in stub files now take on dependencies on a different scope (the global or builtins scopes). This makes sense, because I don't expect that fallback to actually be a lot of extra work (so we don't gain much from this in cold benchmark), but it can generate a lot of extra Salsa query dependencies (so we do gain in the incremental benchmark.)

---

_Comment by @github-actions[bot] on 2025-02-28 03:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @mishamsk on 2025-02-28 03:03_

> It's because only these tests assert (as a bug-catching mechanism, via the PullTypes visitor) that every expression has a type recorded for it. In normal type inference we do not spend the time to do a full AST walk and assert this. The cycle itself does not fail (due to the cycle recovery function) but that cycle recovery function doesn't insert a type for every expression, so it causes these coverage tests to panic due to missing types for some expressions.

Got it, thanks. Now it makes sense. I used bare `println!` to see where the cycle is being triggered, and didn't see the output when running the cli - that's why I thought the cycle wasn't even triggered. But I may just missed it among verbose logs or stdout is just getting captured somewhere along the way.   

> so it's OK to just add a lot of new KNOWN_FAILURES here

well it turned out to be just 3 more files. Checked all of them, seems legit, so added to `KNOWN_FAILURES`

> The other option would be to extract just the "fallback type" part of https://github.com/astral-sh/ruff/pull/14029

From what I can see, you've added the fallback type on the struct and it's usage in getter API's that access hasmaps, but if I am not mistaken it is not yet populated with actual fallbacks depending on the context. So I thought it would require more work to finalize. So I think this can be merged as-is. Especially since it is only 4 new exceptions to tests.

---

_@MichaReiser approved on 2025-02-28 07:55_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:21 on 2025-02-28 14:03_

nit

```suggestion
Unlike regular Python modules, stub files often omit the right-hand side in declarations, including in class
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/stubs/class.md`:27 on 2025-02-28 14:08_

I'm not a huge fan of the term "class or instance" attribute. It doesn't appear in the typing spec, and conceptually I think it's the wrong model to have. The [spec](https://typing.readthedocs.io/en/latest/spec/class-compat.html) says that these are instance attributes that just have class-level defaults, and other than the fact that they can be accessed safely on the class object they behave like "pure" instance attributes in nearly every other way -- so I think that's the better way to think about them.

I'd suggest rewording this paragraph like so:

```suggestion
One implication of this is that we'll always treat symbols in class scope as safe to be
accessed from the class object itself. We'll never infer a "pure instance attribute" from
a stub.
```

---

_@AlexWaygood reviewed on 2025-02-28 14:09_

Thanks, this is great!!

---

_@AlexWaygood reviewed on 2025-02-28 14:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:518 on 2025-02-28 14:17_

it probably doesn't matter much, but it might be slightly more strongly typed if this method took a `file: File` argument rather than an `in_stub: bool` argument, e.g.

<details>

```diff
diff --git a/crates/red_knot_python_semantic/src/semantic_index/builder.rs b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
index 74e3494a6..4f2a7f470 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/builder.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/builder.rs
@@ -353,7 +353,7 @@ impl<'db> SemanticIndexBuilder<'db> {
         #[allow(unsafe_code)]
         // SAFETY: `definition_node` is guaranteed to be a child of `self.module`
         let kind = unsafe { definition_node.into_owned(self.module.clone()) };
-        let category = kind.category(self.file.is_stub(self.db.upcast()));
+        let category = kind.category(self.db, self.file);
         let is_reexported = kind.is_reexported();
         let definition = Definition::new(
             self.db,
diff --git a/crates/red_knot_python_semantic/src/semantic_index/definition.rs b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
index d41c3ccdb..22caa2111 100644
--- a/crates/red_knot_python_semantic/src/semantic_index/definition.rs
+++ b/crates/red_knot_python_semantic/src/semantic_index/definition.rs
@@ -515,7 +515,7 @@ impl DefinitionKind<'_> {
         }
     }
 
-    pub(crate) fn category(&self, in_stub: bool) -> DefinitionCategory {
+    pub(crate) fn category(&self, db: &dyn Db, file: File) -> DefinitionCategory {
         match self {
             // functions, classes, and imports always bind, and we consider them declarations
             DefinitionKind::Function(_)
@@ -546,7 +546,7 @@ impl DefinitionKind<'_> {
             // Annotated assignment is always a declaration. It is also a binding if there is a RHS
             // or if we are in a stub file. Unfortunately, it is common for stubs to omit even an `...` value placeholder.
             DefinitionKind::AnnotatedAssignment(ann_assign) => {
-                if in_stub || ann_assign.value.is_some() {
+                if file.is_stub(db.upcast()) || ann_assign.value.is_some() {
                     DefinitionCategory::DeclarationAndBinding
                 } else {
                     DefinitionCategory::Declaration
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 224086ec4..b5881ddfa 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -119,7 +119,7 @@ fn infer_definition_types_cycle_recovery<'db>(
 ) -> TypeInference<'db> {
     tracing::trace!("infer_definition_types_cycle_recovery");
     let mut inference = TypeInference::empty(input.scope(db));
-    let category = input.kind(db).category(input.file(db).is_stub(db.upcast()));
+    let category = input.kind(db).category(db, input.file(db));
     if category.is_declaration() {
         inference
             .declarations
@@ -905,7 +905,7 @@ impl<'db> TypeInferenceBuilder<'db> {
     fn add_binding(&mut self, node: AnyNodeRef, binding: Definition<'db>, ty: Type<'db>) {
         debug_assert!(binding
             .kind(self.db())
-            .category(self.context.in_stub())
+            .category(self.db(), self.file())
             .is_binding());
         let use_def = self.index.use_def_map(binding.file_scope(self.db()));
         let declarations = use_def.declarations_at_binding(binding);
@@ -943,7 +943,7 @@ impl<'db> TypeInferenceBuilder<'db> {
     ) {
         debug_assert!(declaration
             .kind(self.db())
-            .category(self.context.in_stub())
+            .category(self.db(), self.file())
             .is_declaration());
         let use_def = self.index.use_def_map(declaration.file_scope(self.db()));
         let prior_bindings = use_def.bindings_at_declaration(declaration);
@@ -976,11 +976,11 @@ impl<'db> TypeInferenceBuilder<'db> {
     ) {
         debug_assert!(definition
             .kind(self.db())
-            .category(self.context.in_stub())
+            .category(self.db(), self.file())
             .is_binding());
         debug_assert!(definition
             .kind(self.db())
-            .category(self.context.in_stub())
+            .category(self.db(), self.file())
             .is_declaration());
 
         let (declared_ty, inferred_ty) = match *declared_and_inferred_ty {
```

</details>

I'm also fine with what you have now, though

---

_@AlexWaygood approved on 2025-02-28 14:17_

---

_Comment by @carljm on 2025-02-28 14:28_

> From what I can see, you've added the fallback type on the struct and it's usage in getter API's that access hasmaps, but if I am not mistaken it is not yet populated with actual fallbacks depending on the context.

The only place we want to populate a fallback is when we are creating a `TypeInference` from scratch in cycle recovery, and my PR already does this. It would look different in `main` than in my PR because in `main` there is only one cycle recovery function, so only one place we'd want to use a fallback type (right at that TODO comment about missing expression types).

But since there are so few failures it's fine to just put them in known failures instead!

---

_@mishamsk reviewed on 2025-02-28 15:37_

---

_Review comment by @mishamsk on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:518 on 2025-02-28 15:37_

It may be used in different contexts, including when `in_stub` is a pre-calculated context struct field. So the narrower API makes it less cumbersome to use. I usually prefer functions with the narrowest possible APIs until the number of arguments becomes >4-5. So I'd say add the full `File` when/if more of it's methods are necessary inside the function.

---

_Comment by @mishamsk on 2025-02-28 15:41_

> The only place we want to populate a fallback is when we are creating a TypeInference from scratch in cycle recovery

ah, I think I missed that usage. I was just skimming over on GitHub.

I've applied two comment changes suggested by @AlexWaygood . I think it is good to be merged.

---

_Comment by @AlexWaygood on 2025-02-28 16:45_

Thanks again!

---

_Merged by @AlexWaygood on 2025-02-28 16:45_

---

_Closed by @AlexWaygood on 2025-02-28 16:45_

---

_Branch deleted on 2025-02-28 19:47_

---
