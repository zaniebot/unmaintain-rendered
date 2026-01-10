```yaml
number: 15200
title: "[red-knot] add call checking"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/checksigs
created_at: 2024-12-30T08:22:39Z
updated_at: 2025-03-15T03:04:46Z
url: https://github.com/astral-sh/ruff/pull/15200
synced_at: 2026-01-10T19:49:01Z
```

# [red-knot] add call checking

---

_Pull request opened by @carljm on 2024-12-30 08:22_

## Summary

This implements checking of calls.

I ended up following Micha's original suggestion from back when the signature representation was first introduced, and flattening it to a single array of parameters. This turned out to be easier to manage, because we can represent parameters using indices into that array, and represent the bound argument types as an array of the same length.

Starred and double-starred arguments are still TODO; these won't be very useful until we have generics.

The handling of diagnostics is just hacked into `return_ty_result`, which was already inconsistent about whether it emitted diagnostics or not; now it's even more inconsistent. This needs to be addressed, but could be a follow-up.

The new benchmark errors here surface the need for intersection support in `is_assignable_to`.

Fixes #14161.

## Test Plan

Added mdtests.


---

_Label `red-knot` added by @carljm on 2024-12-30 08:22_

---

_Comment by @github-actions[bot] on 2024-12-30 08:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @carljm on 2025-01-06 17:11_

---

_Review requested from @MichaReiser by @carljm on 2025-01-06 17:11_

---

_Review requested from @AlexWaygood by @carljm on 2025-01-06 17:11_

---

_Review requested from @sharkdp by @carljm on 2025-01-06 17:11_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:76 on 2025-01-06 18:28_

I feel like the thing that's gone wrong here is that they're not allowed to pass an object of type `Literal["foo"]` to the `x` parameter of the `f` function, and the reason why they're not allowed to do that is because `Literal["foo"]` is not assignable to `int`. It feels like this error message jumps to the "why" before explaining the "what". I feel like my ideal error message (when we have rich diagnostics) would be something like this:

```
error: [invalid-argument-type]

Cannot pass object of type `Literal["foo"]` to the `x` parameter of function `f`.

Explanation: the `x` parameter expects a type assignable to `int`,
  but `Literal["foo"]` is not assignable to `int`.
```

I'm okay with something a bit more concise while we're restricted to having diagnostics all on the same line, but I _do_ think we should definitely mention the name of the function in the error message (mypy, pyright and pyre all do)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:86 on 2025-01-06 18:32_

Is it also worth mentioning the index of the parameter if the parameter is positional-only?

```suggestion
# error: 15 [invalid-argument-type] "Cannot pass object of type `Literal["foo"]` to the first parameter (parameter `x`) of function `f`, which expects a type assignable to `int`"
```

I ask this, because there are multiple stdlib functions which are documented as having a positional-only parameter with one name, but when you call `help()` on these functions in a Python REPL, `inspect.signature()` reports that the positional-only parameter has a totally different name. So the name that typeshed gives a parameter might not be the name a user is familiar with for that parameter for all functions.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:96 on 2025-01-06 18:34_

do we want to refer to the parameter as `args` or as `*args` in the error message here? I sorta feel like `*args` is more intuitive

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:126 on 2025-01-06 18:36_

same question here, I'd find referring to the parameter as `**kwargs` rather than `kwargs` a bit more intuitive

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:149 on 2025-01-06 18:37_

Here as well, I think it's important to mention the name of the function in the error message

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:259 on 2025-01-06 18:39_

I feel like the error message at runtime for this is much clearer:

```pycon
>>> def f(x: int) -> int:
...     return 1
...     
>>> f(1, x=2)
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    f(1, x=2)
    ~^^^^^^^^
TypeError: f() got multiple values for argument 'x'
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1686 on 2025-01-06 18:45_

off-topic here (pre-existing issue), but we should emit a diagnostic if somebody tries to call `bool(x)` where `type(x).__bool__` is annotated as returning a type that's not assignable to `bool`, since it causes an exception to be raised at runtime:

```pycon
>>> class Foo:
...     def __bool__(self): return 42
...     
>>> bool(Foo())
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    bool(Foo())
    ~~~~^^^^^^^
