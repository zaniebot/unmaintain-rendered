```yaml
number: 17609
title: "[red-knot] Check for invalid overload usages"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: dhruv/check-invalid-overloads
created_at: 2025-04-24T13:50:43Z
updated_at: 2025-04-30T14:10:42Z
url: https://github.com/astral-sh/ruff/pull/17609
synced_at: 2026-01-10T19:03:00Z
```

# [red-knot] Check for invalid overload usages

---

_Pull request opened by @dhruvmanila on 2025-04-24 13:50_

## Summary

Part of #15383, this PR adds the core infrastructure to check for invalid overloads and adds a diagnostic to raise if there are < 2 overloads for a given definition.

### Design notes

The requirements to check the overloads are:
* Requires `FunctionType` which has the `to_overloaded` method
* The `FunctionType` **should** be for the function that is either the implementation or the last overload if the implementation doesn't exists
* Avoid checking any `FunctionType` that are part of an overload chain
* Consider visibility constraints

This required a couple of iteration to make sure all of the above requirements are fulfilled.

#### 1. Use a set to deduplicate

The logic would first collect all the `FunctionType` that are part of the overload chain except for the implementation or the last overload if the implementation doesn't exists. Then, when iterating over all the function declarations within the scope, we'd avoid checking these functions. But, this approach would fail to consider visibility constraints as certain overloads _can_ be behind a version check. Those aren't part of the overload chain but those aren't a separate overload chain either.

<details><summary>Implementation:</summary>
<p>

```rs
fn check_overloaded_functions(&mut self) {
    let function_definitions = || {
        self.types
            .declarations
            .iter()
            .filter_map(|(definition, ty)| {
                // Filter out function literals that result from anything other than a function
                // definition e.g., imports.
                if let DefinitionKind::Function(function) = definition.kind(self.db()) {
                    ty.inner_type()
                        .into_function_literal()
                        .map(|ty| (ty, definition.symbol(self.db()), function.node()))
                } else {
                    None
                }
            })
    };

    // A set of all the functions that are part of an overloaded function definition except for
    // the implementation function and the last overload in case the implementation doesn't
    // exists. This allows us to collect all the function definitions that needs to be skipped
    // when checking for invalid overload usages.
    let mut overloads: HashSet<FunctionType<'db>> = HashSet::default();

    for (function, _) in function_definitions() {
        let Some(overloaded) = function.to_overloaded(self.db()) else {
            continue;
        };
        if overloaded.implementation.is_some() {
            overloads.extend(overloaded.overloads.iter().copied());
        } else if let Some((_, previous_overloads)) = overloaded.overloads.split_last() {
            overloads.extend(previous_overloads.iter().copied());
        }
    }

    for (function, function_node) in function_definitions() {
        let Some(overloaded) = function.to_overloaded(self.db()) else {
            continue;
        };
        if overloads.contains(&function) {
            continue;
        }

        // At this point, the `function` variable is either the implementation function or the
        // last overloaded function if the implementation doesn't exists.

        if overloaded.overloads.len() < 2 {
            if let Some(builder) = self
                .context
                .report_lint(&INVALID_OVERLOAD, &function_node.name)
            {
                let mut diagnostic = builder.into_diagnostic(format_args!(
                    "Function `{}` requires at least two overloads",
                    &function_node.name
                ));
                if let Some(first_overload) = overloaded.overloads.first() {
                    diagnostic.annotate(
                        self.context
                            .secondary(first_overload.focus_range(self.db()))
                            .message(format_args!("Only one overload defined here")),
                    );
                }
            }
        }
    }
 }
```

</p>
</details> 

#### 2. Define a `predecessor` query

The `predecessor` query would return the previous `FunctionType` for the given `FunctionType` i.e., the current logic would be extracted to be a query instead. This could then be used to make sure that we're checking the entire overload chain once. The way this would've been implemented is to have a `to_overloaded` implementation which would take the root of the overload chain instead of the leaf. But, this would require updates to the use-def map to somehow be able to return the _following_ functions for a given definition.

