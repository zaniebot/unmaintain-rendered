```yaml
number: 21798
title: "[ty] Carry generic context through when converting class into `Callable`"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: dcreager/generic-context-of-callable
created_at: 2025-12-04T19:55:48Z
updated_at: 2025-12-05T13:57:23Z
url: https://github.com/astral-sh/ruff/pull/21798
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Carry generic context through when converting class into `Callable`

---

_Pull request opened by @dcreager on 2025-12-04 19:55_

When converting a class (whether specialized or not) into a `Callable` type, we should carry through any generic context that the constructor has. This includes both the generic context of the class itself (if it's generic) and of the constructor methods (if they are separately generic).

To help test this, this also updates the `generic_context` extension function to work on `Callable` types and unions; and adds a new `into_callable` extension function that works just like `CallableTypeOf`, but on value forms instead of type forms.

Pulled this out of #21551 for separate review.



---

_Review requested from @carljm by @dcreager on 2025-12-04 19:55_

---

_Review requested from @AlexWaygood by @dcreager on 2025-12-04 19:55_

---

_Review requested from @sharkdp by @dcreager on 2025-12-04 19:55_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 19:58_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-04 20:00_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
+ bidict/_base.py:501:9: error[no-matching-overload] No overload of bound method `update` matches arguments
- Found 14 diagnostics
+ Found 15 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/datastructures/structures.py:951:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 367 diagnostics
+ Found 366 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/config.py:361:80: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 226 diagnostics
+ Found 225 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
+ src/typeshed_stats/gather.py:1000:35: warning[unused-ignore-comment] Unused `ty: ignore` directive
- Found 16 diagnostics
+ Found 17 diagnostics

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/_ctypes.pyi:104:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mypy/typeshed/stdlib/_io.pyi:268:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1783 diagnostics
+ Found 1781 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/utils/universal.py:613:9: error[invalid-method-override] Invalid override of method `default_missing`: Definition is incompatible with `PerMachineDefaultable.default_missing`
- Found 1917 diagnostics
+ Found 1918 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- bson/son.py:162:15: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 458 diagnostics
+ Found 457 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-ray/prefect_ray/task_runners.py:242:9: error[invalid-method-override] Invalid override of method `submit`: Definition is incompatible with `TaskRunner.submit`
+ src/prefect/_internal/concurrency/waiters.py:141:9: error[invalid-method-override] Invalid override of method `wait`: Definition is incompatible with `Waiter.wait`
+ src/prefect/_internal/concurrency/waiters.py:259:15: error[invalid-method-override] Invalid override of method `wait`: Definition is incompatible with `Waiter.wait`
+ src/prefect/server/database/dependencies.py:282:9: error[invalid-method-override] Invalid override of method `__get__`: Definition is incompatible with `PrefectDescriptorBase.__get__`
- Found 4952 diagnostics
+ Found 4956 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/testing/internal/test_data.py:140:15: warning[unsupported-base] Unsupported class base with type `<class 'TestItem[Test, Never]'> | <class 'TestItem[Unknown, Unknown]'>`
- Found 8331 diagnostics
+ Found 8332 diagnostics

scipy-stubs (https://github.com/scipy/scipy-stubs)
+ scipy-stubs/_lib/_docscrape.pyi:42:9: error[invalid-method-override] Invalid override of method `__iter__`: Definition is incompatible with `Iterable.__iter__`
- Found 95 diagnostics
+ Found 96 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- Found 38 diagnostics
+ Found 39 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- aiohttp/web_server.py:74:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 179 diagnostics
+ Found 178 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/core/property/container.py:138:9: error[invalid-method-override] Invalid override of method `wrap`: Definition is incompatible with `Property.wrap`
+ src/bokeh/core/property/container.py:165:9: error[invalid-method-override] Invalid override of method `wrap`: Definition is incompatible with `Property.wrap`
- Found 856 diagnostics
+ Found 858 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- django-stubs/contrib/admin/options.pyi:330:106: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- django-stubs/contrib/admin/options.pyi:331:106: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- django-stubs/contrib/admin/options.pyi:332:104: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ django-stubs/core/files/base.pyi:49:9: error[invalid-method-override] Invalid override of method `write`: Definition is incompatible with `IO.write`
- django-stubs/db/models/expressions.pyi:247:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- django-stubs/utils/datastructures.pyi:83:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 440 diagnostics
+ Found 436 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1209:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5514 diagnostics
+ Found 5513 diagnostics

egglog-python (https://github.com/egraphs-good/egglog-python)
+ python/egglog/egraph.py:2259:60: error[invalid-type-arguments] Type `COST@_CostModel` is not assignable to upper bound `_Cost` of type variable `_COST@CostModel`
- Found 1489 diagnostics
+ Found 1490 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review requested from @MichaReiser by @dcreager on 2025-12-04 20:07_

---

_Label `ty` added by @dcreager on 2025-12-04 20:10_

---

_Label `ecosystem-analyzer` added by @dcreager on 2025-12-04 20:10_

---

_Comment by @astral-sh-bot[bot] on 2025-12-04 20:20_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unused-ignore-comment` | 1 | 11 | 0 |
| `invalid-method-override` | 9 | 0 | 0 |
| `unsupported-base` | 3 | 0 | 0 |
| `invalid-type-arguments` | 1 | 0 | 0 |
| `no-matching-overload` | 1 | 0 | 0 |
| **Total** | **15** | **11** | **0** |

**[Full report with detailed diff](https://dcreager-generic-context-of.ecosystem-663.pages.dev/diff)** ([timing results](https://dcreager-generic-context-of.ecosystem-663.pages.dev/timing))




---

_Comment by @dcreager on 2025-12-04 20:27_

The ecosystem changes are largely due to picking up more Liskov violations, like the TODO that was removed from the `liskov.md` mdtest

---

_@carljm approved on 2025-12-05 01:39_

Nice!

---

_Merged by @dcreager on 2025-12-05 13:57_

---

_Closed by @dcreager on 2025-12-05 13:57_

---

_Branch deleted on 2025-12-05 13:57_

---