TypeError: __bool__ should return bool, returned int
```

Maybe you could add a TODO comment, while we're here?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1929 on 2025-01-06 18:48_

I think this TODO is now done with this PR?

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call.rs`:1 on 2025-01-06 18:49_

everything passes for me locally if this is removed!

```suggestion
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:13 on 2025-01-06 18:52_

If you implement the `FromIterator` trait for this struct, then you'll be able to just `.collect()` into a `CallArguments` instance. Here's an example of how I did this in `mro.rs`:

https://github.com/astral-sh/ruff/blob/5e9259c96c910fe9031b7781867c8b82bffa0b26/crates/red_knot_python_semantic/src/types/mro.rs#L160-L164

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1 on 2025-01-06 18:58_

there only seems to be one method in this file that is unused; I would apply this `allow()` attribute to that single method rather than to the whole file

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:42 on 2025-01-06 19:06_

It's quite surprising to implement `IntoIterator` for `&CallArguments` but not have a `CallArguments::iter()` method. If you add one, there's also a logic place you could use it elsewhere! I'd do this:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/call/arguments.rs b/crates/red_knot_python_semantic/src/types/call/arguments.rs
index 4341be43b..c4e0bc13b 100644
--- a/crates/red_knot_python_semantic/src/types/call/arguments.rs
+++ b/crates/red_knot_python_semantic/src/types/call/arguments.rs
@@ -27,6 +27,10 @@ impl<'db> CallArguments<'db> {
     pub(crate) fn first_argument(&self) -> Option<Type<'db>> {
         self.0.front().map(Argument::ty)
     }
+
+    pub(crate) fn iter(&self) -> impl Iterator<Item = &Argument<'db>> {
+        self.0.iter()
+    }
 }
 
 impl<'db, 'a> IntoIterator for &'a CallArguments<'db> {
diff --git a/crates/red_knot_python_semantic/src/types/call/bind.rs b/crates/red_knot_python_semantic/src/types/call/bind.rs
index ba25393ce..37751d7d9 100644
--- a/crates/red_knot_python_semantic/src/types/call/bind.rs
+++ b/crates/red_knot_python_semantic/src/types/call/bind.rs
@@ -22,7 +22,7 @@ pub(crate) fn bind_call<'db>(
     let mut errors = vec![];
     let mut next_positional = 0;
     let mut first_excess_positional = None;
-    for (argument_index, argument) in arguments.into_iter().enumerate() {
+    for (argument_index, argument) in arguments.iter().enumerate() {
         let (index, parameter, argument_ty) = match argument {
             Argument::Positional(ty) => {
                 let Some((index, parameter)) = signature
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-01-06 19:13_

I wonder if this would be a slightly more ergonomic API for your `Signature` struct?

```diff
--- a/crates/red_knot_python_semantic/src/types/call/bind.rs
+++ b/crates/red_knot_python_semantic/src/types/call/bind.rs
@@ -86,9 +86,7 @@ pub(crate) fn bind_call<'db>(
     }
     for (index, bound_ty) in parameter_tys.iter().enumerate() {
         if bound_ty.is_none() {
-            let param = signature
-                .parameter_at_index(index)
-                .expect("parameter_tys array should not be larger than number of parameters");
+            let param = &signature.parameters()[index];
             if param.is_variadic() || param.is_keywords() || param.default_ty().is_some() {
                 // variadic/keywords and defaulted arguments are not required
                 continue;
diff --git a/crates/red_knot_python_semantic/src/types/signatures.rs b/crates/red_knot_python_semantic/src/types/signatures.rs
index 319ec5db5..9e3ecbf4b 100644
--- a/crates/red_knot_python_semantic/src/types/signatures.rs
+++ b/crates/red_knot_python_semantic/src/types/signatures.rs
@@ -1,3 +1,5 @@
+use std::ops::Index;
+
 use super::{definition_expression_ty, Type};
 use crate::Db;
 use crate::{semantic_index::definition::Definition, types::todo_type};
@@ -70,6 +72,10 @@ impl<'db> Signature<'db> {
             .count()
     }
 
+    pub(super) fn parameters(&self) -> &Parameters<'db> {
+        &self.parameters
+    }
+
     pub(crate) fn parameter_at_index(&self, index: usize) -> Option<&Parameter<'db>> {
         self.parameters.0.get(index)
     }
@@ -183,6 +189,14 @@ impl<'db, 'a> IntoIterator for &'a Parameters<'db> {
     }
 }
 
