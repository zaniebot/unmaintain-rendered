```yaml
number: 20598
title: "[ty] defer inference of legacy TypeVar bound/constraints/defaults"
type: pull_request
state: merged
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: cjm/legacytv
created_at: 2025-09-27T01:53:57Z
updated_at: 2025-10-09T23:19:43Z
url: https://github.com/astral-sh/ruff/pull/20598
synced_at: 2026-01-10T17:34:34Z
```

# [ty] defer inference of legacy TypeVar bound/constraints/defaults

---

_Pull request opened by @carljm on 2025-09-27 01:53_

## Summary

This allows us to handle self-referential bounds/constraints/defaults without panicking.

Handles more cases from https://github.com/astral-sh/ty/issues/256

This also changes the way we infer the types of legacy TypeVars. Rather than understanding a constructor call to `typing[_extension].TypeVar` inside of any (arbitrarily nested) expression, and having to use a special `assigned_to` field of the semantic index to try to best-effort figure out what name the typevar was assigned to, we instead understand the creation of a legacy `TypeVar` only in the supported syntactic position (RHS of a simple un-annotated assignment with one target). In any other position, we just infer it as creating an opaque instance of `typing.TypeVar`. (This behavior matches all other type checkers.)

So we now special-case TypeVar creation in `TypeInferenceBuilder`, as a special case of an assignment definition, rather than deeper inside call binding. This does mean we re-implement slightly more of argument-parsing, but in practice this is minimal and easy to handle correctly.

This is easier to implement if we also make the RHS of a simple (no unpacking) one-target assignment statement no longer a standalone expression. Which is fine to do, because simple one-target assignments don't need to infer the RHS more than once. This is a bonus performance (0-3% across various projects) and significant memory-usage win, since most assignment statements are simple one-target assignment statements, meaning we now create many fewer standalone-expression salsa ingredients.

This change does mean that inference of manually-constructed `TypeAliasType` instances can no longer find its Definition in `assigned_to`, which regresses go-to-definition for these aliases. In a future PR, `TypeAliasType` will receive the same treatment that `TypeVar` did in this PR (moving its special-case inference into `TypeInferenceBuilder` and supporting it only in the correct syntactic position, and lazily inferring its value type to support recursion), which will also fix the go-to-definition regression. (I decided a temporary edge-case regression is better in this case than doubling the size of this PR.)

This PR also tightens up and fixes various aspects of the validation of `TypeVar` creation, as seen in the tests.

We still (for now) treat all typevars as instances of `typing.TypeVar`, even if they were created using `typing_extensions.TypeVar`. This means we'll wrongly error on e.g. `T.__default__` on Python 3.11, even if `T` is a `typing_extensions.TypeVar` instance at runtime. We share this wrong behavior with both mypy and pyrefly. It will be easier to fix after we pull in https://github.com/python/typeshed/pull/14840.

There are some issues that showed up here with typevar identity and `MarkTypeVarsInferable`; the fix here (using the new `original` field and `is_identical_to` methods on `BoundTypeVarInstance` and `TypeVarInstance`) is a bit kludgy, but it can go away when we eliminate `MarkTypeVarsInferable`.

## Test Plan

Added and updated mdtests.

### Conformance suite impact

The impact here is all positive:

* We now correctly error on a legacy TypeVar with exactly one constraint type given.
* We now correctly error on a legacy TypeVar with both an upper bound and constraints specified.

### Ecosystem impact

Basically none; in the setuptools case we just issue slightly different errors on an invalid TypeVar definition, due to the modified validation code.


---

_Label `ty` added by @carljm on 2025-09-27 01:53_

---

_Comment by @github-actions[bot] on 2025-09-27 01:55_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-10-09 21:06:16.051150791 +0000
+++ new-output.txt	2025-10-09 21:06:19.328159431 +0000
@@ -1,6 +1,6 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
 fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_type_statement.py`: `PEP695TypeAliasType < 'db >::value_type_(Id(cc17)): execute: too many cycle iterations`
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1643f)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/29ab321/src/function/execute.rs:217:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1603f)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -364,6 +364,7 @@
 generics_base_class.py:49:22: error[too-many-positional-arguments] Too many positional arguments to class `LinkedList`: expected 1, got 2
 generics_base_class.py:61:18: error[too-many-positional-arguments] Too many positional arguments to class `MyDict`: expected 1, got 2
 generics_basic.py:34:12: error[unsupported-operator] Operator `+` is unsupported between objects of type `AnyStr@concat` and `AnyStr@concat`
