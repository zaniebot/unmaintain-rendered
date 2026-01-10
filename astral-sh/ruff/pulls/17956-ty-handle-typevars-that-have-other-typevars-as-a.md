```yaml
number: 17956
title: "[ty] Handle typevars that have other typevars as a default"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/default-typevars
created_at: 2025-05-08T16:15:44Z
updated_at: 2025-05-09T00:19:17Z
url: https://github.com/astral-sh/ruff/pull/17956
synced_at: 2026-01-10T18:57:03Z
```

# [ty] Handle typevars that have other typevars as a default

---

_Pull request opened by @dcreager on 2025-05-08 16:15_

It's possible for a typevar to list another typevar as its default value:

```py
class C[T, U = T]: ...
```

When specializing this class, if a type isn't provided for `U`, we would previously use the default as-is, leaving an unspecialized `T` typevar in the specialization. Instead, we want to use what `T` is mapped to as the type of `U`.

```py
reveal_type(C())  # revealed: C[Unknown, Unknown]
reveal_type(C[int]())  # revealed: C[int, int]
reveal_type(C[int, str]())  # revealed: C[int, str]
```

This is especially important for the `slice` built-in type.

---

_Label `ty` added by @dcreager on 2025-05-08 16:15_

---

_Review requested from @carljm by @dcreager on 2025-05-08 16:15_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-08 16:15_

---

_Review requested from @sharkdp by @dcreager on 2025-05-08 16:15_

---

_Closed by @carljm on 2025-05-08 18:25_

---

_Reopened by @carljm on 2025-05-08 18:25_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:180 on 2025-05-08 18:29_

What about with legacy typevars?

---

_@carljm approved on 2025-05-08 18:30_

Looks good, except I suspect we might need a stronger assurance that the loop terminates?

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:180 on 2025-05-08 22:08_

