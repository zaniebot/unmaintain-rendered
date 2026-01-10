```yaml
number: 21533
title: "[ty] Implement goto-type for inlay type hints"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/type-print
created_at: 2025-11-20T05:07:46Z
updated_at: 2025-11-20T14:46:02Z
url: https://github.com/astral-sh/ruff/pull/21533
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Implement goto-type for inlay type hints

---

_Pull request opened by @Gankra on 2025-11-20 05:07_

This PR generalizes the signature_help system's SignatureWriter which could get the subspans of function parameters.
We now have TypeDetailsWriter which is threaded between type's display implementations via a new `fmt_detailed` method that many of the Display types now have.

With this information we can properly add goto-type targets to our inlay hints. This also lays groundwork for any future "I want to render a type but get spans" work.

Also a ton of lifetimes are introduced to avoid things getting conflated with `'db`.

This PR is broken up into a series of commits:

* Generalizing `SignatureWriter` to `TypeDetailsWriter`, but not using it anywhere else. This commit was confirmed to be a non-functional change (no test results changed)
* Introducing `fmt_detailed` everywhere to thread through `TypeDetailsWriter` and annotate various spans as "being" a given Type -- this is also where I had to reckon with a ton of erroneous `&'db self`. This commit was also confirmed to be a non-functional change.
* Finally, actually using the results for goto-type on inlay hints!
* Regenerating snapshots, fixups, etc.

---

_Label `server` added by @Gankra on 2025-11-20 05:07_

---

_Review requested from @carljm by @Gankra on 2025-11-20 05:07_

---

_Label `ty` added by @Gankra on 2025-11-20 05:07_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-20 05:07_

---

_Review requested from @sharkdp by @Gankra on 2025-11-20 05:07_

---

_Review requested from @dcreager by @Gankra on 2025-11-20 05:07_

---

_Review requested from @MichaReiser by @Gankra on 2025-11-20 05:07_

---

_Comment by @Gankra on 2025-11-20 05:08_

Have had this bee in my bonnet all week the IDE feels SO much better with these being clickable!

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 05:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:566 on 2025-11-20 05:10_

This is the single-most important type span we introduce, since it's nearly every nominal type's name.

---

_@Gankra reviewed on 2025-11-20 05:10_

---

_Comment by @astral-sh-bot[bot] on 2025-11-20 05:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 44 diagnostics
+ Found 43 diagnostics


```

</details>


No memory usage changes detected ✅



---

_@Gankra reviewed on 2025-11-20 05:12_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:690 on 2025-11-20 05:12_

With apologies to `join` enjoyers but I had to hand-roll this gunk everywhere to invoke fmt_detailed

---

_@Gankra reviewed on 2025-11-20 05:13_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:722 on 2025-11-20 05:13_

A lot of the PR is me breaking up a nice pretty `write!` into individual parts to invoke `fmt_detailed` where appropriate.

---

_@Gankra reviewed on 2025-11-20 05:14_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:910 on 2025-11-20 05:14_

Deconflating these lifetimes was unfortunately loadbearing. I had to do that a lot.

---

_@Gankra reviewed on 2025-11-20 05:16_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:1495 on 2025-11-20 05:16_

An incredible victory for not wanting to have to reverse-engineer what parameters belong to nested signatures.

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:886 on 2025-11-20 08:25_

I find the tests here confusing where there's no inlay. Aren't those inlays that are hidden? Why do we render the inlay hint targets for those? I think we should omit those from the tests.

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:283 on 2025-11-20 08:29_

Should we bail early if `std::thread::panicking`?

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:236 on 2025-11-20 08:30_

```suggestion
            && let Ok(end) = details.label.text_len()
```

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:227 on 2025-11-20 08:31_

```suggestion
    start: Option<TextSize>,