#### 3. Create a successor link

This is what Pyrefly uses, we'd create a forward link between two functions that are involved in an overload chain. This means that for a given function, we can get the successor function. This could be used to find the _leaf_ of the overload chain which can then be used with the `to_overloaded` method to get the entire overload chain. But, this would also require updating the use-def map to be able to "see" the _following_ function.

### Implementation 

This leads us to the final implementation that this PR implements which is to consider the overloaded functions using:
* Collect all the **function symbols** that are defined **and** called within the same file. This could potentially be an overloaded function
* Use the public bindings to get the leaf of the overload chain and use that to get the entire overload chain via `to_overloaded` and perform the check

This has a limitation that in case a function redefines an overload, then that overload will not be checked. For example:

```py
from typing import overload

@overload
def f() -> None: ...
@overload
def f(x: int) -> int: ...

# The above overload will not be checked as the below function with the same name
# shadows it

def f(*args: int) -> int: ...
```

## Test Plan

Update existing mdtest and add snapshot diagnostics.


---

_Label `red-knot` added by @dhruvmanila on 2025-04-24 13:50_

---

_Comment by @codspeed-hq[bot] on 2025-04-24 13:56_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Fcheck-invalid-overloads)

### Merging #17609 will **not alter performance**

<sub>Comparing <code>dhruv/check-invalid-overloads</code> (382b15b) with <code>main</code> (7d46579)</sub>



### Summary

`✅ 33` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-04-24 21:35_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1028 on 2025-04-24 21:47_

This is the second part which tries to get the `FunctionType` to check using the public bindings from use-def map.

The `overloaded_function_symbols` is trying to collect all the `ScopedSymbolId` that's an overloaded function. We can't directly use the `FunctionType` there because we need the `FunctionType` corresponding to the final function which should be what `public_bindings` would give.

cc @carljm 

---

_@dhruvmanila reviewed on 2025-04-24 21:47_

---

_@dhruvmanila reviewed on 2025-04-24 21:48_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:4412 on 2025-04-24 21:48_

This is the first part which adds the `FunctionType` that needs to be checked which is only added if it's defined and called in the same scope.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6141 on 2025-04-24 23:08_

This is the sort of API we should generally avoid providing publicly on types (unless it is protected by a local Salsa query), because types travel across modules freely, and anytime type inference on one module would access this field, it would create a cross-module direct AST dependency, causing too much invalidation.

I think the actual usage of this function in this file should actually be OK, but I would still recommend we protect it better from mis-use. One way to do that would be to make it accept a `File` that we expect the function to be defined in, and if that file is not the same as `self.body_scope((db).file(db)`, we short-circuit and return `None`.

Another option would be to make this a method on `TypeInferenceBuilder` instead -- then it doesn't need to take a `File`, it can just check against `self.file()`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1033 on 2025-04-24 23:45_

This line will create extra Salsa dependencies. Since we are taking care above to only include functions defined in this file, that should be OK, but it will still be more efficient if we only do this when we actually need it -- that is, inside the actual error condition `if` body below. Ideally also inside the `if let Some(builder) { ... }` so we don't do it if diagnostics are not enabled here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4406 on 2025-04-24 23:50_

This line is the source of the test failure (and probably most of the regression, too.) If you look at the body of `FunctionType::definition`, you'll see that it accesses the semantic index, which means it has a direct dependency on the AST of the file the function is defined in. We are calling this unconditionally on every `FunctionLiteral` called, including those imported from other modules, so this will create direct dependencies to the ASTs of those modules.

One way we can prevent this is to first check that `function.body_scope(self.db()).file(self.db()) == self.file()` and only if that is true, go ahead and get the definition and do the full scope-match check.

It would probably make sense to add a warning in a doc comment on `FunctionType::definition` also.

---

_@carljm reviewed on 2025-04-24 23:50_