+generics_basic.py:49:44: error[invalid-legacy-type-variable] A `TypeVar` cannot have exactly one constraint
 generics_basic.py:139:5: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_basic.py:140:5: error[type-assertion-failure] Argument does not have asserted type `int`
 generics_basic.py:157:5: error[invalid-argument-type] Method `__getitem__` of type `bound method MyMap1[str, int].__getitem__(key: str, /) -> int` cannot be called with key of type `Literal[0]` on object of type `MyMap1[str, int]`
@@ -575,7 +576,8 @@
 generics_upper_bound.py:43:1: error[type-assertion-failure] Argument does not have asserted type `list[int] | set[int]`
 generics_upper_bound.py:51:8: error[invalid-argument-type] Argument to function `longer` is incorrect: Argument type `Literal[3]` does not satisfy upper bound `Sized` of type variable `ST`
 generics_upper_bound.py:51:11: error[invalid-argument-type] Argument to function `longer` is incorrect: Argument type `Literal[3]` does not satisfy upper bound `Sized` of type variable `ST`
-generics_variance.py:14:6: error[invalid-legacy-type-variable] A legacy `typing.TypeVar` cannot be both covariant and contravariant
+generics_upper_bound.py:56:10: error[invalid-legacy-type-variable] A `TypeVar` cannot have both a bound and constraints
+generics_variance.py:14:6: error[invalid-legacy-type-variable] A `TypeVar` cannot be both covariant and contravariant
 generics_variance.py:26:27: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Iterator[T_co@ImmutableList]`
 generics_variance.py:57:28: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `B_co@func`
 generics_variance.py:175:25: error[non-subscriptable] Cannot subscript object of type `<class 'Contra[typing.TypeVar]'>` with no `__class_getitem__` method
@@ -898,5 +900,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key access on TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 899 diagnostics
+Found 901 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.
```
</details>


---

_Comment by @github-actions[bot] on 2025-09-27 01:56_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/typing_extensions.py:313:12: error[invalid-legacy-type-variable] A legacy `typing.TypeVar` must be immediately assigned to a variable
+ lib/spack/spack/vendor/typing_extensions.py:313:12: error[invalid-legacy-type-variable] A `TypeVar` definition must be a simple variable assignment

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/typing_extensions.py:256:12: error[invalid-legacy-type-variable] A legacy `typing.TypeVar` must be immediately assigned to a variable
+ setuptools/_vendor/typing_extensions.py:256:12: error[invalid-legacy-type-variable] A `TypeVar` definition must be a simple variable assignment
- setuptools/_vendor/typing_extensions.py:1517:27: error[invalid-legacy-type-variable] The `contravariant` parameter of a legacy `typing.TypeVar` cannot have an ambiguous value
- setuptools/_vendor/typing_extensions.py:1517:68: error[invalid-type-form] Variable of type `None` is not allowed in a type expression
+ setuptools/_vendor/typing_extensions.py:1517:48: error[invalid-legacy-type-variable] Starred arguments are not supported in `TypeVar` creation
- Found 800 diagnostics
+ Found 799 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_wheelfile.py:51:22: error[no-matching-overload] No overload of function `field` matches arguments
- Found 52 diagnostics
+ Found 51 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
-     memo fields = ~108MB
+     memo fields = ~103MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~42MB
+     memo metadata = ~40MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~541MB
+ TOTAL MEMORY USAGE: ~515MB
-     struct metadata = ~30MB
+     struct metadata = ~28MB
-     struct fields = ~35MB
+     struct fields = ~33MB
-     memo fields = ~384MB
+     memo fields = ~366MB

```
</details>


---

_Label `ecosystem-analyzer` added by @carljm on 2025-09-27 02:22_

---

_Comment by @github-actions[bot] on 2025-09-27 02:28_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-legacy-type-variable` | 1 | 1 | 0 |
| `invalid-type-form` | 0 | 1 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| **Total** | **2** | **2** | **0** |

**[Full report with detailed diff](https://cjm-legacytv.ecosystem-663.pages.dev/diff)** ([timing results](https://cjm-legacytv.ecosystem-663.pages.dev/timing))


---

_Comment by @codspeed-hq[bot] on 2025-09-27 23:30_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Flegacytv)

### Merging #20598 will **improve performances by 5.56%**

