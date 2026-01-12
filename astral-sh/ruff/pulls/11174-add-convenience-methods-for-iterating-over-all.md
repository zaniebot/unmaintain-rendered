```yaml
number: 11174
title: Add convenience methods for iterating over all parameter nodes in a function
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: parameter-convenience-iterator
created_at: 2024-04-27T12:53:40Z
updated_at: 2024-04-29T11:31:28Z
url: https://github.com/astral-sh/ruff/pull/11174
synced_at: 2026-01-12T15:55:36Z
```

# Add convenience methods for iterating over all parameter nodes in a function

---

_@AlexWaygood_

## Summary

A pretty common pattern in our linter rules is the need to iterate over all parameters in a function. That ends up looking pretty verbose currently -- something like this:

```rs
for parameter in parameters
    .posonlyargs
    .iter()
    .chain(&parameters.args)
    .map(|param| param.parameter)
    .chain(parameters.vararg.as_deref())
    .chain(parameters.kwonlyargs.iter().map(|param| param.parameter))
    .chain(parameters.kwarg.as_deref())
{
    do_the_check()
}
```

It's also pretty easy to get this pattern subtly incorrect: for example, several of our rules currently incorrectly do `parameters.args.iter().chain(&parameters.posonlyargs)`, but positional-only parameters always precede positional-or-keyword parameters in Python.

This PR adds two convenience methods to the `ast::Parameters` struct in `ruff_python_ast/src/nodes.rs`, to make iterating over all parameters more ergonomic. `iter_non_variadic_params()` iterates over just the non-variadic parameters (`posonlyargs`, `args` and `kwonlyargs`), and `iter_all_params()` iterates over all parameters in a function, including any variadic paramerers (`vararg` and `kwarg`). Using these convenience methods in our linter rules significantly reduces duplication.

## Test Plan

`cargo test`. One parser snapshot changes slightly, but I think it's for the better: the syntax errors are now reported in the order that they appear in the source code. Previously, they were reported out of order.


---

_Label `internal` added by @AlexWaygood on 2024-04-27 12:53_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-04-27 12:53_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-04-27 12:53_

---

_Renamed from "Add convenience methods for iterating over all parameters in a function" to "Add convenience methods for iterating over all parameter nodes in a function" by @AlexWaygood on 2024-04-27 12:57_

---

_Comment by @github-actions[bot] on 2024-04-27 13:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3181 on 2024-04-27 17:36_

Nit: I think we should call this `AnyParameterRef` to make it clear that it is a reference and not an owned parameter (we could expose the same iterators with owned values)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-27 17:37_

I would name this `iter()` which is the first method that I normally try on any iterable type. 

You can also consider implementing 

```rust
impl<'a> IntoIterator<Item=&'a AnyParameter> for &'a Parameters {
	// 

}
```

It will probably require you to write a manual implementation of the `Iterator` or write up a very long type for the `IntoIter` type. 

What it gives you is that

```rust
for parameter in &parameters {
	/// 
}
```

works.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/property_with_parameters.rs`:67 on 2024-04-27 17:40_

The advantage of defining your own iterator type (vs using chaining) is that you could implement `ExactSizeIterator` and you could then call `.len()` here.

But I think the better solution here is to add a method to `parameters` to get this length

---

_@MichaReiser approved on 2024-04-27 17:40_

I like this a lot. I did not review all code changes. But we should have done this a long time ago :D

---

_@AlexWaygood reviewed on 2024-04-27 17:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-27 17:46_

> I would name this `iter()` which is the first method that I normally try on any iterable type.

I wondered about this. In the end I opted for the name I have now, as I felt like it emphasised the fact that `iter_all_params()` and `iter_non_variadic_params()` return iterators of different types. But I see your point; I'll go for the more conventional name 👍

---

_@AlexWaygood reviewed on 2024-04-27 18:38_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-27 18:38_

> or write up a very long type for the `IntoIter` type.

Seems like it would be something like

```rs
type ParameterIterator<'a> = Chain<
    Chain<
        Chain<
            Map<
                Chain<
                    std::slice::Iter<'a, ParameterWithDefault>,
                    std::slice::Iter<'a, ParameterWithDefault>,
                >,
                dyn FnOnce(&ParameterWithDefault) -> AnyParameterRef<'a>,
            >,
            std::option::IntoIter<AnyParameterRef<'a>>,
        >,
        Map<
            std::slice::Iter<'a, ParameterWithDefault>,
            dyn FnOnce(ParameterWithDefault) -> AnyParameterRef<'a>,
        >,
    >,
    std::option::IntoIter<AnyParameterRef<'a>>,
