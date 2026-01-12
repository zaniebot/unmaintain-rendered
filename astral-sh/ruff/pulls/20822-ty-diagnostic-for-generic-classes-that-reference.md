```yaml
number: 20822
title: "[ty] Diagnostic for generic classes that reference typevars in enclosing scope"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/inherited-legacy-base
created_at: 2025-10-12T19:16:17Z
updated_at: 2025-10-13T23:30:51Z
url: https://github.com/astral-sh/ruff/pull/20822
synced_at: 2026-01-12T15:57:10Z
```

# [ty] Diagnostic for generic classes that reference typevars in enclosing scope

---

_@dcreager_

Generic classes are not allowed to bind or reference a typevar from an enclosing scope:

```py
def f[T](x: T, y: T) -> None:
    class Ok[S]: ...
    # error: [invalid-generic-class]
    class Bad1[T]: ...
    # error: [invalid-generic-class]
    class Bad2(Iterable[T]): ...

class C[T]:
    class Ok1[S]: ...
    # error: [invalid-generic-class]
    class Bad1[T]: ...
    # error: [invalid-generic-class]
    class Bad2(Iterable[T]): ...
```

It does not matter if the class uses PEP 695 or legacy syntax. It does not matter if the enclosing scope is a generic class or function. The generic class cannot even _reference_ an enclosing typevar in its base class list.

This PR adds diagnostics for these cases.

In addition, the PR adds better fallback behavior for generic classes that violate this rule: any enclosing typevars are not included in the class's generic context. (That ensures that we don't inadvertently try to infer specializations for those typevars in places where we shouldn't.) The `dulwich` ecosystem project has [examples of this](https://github.com/jelmer/dulwich/blob/d912eaaffd60b4978ff92b91a8300e2cd234e0fc/dulwich/config.py#L251) that were causing new false positives on #20677.

---

_Review requested from @carljm by @dcreager on 2025-10-12 19:16_

---

_Review requested from @AlexWaygood by @dcreager on 2025-10-12 19:16_

---

_Review requested from @sharkdp by @dcreager on 2025-10-12 19:16_

---

_Label `ty` added by @dcreager on 2025-10-12 19:16_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:320 on 2025-10-12 19:18_

See the comment below; this is passed in as a callback so that we don't have to query the class's definition unless it actually references legacy typevars in its base class list. Passing the definition in directly was causing some of the salsa query dependency tracking tests to fail.

---

_@dcreager reviewed on 2025-10-12 19:19_

---

_Comment by @github-actions[bot] on 2025-10-12 19:21_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-13 20:39:45.649900870 +0000
+++ new-output.txt	2025-10-13 20:39:48.939910978 +0000
@@ -432,6 +432,8 @@
 generics_scoping.py:15:1: error[type-assertion-failure] Argument does not have asserted type `str`
 generics_scoping.py:42:1: error[type-assertion-failure] Argument does not have asserted type `str`
 generics_scoping.py:43:1: error[type-assertion-failure] Argument does not have asserted type `bytes`
+generics_scoping.py:65:11: error[invalid-generic-class] Generic class `MyGeneric` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
+generics_scoping.py:75:11: error[invalid-generic-class] Generic class `Bad` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
 generics_self_advanced.py:11:24: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@prop1`
 generics_self_advanced.py:18:1: error[type-assertion-failure] Argument does not have asserted type `ParentA`
 generics_self_advanced.py:19:1: error[type-assertion-failure] Argument does not have asserted type `ChildA`
@@ -896,5 +898,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 898 diagnostics
+Found 900 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Label `ecosystem-analyzer` added by @dcreager on 2025-10-12 19:21_

---

_Comment by @github-actions[bot] on 2025-10-12 19:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
nionutils (https://github.com/nion-software/nionutils)
+ nion/utils/Stream.py:552:15: error[invalid-generic-class] Generic class `AValueChangeStreamReactor` must not reference type variables bound in an enclosing scope: `T` referenced in class definition here
- Found 7 diagnostics
+ Found 8 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/config.py:251:15: error[invalid-generic-class] Generic class `UniqueKeysView` must not reference type variables bound in an enclosing scope: `K` referenced in class definition here
+ dulwich/config.py:270:15: error[invalid-generic-class] Generic class `OrderedItemsView` must not reference type variables bound in an enclosing scope: `K` referenced in class definition here
+ dulwich/config.py:270:15: error[invalid-generic-class] Generic class `OrderedItemsView` must not reference type variables bound in an enclosing scope: `V` referenced in class definition here
- Found 184 diagnostics
+ Found 187 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo metadata = ~25MB
+     memo metadata = ~26MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~42MB
+     memo metadata = ~44MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~515MB
+ TOTAL MEMORY USAGE: ~541MB
-     memo metadata = ~89MB
+     memo metadata = ~93MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-10-12 19:26_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-generic-class` | 4 | 0 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| **Total** | **5** | **0** | **0** |

