```yaml
number: 18920
title: "[ty] Fix false positives when subscripting an object inferred as having an `Intersection` type"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - bug
  - internal
  - ty
assignees: []
merged: true
base: main
head: alex/intersection-hotfix
created_at: 2025-06-24T17:51:41Z
updated_at: 2025-06-24T18:39:03Z
url: https://github.com/astral-sh/ruff/pull/18920
synced_at: 2026-01-12T15:56:27Z
```

# [ty] Fix false positives when subscripting an object inferred as having an `Intersection` type

---

_@AlexWaygood_

## Summary

This fixes a regression introduced in #18846. Following that PR, ty will now emit a false-positive diagnostic on code like this:

```py
class Foo: ...

class Bar:
    def __getitem__(self, key: str) -> int:
        return 42

def f(x: Foo):
    if isinstance(x, Bar):
        reveal_type(x["whatever"])  # error[non-subscriptable] "Cannot subscript object of type `Foo` with no `__getitem__` method"
```

But we should avoid emitting a diagnostic if the subscript operation would succeed for _any_ element of the intersection, even if it would fail for some other elements in the intersection.

Fixing this properly will probably involve some tricky refactoring, so for now this is just a hotfix that gets rid of the false-positive diagnostic. (I'm adding the "internal" label because the bug being fixed here hasn't appeared in any release of ty.)

## Test Plan

Added mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-06-24 17:51_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-24 17:51_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-24 17:51_

---

_Label `bug` added by @AlexWaygood on 2025-06-24 17:51_

---

_Label `internal` added by @AlexWaygood on 2025-06-24 17:51_

---

_Label `ty` added by @AlexWaygood on 2025-06-24 17:51_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8153 on 2025-06-24 17:55_

```suggestion
                // TODO: we can map over the intersection and fold the results back into an intersection,
                // but we need to ignore any errors, which means `infer_subscript_expression_types`
                // needs to return a `Result` rather than eagerly emitting diagnostics.
                todo_type!("Subscript expressions on intersections")
```

---

_@carljm approved on 2025-06-24 17:55_

Thank you!

---

_Comment by @github-actions[bot] on 2025-06-24 17:57_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- warning[possibly-unbound-attribute] parso/python/parser.py:120:50: Attribute `value` on type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-attribute] parso/python/parser.py:120:50: Attribute `value` on type `@Todo(Subscript expressions on intersections) | None` is possibly unbound
- warning[possibly-unbound-attribute] parso/python/parser.py:121:26: Attribute `value` on type `Unknown | None` is possibly unbound
+ warning[possibly-unbound-attribute] parso/python/parser.py:121:26: Attribute `value` on type `@Todo(Subscript expressions on intersections) | None` is possibly unbound

