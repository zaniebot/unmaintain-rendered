```yaml
number: 21386
title: "[ty] Silence false-positive diagnostics when using `typing.Dict` or `typing.Callable` as the second argument to `isinstance()`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/stdlib-alias-isinstance
created_at: 2025-11-11T16:53:56Z
updated_at: 2025-11-11T19:30:03Z
url: https://github.com/astral-sh/ruff/pull/21386
synced_at: 2026-01-12T15:57:22Z
```

# [ty] Silence false-positive diagnostics when using `typing.Dict` or `typing.Callable` as the second argument to `isinstance()`

---

_@AlexWaygood_

## Summary

Typeshed annotates the builtin functions `isinstance()` and `issubclass()` like so:

https://github.com/astral-sh/ruff/blob/44b0c9ebac8b63d39632233959ff3ec2694c3ef6/crates/ty_vendored/vendor/typeshed/stdlib/builtins.pyi#L3741-L3760

That leads us to currently reject code such as `isinstance({}, typing.Dict)`, since `typing.Dict` isn't an instance of `type`, an instance of `types.UnionType`, or a tuple thereof:

```pycon
>>> import typing
>>> type(typing.Dict)
<class 'typing._SpecialGenericAlias'>
```

However, you _can_ still nonetheless use it in `isinstance()` checks at runtime, due to some magic the typing module does to make these special forms behave similarly to the stdlib clases that they're aliasing in some ways:

```pycon
>>> isinstance({}, typing.Dict)
True
```

This PR implements that special case.

## Test Plan

Mdtests added


---

_Label `ty` added by @AlexWaygood on 2025-11-11 16:53_

---

_Comment by @astral-sh-bot[bot] on 2025-11-11 16:56_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-11 16:56_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
kornia (https://github.com/kornia/kornia)
- kornia/utils/draw.py:377:29: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
- Found 763 diagnostics
+ Found 762 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_vendor/distlib/compat.py:326:32: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Callable`
- src/pip/_vendor/requests/models.py:213:29: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Callable`
- src/pip/_vendor/requests/models.py:216:71: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Callable`
- Found 578 diagnostics
+ Found 575 diagnostics

ignite (https://github.com/pytorch/ignite)
- tests/ignite/metrics/test_precision_recall_curve.py:177:32: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Tuple`
- Found 2124 diagnostics
+ Found 2123 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/v1/fields.py:666:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/v1/fields.py:743:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/v1/fields.py:684:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
- pydantic/v1/fields.py:694:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Set`
- pydantic/v1/fields.py:704:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.FrozenSet`
- pydantic/v1/fields.py:714:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Deque`
- pydantic/v1/fields.py:725:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.DefaultDict`
- pydantic/v1/fields.py:729:33: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Counter`
- pydantic/v1/schema.py:1059:35: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
- pydantic/v1/schema.py:1072:35: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Set`
- pydantic/v1/schema.py:1076:35: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.FrozenSet`
- pydantic/v1/schema.py:1084:35: error[invalid-argument-type] Argument to function `issubclass` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Dict`
- pydantic/v1/types.py:650:34: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.Callable`
- Found 781 diagnostics
+ Found 772 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 43 diagnostics
+ Found 44 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/_vendor/jaraco/collections/__init__.py:40:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ setuptools/_vendor/typeguard/_checkers.py:938:17: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1177 diagnostics
+ Found 1179 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/externals/_numpydoc/docscrape.py:687:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- sklearn/externals/_numpydoc/docscrape.py:745:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- sklearn/impute/_base.py:516:47: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- sklearn/impute/_base.py:589:35: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- Found 2562 diagnostics
+ Found 2558 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- arviz/plots/backends/bokeh/kdeplot.py:192:33: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- arviz/plots/backends/bokeh/kdeplot.py:229:33: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- Found 823 diagnostics
+ Found 821 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-dask/prefect_dask/task_runners.py:242:44: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- src/integrations/prefect-databricks/prefect_databricks/rest.py:46:26: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Dict`
- src/integrations/prefect-gcp/prefect_gcp/models/cloud_run_v2.py:339:40: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.List`
- src/integrations/prefect-kubernetes/tests/test_credentials.py:172:50: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Dict`
- src/integrations/prefect-kubernetes/tests/test_credentials.py:185:50: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Dict`
- src/integrations/prefect-kubernetes/tests/test_credentials.py:197:50: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Dict`
- Found 3245 diagnostics
+ Found 3239 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- com/win32com/test/testvb.py:172:26: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- com/win32comext/adsi/demos/test.py:235:27: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- win32/Demos/win32netdemo.py:231:64: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- Found 2607 diagnostics
+ Found 2604 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/llmobs/_evaluators/ragas/faithfulness.py:168:43: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | tuple[Unknown, ...]`, found `typing.List`
- Found 8234 diagnostics
+ Found 8233 diagnostics

jax (https://github.com/google/jax)
- jax/_src/custom_partitioning.py:487:60: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- Found 2561 diagnostics
+ Found 2560 diagnostics

sympy (https://github.com/sympy/sympy)
- sympy/core/power.py:739:52: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- sympy/matrices/matrixbase.py:4351:55: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- sympy/matrices/sparse.py:130:33: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
+ sympy/parsing/mathematica.py:981:56: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- sympy/plotting/series.py:1836:26: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- sympy/vector/coordsysrect.py:84:45: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- Found 14448 diagnostics
+ Found 14444 diagnostics

scipy (https://github.com/scipy/scipy)
- scipy/_lib/_docscrape.py:689:38: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- scipy/_lib/_docscrape.py:747:30: error[invalid-argument-type] Argument to function `isinstance` is incorrect: Expected `type | UnionType | tuple[Unknown, ...]`, found `typing.Callable`
- Found 9160 diagnostics
+ Found 9158 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Marked ready for review by @AlexWaygood on 2025-11-11 17:06_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-11 17:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-11 17:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-11 17:06_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:166 on 2025-11-11 18:17_

```suggestion
Certain special forms in the typing module are not instances of `type`, so are strictly-speaking
```

---

_@carljm approved on 2025-11-11 18:20_

Ecosystem report says yeah! Good stuff, thank you.

I wonder if people are also relying on these isinstance/issubclass calls for type narrowing? I think we'd need to do something further to make that work.

---

_Comment by @AlexWaygood on 2025-11-11 18:24_

> I wonder if people are also relying on these isinstance/issubclass calls for type narrowing? I think we'd need to do something further to make that work.

Yes, I agree! But that can be its own PR ;)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/call/builtins.md`:166 on 2025-11-11 18:25_

it me, the professional jorunlaist

---

_@AlexWaygood reviewed on 2025-11-11 18:25_

---

_Merged by @AlexWaygood on 2025-11-11 19:30_

---

_Closed by @AlexWaygood on 2025-11-11 19:30_

---

_Branch deleted on 2025-11-11 19:30_

---