+impl<'db> Index<usize> for Parameters<'db> {
+    type Output = Parameter<'db>;
+
+    fn index(&self, index: usize) -> &Self::Output {
+        &self.0[index]
+    }
+}
+
 #[derive(Clone, Debug, PartialEq, Eq)]
 pub(crate) enum Parameter<'db> {
     /// Positional-only parameter, e.g. `def f(x, /): ...`
```

---

_@AlexWaygood reviewed on 2025-01-06 19:14_

haven't finished reviewing (or reading the diff), but here's what I have so far

---

_@sharkdp reviewed on 2025-01-06 19:19_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:182 on 2025-01-06 19:19_

If we have a function call with multiple missing arguments like
```py
def f(x, y, z) -> int:
    return 1

f()
```
we currently emit three separate diagnostic messages compared to pyright/mypy, which both output a single message like `Missing positional arguments "x", "y", "z" in call to "f`. Not sure if it's worth investing time into, but that seems a bit nicer in terms of UX?

---

_@carljm reviewed on 2025-01-07 04:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1929 on 2025-01-07 04:08_

The TODO is not done, because we aren't checking here whether there were argument type errors, and refusing to use the `__getitem__` return type as the iteration type if so.

This PR certainly gets us much closer to being able to complete this TODO, but the PR is already big enough, I'd rather leave that as a separate follow-up.

---

_@carljm reviewed on 2025-01-07 04:11_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1 on 2025-01-07 04:11_

Good call; I just removed the unused method for now instead.

---

_@carljm reviewed on 2025-01-07 05:10_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-01-07 05:10_

I feel like it would be, except that we lose the parallel between `positional_at_index` and `parameter_at_index`, because `std::ops::Index` can only replace the latter, not the former. So it doesn't feel to me that overall it makes things clearer.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:76 on 2025-01-07 06:51_

"Cannot assign" is not supposed to be the "why", it is the "what" -- "assign" is used here to mean "match an argument to a parameter", similar to how you are using "pass". Pyright uses similar terminology. The verb "to pass" here doesn't feel quite right to me for some reason; you pass an argument to a function, but passing an argument to a parameter sounds wrong to my ear. But there may not be any good reason for that, and I can get over it. Either way, I think the important thing at this point is to make sure we have the right context available to construct the message we want, so on that note I'll add the callable name to the messages.


---

_@carljm reviewed on 2025-01-07 06:51_

---

_@carljm reviewed on 2025-01-07 07:09_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:76 on 2025-01-07 07:09_

(Added)

---

_@carljm reviewed on 2025-01-07 07:39_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:182 on 2025-01-07 07:39_

Good call, fixed.

---

_Comment by @carljm on 2025-01-07 07:50_

Thanks for the reviews! I think I've addressed all comments so far.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/arguments.rs`:23 on 2025-01-07 09:42_

The only call-site of `with_self` clones the `CallArguments`. That makes me think we should just use a `Vec` internally and instead take a `&self` reference here
```suggestion
    pub(crate) fn with_self(&self, self_ty: Type<'db>) -> Self {
        let mut arguments = Vec::with_capacity(self.0.len() + 1);
        arguments.push(Argument::Positional(self_ty));
        arguments.extend_from_slice(&self.0);

        Self(arguments)
    }
```

It not only simplifies the call site, it also has the benefit that all read operations are simpler (it can't be much simpler than a vec)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:59 on 2025-01-07 09:46_

Nit: `count` suggest that this operation is `O(n)`

```suggestion
    pub(crate) fn parameter_len(&self) -> usize {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2025-01-07 09:47_

Nit: `get_parameter` similar to `Vec::get`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:87 on 2025-01-07 09:47_


```suggestion
    pub(crate) fn get_positional(&self, index: usize) -> Option<&Parameter<'db>> {
        if let Some(candidate) = self.parameter_at_index(index) {
            if candidate.is_positional() {
                return Some(candidate);
            }
        }
        None
    }
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:57 on 2025-01-07 09:49_