pyp (https://github.com/hauntsaninja/pyp)
- error[unresolved-attribute] pyp.py:429:34: Type `stmt` has no attribute `body`
- error[unresolved-attribute] pyp.py:432:27: Type `stmt` has no attribute `value`
- error[unresolved-attribute] pyp.py:433:26: Type `stmt` has no attribute `value`
- error[unresolved-attribute] pyp.py:439:75: Type `stmt` has no attribute `value`
- Found 13 diagnostics
+ Found 9 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ warning[unused-ignore-comment] tests/pyutils/test_ref_map.py:52:19: Unused blanket `type: ignore` directive
- Found 381 diagnostics
+ Found 382 diagnostics

kopf (https://github.com/nolar/kopf)
- error[non-subscriptable] kopf/_cogs/structs/dicts.py:63:26: Cannot subscript object of type `KubernetesModel` with no `__getitem__` method
- Found 132 diagnostics
+ Found 131 diagnostics

kornia (https://github.com/kornia/kornia)
- error[non-subscriptable] kornia/augmentation/_2d/geometric/affine.py:135:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/center_crop.py:94:26: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/center_crop.py:94:35: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/center_crop.py:126:24: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/center_crop.py:149:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/crop.py:246:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/resize.py:112:20: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/resized_crop.py:155:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/rotation.py:114:30: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/rotation.py:216:30: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/shear.py:112:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/geometric/translate.py:108:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:97:25: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:97:34: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:114:27: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:114:38: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:131:26: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:131:36: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:148:25: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:148:34: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:89:25: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:89:34: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:106:25: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:106:34: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:196:25: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:196:34: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:213:25: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:213:34: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:94:35: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:94:54: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:112:27: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:112:38: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_3d/geometric/affine.py:168:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_3d/geometric/center_crop.py:91:26: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_3d/geometric/center_crop.py:91:35: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_3d/geometric/center_crop.py:91:44: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/augmentation/_3d/geometric/rotation.py:131:32: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> ParamItem, (s: slice[Any, Any, Any], /) -> list[ParamItem]]` is not callable on object of type `list[ParamItem]`
- error[call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> ParamItem, (s: slice[Any, Any, Any], /) -> list[ParamItem]]` is not callable on object of type `list[ParamItem]`
+ error[invalid-argument-type] kornia/contrib/models/tiny_vit.py:307:21: Argument to bound method `__init__` is incorrect: Expected `int | float`, found `@Todo(Subscript expressions on intersections) | (int & ~list[Unknown]) | (float & ~list[Unknown]) | (list[int | float] & ~list[Unknown])`
- error[non-subscriptable] kornia/contrib/models/tiny_vit.py:129:62: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/contrib/models/tiny_vit.py:129:62: Cannot subscript object of type `float` with no `__getitem__` method
- error[invalid-argument-type] kornia/contrib/models/tiny_vit.py:307:21: Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | (int & Unknown) | (float & Unknown) | (int & ~list[Unknown]) | (float & ~list[Unknown]) | (list[int | float] & ~list[Unknown])`
- error[non-subscriptable] kornia/contrib/models/tiny_vit.py:307:21: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/contrib/models/tiny_vit.py:307:21: Cannot subscript object of type `float` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/adjust.py:53:18: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/adjust.py:111:18: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/adjust.py:368:18: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/adjust.py:423:18: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/adjust.py:496:18: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/adjust.py:549:18: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/adjust.py:875:57: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/adjust.py:875:57: Cannot subscript object of type `float` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/normalize.py:261:12: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/normalize.py:261:12: Cannot subscript object of type `float` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/normalize.py:262:11: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/enhance/normalize.py:262:11: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] kornia/filters/kernels.py:685:24: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels.py:685:43: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[non-subscriptable] kornia/filters/kernels_geometry.py:73:17: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/kernels_geometry.py:73:17: Cannot subscript object of type `float` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/kernels_geometry.py:81:21: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/kernels_geometry.py:81:21: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[non-subscriptable] kornia/filters/kernels_geometry.py:180:21: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/kernels_geometry.py:180:21: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[non-subscriptable] kornia/filters/motion.py:129:27: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/motion.py:129:27: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/motion.py:129:37: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/motion.py:129:37: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/motion.py:129:47: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] kornia/filters/motion.py:129:47: Cannot subscript object of type `int` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]]` is not callable on object of type `tuple[int, int]`
- error[non-subscriptable] kornia/geometry/boxes.py:349:21: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]]` is not callable on object of type `tuple[int, int]`
- error[non-subscriptable] kornia/geometry/boxes.py:352:21: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]]` is not callable on object of type `tuple[int, int]`
- error[non-subscriptable] kornia/geometry/boxes.py:355:22: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]]` is not callable on object of type `tuple[int, int]`
- error[non-subscriptable] kornia/geometry/boxes.py:358:22: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/quaternion.py:156:43: Method `__getitem__` of type `bound method Quaternion.__getitem__(idx: int | slice[Any, Any, Any]) -> Quaternion` is not callable on object of type `Quaternion`
- Found 867 diagnostics
+ Found 782 diagnostics

stone (https://github.com/dropbox/stone)
- error[non-subscriptable] stone/frontend/ir_generator.py:329:35: Cannot subscript object of type `AstNamespace` with no `__getitem__` method
- error[non-subscriptable] stone/frontend/ir_generator.py:329:51: Cannot subscript object of type `AstNamespace` with no `__getitem__` method
- Found 147 diagnostics
+ Found 145 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[call-non-callable] strawberry/file_uploads/utils.py:24:37: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] strawberry/file_uploads/utils.py:29:17: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- Found 374 diagnostics
+ Found 372 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[invalid-argument-type] porcupine/plugins/editorconfig.py:152:49: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `def escape(pattern: AnyStr) -> AnyStr`
- Found 48 diagnostics
+ Found 47 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[non-subscriptable] bson/son.py:155:27: Cannot subscript object of type `<Protocol with members 'keys'>` with no `__getitem__` method
- Found 510 diagnostics
+ Found 509 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[non-subscriptable] ignite/handlers/time_profilers.py:407:27: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] ignite/handlers/time_profilers.py:407:27: Cannot subscript object of type `float` with no `__getitem__` method
- error[non-subscriptable] ignite/handlers/time_profilers.py:407:38: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] ignite/handlers/time_profilers.py:407:38: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:60:69: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:61:68: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:62:66: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:63:70: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:64:67: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:65:69: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:77:46: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:78:45: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:79:43: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:80:46: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:81:43: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:82:45: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:135:69: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:136:68: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:137:66: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:138:70: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:139:67: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/metrics/test_classification_report.py:140:69: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/test_utils.py:49:28: Method `__getitem__` of type `Overload[(index: int) -> Unknown, (index: slice[Any, Any, Any]) -> Sequence[Unknown]]` is not callable on object of type `Sequence[Unknown]`
- error[call-non-callable] tests/ignite/test_utils.py:49:28: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/test_utils.py:49:28: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> bytes]` is not callable on object of type `bytes`
- error[call-non-callable] tests/ignite/test_utils.py:50:28: Method `__getitem__` of type `Overload[(index: int) -> Unknown, (index: slice[Any, Any, Any]) -> Sequence[Unknown]]` is not callable on object of type `Sequence[Unknown]`
- error[call-non-callable] tests/ignite/test_utils.py:50:28: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] tests/ignite/test_utils.py:50:28: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> bytes]` is not callable on object of type `bytes`
- Found 2379 diagnostics
+ Found 2351 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[call-non-callable] cki_lib/misc.py:83:20: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] cki_lib/misc.py:83:69: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] cki_lib/misc.py:110:17: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] cki_lib/misc.py:117:20: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- Found 169 diagnostics
+ Found 165 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[non-subscriptable] pwndbg/chain.py:166:13: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] pwndbg/chain.py:175:13: Cannot subscript object of type `int` with no `__getitem__` method
- error[call-non-callable] pwndbg/commands/context.py:411:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> list[str], (s: slice[Any, Any, Any], /) -> list[list[str]]]` is not callable on object of type `list[list[str]]`
- Found 2318 diagnostics
+ Found 2315 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- error[call-non-callable] dragonchain/contract_invoker/contract_invoker_service.py:97:98: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] dragonchain/contract_invoker/contract_invoker_service.py:98:27: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] dragonchain/contract_invoker/contract_invoker_service.py:99:13: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- Found 321 diagnostics
+ Found 318 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[call-non-callable] src/poetry/console/commands/run.py:73:22: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- Found 952 diagnostics
+ Found 951 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[non-subscriptable] src/urllib3/_collections.py:363:31: Cannot subscript object of type `<Protocol with members 'keys'>` with no `__getitem__` method
- error[non-subscriptable] src/urllib3/_collections.py:363:31: Cannot subscript object of type `<Protocol with members '__getitem__'>` with no `__getitem__` method
- Found 512 diagnostics
+ Found 510 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[call-non-callable] src/werkzeug/sansio/response.py:608:50: Method `__getitem__` of type `bound method WWWAuthenticate.__getitem__(key: str) -> str | None` is not callable on object of type `WWWAuthenticate`
- error[call-non-callable] src/werkzeug/sansio/response.py:610:25: Method `__getitem__` of type `bound method WWWAuthenticate.__getitem__(key: str) -> str | None` is not callable on object of type `WWWAuthenticate`
- error[call-non-callable] src/werkzeug/test.py:361:43: Method `__getitem__` of type `bound method Authorization.__getitem__(name: str) -> str | None` is not callable on object of type `Authorization`
- error[call-non-callable] src/werkzeug/test.py:361:64: Method `__getitem__` of type `bound method Authorization.__getitem__(name: str) -> str | None` is not callable on object of type `Authorization`
- Found 430 diagnostics
+ Found 426 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[call-non-callable] cloudinit/distros/ug_util.py:229:13: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
- Found 728 diagnostics
+ Found 727 diagnostics