```

Seems better to error early rather than silently skipping if `TextSize::try_from` fails in the `Drop`

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:690 on 2025-11-20 08:33_

You could introduce a `FmtDetailed` or `DisplayDetailed` trait and then preserve the existing formatting, making the overall code more like `Display` and `Debug`


```patch
Index: crates/ty_python_semantic/src/types/display.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ty_python_semantic/src/types/display.rs b/crates/ty_python_semantic/src/types/display.rs
--- a/crates/ty_python_semantic/src/types/display.rs	(revision 13fecff48afe8065b05e012e7f0cc5301bd244c4)
+++ b/crates/ty_python_semantic/src/types/display.rs	(date 1763628882673)
@@ -190,7 +190,17 @@
             payload: Some(detail),
         }
     }
+
+    fn join<'c>(&'c mut self, separator: &'static str) -> Join<'a, 'b, 'c, 'db> {
+        Join {
+            fmt: self,
+            separator,
+            result: Ok(()),
+            seen_first: false,
+        }
+    }
 }
+
 impl std::fmt::Write for TypeWriter<'_, '_, '_> {
     fn write_str(&mut self, val: &str) -> fmt::Result {
         match self {
@@ -205,6 +215,46 @@
     }
 }
 
+trait FmtDetailed<'db> {
+    fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result;
+}
+
+struct Join<'a, 'b, 'c, 'db> {
+    fmt: &'c mut TypeWriter<'a, 'b, 'db>,
+    separator: &'static str,
+    result: fmt::Result,
+    seen_first: bool,
+}
+
+impl<'db> Join<'_, '_, '_, 'db> {
+    fn entry(&mut self, item: &dyn FmtDetailed<'db>) -> &mut Self {
+        if self.seen_first {
+            self.result = self
+                .result
+                .and_then(|()| self.fmt.write_str(self.separator));
+        } else {
+            self.seen_first = true;
+        }
+        self.result = self.result.and_then(|()| item.fmt_detailed(self.fmt));
+        self
+    }
+
+    fn entries<I, F>(&mut self, items: I) -> &mut Self
+    where
+        I: IntoIterator<Item = F>,
+        F: FmtDetailed<'db>,
+    {
+        for item in items {
+            self.entry(&item);
+        }
+        self
+    }
+
+    fn finish(&mut self) -> fmt::Result {
+        self.result
+    }
+}
+
 pub enum TypeDetail<'db> {
     /// Dummy item to indicate a function signature's parameters have started
     SignatureStart,
@@ -412,7 +462,9 @@
             TypeWriter::Formatter(_) => unreachable!("Expected Details variant"),
         }
     }
+}
 
+impl<'db> FmtDetailed<'db> for DisplayType<'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         let representation = self.ty.representation(self.db, self.settings.clone());
         match self.ty {
@@ -504,7 +556,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> ClassDisplay<'db> {
+impl<'db> FmtDetailed<'db> for ClassDisplay<'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         let qualification_level = self.settings.qualified.get(&**self.class.name(self.db));
         if qualification_level.is_some() {
@@ -556,7 +608,7 @@
     }
 }
 
-impl<'db> DisplayRepresentation<'db> {
+impl<'db> FmtDetailed<'db> for DisplayRepresentation<'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         match self.ty {
             Type::Dynamic(dynamic) => write!(f, "{dynamic}"),
@@ -678,17 +730,15 @@
                             f.write_str("Overload[")?;
                         }
                         let separator = if self.settings.multiline { "\n" } else { ", " };
-                        let mut is_first = true;
+                        let mut join = f.join(separator);
                         for signature in signatures {
-                            if !is_first {
-                                f.write_str(separator)?;
-                            }
-                            is_first = false;
-                            signature
-                                .bind_self(self.db, Some(typing_self_ty))
-                                .display_with(self.db, self.settings.clone())
-                                .fmt_detailed(f)?;
+                            join.entry(
+                                &signature
+                                    .bind_self(self.db, Some(typing_self_ty))
+                                    .display_with(self.db, self.settings.clone()),
+                            );
                         }