Could we use `rfind`, considering that `kwargs` arguments are normally at the end?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:107 on 2025-01-07 09:49_

What happens if there's more than one parameter with the same name? 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:14 on 2025-01-07 09:50_

What happens if this order is no longer guaranteed, e.g. because there's invalid syntax?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:217 on 2025-01-07 09:51_

To avoid creating a `Name` just for lookup


```suggestion
    pub(crate) fn callable_by_name(&self, name: &str) -> bool {
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:303 on 2025-01-07 09:53_

Would it be useful to have this be an `Option<Type<'db>>` so that call sites can distinguish between an annotation was present vs the annotation is `Any`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/signatures.rs`:192 on 2025-01-07 09:54_

The existence of having both `Parameter` and `Param` is somewhat confusing. Should this be `ParameterKind` or `AnyParameter` and `Param` gets renamed to `Parameter` (and `ParamWithDefault` to `ParameterWithDefault`)?

The alternative is to change the representation to:

```rust
#[derive(Debug, Clone, PartialEq, Eq)]
struct Paramter<'db> {
    name: Option<Name,
    annotated_ty: Type<'db>,
    kind: ParameterKind<'db>
}

#[derive(Debug, Copy, Clone, PartialEq, Eq)]
enum ParameterKind<'db> {
    PositionalOnly { default: Type<'db> },
    PositionalOrKeyword { default: Type<'db> },
    Variadic,
    KeywordOnly { default: Type<'db> },
    Keywords,
}
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:1534 on 2025-01-07 09:59_

It's unfortunate that we have to allocate a `Vec` for checking those calls. I guess they are infrequent enough that it doesn't matter.

We could consider introducing a `CallArgumentsRef` or similar which wraps an `&[Argument]` slice instead, similar to `IndexSlice` and `IndexVec` (may require some unsafe code to transmute between the two implementations). 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:48 on 2025-01-07 10:06_

Nit: The `unknown` prefix feel a bit redundant. I'd just go with `argument_name` and `argument_index`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:131 on 2025-01-07 10:08_

I think using a `Vec` here is fine considering that errors shouldn't be that common. We could even consider boxing `errors` to reduce the stack size of `CallBinding`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:179 on 2025-01-07 10:09_

Can we add some documentation to this type (or better, come up with a better name). The name is fairly meaningless to me (I know it has something to do with parameter)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:22 on 2025-01-07 10:12_

Should this be named `argument_tys` considering that we push the argument (and not parameter) types?

---

_@MichaReiser approved on 2025-01-07 10:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:76 on 2025-01-07 10:49_

On further reflection, I wonder if we should mention the index whenever the _argument_ is _passed_ positionally, even if it's a positional-or-keyword _parameter_. Otherwise if you have lots of arguments (`foo(1, 2, 3, 4, 5)`) and the type checker complains about "parameter `x`", you have to lookup the definition of `foo` to figure out what the type checker is complaining about.

This issue is obviously partly obviated if we display the range as part of the diagnostic, eg.

```
foo(1, 2, 3, 4, 5)
             ^
error: [invalid-argument-type]
```

but that doesn't help users who use `--output-format=concise`. (I assume/hope we'll implement that option for red-knot in a similar way to how we already have it for Ruff. The verbose error format is fantastic if there's only a few errors, but it makes the errors much more overwhelming on the command line if there are many errors, e.g. if you're running Ruff or red-knot on a codebase for the first time.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:180 on 2025-01-07 10:49_

```suggestion
# error: 13 [missing-argument] "No argument provided for required parameter `x` of function `f`"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:247 on 2025-01-07 10:50_

```suggestion
# error: 20 [unknown-argument] "Argument `y` does not match any known parameter of function `f`"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:257 on 2025-01-07 10:50_

```suggestion
# error: 18 [parameter-already-assigned] "Multiple values provided for parameter `x` of function `f`"
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-01-07 11:14_