<sub>Comparing <code>cjm/legacytv</code> (75eda5e) with <code>main</code> (b086ffe)</sub>



### Summary

`⚡ 1` improvement  
`✅ 20` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` small[freqtrade] ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Flegacytv?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bfreqtrade%5D&runnerMode=WallTime) | 5.2 s | 4.9 s | +5.56% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Flegacytv?sectionId=benchmark-comparison-section-baseline-result-skipped).


---

_Label `ecosystem-analyzer` removed by @carljm on 2025-10-06 20:54_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-10-06 20:54_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-10-08 00:15_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-10-08 00:15_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-10-08 00:26_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-10-08 00:26_

---

_Label `ecosystem-analyzer` removed by @carljm on 2025-10-08 23:54_

---

_Label `ecosystem-analyzer` added by @carljm on 2025-10-08 23:55_

---

_Marked ready for review by @carljm on 2025-10-09 00:02_

---

_Review requested from @AlexWaygood by @carljm on 2025-10-09 00:02_

---

_Review requested from @sharkdp by @carljm on 2025-10-09 00:02_

---

_Review requested from @dcreager by @carljm on 2025-10-09 00:02_

---

_Review requested from @MichaReiser by @carljm on 2025-10-09 00:02_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/goto_type_definition.rs`:311 on 2025-10-09 06:50_

Can we create an issue for this. It's otherwise hard to remember

---

_@MichaReiser reviewed on 2025-10-09 06:54_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:3 on 2025-10-09 12:45_

```suggestion
We use TypeVar defaults here, which were added in Python 3.13 for legacy typevars.
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:3 on 2025-10-09 12:47_

(Though as we've discussed, we should really permit users to pass `default=whatever` even on lower Python versions if the user is making use of `typing_extensions.TypeVar`. Doesn't need to be fixed in this PR, but the prose could possibly be adjusted here to reflect that?)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:42 on 2025-10-09 12:51_

these assertions still pass (and still seem useful) -- what's the motivation for getting rid of them?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:74 on 2025-10-09 12:52_

similarly here, what's the motivation for getting rid of the error message from the assertion?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:105 on 2025-10-09 12:54_

are these mandated by the spec? They almost feel like lints to me, given that (I assume) we'll infer the type just fine for each value passed into each parameter

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:158 on 2025-10-09 12:54_

I think we probably only need one copy of this assertion? 😄

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:265 on 2025-10-09 12:57_

I still think it would be fine to treat stubs the same way as runtime files here -- I'd happily make the necessary changes to typeshed... but I guess other stub file authors may be relying on the behaviour of mypy and pyright here, and might get annoyed with us if we complained about this. Blegh.

Maybe it's worth saying explicitly in the prose that this isn't specified very well, but that we emulate other type checkers' behaviour here for maximum compatibility?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:404 on 2025-10-09 13:02_

hrm... on line 351 you said that they _cannot_ be cyclic, but here you say that they _can_ "cyclically" refer back to their own type?

Though FWIW I think it's debatable whether this example really counts as a cycle. `G` as it appears in the bound of `list[G]` here is syntactic sugar for the default specialization of `G`, which is `G[Unknown]` -- so this type variable's upper bound is `list[G[Unknown]]`, which is not a type that refers back to `T` in any way since `T` has been fully "specialized away" from the type.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:388 on 2025-10-09 13:03_

```suggestion
Defaults can be generic, but can only refer to typevars already defined in the same scope:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:438 on 2025-10-09 13:05_

```suggestion
# TODO: should never leak a typevar like this in type inference
reveal_type(D().x)  # revealed: V@D
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:462 on 2025-10-09 13:08_

did this break in an earlier version of this PR? It seems to work fine on `main`: https://play.ty.dev/d6e5578c-31fa-4e0e-b54d-64c920e7a478

---

_@AlexWaygood reviewed on 2025-10-09 13:10_

Only reviewed the tests so far

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1632 on 2025-10-09 14:50_

```suggestion
                // Optimization for the common case: if there's just one target, and it's not an
                // unpacking, and the target is a simple name, we don't need the RHS to be a
                // standalone expression at all.
                if let [target @ ast::Expr::Name(_)] = &node.targets[..]