---

_Closed by @dhruvmanila on 2025-04-25 00:23_

---

_Reopened by @dhruvmanila on 2025-04-25 00:23_

---

_@dhruvmanila reviewed on 2025-04-25 13:58_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:6141 on 2025-04-25 13:58_

I've added an assertion to this function. Note that the same is being used for other methods like `full_range` and `focus_range` but I think those can come from other file because they're being used in the language server.

---

_@dhruvmanila reviewed on 2025-04-25 13:59_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1033 on 2025-04-25 13:59_

I'm not sure if it's possible to move it inside the `if let Some(builder) { ... }` branch because the `report_lint` requires the `Ranged` value which is the function name node. I don't think I can use `focus_range` because that also accesses the `node` in the same way as this method.

---

_@dhruvmanila reviewed on 2025-04-25 14:01_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:4406 on 2025-04-25 14:01_

Thank you, this was the case. I've added a warning note as well.

---

_Marked ready for review by @dhruvmanila on 2025-04-25 14:18_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-25 14:18_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-25 14:18_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-25 14:18_

---

_Comment by @dhruvmanila on 2025-04-25 14:18_

This PR only adds a single check, I'm thinking of creating a "help wanted" issue that lists down the other checks as the core infrastructure is now in place.

---

_Review requested from @carljm by @dhruvmanila on 2025-04-25 14:18_

---

_Comment by @AlexWaygood on 2025-04-25 17:05_

> ## `mypy_primer` results
> Changes were detected when running on open source projects
> 
> ```diff
> pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
> + error[lint:invalid-overload] /tmp/mypy_primer/projects/pytest-robotframework/pytest_robotframework/__init__.py:327:9: Function `__call__` requires at least two overloads
> + error[lint:invalid-overload] /tmp/mypy_primer/projects/pytest-robotframework/pytest_robotframework/__init__.py:470:5: Function `keyword` requires at least two overloads
> - Found 357 diagnostics
> + Found 359 diagnostics
> 
> pydantic (https://github.com/pydantic/pydantic)
> + error[lint:invalid-overload] /tmp/mypy_primer/projects/pydantic/pydantic/json_schema.py:2539:9: Function `__init__` requires at least two overloads
> - Found 923 diagnostics
> + Found 924 diagnostics
> 
> setuptools (https://github.com/pypa/setuptools)
> + error[lint:invalid-overload] /tmp/mypy_primer/projects/setuptools/setuptools/_distutils/sysconfig.py:578:5: Function `get_config_var` requires at least two overloads
> - Found 1084 diagnostics
> + Found 1085 diagnostics
> 
> django-stubs (https://github.com/typeddjango/django-stubs)
> + error[lint:invalid-overload] /tmp/mypy_primer/projects/django-stubs/django-stubs/db/models/constraints.pyi:61:9: Function `__init__` requires at least two overloads
> + error[lint:invalid-overload] /tmp/mypy_primer/projects/django-stubs/django-stubs/utils/html.pyi:27:5: Function `format_html` requires at least two overloads
> + error[lint:invalid-overload] /tmp/mypy_primer/projects/django-stubs/django-stubs/contrib/admin/options.pyi:136:9: Function `lookup_allowed` requires at least two overloads
> - Found 1415 diagnostics
> + Found 1418 diagnostics
> ```

These all look like false positives where one of the overloads is additionally decorated with `@deprecated`. I guess that's because `deprecated` is actually a class (`@deprecated` created a callable instance of `typing_extensions.deprecated`), and `deprecated.__call__` [is a generic function](https://github.com/python/typeshed/blob/ebf4d17c8d1ba2be348c51be39faec4d0303d8fa/stdlib/typing_extensions.pyi#L476), and we don't understand generic functions that use old-style `TypeVar`s yet.

---

_Comment by @dhruvmanila on 2025-04-25 17:08_

