```yaml
number: 17428
title: "[red-knot] Dataclasses: synthesize `__init__` with proper signature"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/dataclasses-pt3
created_at: 2025-04-16T12:14:00Z
updated_at: 2025-04-17T07:49:04Z
url: https://github.com/astral-sh/ruff/pull/17428
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Dataclasses: synthesize `__init__` with proper signature

---

_@sharkdp_

## Summary

This changeset allows us to generate the signature of synthesized `__init__` functions in dataclasses by analyzing the fields on the class (and its superclasses). There are certain things that I have not yet attempted to model in this PR, like `kw_only`, [`dataclasses.KW_ONLY`](https://docs.python.org/3/library/dataclasses.html#dataclasses.KW_ONLY) or functionality around [`dataclasses.field`](https://docs.python.org/3/library/dataclasses.html#dataclasses.field).

depends on https://github.com/astral-sh/ruff/pull/17406

ticket: https://github.com/astral-sh/ruff/issues/16651

## Ecosystem analysis

These two seem to depend on missing features in generics (see [relevant code here](https://github.com/konradhalas/dacite/blob/9898ccbb783e7e6a35ae165e7deb9fa84edfe21c/tests/core/test_generics.py#L54)):

> ```diff
> + error[lint:unknown-argument] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:54:24: Argument `x` does not match any known parameter
> + error[lint:unknown-argument] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:54:38: Argument `y` does not match any known parameter
> ```



These two are true positives :partying_face:. See [relevant code here](https://github.com/konradhalas/dacite/blob/9898ccbb783e7e6a35ae165e7deb9fa84edfe21c/tests/core/test_config.py#L154-L161).

> ```diff
> + error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:161:24: Argument to this function is incorrect: Expected `int`, found `Literal["test"]`
> + error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:172:24: Argument to this function is incorrect: Expected `int | float`, found `Literal["test"]`
> ```


This one depends on `**` unpacking of dictionaries, which we don't support yet:

> ```diff
> + error[lint:missing-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/globals.py:218:11: No arguments provided for required parameters `new`, `old`, `repo`, `type_checker`, `mypyc_compile_level`, `custom_typeshed_repo`, `new_typeshed`, `old_typeshed`, `new_prepend_path`, `old_prepend_path`, `additional_flags`, `project_selector`, `known_dependency_selector`, `local_project`, `expected_success`, `project_date`, `shard_index`, `num_shards`, `output`, `old_success`, `coverage`, `bisect`, `bisect_output`, `validate_expected_success`, `measure_project_runtimes`, `concurrency`, `base_dir`, `debug`, `clear`
> ```



## Test Plan

New Markdown tests.

---

_Label `red-knot` added by @sharkdp on 2025-04-16 12:14_

---

_Comment by @github-actions[bot] on 2025-04-16 12:16_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:54:24: Argument `x` does not match any known parameter
+ error[lint:unknown-argument] /tmp/mypy_primer/projects/dacite/tests/core/test_generics.py:54:38: Argument `y` does not match any known parameter
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:161:24: Argument to this function is incorrect: Expected `int`, found `Literal["test"]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dacite/tests/core/test_config.py:172:24: Argument to this function is incorrect: Expected `int | float`, found `Literal["test"]`
- Found 154 diagnostics
+ Found 158 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ error[lint:missing-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/globals.py:218:11: No arguments provided for required parameters `new`, `old`, `repo`, `type_checker`, `mypyc_compile_level`, `custom_typeshed_repo`, `new_typeshed`, `old_typeshed`, `new_prepend_path`, `old_prepend_path`, `additional_flags`, `project_selector`, `known_dependency_selector`, `local_project`, `expected_success`, `project_date`, `shard_index`, `num_shards`, `output`, `old_success`, `coverage`, `bisect`, `bisect_output`, `validate_expected_success`, `measure_project_runtimes`, `concurrency`, `base_dir`, `debug`, `clear`
- Found 9 diagnostics
+ Found 10 diagnostics

```
</details>


---

_Closed by @sharkdp on 2025-04-16 18:36_

---

_Reopened by @sharkdp on 2025-04-16 18:36_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:1027 on 2025-04-16 20:12_

This is a bit of a hack to avoid things like function definitions and nested class definitions showing up as dataclass fields. The filtering by `AnnotatedAssignment` is correct, I think, but we do not correctly handle weird things like
```py
class C:
    if flag():
        def attr(): ...
    else:
        attr: int = 1
```

Another option to solve this might be to pass down some kind of definition-kind-filter to `symbol_from_declarations`? Thoughts?

---

_@sharkdp reviewed on 2025-04-16 20:12_

---

_@sharkdp reviewed on 2025-04-16 20:15_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:1036 on 2025-04-16 20:15_

In this PR, I did not attempt to model any sort of possibly-unbound handling. It's also possible that there are edge cases with unions of dataclasses or dataclasses with unions of attributes that we don't handle correctly yet. I would like to postpone that to a post-alpha follow up, if that sounds okay. It's certainly not a problem on any of the ecosystem projects, because it would have shown up in new diagnostics otherwise.

---

_@sharkdp reviewed on 2025-04-16 20:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclasses.md`:59 on 2025-04-16 20:18_

This is still not solved. Turning `FunctionType` into an enum sounds painful :upside_down_face:, but I'll look into it eventually.

---

_Marked ready for review by @sharkdp on 2025-04-16 20:22_

---

_Review requested from @carljm by @sharkdp on 2025-04-16 20:22_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-16 20:22_

---

_Review requested from @dcreager by @sharkdp on 2025-04-16 20:22_

---

_Comment by @carljm on 2025-04-16 23:58_

> These two seem to depend on missing features in generics

I don't think so? It looks to me like they depend on missing support for dataclass inheritance. The dataclass `B` should inherit the fields `x` and `y` from `A`, and they should be part of its synthesized `__init__` method. I think this will result in false positives if we don't support it, so we may not want to land `__init__` synthesis for super long without supporting this inheritance feature.

EDIT: sorry, ignore this! Should have read the PR first. Clearly it does support dataclass fields inheritance, so I think you're right that the issue here is support for inheriting from a generic class. I think maybe https://github.com/astral-sh/ruff/pull/17434 will fix this?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1027 on 2025-04-17 00:17_

 A filter for `symbol_from_declarations` seems reasonable -- or it could even be a filter in the use-def map fetching?

The even trickier part about supporting something like this is that I think it would mean we'd have to generate a union of `__init__` methods with different signatures? It seems closely-related to the possibly-unbound handling you mention below, in that sense.

I definitely think this can be a TODO for now, I don't think any other type checker supports it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1036 on 2025-04-17 00:18_

Yes I think it's fine for this to be post-alpha, even post-beta probably.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:919 on 2025-04-17 00:20_

Amazing.

---

_@carljm approved on 2025-04-17 00:21_

Impeccable work as usual.

---

_@carljm reviewed on 2025-04-17 00:23_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/dataclasses.md`:59 on 2025-04-17 00:23_

I think it won't be too bad? It's really more like putting a new wrapper enum around the existing `FunctionType` (though the new wrapper enum should probably get the name `FunctionType`.) Some APIs of `FunctionType` will be easy to proxy (e.g. signature), and some are easy because we can just give up (no way to provide a definition location to support goto-type-definition for a synthetic function). Not sure if there are some that may be tricky.

---

_@sharkdp reviewed on 2025-04-17 07:18_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/class.rs`:1027 on 2025-04-17 07:18_

I added a comment for now.

---

_Merged by @sharkdp on 2025-04-17 07:30_

---

_Closed by @sharkdp on 2025-04-17 07:30_

---

_Branch deleted on 2025-04-17 07:31_

---

_@sharkdp reviewed on 2025-04-17 07:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclasses.md`:59 on 2025-04-17 07:49_

Thanks. I think I will I'll postpone this until after @dhruvmanila's overload work has been merged, as it looks to me like that would introduce conflicts, that we can easily avoid by just waiting a bit.

---