+                        join.finish()?;
                         if !self.settings.multiline {
                             f.write_str("]")?;
                         }
@@ -875,7 +925,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayTuple<'_, 'db> {
+impl<'db> FmtDetailed<'db> for DisplayTuple<'_, 'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         f.write_str("tuple[")?;
         match self.tuple {
@@ -968,7 +1018,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayOverloadLiteral<'db> {
+impl<'db> FmtDetailed<'db> for DisplayOverloadLiteral<'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         let signature = self.literal.signature(self.db);
         let type_parameters = DisplayOptionalGenericContext {
@@ -1012,7 +1062,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayFunctionType<'db> {
+impl<'db> FmtDetailed<'db> for DisplayFunctionType<'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         let signature = self.ty.signature(self.db);
 
@@ -1088,7 +1138,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayGenericAlias<'db> {
+impl<'db> FmtDetailed<'db> for DisplayGenericAlias<'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         if let Some(tuple) = self.specialization.tuple(self.db) {
             tuple
@@ -1160,7 +1210,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayOptionalGenericContext<'_, 'db> {
+impl<'db> FmtDetailed<'db> for DisplayOptionalGenericContext<'_, 'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         if let Some(generic_context) = self.generic_context {
             generic_context
@@ -1187,14 +1237,6 @@
 }
 
 impl<'db> DisplayGenericContext<'_, 'db> {
-    fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
-        if self.full {
-            self.fmt_full(f)
-        } else {
-            self.fmt_normal(f)
-        }
-    }
-
     fn fmt_normal(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         let variables = self.generic_context.variables(self.db);
 
@@ -1229,6 +1271,16 @@
     }
 }
 
+impl<'db> FmtDetailed<'db> for DisplayGenericContext<'_, 'db> {
+    fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
+        if self.full {
+            self.fmt_full(f)
+        } else {
+            self.fmt_normal(f)
+        }
+    }
+}
+
 impl Display for DisplayGenericContext<'_, '_> {
     fn fmt(&self, f: &mut Formatter<'_>) -> fmt::Result {
         self.fmt_detailed(&mut TypeWriter::Formatter(f))
@@ -1276,14 +1328,6 @@
 }
 
 impl<'db> DisplaySpecialization<'db> {
-    fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
-        if self.full {
-            self.fmt_full(f)
-        } else {
-            self.fmt_normal(f)
-        }
-    }
-
     fn fmt_normal(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         f.write_char('[')?;
         let types = self.specialization.types(self.db);
@@ -1320,6 +1364,16 @@
     }
 }
 
+impl<'db> FmtDetailed<'db> for DisplaySpecialization<'db> {
+    fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
+        if self.full {
+            self.fmt_full(f)
+        } else {
+            self.fmt_normal(f)
+        }
+    }
+}
+
 impl Display for DisplaySpecialization<'_> {
     fn fmt(&self, f: &mut Formatter<'_>) -> fmt::Result {
         self.fmt_detailed(&mut TypeWriter::Formatter(f))
@@ -1370,7 +1424,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayCallableType<'_, 'db> {
+impl<'db> FmtDetailed<'db> for DisplayCallableType<'_, 'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         match self.signatures.overloads.as_slice() {
             [signature] => signature
@@ -1444,7 +1498,9 @@
             TypeWriter::Formatter(_) => unreachable!("Expected Details variant"),
         }
     }
+}
 
+impl<'db> FmtDetailed<'db> for DisplaySignature<'_, 'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         // Immediately write a marker signaling we're starting a signature
         let _ = f.with_detail(TypeDetail::SignatureStart);