>;
```

---

_@MichaReiser reviewed on 2024-04-27 19:11_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-27 19:11_

Hehe yep :D The dyn part surprises me a bit

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/property_with_parameters.rs`:67 on 2024-04-27 21:38_

> But I think the better solution here is to add a method to `parameters` to get this length

Currently the rule is doing something a little odd: it's checking how many nonvariadic parameters there are in this function, but paying no attention to how many variadic parameters there are. That makes no sense to me based on what the rule is supposed to be checking: I think it makes just as much sense to emit an error if an `@property`-decorated function has `*args` or `**kwargs` parameters, as that's just as invalid.

I think maybe I'll just revert all changes to this file for now, and fix the bug (at least, I think it's a bug) in a separate PR, with added tests.

---

_@AlexWaygood reviewed on 2024-04-27 21:38_

---

_@AlexWaygood reviewed on 2024-04-27 21:56_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-27 21:56_

> The dyn part surprises me a bit

Yeah, that part is the only bit that doesn't compile, and I don't know how to make it compile :P

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-28 07:34_

It might be easier to write your own iterator type. It will require a lot of boilerplate 

---

_@MichaReiser reviewed on 2024-04-28 07:34_

---

_@AlexWaygood reviewed on 2024-04-28 14:44_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-28 14:44_

I managed to get a custom iterator type working. It did indeed take a lot of boilerplate. I'm not sure that it's worth it right now, as there would only be three callsites currently where we'd be able to directly make use of the `impl<'a> IntoIterator for &'a Parameters` implementation:

<details>

