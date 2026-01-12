```yaml
number: 13449
title: "[red-knot] Add `Type::bool` and boolean expression inference"
type: pull_request
state: merged
author: TomerBin
labels:
  - ty
assignees: []
merged: true
base: main
head: tomer/boolean-expression
created_at: 2024-09-22T23:41:47Z
updated_at: 2024-09-25T00:11:47Z
url: https://github.com/astral-sh/ruff/pull/13449
synced_at: 2026-01-12T15:55:44Z
```

# [red-knot] Add `Type::bool` and boolean expression inference

---

_@TomerBin_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Add boolean operator (`and` and `or`) support for several types. I'm pretty new to Rust so any feedback would be great. 

A new `Type::bool` function is created which may also help in https://github.com/astral-sh/ruff/pull/13432 as well.
Contributes to [#12701](https://github.com/astral-sh/ty/issues/244)

## Test Plan
Cargo tests
<!-- How was it tested? -->


---

_Review requested from @carljm by @TomerBin on 2024-09-22 23:41_

---

_Review requested from @MichaReiser by @TomerBin on 2024-09-22 23:41_

---

_Review requested from @AlexWaygood by @TomerBin on 2024-09-22 23:41_

---

_Comment by @github-actions[bot] on 2024-09-22 23:55_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:535 on 2024-09-23 00:05_

Not necessarily! Most classes are always truthy, but not if the class has a custom metaclass that defines `__bool__` or `__len__`:

```pycon
>>> class Meta(type):
...     def __bool__(cls): return False
... 
>>> class Foo(metaclass=Meta): pass
... 
>>> bool(Foo)
False
```

The exact details around the method lookup will be somewhat fiddly (for both instances and classes -- you need to check both `__bool__` and `__len__`), so I think it's fine to just return `None` for both this variant and `Type::Instance` for now. But we should add a TODO so that we remember to add some logic doing the method lookup for these variants at some point. (If a type has a `__bool__` method that is annotated as returning `Literal[False]`, or a `__len__` method annotated as returning `Literal[0]`, we should treat it as always falsey -- and similarly for `__bool__` and `__len__` methods that are annotated as returning `Literal[True]` / `Literal[<some nonzero number>]`.)

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:551 on 2024-09-23 00:07_

I think you should be able to iterate over the union elements directly here without collecting, which should be cheaper:

```suggestion
                let mut found_true = false;
                let mut found_false = false;
                for element in union.elements(db) {
                    match element.bool(db) {
                        Some(true) => found_true = true,
                        Some(false) => found_false = true,
                        None => return None,
                    }
                }
```

---

_@AlexWaygood reviewed on 2024-09-23 00:11_

Nice, thank you! Not a complete review (I'm on holiday at the moment), just a couple of things I spotted:

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:531 on 2024-09-23 08:29_

Nit
```suggestion
            Type::Any | Type::Never | Type::Unbound => None,
```

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:526 on 2024-09-23 08:44_

I don't know the answer to this. @AlexWaygood and @carljm will have a better intuition for this but I wonder if this method should return a `Type` instead and leave it to the caller to test if the type is equal to `true` or `false` (it may not be relevant for all callers). 

IMO, doing so would fit nicer into the `Type` API where most methods are about type transformations. We could then add helper methods for `type.is_true()` and `type.is_false()` to test if the value is true. 

Either way, don't change anything before either Alex or Carl reply ;)

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types.rs`:560 on 2024-09-23 08:47_

To avoid iterating till the end if both true and false are found.
```suggestion
							let mut result: Option<bool> = None;

                for value in union.elements(db) {
                    if let Some(value) = value.bool(db) {
                        if result.is_some_and(|v| value != value) {
                            return None;
                        }
                        result = Some(value);
                    } else {
                        return None;
                    }
                }

                result
```

---

_@MichaReiser reviewed on 2024-09-23 08:47_

Nice!

---

_Label `red-knot` added by @MichaReiser on 2024-09-23 08:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6095 on 2024-09-23 18:28_

Nit: it's not clear why these assertions all use `r#` strings (seemingly unnecessarily) when the first assertion above doesn't. Maybe make them all consistent? (In the below tests as well.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2335 on 2024-09-23 19:43_

This line needs to be handled the same as the next line; push to `value_types` and `break`. There might have been some previous values with statically unknown bool type, and we still have to account for the possibility that evaluation short-circuited there.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6086 on 2024-09-23 19:46_

We should add a test `h = foo() or True` here, with resulting type for `h` as `str | Literal[True]`. Without this test, we can replace "push to `value_types` and break" above with `return *value_type;` (which is wrong) and tests will still pass.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:6122 on 2024-09-23 20:34_

This assertion is wrong. `foo()` could return the empty string, in which case the type of the expression would be `str`, not `Literal[False]`. This is the bug mentioned above, where we need to push to `value_types` and break instead of just returning a single value.
```suggestion
        assert_public_ty(&db, "/src/a.py", "c", r#"str | Literal[False]"#);
```

---

_@AlexWaygood reviewed on 2024-09-23 21:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:526 on 2024-09-23 21:18_

I was wondering about whether this should return a custom `Truthiness` enum, e.g.

```rs
#[derive(Debug, Copy, Clone, PartialEq, Eq)]
enum Truthiness {
    AlwaysTrue,
    AlwaysFalse,
    Ambiguous,
}

impl Truthiness {
    fn negate(self) -> Self {
        match self {
            Self::AlwaysTrue => Self::AlwaysFalse,
            Self::AlwaysFalse => Self::AlwaysTrue,
            Self::Ambiguous => Self::Ambiguous
        }
    }

    fn into_type(self, db: &dyn Db) -> Type {
        match self {
            Self::AlwaysTrue => Type::BooleanLiteral(true),
            Self::AlwaysFalse => Type::BooleanLiteral(false),
            Self::Ambiguous => builtins_symbol_ty(db, "bool").to_instance(db),
        }
    }
}

impl From<bool> for Truthiness {
    fn from(value: bool) -> Self {
        if value {
            Truthiness::AlwaysTrue
        } else {
            Truthiness::AlwaysFalse
        }
    }
}
```

This has a few advantages over returning `Option<bool>`:
- It's more expressive of the fact that there are really three distinct states being expressed by the return type (and expresses what these states are)
- The `negate` method will make it easier to implement support for the `not` operator in a generalised way in the future
- Returning a custom enum will make it more adaptable in the future for when we implement support in this method for arbitrary `__bool__` and `__len__` methods. Once we add that support, we may need to add some way of bubbling up errors from `Type::call()` in this method.

---

_@AlexWaygood reviewed on 2024-09-23 21:20_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:526 on 2024-09-23 21:20_

Bikesheddy comment, but my preferred name for the method would also be `boolean_eval`, since we're evaluating the booleanness of the type :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:526 on 2024-09-23 22:06_

I think the custom enum makes sense here, because it expresses the semantics clearly and allows the caller to decide whether the actual BooleanLiteral or Instance(bool) type needs to be reified or not. It fits in well with our existing pattern with `IterationOutcome` and `CallOutcome`, where these methods can return an enum that lazily allows the caller to make decisions about how to handle the results.

I'm not such a big fan of `boolean_eval` as the name, mostly because like 90% of what we do in the type checker could be described using the word "eval" if we chose to; it's a very generic term. I think either just `bool` or `boolean` is fine.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2328 on 2024-09-23 22:12_

Given that `UnionType::from_elements` takes an iterator, I think we can avoid all allocations here. It's slightly tricky because we do need to `infer_expression` on every value, but we also would like to short-circuit truthiness eval on some of them. But I think it is worth doing. Here's what I came up, which passes all tests (including the ones I suggested adding below):

<details>

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 4decd441c..9d84f40cf 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -2321,30 +2321,38 @@ impl<'db> TypeInferenceBuilder<'db> {
             op,
             values,
         } = bool_op;
-        let mut value_tys = Vec::new();
-        let inferred_types: Vec<Type<'db>> = values
-            .iter()
-            .map(|value| self.infer_expression(value))
-            .collect();
-        for (i, value_type) in inferred_types.iter().enumerate() {
-            let boolean_value = value_type.bool(self.db);
-            if let Some(boolean_value) = boolean_value {
-                let is_last = i == values.len() - 1;
-                match (boolean_value, is_last, op) {
-                    (true, false, ast::BoolOp::And) => continue,
-                    (false, _, ast::BoolOp::And) => return *value_type,
-                    (true, _, ast::BoolOp::Or) => {
-                        value_tys.push(value_type);
-                        break;
+        let mut done = false;
+        UnionType::from_elements(
+            self.db,
+            values.iter().enumerate().map(|(i, value)| {
+                // We need to infer the type of every expression (that's an invariant maintained by
+                // type inference), even if we can short-circuit boolean evaluation of some of
+                // those types.
+                let value_ty = self.infer_expression(value);
+                if done {
+                    Type::Never
+                } else {
+                    if let Some(boolean_value) = value_ty.bool(self.db) {
+                        let is_last = i == values.len() - 1;
+                        match (boolean_value, is_last, op) {
+                            (true, false, ast::BoolOp::And) => Type::Never,
+                            (false, _, ast::BoolOp::And) => {
+                                done = true;
+                                value_ty
+                            }
+                            (true, _, ast::BoolOp::Or) => {
+                                done = true;
+                                value_ty
+                            }
+                            (false, false, ast::BoolOp::Or) => Type::Never,
+                            (_, true, _) => value_ty,
+                        }
+                    } else {
+                        value_ty
                     }
-                    (false, false, ast::BoolOp::Or) => continue,
-                    (_, true, _) => value_tys.push(value_type),
                 }
-            } else {
-                value_tys.push(value_type);
-            }
-        }
-        UnionType::from_elements(self.db, value_tys)
+            }),
+        )
     }
```

</details>

---

_@carljm requested changes on 2024-09-23 22:13_

Thank you, this is awesome! I think there's one bug, a couple tests that would be good to add, and I think we can also avoid allocating vectors in `infer_boolean_expression`.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:526 on 2024-09-23 22:15_

I'm okay with leaving it as `bool`

---

_@AlexWaygood reviewed on 2024-09-23 22:15_

---

_Comment by @TomerBin on 2024-09-24 11:56_

Thanks for these great comments, I'll address them soon.

Something else I was thinking about is to use `Type::bool()` when inferring the return value of the `bool` function (ie. understanding x is True in `x = bool(1)`)
It would also be beneficial for testing the bool utility.

What do you think about this? Should we do that? Maybe in another PR? 

---

_Comment by @carljm on 2024-09-24 14:00_

> Something else I was thinking about is to use Type::bool() when inferring the return value of the bool function (ie. understanding x is True in x = bool(1))
> It would also be beneficial for testing the bool utility.
> 
> What do you think about this? Should we do that? Maybe in another PR?

Yes, that's something we will want. Definitely recommend separate PR for this, as there will be some decisions to make there about how to detect that we are calling the builtin bool type. 

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:551 on 2024-09-24 22:02_

```suggestion
                            result = Some(truthiness);
```

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:569 on 2024-09-24 22:02_

```suggestion
            Type::IntLiteral(num) => Truthiness::from(*num != 0),
            Type::BooleanLiteral(bool) => Truthiness::from(*bool),
            Type::StringLiteral(str) => Truthiness::from(!str.value(db).is_empty()),
            Type::LiteralString => Truthiness::Ambiguous,
            Type::BytesLiteral(bytes) => Truthiness::from(!bytes.value(db).is_empty()),
            Type::Tuple(items) => Truthiness::from(!items.elements(db).is_empty()),
```

---

_@AlexWaygood reviewed on 2024-09-24 22:04_

nit: I prefer to use `::from` methods rather than `.into()` methods where possible, as it's much clearer when reading things outside of an IDE what exactly you're converting the type _into_

---

_@TomerBin reviewed on 2024-09-24 22:21_

---

_Review comment by @TomerBin on `crates/red_knot_python_semantic/src/types.rs`:569 on 2024-09-24 22:21_

Cool! fixed :)

---

_@carljm approved on 2024-09-24 22:39_

This looks great to me! Thank you!!

---

_Comment by @TomerBin on 2024-09-24 22:46_

Thanks again for your well explained and welcoming comments ☺️

---

_@AlexWaygood approved on 2024-09-24 23:58_

Thanks! :-D

---

_Renamed from "red-knot: Add boolean operator" to "[red-knot] Add `Type::bool` and boolean expression inference" by @AlexWaygood on 2024-09-24 23:59_

---

_Merged by @AlexWaygood on 2024-09-25 00:02_

---

_Closed by @AlexWaygood on 2024-09-25 00:02_

---