apprise (https://github.com/caronc/apprise)
- error[unresolved-attribute] test/test_apprise_attachments.py:98:12: Type `AttachBase` has no attribute `cache`
- error[unresolved-attribute] test/test_apprise_attachments.py:177:12: Type `AttachBase` has no attribute `cache`
- error[unresolved-attribute] test/test_apprise_attachments.py:178:12: Type `AttachBase` has no attribute `cache`
- error[unresolved-attribute] test/test_apprise_attachments.py:179:12: Type `AttachBase` has no attribute `cache`
- Found 4303 diagnostics
+ Found 4299 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- error[non-subscriptable] com/win32com/test/GenTestScripts.py:71:47: Cannot subscript object of type `OSError` with no `__getitem__` method
- error[non-subscriptable] com/win32com/test/GenTestScripts.py:77:47: Cannot subscript object of type `OSError` with no `__getitem__` method
- Found 2119 diagnostics
+ Found 2117 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- error[call-non-callable] schema_salad/cpp_codegen.py:616:16: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:618:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:623:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:625:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:630:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:635:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:640:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:645:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:647:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:649:18: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:654:29: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:655:30: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:655:54: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:655:54: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:659:28: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:659:28: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:662:29: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:662:29: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:663:49: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:663:49: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:669:32: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:669:56: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:669:56: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:673:29: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:673:29: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:681:32: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:681:56: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:681:56: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:685:30: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:685:30: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:693:32: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:693:56: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:693:56: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:697:25: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:697:25: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:701:21: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:701:21: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/cpp_codegen.py:705:20: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:706:60: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:707:34: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/cpp_codegen.py:709:49: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:411:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:411:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:415:38: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:415:38: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:424:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:424:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:428:38: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:428:38: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:444:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:444:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:446:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:446:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:452:54: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:452:54: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:453:45: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:453:45: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:455:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:455:44: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:461:73: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:461:73: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:465:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:465:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:470:46: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:470:46: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:479:59: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:479:59: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/dotnet_codegen.py:490:51: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/dotnet_codegen.py:490:51: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:433:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:433:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:437:38: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:437:38: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:453:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:453:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:457:38: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:457:38: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:474:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:474:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:476:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:476:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:481:71: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:481:71: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:484:54: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:484:54: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:485:45: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:485:45: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:500:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:500:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:505:46: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:505:46: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:514:53: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:514:53: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/java_codegen.py:568:51: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/java_codegen.py:568:51: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/jsonld_context.py:54:24: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/jsonld_context.py:55:29: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/jsonld_context.py:211:17: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[non-subscriptable] schema_salad/jsonld_context.py:211:17: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] schema_salad/jsonld_context.py:211:17: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] schema_salad/jsonld_context.py:211:30: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[non-subscriptable] schema_salad/jsonld_context.py:211:30: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] schema_salad/jsonld_context.py:211:30: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] schema_salad/jsonld_context.py:252:9: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[non-subscriptable] schema_salad/jsonld_context.py:252:9: Cannot subscript object of type `int` with no `__getitem__` method
- error[non-subscriptable] schema_salad/jsonld_context.py:252:9: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] schema_salad/makedoc.py:342:33: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/makedoc.py:343:33: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/makedoc.py:352:29: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/metaschema.py:224:74: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/metaschema.py:235:74: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/metaschema.py:996:41: Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` is not callable on object of type `MutableSequence[Any]`
- error[call-non-callable] schema_salad/metaschema.py:1000:23: Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` is not callable on object of type `MutableSequence[Any]`
- error[call-non-callable] schema_salad/metaschema.py:1020:29: Method `__getitem__` of type `Overload[(index: int) -> Any, (index: slice[Any, Any, Any]) -> MutableSequence[Any]]` is not callable on object of type `MutableSequence[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:387:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:387:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:391:38: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:391:38: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:398:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:398:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:402:38: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:402:38: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:404:36: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:404:36: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:419:48: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:419:48: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:423:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:423:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:424:28: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:424:28: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:427:27: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:427:27: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:437:40: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:437:40: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:440:72: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:440:72: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:442:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:442:44: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:448:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:448:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:454:40: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:454:40: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:456:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:456:44: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:464:16: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:464:16: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:469:46: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not callable on object of type `list[Any]`
- error[call-non-callable] schema_salad/python_codegen.py:469:46: Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` is not callable on object of type `str`
- error[call-non-callable] schema_salad/python_codegen.py:474:70: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Any, (s: slice[Any, Any, Any], /) -> list[Any]]` is not ...*[Comment body truncated]*

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:8153 on 2025-06-24 18:00_

"Ignore any errors" is something I might have to think about a bit... what about something like `Foo & Bar` here?

```py
class Foo:
    __getitem__ = None

class Bar:
    def __getitem__(self, key) -> int:
        return 42
```

`(Foo & Bar).__getitem__` simplifies to `Never`, I suppose, so if you attempt a subscript operation on an object of type `Foo & Bar`... I suppose _ideally_ we should emit a warning about unreachable code but avoid any errors about the implicit call to `__getitem__`?

We definitely need to avoid any errors to do with missing methods if any element has the method, though

---

_@AlexWaygood reviewed on 2025-06-24 18:00_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:8153 on 2025-06-24 18:36_

I added a slightly more qualified comment in https://github.com/astral-sh/ruff/pull/18920/commits/2af7148a40bce070ce466b981e5fb3886034ae8b

---

_@AlexWaygood reviewed on 2025-06-24 18:36_

---

_Merged by @AlexWaygood on 2025-06-24 18:39_

---

_Closed by @AlexWaygood on 2025-06-24 18:39_

---

_Branch deleted on 2025-06-24 18:39_

---