Right... but maybe you don't need that method either ;) here are the changes I think I'd make to the API:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/call/bind.rs b/crates/red_knot_python_semantic/src/types/call/bind.rs
index baa47b362..eb8f2ed93 100644
--- a/crates/red_knot_python_semantic/src/types/call/bind.rs
+++ b/crates/red_knot_python_semantic/src/types/call/bind.rs
@@ -18,7 +18,8 @@ pub(crate) fn bind_call<'db>(
     signature: &Signature<'db>,
     callable_ty: Option<Type<'db>>,
 ) -> CallBinding<'db> {
-    let param_count = signature.parameter_count();
+    let parameters = signature.parameters();
+    let param_count = parameters.len();
     let mut parameter_tys = vec![None; param_count];
     let mut errors = vec![];
     let mut next_positional = 0;
@@ -26,10 +27,11 @@ pub(crate) fn bind_call<'db>(
     for (argument_index, argument) in arguments.iter().enumerate() {
         let (index, parameter, argument_ty) = match argument {
             Argument::Positional(ty) => {
-                let Some((index, parameter)) = signature
-                    .positional_at_index(next_positional)
+                let Some((index, parameter)) = parameters
+                    .positional_only()
+                    .nth(next_positional)
                     .map(|param| (next_positional, param))
-                    .or_else(|| signature.variadic_parameter())
+                    .or_else(|| parameters.variadic())
                 else {
                     first_excess_positional.get_or_insert(argument_index);
                     next_positional += 1;
@@ -39,9 +41,9 @@ pub(crate) fn bind_call<'db>(
                 (index, parameter, ty)
             }
             Argument::Keyword { name, ty } => {
-                let Some((index, parameter)) = signature
+                let Some((index, parameter)) = parameters
                     .keyword_by_name(name)
-                    .or_else(|| signature.keywords_parameter())
+                    .or_else(|| parameters.keyword_variadic())
                 else {
                     errors.push(CallBindingError::UnknownArgument {
                         unknown_name: name.clone(),
@@ -81,16 +83,14 @@ pub(crate) fn bind_call<'db>(
     if let Some(first_excess_argument_index) = first_excess_positional {
         errors.push(CallBindingError::TooManyPositionalArguments {
             first_excess_argument_index,
-            expected_positional_count: signature.positional_parameter_count(),
+            expected_positional_count: parameters.positional_only().count(),
             provided_positional_count: next_positional,
         });
     }
     let mut missing = vec![];
     for (index, bound_ty) in parameter_tys.iter().enumerate() {
         if bound_ty.is_none() {
-            let param = signature
-                .parameter_at_index(index)
-                .expect("parameter_tys array should not be larger than number of parameters");
+            let param = &parameters[index];
             if param.is_variadic() || param.is_keywords() || param.default_ty().is_some() {
                 // variadic/keywords and defaulted arguments are not required
                 continue;
diff --git a/crates/red_knot_python_semantic/src/types/signatures.rs b/crates/red_knot_python_semantic/src/types/signatures.rs
index 3f23b5a3f..e8e52b4e9 100644
--- a/crates/red_knot_python_semantic/src/types/signatures.rs
+++ b/crates/red_knot_python_semantic/src/types/signatures.rs
@@ -1,3 +1,5 @@
+use std::ops::Index;
+
 use super::{definition_expression_ty, Type};
 use crate::Db;
 use crate::{semantic_index::definition::Definition, types::todo_type};
@@ -55,70 +57,15 @@ impl<'db> Signature<'db> {
         }
     }
 
-    /// Return number of parameters in this signature.
-    pub(crate) fn parameter_count(&self) -> usize {
-        self.parameters.0.len()
-    }
-
-    /// Return number of positional parameters this signature can accept.
-    ///
-    /// Doesn't account for variadic parameter.
-    pub(crate) fn positional_parameter_count(&self) -> usize {
-        self.parameters
-            .into_iter()
-            .take_while(|param| param.is_positional())
-            .count()
-    }
-
-    pub(crate) fn parameter_at_index(&self, index: usize) -> Option<&Parameter<'db>> {
-        self.parameters.0.get(index)
-    }
-
-    /// Return positional parameter at given index, or `None` if `index` is out of range.
-    ///
-    /// Does not return variadic parameter.
-    pub(crate) fn positional_at_index(&self, index: usize) -> Option<&Parameter<'db>> {
-        if let Some(candidate) = self.parameter_at_index(index) {
-            if candidate.is_positional() {
-                return Some(candidate);
-            }
-        }
-        None
-    }
-
-    /// Return the variadic parameter (`*args`), if any, and its index, or `None`.
-    pub(crate) fn variadic_parameter(&self) -> Option<(usize, &Parameter<'db>)> {
-        self.parameters
-            .into_iter()
-            .enumerate()
-            .find(|(_, parameter)| parameter.is_variadic())
-    }
-
-    /// Return parameter (with index) for given name, or `None` if no such parameter.
-    ///
-    /// Does not return keywords (`**kwargs`) parameter.
-    pub(crate) fn keyword_by_name(
-        &self,
-        name: &ast::name::Name,
-    ) -> Option<(usize, &Parameter<'db>)> {
-        self.parameters
-            .into_iter()
-            .enumerate()
-            .find(|(_, parameter)| parameter.callable_by_name(name))
-    }
-
-    /// Return the keywords parameter (`**kwargs`), if any, and its index, or `None`.
-    pub(crate) fn keywords_parameter(&self) -> Option<(usize, &Parameter<'db>)> {
-        self.parameters
-            .into_iter()
-            .enumerate()
-            .find(|(_, parameter)| parameter.is_keywords())
+    /// Return the parameters in this signature.
+    pub(crate) fn parameters(&self) -> &Parameters<'db> {
+        &self.parameters
     }
 }
 
 // TODO: use SmallVec here once invariance bug is fixed
 #[derive(Clone, Debug, PartialEq, Eq)]
-pub(super) struct Parameters<'db>(Vec<Parameter<'db>>);
+pub(crate) struct Parameters<'db>(Vec<Parameter<'db>>);
 
 impl<'db> Parameters<'db> {
     /// Return todo parameters: (*args: Todo, **kwargs: Todo)
@@ -172,6 +119,44 @@ impl<'db> Parameters<'db> {
                 .collect(),
         )
     }
+
+    pub(crate) fn len(&self) -> usize {
+        self.0.len()
+    }
+
+    pub(crate) fn iter(&self) -> impl Iterator<Item = &Parameter<'db>> {
+        self.0.iter()
+    }
+
+    pub(crate) fn positional_only(&self) -> impl Iterator<Item = &Parameter<'db>> {
+        self.iter().filter(|param| param.is_positional())
+    }
+
+    /// Return the variadic parameter (`*args`), if any, and its index, or `None`.
+    pub(crate) fn variadic(&self) -> Option<(usize, &Parameter<'db>)> {
+        self.iter()
+            .enumerate()
+            .find(|(_, parameter)| parameter.is_variadic())
+    }
+
+    /// Return parameter (with index) for given name, or `None` if no such parameter.
+    ///
+    /// Does not return keywords (`**kwargs`) parameter.
+    pub(crate) fn keyword_by_name(
+        &self,
+        name: &ast::name::Name,
+    ) -> Option<(usize, &Parameter<'db>)> {
+        self.iter()
+            .enumerate()
+            .find(|(_, parameter)| parameter.callable_by_name(name))
+    }
+
+    /// Return the keywords parameter (`**kwargs`), if any, and its index, or `None`.
+    pub(crate) fn keyword_variadic(&self) -> Option<(usize, &Parameter<'db>)> {
+        self.iter()
+            .enumerate()
+            .find(|(_, parameter)| parameter.is_keywords())
+    }
 }
 
 impl<'db, 'a> IntoIterator for &'a Parameters<'db> {
@@ -183,6 +168,14 @@ impl<'db, 'a> IntoIterator for &'a Parameters<'db> {
     }
 }
 
+impl<'db> Index<usize> for Parameters<'db> {
+    type Output = Parameter<'db>;
+
+    fn index(&self, index: usize) -> &Self::Output {
+        &self.0[index]
+    }
+}
+
 #[derive(Clone, Debug, PartialEq, Eq)]
 pub(crate) enum Parameter<'db> {
     /// Positional-only parameter, e.g. `def f(x, /): ...`
```

---

_@AlexWaygood reviewed on 2025-01-07 11:14_

(Still not a full review)

---

_@AlexWaygood reviewed on 2025-01-07 16:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:86 on 2025-01-07 16:32_

I think we should use 1-based indexing for the parameter number -- computers think of numbers as starting at 0, but that's not how humans think, and the error message is for humans

```suggestion
# error: 15 [invalid-argument-type] "Object of type `Literal["foo"]` cannot be assigned to parameter 1 (`x`) of function `f`; expected type `int`"
```

This struct can help:

https://github.com/astral-sh/ruff/blob/c75fb4a504513313fafa3fe6391a0cac1fec9a80/crates/ruff_source_file/src/line_index.rs#L338-L345

---

_@carljm reviewed on 2025-01-07 17:17_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:107 on 2025-01-07 17:17_

That's an invalid signature, so I think the bar for what we do there is pretty low: I'd be happy with any semi-reasonable behavior that doesn't panic. Always assigning a keyword argument of that name to the first parameter with the name seems fine.

---

_@carljm reviewed on 2025-01-07 17:22_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:14 on 2025-01-07 17:22_

This comment isn't meant to imply that we can rely on the signature being valid, and I don't think we currently do. I can adjust the comment to clarify that.

---

_@carljm reviewed on 2025-01-07 17:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:179 on 2025-01-07 17:25_

Any better name suggestions? I had trouble coming up with a good name. It's "the information we need to report an error regarding a particular parameter."

I can add a doc comment.

---

_@carljm reviewed on 2025-01-07 17:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:22 on 2025-01-07 17:26_

I don't think so, because the most important fact about it is that each index in this vector corresponds to a parameter, in the order the parameters exist in the signature. There may be fewer arguments, in a different order (if keyword arguments are used). So these are the types assigned to each parameter, at this call site.

---

_@carljm reviewed on 2025-01-07 17:32_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:131 on 2025-01-07 17:32_

Ok, I can remove the TODO comment.

Not going to add the extra layer of indirection in this PR, I think that should be motivated separately by profiling showing it's a useful tradeoff.

(If overall memory usage of `CallBinding` is an issue, we may want to consider limiting the assigned parameter types that we store, or only storing parameter types at all when the callable is a known-function. We only actually need the assigned parameter types in the binding when we are special-casing a known callable.)

---

_@carljm reviewed on 2025-01-07 17:34_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1534 on 2025-01-07 17:34_

Makes sense; I'll leave this as a potential future optimization if we see this as a significant cost in profiling.

---

_@carljm reviewed on 2025-01-07 17:50_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-01-07 17:50_

I like just returning a reference to `Parameters` and exposing API there, rather than passing everything through methods on `Signature`; I thought about that when writing it but didn't get around to trying it.

Still not sure about `positional_only` and how different it makes it from the caller side to `get_positional` vs just `get`. Also I think the semantics here are not right; it needs to be "return the nth parameter, if it can be assigned positionally" not "return the nth positional parameter". These should be the same thing for a valid signature, but we can't assume a valid signature.

I'll play with this.

---

_@AlexWaygood reviewed on 2025-01-07 17:52_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-01-07 17:52_

> These should be the same thing for a valid signature, but we can't assume a valid signature.

Ah, I thought we had said that for invalid syntax we'd just assume something similar to what your `Parameters::todo()` method returns? Don't have a strong opinion on this though -- it was just what I was basing some of my suggestions on

---

_@carljm reviewed on 2025-01-07 18:20_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:75 on 2025-01-07 18:20_

Following Alex's comment, I'm moving this to `signature.parameters().get()` instead of `signature.get_parameter()`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:107 on 2025-01-07 18:21_

Clarified this behavior in the doc comment.

---

_@carljm reviewed on 2025-01-07 18:21_

---

_@carljm reviewed on 2025-01-07 18:25_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-01-07 18:25_

20d0851214a026f6c61ec23b22d74c9205d9233f

---

_@carljm reviewed on 2025-01-07 18:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:91 on 2025-01-07 18:26_

Thanks for the comments, this is definitely an improvement!

---

_@carljm reviewed on 2025-01-07 18:26_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:59 on 2025-01-07 18:26_

Fixed as part of 20d0851214a026f6c61ec23b22d74c9205d9233f

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:143 on 2025-01-07 18:29_

to me this seems simple enough that I would just inline it at callsites :P but no strong opinion

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:159 on 2025-01-07 18:31_

```suggestion
        self.get(index)
            .and_then(|candidate| candidate.is_positional().then_some(candidate))
```

---

_@AlexWaygood reviewed on 2025-01-07 18:31_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:303 on 2025-01-07 18:41_

Not sure if it will ever be useful/needed, but it does seem better to preserve the more structured representation. 53b35ec0afee001b9ea6cf7371ac9197505da65e

---

_@carljm reviewed on 2025-01-07 18:41_

---

_@carljm reviewed on 2025-01-07 18:56_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:76 on 2025-01-07 18:56_

Makes sense. 17adaa581077e0aa146200483d175282628425c2

One edge case I noted is that if the argument was passed positionally, but matched to the variadic parameter, I don't think we should describe it using the parameter index, because that won't necessarily be the same as the argument index. E.g. `def f(*args: int): ...` and then `f(10, "foo")` the error should just say "cannot be assigned to parameter `*args`" not "cannot be assigned to parameter 1 (`*args`)", which is confusing since the error is about argument 2, not argument 1.

---

_@carljm reviewed on 2025-01-07 19:00_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:143 on 2025-01-07 19:00_

Ok, sure. I also noticed looking at this that the name `positional_only` for the iterator-returning method is bad, due to confusion with positional-only parameters. This method iterates only positional parameters, which is not the same thing as returning only positional-only parameters 😆 Renamed it to just `positional`

---

_@carljm reviewed on 2025-01-07 19:02_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:159 on 2025-01-07 19:02_

Not totally convinced this is more readable, but it is shorter, so sure :)

---

_@AlexWaygood reviewed on 2025-01-07 19:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/signatures.rs`:159 on 2025-01-07 19:03_

hehe, I'm not wedded to it! You can ignore this if you don't like it :-)