```

also, this seems like quite a big win from the Codspeed report -- I think it could also be safely applied for lots of non-name expressions? `x.foo = 42` and `x[0] = 42` can also definitely be excluded from being possible unpackable assignments.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7957 on 2025-10-09 14:53_

```suggestion
        let ty = match definition.kind(db) {
            // PEP 695 typevar
            DefinitionKind::TypeVar(typevar) => {
                let typevar_node = typevar.node(&module);
                definition_expression_type(
                    db,
                    definition,
                    typevar_node.bound.as_ref()?,
                )
            }
            // legacy typevar
            DefinitionKind::Assignment(assignment) => {
                let call_expr = assignment.value(&module).as_call_expr()?;
                let expr = &call_expr.arguments.find_keyword("bound")?.value;
                definition_expression_type(db, definition, expr)
            }
            _ => return None,
        };
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7996 on 2025-10-09 14:55_

```suggestion
        let ty = match definition.kind(db) {
            // PEP 695 typevar
            DefinitionKind::TypeVar(typevar) => {
                let typevar_node = typevar.node(&module);
                definition_expression_type(db, definition, typevar_node.bound.as_ref()?)
                    .into_union()?
            }
            // legacy typevar
            DefinitionKind::Assignment(assignment) => {
                let call_expr = assignment.value(&module).as_call_expr()?;
                // We don't use `UnionType::from_elements` or `UnionBuilder` here,
                // because we don't want to simplify the list of constraints as we would with
                // an actual union type.
                // TODO: We probably shouldn't use `UnionType` to store these at all? TypeVar
                // constraints are not a union.
                UnionType::new(
                    db,
                    call_expr
                        .arguments
                        .args
                        .iter()
                        .skip(1)
                        .map(|arg| definition_expression_type(db, definition, arg))
                        .collect::<Box<_>>(),
                )
            }
            _ => return  None,
        };
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4029 on 2025-10-09 15:17_

a little less nested indentation:

```suggestion
                let value_ty = if let Some(standalone_expression) = self.index.try_expression(value)
                {
                    self.infer_standalone_expression_impl(value, standalone_expression, tcx)
                } else if let ast::Expr::Call(call_expr) = value {
                    // If the RHS is not a standalone expression, this is a simple assignment
                    // (single target, no unpackings). That means it's a valid syntactic form
                    // for a legacy TypeVar creation; check for that.
                    let callable_type = self.infer_maybe_standalone_expression(
                        call_expr.func.as_ref(),
                        TypeContext::default(),
                    );

                    let typevar_class = callable_type
                        .into_class_literal()
                        .and_then(|cls| cls.known(self.db()))
                        .filter(|cls| {
                            matches!(cls, KnownClass::TypeVar | KnownClass::ExtensionsTypeVar)
                        });

                    let ty = if let Some(typevar_class) = typevar_class {
                        self.infer_legacy_typevar(target, call_expr, definition, typevar_class)
                    } else {
                        self.infer_call_expression_impl(call_expr, callable_type, tcx)
                    };
                    self.store_expression_type(value, ty);
                    ty
                } else {
                    self.infer_expression(value, tcx)
                };
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:105 on 2025-10-09 15:25_

Oh, I see that with the new implementation of `TypeVar` definition parsing, this probably simplifies the implementation a lot. So feel free to disregard this comment.

---

_@AlexWaygood reviewed on 2025-10-09 15:26_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4113 on 2025-10-09 15:29_

maybe a slightly clearer error message?

```suggestion
                                "The `covariant` parameter of `typing.TypeVar` \
                                cannot have an ambiguous truthiness",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4130 on 2025-10-09 15:29_

```suggestion
                                cannot have an ambiguous truthiness",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4201 on 2025-10-09 15:32_

Hmm:

```
error[invalid-legacy-type-variable]: The first argument to `typing.TypeVar` must be a string literal.
 --> foo.py:3:5
  |
1 | import typing
2 |
3 | T = typing.TypeVar()
  |     ^^^^^^^^^^^^^^^^
  |
info: rule `invalid-legacy-type-variable` is enabled by default

Found 1 diagnostic
```

shouldn't the error message there be something like "must pass a `name` parameter to `typing.TypeVar`"? It sounds like it's complaining about the type of the first argument, but no arguments were passed

