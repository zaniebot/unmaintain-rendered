```yaml
number: 815
title: "Remove `pull-types:skip` directive in `callable.md` mdtest"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - server
  - fatal
assignees: []
created_at: 2025-07-11T11:43:15Z
updated_at: 2025-08-05T16:17:18Z
url: https://github.com/astral-sh/ty/issues/815
synced_at: 2026-01-10T02:06:24Z
```

# Remove `pull-types:skip` directive in `callable.md` mdtest

---

_Issue opened by @AlexWaygood on 2025-07-11 11:43_

If this diff is applied to the Ruff main branch:

````diff
diff --git a/crates/ty_python_semantic/resources/mdtest/annotations/callable.md b/crates/ty_python_semantic/resources/mdtest/annotations/callable.md
index 688dcb321a..60ecbe13b7 100644
--- a/crates/ty_python_semantic/resources/mdtest/annotations/callable.md
+++ b/crates/ty_python_semantic/resources/mdtest/annotations/callable.md
@@ -58,8 +58,6 @@ def _(c: Callable[[int, 42, str, False], None]):
 
 ### Missing return type
 
-<!-- pull-types:skip -->
-
 Using a parameter list:
 
 ```py
````

Then the mdtest fails:

```
failures:

---- mdtest__annotations_callable stdout ----

callable.md - Callable - Invalid forms - Missing return type (c92a50e3fcef65ab)

  crates/ty_python_semantic/resources/mdtest/annotations/callable.md:64 Attempting to "pull types" caused a panic at crates/ty_python_semantic/src/semantic_model.rs:288:44
  crates/ty_python_semantic/resources/mdtest/annotations/callable.md:64 Note: either fix the panic or add the `<!-- pull-types:skip -->` directive to this test
  crates/ty_python_semantic/resources/mdtest/annotations/callable.md:64 
  crates/ty_python_semantic/resources/mdtest/annotations/callable.md:64 Failed to retrieve the inferred type for an `ast::Expr` node passed to `TypeInference::expression_type()`. The `TypeInferenceBuilder` should infer and store types for all `ast::Expr` nodes in any `TypeInference` region it analyzes.
  crates/ty_python_semantic/resources/mdtest/annotations/callable.md:64 
  crates/ty_python_semantic/resources/mdtest/annotations/callable.md:64 run with `RUST_BACKTRACE=1` environment variable to display a backtrace

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='callable.md - Callable - Invalid forms - Missing return type (c92a50e3fcef65ab)'
MDTEST_TEST_FILTER='callable.md - Callable - Invalid forms - Missing return type (c92a50e3fcef65ab)' cargo test -p ty_python_semantic --test mdtest -- mdtest__annotations_callable

--------------------------------------------------
```

That indicates that the ty server would crash if you parameterized `Callable` in exactly the wrong way and then hovered over it in an IDE. We should fix that by making sure we store a `Type` for each sub-expression even if it's an invalid type annotation.

This is the last use of the `pull-types:skip` directive remaining in our mdtests.

---

_Label `help wanted` added by @AlexWaygood on 2025-07-11 11:43_

---

_Label `server` added by @AlexWaygood on 2025-07-11 11:43_

---

_Label `fatal` added by @AlexWaygood on 2025-07-11 11:43_

---

_Comment by @lipefree on 2025-07-14 06:59_

I will be happy to look into this one !

---

_Assigned to @lipefree by @AlexWaygood on 2025-07-14 16:51_

---

_Comment by @AlexWaygood on 2025-08-05 16:17_

Fixed in https://github.com/astral-sh/ruff/pull/19517

---

_Closed by @AlexWaygood on 2025-08-05 16:17_

---