```diff
diff --git a/crates/ruff_linter/src/checkers/ast/mod.rs b/crates/ruff_linter/src/checkers/ast/mod.rs
index c4794749f..566924b53 100644
--- a/crates/ruff_linter/src/checkers/ast/mod.rs
+++ b/crates/ruff_linter/src/checkers/ast/mod.rs
@@ -604,7 +604,7 @@ impl<'a> Visitor<'a> for Checker<'a> {
                     self.visit_type_params(type_params);
                 }
 
-                for parameter in parameters.iter() {
+                for parameter in &**parameters {
                     if let Some(expr) = parameter.annotation() {
                         if singledispatch && !parameter.is_variadic() {
                             self.visit_runtime_required_annotation(expr);
diff --git a/crates/ruff_python_ast/src/node.rs b/crates/ruff_python_ast/src/node.rs
index 3599e49c8..d3192e275 100644
--- a/crates/ruff_python_ast/src/node.rs
+++ b/crates/ruff_python_ast/src/node.rs
@@ -4222,7 +4222,7 @@ impl AstNode for Parameters {
     where
         V: PreorderVisitor<'a> + ?Sized,
     {
-        for parameter in self.iter() {
+        for parameter in self {
             match parameter {
                 AnyParameterRef::NonVariadic(parameter_with_default) => {
                     visitor.visit_parameter_with_default(parameter_with_default);
diff --git a/crates/ruff_python_ast/src/nodes.rs b/crates/ruff_python_ast/src/nodes.rs
index b464ffe68..568703fba 100644
--- a/crates/ruff_python_ast/src/nodes.rs
+++ b/crates/ruff_python_ast/src/nodes.rs
@@ -2,6 +2,7 @@
 
 use std::fmt;
 use std::fmt::Debug;
+use std::iter::FusedIterator;
 use std::ops::Deref;
 use std::slice::{Iter, IterMut};
 use std::sync::OnceLock;
@@ -3271,14 +3272,8 @@ impl Parameters {
     }
 
     /// Returns an iterator over all parameters included in this [`Parameters`] node.
-    pub fn iter(&self) -> impl Iterator<Item = AnyParameterRef> {
-        self.posonlyargs
-            .iter()
-            .chain(&self.args)
-            .map(AnyParameterRef::NonVariadic)
-            .chain(self.vararg.as_deref().map(AnyParameterRef::Variadic))
-            .chain(self.kwonlyargs.iter().map(AnyParameterRef::NonVariadic))
-            .chain(self.kwarg.as_deref().map(AnyParameterRef::Variadic))
+    pub fn iter(&self) -> ParametersIterator {
+        ParametersIterator::new(self)
     }
 
     /// Returns the total number of parameters included in this [`Parameters`] node.
@@ -3305,6 +3300,81 @@ impl Parameters {
     }
 }
 
+pub struct ParametersIterator<'a> {
+    parameters: &'a Parameters,
+    index: usize,
+}
+
+impl<'a> ParametersIterator<'a> {
+    fn new(parameters: &'a Parameters) -> Self {
+        Self {
+            parameters,
+            index: 0,
+        }
+    }
+}
+
+impl<'a> Iterator for ParametersIterator<'a> {
+    type Item = AnyParameterRef<'a>;
+
+    fn next(&mut self) -> Option<Self::Item> {
+        if self.index >= self.parameters.len() {
+            return None;
+        }
+
+        let mut index = self.index;
+        self.index += 1;
+
+        let Parameters {
+            range: _,
+            posonlyargs,
+            args,
+            vararg,
+            kwonlyargs,
+            kwarg,
+        } = self.parameters;
+
+        if index < posonlyargs.len() {
+            return Some(AnyParameterRef::NonVariadic(&posonlyargs[index]));
+        }
+        index -= posonlyargs.len();
+        if index < args.len() {
+            return Some(AnyParameterRef::NonVariadic(&args[index]));
+        }
+        index -= args.len();
+        if let Some(vararg) = vararg {
+            if index == 0 {
+                return Some(AnyParameterRef::Variadic(vararg));
+            }
+            index -= 1;
+        }
+        if index < kwonlyargs.len() {
+            return Some(AnyParameterRef::NonVariadic(&kwonlyargs[index]));
+        }
+        if let Some(kwarg) = kwarg.as_deref() {
+            Some(AnyParameterRef::Variadic(kwarg))
+        } else {
+            unreachable!()
+        }
+    }
+
+    fn size_hint(&self) -> (usize, Option<usize>) {
+        let remaining = self.parameters.len() - self.index;
+        (remaining, Some(remaining))
+    }
+}
+
+impl<'a> ExactSizeIterator for ParametersIterator<'a> {}
+impl<'a> FusedIterator for ParametersIterator<'a> {}
+
+impl<'a> IntoIterator for &'a Parameters {
+    type IntoIter = ParametersIterator<'a>;
+    type Item = AnyParameterRef<'a>;
+    fn into_iter(self) -> Self::IntoIter {
+        self.iter()
+    }
+}
+
 /// An alternative type of AST `arg`. This is used for each function argument that might have a default value.
 /// Used by `Arguments` original type.
 ///
diff --git a/crates/ruff_python_parser/src/parser/statement.rs b/crates/ruff_python_parser/src/parser/statement.rs
index 80603ecbc..69d7ec8a5 100644
--- a/crates/ruff_python_parser/src/parser/statement.rs
+++ b/crates/ruff_python_parser/src/parser/statement.rs
@@ -3374,7 +3374,7 @@ impl<'src> Parser<'src> {
         let mut all_arg_names =
             FxHashSet::with_capacity_and_hasher(parameters.len(), BuildHasherDefault::default());
 
-        for parameter in parameters.iter() {
+        for parameter in parameters {
             let range = parameter.name().range();
             let param_name = parameter.name().as_str();
             if !all_arg_names.insert(param_name) {
```

</details>

What do you think? Worth it or not?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-29 07:06_

I personally don't mind the iterator boiler plate. It's code that we write once and we benefit from an improved API in all our downstream crates. 

I would probably write this without using indices (which is closer to the iterator chain). 