Similarly, I think it's totally fine if we don't want to support this (I don't think any other type checker does), but this isn't a _great_ set of error messages here:

```
error[invalid-legacy-type-variable]: The first argument to `typing.TypeVar` must be a string literal.
 --> foo.py:3:5
  |
1 | from typing import TypeVar
2 |
3 | T = TypeVar(name="T")
  |     ^^^^^^^^^^^^^^^^^
  |
info: rule `invalid-legacy-type-variable` is enabled by default

error[invalid-legacy-type-variable]: Unknown keyword argument `name` in `typing.TypeVar` creation
 --> foo.py:3:13
  |
1 | from typing import TypeVar
2 |
3 | T = TypeVar(name="T")
  |             ^^^^^^^^
  |
info: rule `invalid-legacy-type-variable` is enabled by default
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4203 on 2025-10-09 15:33_

```suggestion
        let ast::Expr::Name(ast::ExprName { id, .. }) = target_name else {
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4206 on 2025-10-09 15:33_

```suggestion
                "A `typing.TypeVar` definition must be a simple variable assignment",
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4268 on 2025-10-09 15:35_

```suggestion
        let ast::Expr::Call(ast::ExprCall { arguments, .. }) = value else {
            return;
        };
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4182 on 2025-10-09 15:49_

You're not calling `self.infer_expression()` on `kwarg.value` here, which then results in `Unknown` if you hover over `infer_variance` in the TypeVar call in the playground. This is a screenshot from a playground build on your branch:

<img width="1184" height="298" alt="image" src="https://github.com/user-attachments/assets/b113b9a0-9ab2-4831-8eff-a951551d0cbb" />

```suggestion
                    // TODO support `infer_variance` in legacy TypeVars
                    self.infer_expression(&kwarg.value, TypeContext::default());
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:4205 on 2025-10-09 15:50_

same here -- for an unknown keyword argument, you're never recursing into the value of the argument to infer the type at the moment

```suggestion
                    );
                    self.infer_expression(&kwarg.value, TypeContext::default());
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:6452 on 2025-10-09 15:51_

should this be

```suggestion
                if matches!(class.known(self.db(), Some(KnownClass::TypeVar | KnownClass::ExtensionsTypeVar)) {
```

?

---

_@AlexWaygood approved on 2025-10-09 15:51_

🚀

---

_@carljm reviewed on 2025-10-09 16:22_

---

_Review comment by @carljm on `crates/ty_ide/src/goto_type_definition.rs`:311 on 2025-10-09 16:22_

I don't think it will be hard to remember, because I have to fix it in order to fix the cases in https://github.com/astral-sh/ty/issues/256 related to `TypeAliasType`. So in that sense we already have an issue that ensures we won't forget it.

---

_@carljm reviewed on 2025-10-09 16:23_

---

_Review comment by @carljm on `crates/ty_ide/src/goto_type_definition.rs`:311 on 2025-10-09 16:23_

(And I plan to do this PR next.)

---

_@carljm reviewed on 2025-10-09 16:26_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:3 on 2025-10-09 16:26_

> we should really permit users to pass `default=whatever` even on lower Python versions if the user is making use of `typing_extensions.TypeVar`

This PR already does that (and has tests for it, in another file below).

The thing this PR doesn't do is allow you to access the `__default__` attribute on the resulting typevar created via `typing_extensions.TypeVar`, because we don't yet track how a given TypeVar was originally created.

The change here is because some tests in this file import `typing.TypeVar`, not `typing_extensions.TypeVar`. Would you rather I change them all to import from `typing_extensions`, instead of changing the Python version for this file?

---

_@carljm reviewed on 2025-10-09 16:30_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:42 on 2025-10-09 16:30_

It seemed like an odd thing to test in this file, because it includes this diagnostic "function call not allowed in type expression", which is arguably the more basic/important error in this case, and is not an error related to TypeVars at all. So I replaced the test with a mostly-equivalent version that only tests the (same) TypeVar-related diagnostic, without the distraction of the unrelated diagnostic.

---

_@carljm reviewed on 2025-10-09 16:32_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:74 on 2025-10-09 16:32_

I think I got rid of the error message with the intent to snapshot-diagnostics instead, but then decided against that because these tests make too much use of `reveal_type`, which clutters diagnostic snapshots horribly.

I can put the error message back, but it feels odd/arbitrary to inconsistently have one or two error messages tested and the majority not, for no particular reason.

I could consistently add error messages everywhere, but in general I prefer not doing that, because they are painful to update when error messages change. I'd rather use snapshot-diagnostics for testing the content of diagnostic messages.

I can add a dedicated set of snapshot-diagnostics tests for all the cases tested here?

---

_@carljm reviewed on 2025-10-09 16:35_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:105 on 2025-10-09 16:35_

Yes, it was a combination of "easier to implement without it", "no other type checker supports it", and "given the other restrictions on TypeVar argument values, it seems unlikely there's a compelling use case for this."

If we ever run into a compelling use case, it wouldn't actually be _that_ hard to add support for it later. But doesn't seem worth it right now.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:158 on 2025-10-09 16:36_

Huh, weird! Good catch :)