@@ -1564,7 +1620,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayParameter<'_, 'db> {
+impl<'db> FmtDetailed<'db> for DisplayParameter<'_, 'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         if let Some(name) = self.param.display_name() {
             f.write_str(&name)?;
@@ -1666,7 +1722,7 @@
     max_when_elided: 3,
 };
 
-impl<'db> DisplayUnionType<'_, 'db> {
+impl<'db> FmtDetailed<'db> for DisplayUnionType<'_, 'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         fn is_condensable(ty: Type<'_>) -> bool {
             matches!(
@@ -1832,7 +1888,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayIntersectionType<'_, 'db> {
+impl<'db> FmtDetailed<'db> for DisplayIntersectionType<'_, 'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         let tys = self
             .ty
@@ -1887,7 +1943,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayMaybeNegatedType<'db> {
+impl<'db> FmtDetailed<'db> for DisplayMaybeNegatedType<'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         if self.negated {
             f.write_str("~")?;
@@ -1913,7 +1969,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayMaybeParenthesizedType<'db> {
+impl<'db> FmtDetailed<'db> for DisplayMaybeParenthesizedType<'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         let write_parentheses = |f: &mut TypeWriter<'_, '_, 'db>| {
             f.write_char('(')?;
@@ -2001,7 +2057,7 @@
     settings: DisplaySettings<'db>,
 }
 
-impl<'db> DisplayTypeArray<'_, 'db> {
+impl<'db> FmtDetailed<'db> for DisplayTypeArray<'_, 'db> {
     fn fmt_detailed(&self, f: &mut TypeWriter<'_, '_, 'db>) -> fmt::Result {
         let mut is_first = true;
         for ty in self.types {
```



---

_@MichaReiser reviewed on 2025-11-20 09:09_

The implementation looks good to me. I've one suggestion that should help to reduce the necessary changes because of `Join`. 

I do find the test very confusing as we now see go to targets in places where we don't show any inlay hints. I think we should change this before landing this PR

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:473 on 2025-11-20 12:51_

It would be cool if we could also have goto-definition for the `Literal` part of e.g. `Literal[42]`. We don't have any documentation for `typing.Literal` in our vendored typeshed stubs right now (because we currently only add docstrings for classes, functions and modules), but it would be nice to add that capability to docstring-adder at some point. Then when you goto definition for [the symbol `Literal`](https://github.com/astral-sh/ruff/blob/02c102da885922906aa57a0468fde909cbbd2ba4/crates/ty_vendored/vendor/typeshed/stdlib/typing.pyi#L351) you'd see all of this lovely explanation in the stub file to tell you how `Literal` types work (this is the docstring for the symbol at runtime):

```
% uvx python -m pydoc typing.Literal   
Help on _TypedCacheSpecialForm in typing:

typing.Literal = typing.Literal
    Special typing form to define literal types (a.k.a. value types).

    This form can be used to indicate to type checkers that the corresponding
    variable or function parameter has a value equivalent to the provided
    literal (or one of several literals)::

        def validate_simple(data: Any) -> Literal[True]:  # always returns True
            ...

        MODE = Literal['r', 'rb', 'w', 'wb']
        def open_helper(file: str, mode: MODE) -> str:
            ...

        open_helper('/some/path', 'r')  # Passes type check
        open_helper('/other/path', 'typo')  # Error in type checker

    Literal[...] cannot be subclassed. At runtime, an arbitrary value
    is allowed as type argument to Literal[...], but type checkers may
    impose restrictions.
```

The symbol `typing.Literal` itself has type `Type::SpecialForm(SpecialFormType::Literal)` in our model, so would you do that like this?

```suggestion
                f.with_detail(TypeDetail::Type(Type::SpecialForm(SpecialFormType::Literal))).write_str("Literal")?;
                f.write_char('[')?;
```

---

_@Gankra reviewed on 2025-11-20 13:37_

---

_Review comment by @Gankra on `crates/ty_ide/src/inlay_hints.rs`:886 on 2025-11-20 13:37_

Fixed (the test spans referenced the original doc and not the inlayed one)

---

_@Gankra reviewed on 2025-11-20 13:38_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:283 on 2025-11-20 13:38_

I don't think that's necessary?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:509 on 2025-11-20 13:45_

Are those to be defined diagnostics?

---

_@MichaReiser approved on 2025-11-20 13:47_

---

_@MichaReiser reviewed on 2025-11-20 13:47_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/inlay_hints.rs`:886 on 2025-11-20 13:47_

Okay, that makes way more sense now :)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:611 on 2025-11-20 13:50_

if the type is `Type::Dynamic(DynamicType::Any)`, it would be cool if we could goto the definition for the `Any` symbol (with has type `Type::SpecialForm(SpecialFormType::Any)` in our model)

---

_Comment by @Gankra on 2025-11-20 13:55_

Micha you're god damn incredible, the join impl Just Worked.

---

_@Gankra reviewed on 2025-11-20 14:00_

---

_Review comment by @Gankra on `crates/ty_ide/src/inlay_hints.rs`:509 on 2025-11-20 14:00_

I have to gather them up transiently while we create the annotated file, and then create the annotated file, and then create them properly.

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:1 on 2025-11-20 14:02_

Could you also add tests for generic-alias types, e.g.

```py
class Foo[T]: ...

a [: <class 'Foo[int]'>] = Foo[int]
```

and subclass-of types, e.g.

```py
def f(x: list[str]):
    y [: type[list[str]]] = type(x)
```

For the first one I'd expect goto targets for both `Foo` and `int` in `<class 'Foo[int]>`. For the second one I'd expect goto targets for both `list `and `str` in `type[list[str]]`

---

_@AlexWaygood reviewed on 2025-11-20 14:03_

Very cool!

---

_Comment by @AlexWaygood on 2025-11-20 14:04_

looks like you'll need to add some snapshot filters to get snapshots that include filepaths working on Windows 🙃

---

_@Gankra reviewed on 2025-11-20 14:13_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:473 on 2025-11-20 14:13_

I know you literally gave me the implementation but I'd like to keep these things to followups (I'm terrified of this PR getting merge conflicted to hell).

---

_@Gankra reviewed on 2025-11-20 14:13_

---

_Review comment by @Gankra on `crates/ty_python_semantic/src/types/display.rs`:611 on 2025-11-20 14:13_

(similarly leaving as a followup)

---

_@Gankra reviewed on 2025-11-20 14:14_

---

_Review comment by @Gankra on `crates/ty_ide/src/inlay_hints.rs`:1 on 2025-11-20 14:14_

Cool with this also being a followup?

---

_@AlexWaygood reviewed on 2025-11-20 14:14_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/display.rs`:473 on 2025-11-20 14:14_

np

---

_@AlexWaygood reviewed on 2025-11-20 14:14_

---

_Review comment by @AlexWaygood on `crates/ty_ide/src/inlay_hints.rs`:1 on 2025-11-20 14:14_

of course!

---

_Comment by @Gankra on 2025-11-20 14:28_

Ughhhh

```
      299   299 │ info[inlay-hint-location]: Inlay Hint Target
      300       │-  --> stdlib/string/templatelib.pyi:10:7
            300 │+  --> stdlib\string\templatelib.pyi:10:7
```

This is really annoying because `\` can appear in like `\x08` and I think I'm just gonna ask everyone to agree to not Notice I'm clobbering those in the inlay hint tests.

---

_Comment by @Gankra on 2025-11-20 14:35_

Actually I'm extremely confused why this isn't a problem in any of the goto tests

---

_Comment by @Gankra on 2025-11-20 14:39_

(More followups for the followup pile I guess)

---

_Merged by @Gankra on 2025-11-20 14:40_

---

_Closed by @Gankra on 2025-11-20 14:40_

---

_Branch deleted on 2025-11-20 14:40_

---

_Comment by @MatthewMckee4 on 2025-11-20 14:46_

@Gankra, Sick!

---