This is now moot, but for posterity, the [typing spec requires](https://typing.python.org/en/latest/spec/generics.html#using-another-type-parameter-as-default) defaults to only refer to typevars that appear earlier in the generic context.

---

_Comment by @github-actions[bot] on 2025-05-08 22:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
python-chess (https://github.com/niklasf/python-chess)
- error[lint:call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`

kornia (https://github.com/kornia/kornia)
- error[lint:call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict[Unknown, Unknown] | list | None`
+ error[lint:call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict[Unknown, Unknown] | list | None`
- error[lint:call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict[Unknown, Unknown] | list | None`
+ error[lint:call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict[Unknown, Unknown] | list | None`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/quaternion.py:156:43: Method `__getitem__` of type `Unknown | (bound method Quaternion.__getitem__(idx: int | slice[Any, _StartT_co, _StartT_co | _StopT_co]) -> Quaternion)` is not callable on object of type `Unknown | Quaternion`
+ error[lint:call-non-callable] kornia/geometry/quaternion.py:156:43: Method `__getitem__` of type `Unknown | (bound method Quaternion.__getitem__(idx: int | slice[Any, Any, Any]) -> Quaternion)` is not callable on object of type `Unknown | Quaternion`

ignite (https://github.com/pytorch/ignite)
- error[lint:call-non-callable] tests/ignite/metrics/test_accuracy.py:317:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_accuracy.py:317:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_accuracy.py:375:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_accuracy.py:375:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_accuracy.py:376:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_accuracy.py:376:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_accuracy.py:444:33: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_accuracy.py:444:33: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_classification_report.py:28:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_classification_report.py:28:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_classification_report.py:102:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_classification_report.py:102:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_classification_report.py:103:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_classification_report.py:103:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_epoch_metric.py:171:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_epoch_metric.py:171:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_fbeta.py:135:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_fbeta.py:135:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_mean_pairwise_distance.py:68:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_mean_pairwise_distance.py:69:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_mean_pairwise_distance.py:90:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_mean_pairwise_distance.py:91:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_metrics_lambda.py:440:28: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_metrics_lambda.py:440:28: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_metrics_lambda.py:441:28: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_metrics_lambda.py:441:28: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_metrics_lambda.py:489:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_metrics_lambda.py:489:13: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_precision.py:351:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_precision.py:351:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_precision.py:403:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_precision.py:403:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_precision.py:404:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_precision.py:404:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_psnr.py:103:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_psnr.py:103:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_psnr.py:104:16: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_psnr.py:104:16: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_recall.py:354:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_recall.py:354:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_recall.py:406:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_recall.py:406:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_recall.py:407:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_recall.py:407:21: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_ssim.py:292:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_ssim.py:292:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_ssim.py:293:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_ssim.py:293:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
- error[lint:call-non-callable] tests/ignite/metrics/test_top_k_categorical_accuracy.py:72:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`
+ error[lint:call-non-callable] tests/ignite/metrics/test_top_k_categorical_accuracy.py:72:17: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | int | float | list`

tornado (https://github.com/tornadoweb/tornado)
- error[lint:unsupported-operator] tornado/template.py:829:14: Operator `<` is not supported for types `slice[Any, _StartT_co, _StartT_co | _StopT_co]` and `int`, in comparing `int | slice[Any, _StartT_co, _StartT_co | _StopT_co]` with `Literal[0]`
+ error[lint:unsupported-operator] tornado/template.py:829:14: Operator `<` is not supported for types `slice[Any, Any, Any]` and `int`, in comparing `int | slice[Any, Any, Any]` with `Literal[0]`

paasta (https://github.com/yelp/paasta)
- error[lint:call-non-callable] paasta_tools/contrib/bounce_log_latency_parser.py:48:54: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] paasta_tools/contrib/bounce_log_latency_parser.py:48:54: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] paasta_tools/contrib/bounce_log_latency_parser.py:49:43: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] paasta_tools/contrib/bounce_log_latency_parser.py:49:43: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] paasta_tools/contrib/bounce_log_latency_parser.py:50:43: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] paasta_tools/contrib/bounce_log_latency_parser.py:50:43: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`

optuna (https://github.com/optuna/optuna)
- error[lint:call-non-callable] tests/test_cli.py:1218:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1218:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
- error[lint:call-non-callable] tests/test_cli.py:1219:25: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1219:25: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
- error[lint:call-non-callable] tests/test_cli.py:1220:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1220:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
- error[lint:call-non-callable] tests/test_cli.py:1274:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1274:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
- error[lint:call-non-callable] tests/test_cli.py:1275:25: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1275:25: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
- error[lint:call-non-callable] tests/test_cli.py:1276:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1276:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
- error[lint:call-non-callable] tests/test_cli.py:1314:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1314:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
- error[lint:call-non-callable] tests/test_cli.py:1315:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1315:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
- error[lint:call-non-callable] tests/test_cli.py:1352:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`
+ error[lint:call-non-callable] tests/test_cli.py:1352:20: Method `__getitem__` of type `Any | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Any | list`

mypy (https://github.com/python/mypy)
- error[lint:call-non-callable] mypy/typeanal.py:2089:9: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] mypy/typeanal.py:2089:9: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] mypy/typeanal.py:2090:48: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] mypy/typeanal.py:2090:48: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] mypyc/ir/rtypes.py:123:61: Method `__getitem__` of type `Unknown | (bound method dict[str, Any].__getitem__(key: str, /) -> Any) | (Overload[(key: SupportsIndex | slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> LiteralString, (key: SupportsIndex | slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> str])` is not callable on object of type `Unknown | dict[str, Any] | str`
+ error[lint:call-non-callable] mypyc/ir/rtypes.py:123:61: Method `__getitem__` of type `Unknown | (bound method dict[str, Any].__getitem__(key: str, /) -> Any) | (Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str])` is not callable on object of type `Unknown | dict[str, Any] | str`

setuptools (https://github.com/pypa/setuptools)
- error[lint:call-non-callable] setuptools/tests/environment.py:84:16: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[Unknown, Unknown]`
+ error[lint:call-non-callable] setuptools/tests/environment.py:84:16: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[Unknown, Unknown]`

static-frame (https://github.com/static-frame/static-frame)
- error[lint:unsupported-operator] static_frame/core/util.py:2132:16: Operator `>=` is not supported for types `_StartT_co` and `int`
- error[lint:unsupported-operator] static_frame/core/util.py:2138:16: Operator `+` is unsupported between objects of type `_StartT_co` and `Literal[1]`
- Found 2616 diagnostics
+ Found 2614 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[lint:unsupported-operator] pymongo/asynchronous/cursor.py:621:29: Operator `-` is unsupported between objects of type `_StartT_co` and `Literal[0] | Any`
- error[lint:unsupported-operator] pymongo/synchronous/cursor.py:619:29: Operator `-` is unsupported between objects of type `_StartT_co` and `Literal[0] | Any`
- Found 555 diagnostics
+ Found 553 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[lint:call-non-callable] pwndbg/commands/binder.py:148:29: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex | slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> LiteralString, (key: SupportsIndex | slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> str])` is not callable on object of type `Unknown | str`
+ error[lint:call-non-callable] pwndbg/commands/binder.py:148:29: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str])` is not callable on object of type `Unknown | str`

apprise (https://github.com/caronc/apprise)
- error[lint:call-non-callable] apprise/plugins/apprise_api.py:320:13: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(key: SupportsIndex | slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> LiteralString, (key: SupportsIndex | slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> str])` is not callable on object of type `dict[Unknown, Unknown] | str`
+ error[lint:call-non-callable] apprise/plugins/apprise_api.py:320:13: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str])` is not callable on object of type `dict[Unknown, Unknown] | str`

sphinx (https://github.com/sphinx-doc/sphinx)
- error[lint:call-non-callable] sphinx/config.py:205:16: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[Unknown, Unknown, Unknown]`
+ error[lint:call-non-callable] sphinx/config.py:205:16: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[Unknown, Unknown, Unknown]`
- error[lint:call-non-callable] sphinx/util/inventory.py:319:16: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[Unknown, Unknown, Unknown, Unknown]`
+ error[lint:call-non-callable] sphinx/util/inventory.py:319:16: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple[Unknown, Unknown, Unknown, Unknown]`

bokeh (https://github.com/bokeh/bokeh)
- error[lint:unsupported-operator] src/bokeh/models/sources.py:721:49: Operator `>` is not supported for types `_StartT_co` and `int`, in comparing `@Todo(Support for `typing.TypeAlias`) & _StartT_co` with `int`
- error[lint:unsupported-operator] src/bokeh/models/sources.py:926:33: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`
- error[lint:unsupported-operator] src/bokeh/models/sources.py:927:33: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `_StartT_co | _StopT_co` with `Literal[0]`
- Found 984 diagnostics
+ Found 981 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:call-non-callable] ddtrace/contrib/internal/botocore/services/kinesis.py:61:32: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] ddtrace/contrib/internal/botocore/services/kinesis.py:61:32: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] ddtrace/contrib/internal/botocore/services/kinesis.py:62:36: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] ddtrace/contrib/internal/botocore/services/kinesis.py:62:36: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] ddtrace/contrib/internal/botocore/services/kinesis.py:62:36: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] ddtrace/contrib/internal/botocore/services/kinesis.py:62:36: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] ddtrace/contrib/internal/botocore/services/kinesis.py:62:36: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] ddtrace/contrib/internal/botocore/services/kinesis.py:62:36: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`

jax (https://github.com/google/jax)
- error[lint:not-iterable] jax/_src/numpy/index_tricks.py:85:86: Object of type `slice[Any, _StartT_co, _StartT_co | _StopT_co] | (@Todo(full tuple[...] support) & ~slice[Any, _StartT_co, _StartT_co | _StopT_co])` may not be iterable
+ error[lint:not-iterable] jax/_src/numpy/index_tricks.py:85:86: Object of type `slice[Any, Any, Any] | (@Todo(full tuple[...] support) & ~slice[Any, Any, Any])` may not be iterable
- error[lint:not-iterable] jax/_src/numpy/index_tricks.py:130:86: Object of type `slice[Any, _StartT_co, _StartT_co | _StopT_co] | (@Todo(full tuple[...] support) & ~slice[Any, _StartT_co, _StartT_co | _StopT_co])` may not be iterable
+ error[lint:not-iterable] jax/_src/numpy/index_tricks.py:130:86: Object of type `slice[Any, Any, Any] | (@Todo(full tuple[...] support) & ~slice[Any, Any, Any])` may not be iterable
- warning[lint:possibly-unbound-attribute] jax/_src/numpy/lax_numpy.py:7986:11: Attribute `__jax_array__` on type `(@Todo(Inference of subscript on special form) & ~slice[Any, _StartT_co, _StartT_co | _StopT_co]) | slice[Any, _StartT_co, _StartT_co | _StopT_co] | int | Unknown` is possibly unbound
+ warning[lint:possibly-unbound-attribute] jax/_src/numpy/lax_numpy.py:7986:11: Attribute `__jax_array__` on type `(@Todo(Inference of subscript on special form) & ~slice[Any, Any, Any]) | slice[Any, Any, Any] | int | Unknown` is possibly unbound
- error[lint:call-non-callable] jax/_src/pallas/pallas_call.py:815:17: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple`
+ error[lint:call-non-callable] jax/_src/pallas/pallas_call.py:815:17: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple`
- error[lint:call-non-callable] jax/_src/pallas/pallas_call.py:857:11: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] jax/_src/pallas/pallas_call.py:857:11: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] jax/experimental/mosaic/gpu/fragmented_array.py:2201:7: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] jax/experimental/mosaic/gpu/fragmented_array.py:2201:7: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:unsupported-operator] jax/experimental/mosaic/gpu/utils.py:655:10: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | Unknown` with `Literal[0]`
- error[lint:unsupported-operator] jax/experimental/mosaic/gpu/utils.py:658:42: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | Unknown` with `Literal[0]`
- error[lint:invalid-argument-type] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_kernel.py:1322:9: Argument to this function is incorrect: Expected `Slice`, found `Unknown | slice[Any, _StartT_co, _StartT_co | _StopT_co] | Slice`
+ error[lint:invalid-argument-type] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_kernel.py:1322:9: Argument to this function is incorrect: Expected `Slice`, found `Unknown | slice[Any, Any, Any] | Slice`
- error[lint:invalid-argument-type] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_kernel.py:1713:9: Argument to this function is incorrect: Expected `Slice`, found `Unknown | slice[Any, _StartT_co, _StartT_co | _StopT_co] | Slice`
+ error[lint:invalid-argument-type] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_kernel.py:1713:9: Argument to this function is incorrect: Expected `Slice`, found `Unknown | slice[Any, Any, Any] | Slice`
- error[lint:unsupported-operator] jax/experimental/pallas/ops/tpu/splash_attention/splash_attention_mask.py:524:10: Operator `<=` is not supported for types `_StartT_co` and `int`, in comparing `int | _StartT_co` with `int`
- Found 2973 diagnostics
+ Found 2970 diagnostics

scipy (https://github.com/scipy/scipy)
- error[lint:call-non-callable] scipy/_lib/_util.py:316:23: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple`
+ error[lint:call-non-callable] scipy/_lib/_util.py:316:23: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, Any, Any], /) -> @Todo(full tuple[...] support)]` is not callable on object of type `tuple`
- error[lint:unsupported-operator] scipy/signal/_signaltools.py:2905:14: Operator `>` is not supported for types `_StartT_co` and `Literal[0]`
- error[lint:unsupported-operator] scipy/signal/_signaltools.py:2907:25: Operator `<` is not supported for types `Literal[0]` and `_StartT_co`
- error[lint:unsupported-operator] scipy/signal/_signaltools.py:2919:25: Operator `<` is not supported for types `Literal[0]` and `_StartT_co`
- error[lint:unsupported-operator] scipy/signal/_signaltools.py:2924:12: Operator `>` is not supported for types `_StartT_co` and `Literal[0]`
- error[lint:call-non-callable] scipy/stats/tests/test_morestats.py:390:18: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] scipy/stats/tests/test_morestats.py:390:18: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- Found 7362 diagnostics
+ Found 7358 diagnostics

sympy (https://github.com/sympy/sympy)
- error[lint:unsupported-operator] sympy/sets/fancysets.py:867:28: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:869:30: Operator `>` is not supported for types `_StartT_co` and `Literal[1]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[1]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:873:26: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `Unknown & _StartT_co & ~None` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:874:28: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:879:28: Operator `>` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:884:28: Operator `>` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:892:28: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:896:26: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `Unknown & _StartT_co & ~None` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:899:28: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:903:26: Operator `>` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `Unknown & _StartT_co & ~None & ~Literal[0]` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:907:28: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:909:30: Operator `>` is not supported for types `_StartT_co` and `Literal[1]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[1]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:913:26: Operator `<` is not supported for types `_StartT_co` and `Literal[0]`, in comparing `Unknown & _StartT_co & ~None` with `Literal[0]`
- error[lint:unsupported-operator] sympy/sets/fancysets.py:914:28: Operator `>` is not supported for types `_StartT_co` and `Literal[1]`, in comparing `(Unknown & _StartT_co & ~AlwaysFalsy) | (Unknown & _StopT_co & ~AlwaysFalsy) | Literal[1]` with `Literal[1]`
- Found 17004 diagnostics
+ Found 16990 diagnostics

manticore (https://github.com/trailofbits/manticore)
- error[lint:call-non-callable] tests/wasm/json2mc.py:103:13: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] tests/wasm/json2mc.py:103:13: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] tests/wasm/json2mc.py:159:65: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] tests/wasm/json2mc.py:159:65: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] tests/wasm/json2mc.py:161:13: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] tests/wasm/json2mc.py:161:13: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-05-08 22:13_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Fdefault-typevars)

### Merging #17956 will **not alter performance**

<sub>Comparing <code>dcreager/default-typevars</code> (ab9f364) with <code>main</code> (9b694ad)</sub>



### Summary

`✅ 33` untouched benchmarks  





---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:176 on 2025-05-08 22:15_

Famous last words!  This loop did _not_ always terminate — though the rationale below about typevar defaults only being able to refer "leftwards" is still true.

The issue was that we were substituting for _all_ occurrences of each typevar, not just the ones that occur in a default value.  So in something like

```py
class C[T]: ...
def f[T]() -> C[list[T]]: ...
```

we would specialize `C` with `T = list[T]` (the two `T`s referring to different typevar bindings), and the fixed point logic would fall over trying to expand that:

```
C[list[T]]
C[list[list[T]]
C[list[list[list[T]]]
... and so on
```

To fix this, I'm no longer doing a fixed-point loop, I'm stepping through the mappings in the specialization, and for each one that is using the default, applying the specialization _once_.  Note that we also have to take care to only apply the portion of the specialization that appears "earlier" in the generic context, which is where the new `TypeMapping` enum (in particular its `Partial` variant) comes into play.

---

_@dcreager reviewed on 2025-05-08 22:15_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:170 on 2025-05-08 22:58_

Not sold on this name -- it feels like saying it will leave you with an incomplete specialization, but I don't think it will; it really is doing _more_ of the job than `specialize` does (that is, it also handles filling in defaults). Or in other words, if you are looking at some specializing Python code like `SomeGeneric[int, str]`, it maps more closely to this method than it does to `specialize`. So it feels like this method should maybe be the one named `specialize`. Or at least something like `specialize_with_defaults`, rather than `specialize_partial`?

---

_Merged by @dcreager on 2025-05-08 23:01_

---

_Closed by @dcreager on 2025-05-08 23:01_

---

_Branch deleted on 2025-05-08 23:01_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/generics.rs`:185 on 2025-05-08 23:05_

I don't think this comment accurately describes the below code anymore? We are just doing a single pass, not iterating to a fixed point.

---

_@carljm approved on 2025-05-08 23:08_

Thank you!

I think my main concern here is that this doesn't seem to address the issue that caused the problem with the first version of this PR: the fact that none of our specialization-handling distinguishes two logically entirely separate uses of the same legacy typevar. I would feel more comfortable if we either addressed that directly by actually using different objects for each use of a legacy typevar in a generic scope, or if we had somewhere a more rigorous breakdown of why that isn't necessary.

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:170 on 2025-05-09 00:18_

Ah, I was thinking of the inputs as partial, not the outputs. (The slice of types you pass in is only partially full.) Regardless, I like `specialize_with_defaults` better

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:185 on 2025-05-09 00:18_

Yes, good catch!

---

_@dcreager reviewed on 2025-05-09 00:19_

> I think my main concern here is that this doesn't seem to address the issue that caused the problem with the first version of this PR: the fact that none of our specialization-handling distinguishes two logically entirely separate uses of the same legacy typevar. I would feel more comfortable if we either addressed that directly by actually using different objects for each use of a legacy typevar in a generic scope, or if we had somewhere a more rigorous breakdown of why that isn't necessary.

I'll tackle this in a follow-on PR

---