---

_@carljm reviewed on 2025-01-07 19:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:86 on 2025-01-07 19:07_

Done. I don't think there's any need for `OneIndexed` here, though -- I think it's less confusing if we continue to use exclusively zero-based indices in the code, and just `+ 1` at the display layer. At least for now; maybe this will change as the diagnostic system gets more sophisticated.

---

_@carljm reviewed on 2025-01-07 19:08_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/call/function.md`:86 on 2025-01-07 19:08_

b2e3bf1ae58ea9034d112a3cfd06fc85e56a207f

---

_@carljm reviewed on 2025-01-07 19:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:22 on 2025-01-07 19:21_

Added a comment to clarify this.

---

_@carljm reviewed on 2025-01-07 19:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:179 on 2025-01-07 19:21_

Doc comment added.

---

_@carljm reviewed on 2025-01-07 20:06_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:192 on 2025-01-07 20:06_

I went with the `ParameterKind` approach, I think it works out pretty well: e338e9900a2adbc876fef7f00ee4c9b3f9eb803f

---

_@carljm reviewed on 2025-01-07 20:14_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:107 on 2025-01-07 20:14_

Also added a test for this case.

---

_@carljm reviewed on 2025-01-07 20:29_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:14 on 2025-01-07 20:29_

Also added some tests to show we handle some invalid-syntax cases without panic.

---

_Comment by @carljm on 2025-01-07 20:30_

Thanks for all the great review comments! I think I've addressed them all, planning to land if CI is green.

---

_Merged by @carljm on 2025-01-07 20:39_

---

_Closed by @carljm on 2025-01-07 20:39_

---

_Branch deleted on 2025-01-07 20:39_

---