---

_@carljm reviewed on 2025-10-09 16:36_

---

_@carljm reviewed on 2025-10-09 16:41_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:265 on 2025-10-09 16:41_

Yes, the fact that other type checkers allow this is what convinced me we should too. And to be honest, I think other type checkers are right to allow it; there's really only upside. The point of a type checker is to catch things that will error at runtime. A TypeVar definition in a stub file that uses more recent features will not error at runtime. So what's the point of being pedantic about making users use `typing_extensions.TypeVar` in a stub file?

---

_@carljm reviewed on 2025-10-09 16:41_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:265 on 2025-10-09 16:41_

I can extend the prose here a bit, but I'm inclined to actually make the case for the feature, not just say "other type checkers do it, so we also do it for compatibility."

---

_@carljm reviewed on 2025-10-09 16:45_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:316 on 2025-10-09 16:45_

Note that we (correctly) don't error on the TypeVar creation here. The only part of this test that is a TODO is accessing the `__default__` attribute afterwards.

---

_@carljm reviewed on 2025-10-09 16:54_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:404 on 2025-10-09 16:54_

> hrm... on line 351 you said that they _cannot_ be cyclic, but here you say that they _can_ "cyclically" refer back to their own type?

I said they "cannot be generic, cyclic or otherwise", which does not say they can't be cyclic.

The bound here is not generic, because `list[G]` is a fully specialized type (as you point out below), so the above statement doesn't apply here.

> Though FWIW I think it's debatable whether this example really counts as a cycle. `G` as it appears in the bound of `list[G]` here is syntactic sugar for the default specialization of `G`, which is `G[Unknown]` -- so this type variable's upper bound is `list[G[Unknown]]`, which is not a type that refers back to `T` in any way since `T` has been fully "specialized away" from the type.

It counts as a cycle in the way that's IMO most relevant, which is that it causes a divergent Salsa query cycle and panics on current main branch :) The cycle occurs because when specializing `G`, we have to compare the specialization type against the upper bound of the typevar of `G`, which means we have to evaluate `G`, which is `G[Unknown]`, which means we have to specialize `G`... and we are in a cycle.

I think there might be an alternative fix for this specific cycle, where we special-case "unknown specialization" and don't bother checking for assignability to the upper bound (since we know that `Unknown` will be assignable to it, whatever it is). But we don't bother with that in this PR, since the more general laziness fix handles it anyway. I might try it (separately) and see if it shows a meaningful performance benefit, though.

---

_@carljm reviewed on 2025-10-09 16:58_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:462 on 2025-10-09 16:58_

Yes, it broke in an earlier version of the PR. This is the bug that is fixed with the new `TypeVarInstance::original` field and the new `is_identical_to` methods.

---

_@carljm reviewed on 2025-10-09 17:02_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1632 on 2025-10-09 17:02_

I did that in an earlier version of the PR, but doing it for attribute targets breaks our implicit instance attributes handling. I might be able to do it for subscript expressions? But I suspect they are not common enough to move the needle noticeably on performance. I can experiment with that separately, but there's no need to include it in this PR.

---

_@AlexWaygood reviewed on 2025-10-09 17:19_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1632 on 2025-10-09 17:19_

Interesting. And, yeah, definitely not a blocker!

---

_@AlexWaygood reviewed on 2025-10-09 17:23_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:3 on 2025-10-09 17:23_

> Would you rather I change them all to import from `typing_extensions`, instead of changing the Python version for this file?

Yeah, that does make more sense to me -- I think it's nice to try to avoid using version blocks like this where possible, or it makes it look a little like the feature is only available on newer versions of Python, and we'll emit an error on it on older versions.

It obviously doesn't matter a huge amount, but it made me raise my eyebrow when reading through the tests :-)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:74 on 2025-10-09 17:25_

> I could consistently add error messages everywhere, but in general I prefer not doing that, because they are painful to update when error messages change. I'd rather use snapshot-diagnostics for testing the content of diagnostic messages.
>
> I can add a dedicated set of snapshot-diagnostics tests for all the cases tested here?