**[Full report with detailed diff](https://dcreager-inherited-legacy-ba.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-inherited-legacy-ba.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-10-12 19:38_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Finherited-legacy-base)

### Merging #20822 will **not alter performance**

<sub>Comparing <code>dcreager/inherited-legacy-base</code> (8823fd1) with <code>main</code> (2b729b4)</sub>



### Summary

`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Finherited-legacy-base?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1481 on 2025-10-12 20:32_

nit: I think these can both be `FxIndexSet`s, and `FxIndexSet` is a simpler type than `FxOrderSet`. They're both ordered according to insertion order; IIRC the only difference is that `FxOrderSet` is also hashable, but that doesn't seem to be necessary here.

```suggestion
            typevars: RefCell<FxIndexSet<BoundTypeVarInstance<'db>>>,
            seen_types: RefCell<FxIndexSet<NonAtomicType<'db>>>,
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1514 on 2025-10-12 20:32_

nit: if you added `#[derive(Default)]` to the `CollectTypeVars` struct, this could just be:

```suggestion
        let visitor = CollectTypeVars::default();
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:860 on 2025-10-12 20:36_

nit: the level of nesting here is huge. Could everything under this `if let` maybe be factored out into a standalone method/function, so that we could use early-return style to reduce the indentation?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:892 on 2025-10-12 20:56_

`full_range` here will be a huge range if the class has lots of methods in it. Additionally, it can be a bit hard to tell the two highlighted ranges apart from each other currently:

<img width="2182" height="398" alt="image" src="https://github.com/user-attachments/assets/6453b99f-7a9c-4db1-afb6-2bd252156edc" />

What about something like this?

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 894ef87710..aa52a3c0d1 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -8190,6 +8190,13 @@ impl<'db> BindingContext<'db> {
             BindingContext::Synthetic => None,
         }
     }
+
+    fn into_definition(self) -> Option<Definition<'db>> {
+        match self {
+            BindingContext::Definition(definition) => Some(definition),
+            BindingContext::Synthetic => None,
+        }
+    }
 }
 
 /// A type variable that has been bound to a generic context, and which can be specialized to a
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index dcbb4a5118..0ccbb658e6 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -866,28 +866,28 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                         {
                             if let Some(builder) = self
                                 .context
-                                .report_lint(&INVALID_GENERIC_CLASS, &class_node.name)
+                                .report_lint(&INVALID_GENERIC_CLASS, class.header_range(self.db()))
                             {
                                 let mut diagnostic = builder.into_diagnostic(format_args!(
                                     "Generic class `{}` must not reference type variables \
                                     bound in an enclosing scope",
                                     class_node.name,
                                 ));
-                                if let BindingContext::Definition(other_definition) =
-                                    other_typevar.binding_context(self.db())
+                                diagnostic.set_primary_message(format_args!(
+                                    "`{self_typevar_name}` referenced in class definition here"
+                                ));
+                                if let Some(class) = other_typevar
+                                    .binding_context(self.db())
+                                    .into_definition()
+                                    .and_then(|definition| {
+                                        binding_type(self.db(), definition).into_class_literal()
+                                    })
                                 {
                                     diagnostic.annotate(
-                                        Annotation::primary(
-                                            other_definition
-                                                .full_range(self.db(), self.module())
-                                                .into(),
-                                        )
-                                        .message(
-                                            format_args!(
-                                                "Type variable `{self_typevar_name}` is bound \
-                                                in this enclosing scope",
-                                            ),
-                                        ),
+                                        Annotation::secondary(class.header_span(self.db()))
+                                        .message(format_args!(
+                                            "Type variable `{self_typevar_name}` bound in this enclosing scope",
+                                        )),
                                     );
                                 }
                             }
```

which renders like this:

<img width="2166" height="442" alt="image" src="https://github.com/user-attachments/assets/94ea8040-d32f-49c1-84ec-8d2ab9ad8336" />


---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/scoping.md`:272 on 2025-10-12 20:57_

nit: since you've enabled snapshots, I think I'd avoid asserting the precise error messages here. It can be annoying to update loads of mdtest assertions whenever you change an error message

---

_@AlexWaygood reviewed on 2025-10-12 20:58_

Nice!

---

_@MichaReiser reviewed on 2025-10-13 06:37_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/generics.rs`:320 on 2025-10-13 06:37_

That's probably worth an inline comment as it's non-obvious for future readers. 

However, I do think what you did here is cheating the test rather than fixing the underlying issue. The problem is that calling `definition` adds a dependency on the ast node. The way you cheated the test is by early-returning in the case we're testing, a class with no type vars! But the same problem still exists for classes with type variables. 

The more proper fix here is probably to make whatever calls `from_base_classes` to be behind a salsa query (at least, in positions where we can reach this call across file boundaries). 

---

_@MichaReiser reviewed on 2025-10-13 06:38_

I'm surprised by the performance regression. This doesn't seem to add that much complexity. It might be worth taking a look at what's happening.

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/scoping.md`:272 on 2025-10-13 13:34_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:1481 on 2025-10-13 13:35_

TIL that difference between `IndexSet` and `OrderSet`! Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:1514 on 2025-10-13 13:36_

Done

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:892 on 2025-10-13 13:38_

TIL `header_range`, thank you! Done. Similarly, I used the signature span if the enclosing generic context is from a function.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:860 on 2025-10-13 14:00_

Done. I moved this into a new `report_foo` function alongside the others in diagnostic.rs

---

_@dcreager reviewed on 2025-10-13 14:01_

---

_@AlexWaygood reviewed on 2025-10-13 14:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/snapshots/scoping.md_-_Scoping_rules_for_ty…_-_Nested_formal_typeva…_-_Generic_class_within…_(3259718bf20b45a2).snap`:38 on 2025-10-13 14:06_

heh, this seems like a pre-existing bug in `ClassLiteral::header_span()` that it doesn't cover the type parameters

---

_@dcreager reviewed on 2025-10-13 14:07_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:320 on 2025-10-13 14:07_

> The more proper fix here is probably to make whatever calls `from_base_classes` to be behind a salsa query (at least, in positions where we can reach this call across file boundaries).

That was an easier real fix than I expected! Done

---

_@dcreager reviewed on 2025-10-13 14:07_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/snapshots/scoping.md_-_Scoping_rules_for_ty…_-_Nested_formal_typeva…_-_Generic_class_within…_(3259718bf20b45a2).snap`:38 on 2025-10-13 14:07_

Yep I saw that too

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/diagnostic.rs`:3086 on 2025-10-13 14:10_

you could use early-return style to reduce the indentation here even more, now that it's a standalone function:

```suggestion
    let Some(builder) = context.report_lint(&INVALID_GENERIC_CLASS, class.header_range(db)) else {
        return;
    };
    let mut diagnostic = builder.into_diagnostic(format_args!(
        "Generic class `{}` must not reference type variables bound in an enclosing scope",
        class_node.name,
    ));
    diagnostic.set_primary_message(format_args!(
        "`{typevar_name}` referenced in class definition here"
    ));
    let Some(other_definition) = other_typevar.binding_context(db).definition() else {
        return;
    };
    let span = match binding_type(db, other_definition) {
        Type::ClassLiteral(class) => Some(class.header_span(db)),
        Type::FunctionLiteral(function) => function.spans(db).map(|spans| spans.signature),
        _ => return,
    };
    if let Some(span) = span {
        diagnostic.annotate(Annotation::secondary(span).message(format_args!(
            "Type variable `{typevar_name}` is bound in this enclosing scope",
        )));
    }
```

---

_@AlexWaygood reviewed on 2025-10-13 14:10_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/diagnostic.rs`:3086 on 2025-10-13 14:12_

Mucho gusto

---

_@dcreager reviewed on 2025-10-13 14:12_

---

_Comment by @AlexWaygood on 2025-10-13 14:26_

> I'm surprised by the performance regression. This doesn't seem to add that much complexity. It might be worth taking a look at what's happening.

It looks like this may have been fixed by adding the Salsa caching, though there is now a memory-usage regression being reported by mypy_primer

---

_Comment by @dcreager on 2025-10-13 14:30_

> though there is now a memory-usage regression being reported by mypy_primer

That is unsurprising, since we'll have a cached result for the newly cached function for every non-generic class. (And technically for every generic class that is generic via inheritance using legacy typevars) It's not a huge regression, so I would lean towards accepting it.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:1525 on 2025-10-13 16:42_

```suggestion
            walk_generic_context(db, generic_context, &visitor);
```

(requires importing `walk_generic_context` from `generics.rs`)

---

_@AlexWaygood approved on 2025-10-13 16:46_

Nice!

---

_@dcreager reviewed on 2025-10-13 20:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:1525 on 2025-10-13 20:08_

I could've sworn I tried that! Ah well, done

---

_@MichaReiser reviewed on 2025-10-13 20:18_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/class.rs`:1467 on 2025-10-13 20:18_

Regarding the memory regression. Would it be possible to only cache the cases where we have a type var? Or is this already what this is doing :)

---

_@dcreager reviewed on 2025-10-13 20:19_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/class.rs`:1467 on 2025-10-13 20:19_

I don't think we can without walking the base class in the same way

---

_@MichaReiser reviewed on 2025-10-13 20:22_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer/builder.rs`:867 on 2025-10-13 20:22_

Should we move the check whether the rule is enabled further up (to avoid the entire scope walking?)

It might require extracting the following into a `lint_severity(name: &LintName) -> Option<Severity>` helper

https://github.com/astral-sh/ruff/blob/2ce3aba458d0ee6760639485a9857c4c89af63ac/crates/ty_python_semantic/src/types/context.rs#L404-L431

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:867 on 2025-10-13 20:26_

Could also experiment with reviving https://github.com/astral-sh/ruff/pull/18327?

---

_@AlexWaygood reviewed on 2025-10-13 20:26_

---

_@dcreager reviewed on 2025-10-13 20:38_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer/builder.rs`:867 on 2025-10-13 20:38_

Done

---

_Merged by @dcreager on 2025-10-13 23:30_

---

_Closed by @dcreager on 2025-10-13 23:30_

---

_Branch deleted on 2025-10-13 23:30_

---
