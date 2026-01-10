```yaml
number: 12770
title: "Treat `type(Protocol)` et al as metaclass base"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/meta
created_at: 2024-08-09T02:04:55Z
updated_at: 2024-08-09T20:19:45Z
url: https://github.com/astral-sh/ruff/pull/12770
synced_at: 2026-01-10T21:47:02Z
```

# Treat `type(Protocol)` et al as metaclass base

---

_Pull request opened by @charliermarsh on 2024-08-09 02:04_

## Summary

Closes https://github.com/astral-sh/ruff/issues/12736.


---

_Review requested from @AlexWaygood by @charliermarsh on 2024-08-09 02:04_

---

_@charliermarsh reviewed on 2024-08-09 02:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/class.rs`:26 on 2024-08-09 02:05_

I had to change this to make it more flexible, which in turn required changing all the call sites mechanically.

---

_@charliermarsh reviewed on 2024-08-09 02:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/class.rs`:146 on 2024-08-09 02:05_

This is the only logic change.

---

_@charliermarsh reviewed on 2024-08-09 02:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/class.rs`:143 on 2024-08-09 02:05_

Are any of the others allowed as calls? I don't think so.

---

_@charliermarsh reviewed on 2024-08-09 02:05_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/class.rs`:144 on 2024-08-09 02:05_

Can any of these be subscripted? I don't think so...

---

_Label `bug` added by @charliermarsh on 2024-08-09 02:06_

---

_@MichaReiser reviewed on 2024-08-09 06:07_

---

_Review comment by @MichaReiser on `crates/ruff_python_semantic/src/analyze/class.rs`:26 on 2024-08-09 06:07_

It may be worth to introduce a new method instead:

```rust
pub fn any_base_class(class_def: &ast::StmtClassDef, semantic: &SemanticModel, func: &dyn Fn(&Expr) -> bool) -> bool {
	// The body of `any_qualified_name` after your refactor
	...
}


pub fn any_qualified_base_class(class_def, &ast::StmtClassDef, semantic: &SemanticModel, func: &dyn Fn(QualifiedName) -> bool) -> bool {
	any_base_class(class_def, semantic, |expr| semantic.resolve_qualified_name(map_subscript(expr)).is_some_and_func(func))
}
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:143 on 2024-08-09 09:02_

Not in the same way... But you need to distinguish between the two different ways of calling type here. X is a metaclass but Y is not:

```pycon
>>> class X(type(int)): pass
... 
>>> class Y(type('Foo', (), {})): pass
... 
>>> class A(metaclass=X): pass
... 
>>> class B(metaclass=Y): pass
... 
Traceback (most recent call last):
  File "<string>", line 1, in <module>
TypeError: Y() takes no arguments
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:144 on 2024-08-09 09:03_

builtins.type can

```pycon
>>> class X(type[int]): pass
... 
>>> X.__mro__
(<class '__main__.X'>, <class 'type'>, <class 'object'>)
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:26 on 2024-08-09 09:05_

+1 for Micha's suggestion. This change otherwise makes most callsites a lot more verbose.

One _small advantage_ of the way Charlie currently has it is that it means that the caller is forced to consider whether the bases could actually be subscript expressions or not. In a lot of situations it's not strictly correct to use `map_subscript` on a class base, but the previous function was doing it unconditionally on all class bases it iterated over. But I just don't think the theoretical correctness issue there is that significant, really.

---

_@AlexWaygood requested changes on 2024-08-09 09:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/class.rs`:26 on 2024-08-09 13:26_

Honestly I don't agree -- it's actually way more consistent (and flexible) for us to pass back the expression, and we use `resolve_qualified_name` for all related checks elsewhere -- but I also don't feel strongly. I'll just change it.

---

_@charliermarsh reviewed on 2024-08-09 13:26_

---

_@AlexWaygood reviewed on 2024-08-09 13:28_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:26 on 2024-08-09 13:28_

I don't feel too strongly about this point either tbh. It just seems a shame that so many callsites get more verbose when we only need the extra flexibility in one instance (currently)

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-08-09 17:33_

---

_Comment by @codspeed-hq[bot] on 2024-08-09 17:39_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/charlie/meta)

### Merging #12770 will **not alter performance**

<sub>Comparing <code>charlie/meta</code> (f98f274) with <code>main</code> (37b9bac)</sub>



### Summary

`✅ 32` untouched benchmarks






---

_@AlexWaygood reviewed on 2024-08-09 17:42_

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:143 on 2024-08-09 17:42_

Not sure this has been addressed. A `type()` call is only likely to create a metaclass if it's the one-argument `type()` call -- the three-argument `type()` call is just going to create an instance of `type` (i.e., a normal class). So I think we should do something like this:

```diff
diff --git a/crates/ruff_python_semantic/src/analyze/class.rs b/crates/ruff_python_semantic/src/analyze/class.rs
index 73ab1c58a..7277fad21 100644
--- a/crates/ruff_python_semantic/src/analyze/class.rs
+++ b/crates/ruff_python_semantic/src/analyze/class.rs
@@ -124,13 +124,16 @@ pub fn is_enumeration(class_def: &ast::StmtClassDef, semantic: &SemanticModel) -
 /// Returns `true` if the given class is a metaclass.
 pub fn is_metaclass(class_def: &ast::StmtClassDef, semantic: &SemanticModel) -> bool {
     any_base_class(class_def, semantic, &|expr| match expr {
-        Expr::Call(ast::ExprCall { func, .. }) => {
+        Expr::Call(ast::ExprCall {
+            func, arguments, ..
+        }) => {
             // Ex) `class Foo(type(Protocol)): ...`
-            semantic
-                .resolve_qualified_name(func.as_ref())
-                .is_some_and(|qualified_name| {
-                    matches!(qualified_name.segments(), ["" | "builtins", "type"])
-                })
+            arguments.len() == 1
+                && semantic
+                    .resolve_qualified_name(func.as_ref())
+                    .is_some_and(|qualified_name| {
+                        matches!(qualified_name.segments(), ["" | "builtins", "type"])
+                    })
         }
```

---

_Comment by @github-actions[bot] on 2024-08-09 17:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@charliermarsh reviewed on 2024-08-09 19:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_semantic/src/analyze/class.rs`:143 on 2024-08-09 19:30_

I considered this not worth supporting but it's simple enough if you prefer!

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:136 on 2024-08-09 19:33_

```suggestion
                && semantic
                    .match_builtin_expr(func.as_ref(), "type")
```

---

_Review comment by @AlexWaygood on `crates/ruff_python_semantic/src/analyze/class.rs`:143 on 2024-08-09 19:34_

```suggestion
                .match_builtin_expr(value.as_ref(), "type")
```

---

_@AlexWaygood approved on 2024-08-09 19:34_

---

_Comment by @charliermarsh on 2024-08-09 20:05_

Thanks Alex!

---

_Merged by @charliermarsh on 2024-08-09 20:10_

---

_Closed by @charliermarsh on 2024-08-09 20:10_

---

_Branch deleted on 2024-08-09 20:10_

---