```rust
struct ParametersIter<'a> {
    posonlyargs: std::slice::Iter<'a, ParameterWithDefault>,
    args: std::slice::Iter<'a, ParameterWithDefault>,
    vararg: Option<&'a Parameter>,
    kwonlyargs: std::slice::Iter<'a, ParameterWithDefault>,
    kwarg: Option<&'a Parameter>,
}

impl<'a> Iterator for ParametersIter<'a> {
    type Item = AnyParameterRef<'a>;

    fn next(&mut self) -> Option<Self::Item> {
        if let Some(posonly) = self.posonlyargs.next() {
            return Some(AnyParameterRef::NonVariadic(posonly));
        }

        if let Some(arg) = self.args.next() {
            return Some(AnyParameterRef::NonVariadic(arg));
        }

        if let Some(vararg) = self.vararg.take() {
            return Some(AnyParameterRef::Variadic(vararg));
        }

        if let Some(kwonly) = self.kwonlyargs.next() {
            return Some(AnyParameterRef::NonVariadic(kwonly));
        }

        Some(AnyParameterRef::Variadic(self.kwarg.take()?))
    }

    fn size_hint(&self) -> (usize, Option<usize>) {
        let posonlyargs_len = self.posonlyargs.len();
        let args_len = self.args.len();
        let kwonlyargs_len = self.kwonlyargs.len();
        let vararg_len = self.vararg.is_some() as usize;
        let kwarg_len = self.kwarg.is_some() as usize;

        let compute_upper = || {
            posonlyargs_len
                .checked_add(args_len)?
                .checked_add(kwonlyargs_len)?
                .checked_add(vararg_len)?
                .checked_add(kwarg_len)
        };

        let lower = posonlyargs_len
            .saturating_add(args_len)
            .saturating_add(kwonlyargs_len)
            .saturating_add(vararg_len)
            .saturating_add(kwarg_len);

        let upper = compute_upper();
        (lower, upper)
    }
}

impl FusedIterator for ParametersIter<'_> {}

impl DoubleEndedIterator for ParametersIter<'_> {
    fn next_back(&mut self) -> Option<Self::Item> {
        if let Some(kwarg) = self.kwarg.take() {
            return Some(AnyParameterRef::Variadic(kwarg));
        }

        if let Some(kwonly) = self.kwonlyargs.next() {
            return Some(AnyParameterRef::NonVariadic(kwonly));
        }

        if let Some(vararg) = self.vararg.take() {
            return Some(AnyParameterRef::Variadic(vararg));
        }

        if let Some(arg) = self.args.next() {
            return Some(AnyParameterRef::NonVariadic(arg));
        }

        Some(AnyParameterRef::NonVariadic(self.posonlyargs.next()?))
    }
}
```

If we want to be picky, then implementing `ExactSizeIterator` isn't possible because adding the length of two vectors can, in theory, overflow. 

---

_@MichaReiser reviewed on 2024-04-29 07:06_

---

_@dhruvmanila approved on 2024-04-29 08:22_

---

_@AlexWaygood reviewed on 2024-04-29 08:47_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-29 08:47_

> If we want to be picky, then implementing `ExactSizeIterator` isn't possible because adding the length of two vectors can, in theory, overlap.

I had thought that there was a hard limit of 255 on the number of parameters a Python function was allowed to have. But it seems like that was actually removed many versions ago. So this indeed isn't safe; thanks!

---

_@MichaReiser reviewed on 2024-04-29 09:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3275 on 2024-04-29 09:13_

We could argue that implementing `ExactSizeIterator` is safe for us (by calling .unwrap() on the `checked_add chain`). it is safe because no program with a max text size of 4GB can have more than u32 parameters. However, that's only true on platforms where `usize` is at least a `u32`.

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:3302 on 2024-04-29 09:14_

Oh, I suppose this could theoretically overflow, given that Python functions can have an arbitrary number of parameters, so the number of parameters could theoretically exceed `usize::MAX`. This isn't a new issue -- we were already doing this calculation in `crates/ruff_python_formatter/src/other/parameters.rs` and `crates/ruff_python_parser/src/parser/statement.rs`. But maybe we should do `checked_add()` here and return an `Option`?

The formatter function that uses this method returns a `Result`, so we should be able to gracefully handle an `Option` there. The parser function that calls this might be slightly trickier to refactor.

---

_@AlexWaygood reviewed on 2024-04-29 09:14_

---

_@MichaReiser reviewed on 2024-04-29 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:3302 on 2024-04-29 09:18_

I would need to look into the supported platforms. But I think it's safe to say that it's impossible that this overflows for files that are <= 4GB and the platform uses at least a `u32` as `usize`.

---

_Merged by @AlexWaygood on 2024-04-29 10:36_

---

_Closed by @AlexWaygood on 2024-04-29 10:36_

---

_Branch deleted on 2024-04-29 10:36_

---

_@AlexWaygood reviewed on 2024-04-29 11:31_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/rules/property_with_parameters.rs`:67 on 2024-04-29 11:31_

Followup PR at #11200

---