> These all look like false positives where one of the overloads is additionally decorated with `@deprecated`. I guess that's because `deprecated` is actually a class (`@deprecated` created a callable instance of `typing_extensions.deprecated`), and `deprecated.__call__` [is a generic function](https://github.com/python/typeshed/blob/ebf4d17c8d1ba2be348c51be39faec4d0303d8fa/stdlib/typing_extensions.pyi#L476), and we don't understand generic functions that use old-style `TypeVar`s yet.

Yes, sorry I meant to add that to the PR description as I looked at them but somehow missed to do so. Thank you for looking at it.

We don't support `@deprecated` decorator yet. In general, I think this issue will pop-up if there are any other non-special cased decorator that are present in addition to `@overload` because the inferred type would change to something other than `FunctionType` which is then not included in the overload chain.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6149 on 2025-04-25 21:32_

nit: I'd probably make this just a `debug_assert_eq!`, we probably don't need to pay for this check in release builds, it should be an issue that is caught in dev

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:6180 on 2025-04-25 21:33_

```suggestion
    /// a cross-module dependency directly on the full AST which will lead to
    /// cache over-invalidation.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4411 on 2025-04-25 21:41_

minor nit: given that we now do this check a couple places, it could be a method `FunctionType::defined_in_file(db, file)`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Invalid_-_At_least_two_overloads.snap`:32 on 2025-04-25 21:45_

nit: This reads slightly off to me, because the function doesn't _require_ at least two overloads, it could have zero and that would also be fine. I think one way to address this would be to say "Overloaded function `func1`..." instead of just "Function `func1`..." -- if it is specified that it is an overloaded function, then it is true that it requires at least two overloads.

---

_@carljm reviewed on 2025-04-25 21:49_

Looks great!

---

_@dhruvmanila reviewed on 2025-04-25 22:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:4411 on 2025-04-25 22:26_

Are you referring to the `function.body_scope(self.db()).file(self.db())` part or the entire equality check? I'm guessing it's the former as I couldn't find the latter part.

---

_@dhruvmanila reviewed on 2025-04-25 22:26_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Invalid_-_At_least_two_overloads.snap`:32 on 2025-04-25 22:26_

Yeah, that makes sense. Thanks!

---

_@carljm reviewed on 2025-04-25 22:37_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:4411 on 2025-04-25 22:37_

I meant the entire equality check. Don't we do it both here and in the assertion you added in `FunctionType::node`?

I mean, extracting a `FunctionType::file()` method isn't necessarily a bade idea either...

---

_@dhruvmanila reviewed on 2025-04-28 13:58_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:4411 on 2025-04-28 13:58_

> I mean, extracting a `FunctionType::file()` method isn't necessarily a bade idea either...

Yeah, I've added this instead of the equality check method.

---

_Review requested from @carljm by @dhruvmanila on 2025-04-28 13:58_

---

_Comment by @dhruvmanila on 2025-04-29 16:01_

I looked at the mypy-primer failure in https://github.com/astral-sh/ruff/pull/17681 which is on `porcupine` project, it started happening after https://github.com/astral-sh/ruff/pull/16538 was merged. It seems that for the following code:

```py
from typing import Any, Callable, TypeVar

_T = TypeVar("_T")


# https://github.com/python/typing/issues/769
def copy_type(f: _T) -> Callable[[Any], _T]:
    return lambda x: x


@copy_type(open)
def backup_open():
    pass
```

The inference builder binds the `backup_open` definition to the builtin `open` function which looks correct. This means that the overload check for `backup_open` which is defined in the current scope is actually being done on `open` which is defined elsewhere.

---

_@carljm approved on 2025-04-29 21:03_

Looks great!

---

_Merged by @dhruvmanila on 2025-04-30 14:07_

---

_Closed by @dhruvmanila on 2025-04-30 14:07_

---

_Branch deleted on 2025-04-30 14:07_

---

_Label `diagnostics` added by @dhruvmanila on 2025-04-30 14:10_

---