That's fair. Adding a dedicated set of snapshot-diagnostics tests sounds like a good solution!

---

_@AlexWaygood reviewed on 2025-10-09 17:25_

---

_@AlexWaygood reviewed on 2025-10-09 17:37_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:265 on 2025-10-09 17:37_

Meh. Yeah, it's useful, sure, but so would a number of other ad-hoc special cases that you _could_ apply to stubs if you wanted to. I feel like there's also a strong case to say that "stubs should mostly work the same way as runtime files, except for a narrow series of exceptions", and I'm not sure that this exception is really useful _enough_. There's also a strong benefit in keeping special cases to a minimum -- it reduces the cognitive overhead of understanding stub files. And `typing_extensions` is always available as an import in stub files, even if you don't depend on it, because typeshed pretends it's part of the stdlib, so I'm just really not sure it brings much benefit.

To me it feels kinda confusing that we'd complain about `typing.TypeVar(infer_variance=True)` in a `.py` file on Python <3.12 but not in a stub file. I'm not suggesting we change the behaviour, given that compatibility with other type checkers is important, but it's not a special case I'd introduce if I were starting from a blank slate, personally

---

_@AlexWaygood reviewed on 2025-10-09 17:38_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:404 on 2025-10-09 17:38_

> I said they "cannot be generic, cyclic or otherwise", which does not say they can't be cyclic.

oh, sorry. I misread that as being "three separate things that they cannot be" rather than "one thing that they cannot be, which has two subcategories"

---

_@carljm reviewed on 2025-10-09 18:53_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:6452 on 2025-10-09 18:53_

Yes, thanks! I'll also add a test for this.

---

_@carljm reviewed on 2025-10-09 19:42_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer/builder.rs`:4201 on 2025-10-09 19:42_

I improved the error message for the first case.

I checked the second case; mypy does not support it, but pyright and pyrefly both do. It's not hard to support, so I will go ahead and add support.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:74 on 2025-10-09 19:42_

Done.

---

_@carljm reviewed on 2025-10-09 19:42_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/diagnostics/legacy_typevars.md`:100 on 2025-10-09 20:17_

The same applies for `infer_variance=True`... Though we can defer adding that check until we add full support for that keyword, I suppose

---

_@AlexWaygood approved on 2025-10-09 20:20_

---

_@carljm reviewed on 2025-10-09 20:58_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/diagnostics/legacy_typevars.md`:100 on 2025-10-09 20:58_

I went ahead and added this check for consistency.

---

_Merged by @carljm on 2025-10-09 21:08_

---

_Closed by @carljm on 2025-10-09 21:08_

---

_Branch deleted on 2025-10-09 21:08_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1632 on 2025-10-09 22:59_

Testing the impact of this at https://github.com/astral-sh/ruff/pull/20795

---

_@carljm reviewed on 2025-10-09 22:59_

---

_@carljm reviewed on 2025-10-09 23:15_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variables.md`:404 on 2025-10-09 23:15_

> The cycle occurs because when specializing `G`, we have to compare the specialization type against the upper bound of the typevar of `G`, which means we have to evaluate `G`, which is `G[Unknown]`, which means we have to specialize `G`... and we are in a cycle.

This was not actually an accurate summary of why there was a cycle here, before this PR. The correct description is this: as part of inferring the type of the symbol `T`, we had to eagerly infer the type of its upper bound, which required knowing the type of `G`, and since `G` is generic over `T`, that requires knowing the type of `T`, and right there we have a cycle.

I think this is reasonably described as a cycle between the definitions of `T` and `G`.

The "alternative fix" I described above is not actually an alternative at all -- we still need the lazy bounds to even get to the point of having independently-inferrable types for `T` and `G`. And it's not an optimization, either, because we actually already do this -- we don't check against upper bounds when applying an unknown or default specialization.

---

_@carljm reviewed on 2025-10-09 23:17_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/semantic_index/builder.rs`:1632 on 2025-10-09 23:17_

As I'd suspected, the impact does not appear to be detectable in either memory usage or CPU. Given that I think it makes the invariants slightly harder to understand, and requires changing a use of `infer_standalone_expression` to `infer_maybe_standalone_expression`, I'm going to close that PR.

---

_Comment by @carljm on 2025-10-09 23:19_

Thanks @AlexWaygood for the excellent review here!

---
