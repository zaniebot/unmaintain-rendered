```yaml
number: 18846
title: "[ty] add support for mapped union and intersection subscript loads"
type: pull_request
state: merged
author: suneettipirneni
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: feature/mapped-union-intersection-loads
created_at: 2025-06-21T03:53:22Z
updated_at: 2025-06-23T16:38:02Z
url: https://github.com/astral-sh/ruff/pull/18846
synced_at: 2026-01-12T15:56:26Z
```

# [ty] add support for mapped union and intersection subscript loads

---

_@suneettipirneni_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Note this modifies the diagnostics a bit. Previously performing subscript access on something like `NotSubscriptable1 | NotSubscriptable2` would report the full type as not being subscriptable:

```
[non-subscriptable] "Cannot subscript object of type `NotSubscriptable1 | NotSubscriptable2` with no `__getitem__` method"
```

Now each erroneous constituent has a separate error:

```
[non-subscriptable] "Cannot subscript object of type `NotSubscriptable2` with no `__getitem__` method"
[non-subscriptable] "Cannot subscript object of type `NotSubscriptable1` with no `__getitem__` method"
```

Closes https://github.com/astral-sh/ty/issues/625

## Test Plan

 mdtest


---

_Comment by @github-actions[bot] on 2025-06-21 04:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- warning[possibly-unbound-attribute] parso/python/parser.py:120:50: Attribute `value` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
+ warning[possibly-unbound-attribute] parso/python/parser.py:120:50: Attribute `value` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] parso/python/parser.py:121:26: Attribute `value` on type `@Todo(map_with_boundness: intersections with negative contributions) | None` is possibly unbound
+ warning[possibly-unbound-attribute] parso/python/parser.py:121:26: Attribute `value` on type `Unknown | None` is possibly unbound

pyp (https://github.com/hauntsaninja/pyp)
+ error[unresolved-attribute] pyp.py:429:34: Type `stmt` has no attribute `body`
+ error[unresolved-attribute] pyp.py:432:27: Type `stmt` has no attribute `value`
+ error[unresolved-attribute] pyp.py:433:26: Type `stmt` has no attribute `value`
+ error[unresolved-attribute] pyp.py:439:75: Type `stmt` has no attribute `value`
- Found 9 diagnostics
+ Found 13 diagnostics

yarl (https://github.com/aio-libs/yarl)
+ error[index-out-of-bounds] yarl/_url.py:556:44: Index 1 is out of bounds for tuple `tuple[Unknown | tuple[str, str, str, str, str]]` with length 1
+ error[index-out-of-bounds] yarl/_url.py:558:19: Index 1 is out of bounds for tuple `tuple[Unknown | tuple[str, str, str, str, str]]` with length 1
- Found 158 diagnostics
+ Found 160 diagnostics

Expression (https://github.com/cognitedata/Expression)
- warning[possibly-unbound-implicit-call] tests/test_seq.py:15:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq.py:27:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq.py:38:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:16:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:37:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:57:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:77:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:89:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:102:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:118:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:138:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:143:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:157:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:171:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:183:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:195:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:207:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:224:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/test_seq_builder.py:241:6: Method `__getitem__` of type `Unknown | <class 'SeqBuilder'>` is possibly unbound
- Found 254 diagnostics
+ Found 235 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- warning[possibly-unbound-implicit-call] tests/execution/test_resolve.py:322:20: Method `__getitem__` of type `list[SourceLocation] | None` is possibly unbound
+ error[non-subscriptable] tests/execution/test_resolve.py:322:20: Cannot subscript object of type `None` with no `__getitem__` method
- warning[unused-ignore-comment] tests/pyutils/test_ref_map.py:52:19: Unused blanket `type: ignore` directive
- Found 386 diagnostics
+ Found 385 diagnostics

dulwich (https://github.com/dulwich/dulwich)
+ error[non-subscriptable] dulwich/config.py:119:21: Cannot subscript object of type `Iterable[Unknown]` with no `__getitem__` method
+ error[non-subscriptable] dulwich/config.py:119:38: Cannot subscript object of type `Iterable[Unknown]` with no `__getitem__` method
+ error[unresolved-attribute] dulwich/pack.py:2035:31: Type `int` has no attribute `type_num`
- Found 171 diagnostics
+ Found 174 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[unsupported-operator] comtypes/tools/codegenerator/comments.py:24:71: Operator `not in` is not supported for types `str` and `None`, in comparing `Literal["out"]` with `@Todo(Type::Intersection.call()) | str | list[str] | None`
- error[unsupported-operator] comtypes/tools/codegenerator/comments.py:25:72: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["out"]` with `@Todo(Type::Intersection.call()) | str | list[str] | None`
- Found 547 diagnostics
+ Found 545 diagnostics

black (https://github.com/psf/black)
- error[unsupported-operator] src/black/parsing.py:80:37: Operator `-` is unsupported between objects of type `Unknown | str | int` and `Literal[1]`
- error[invalid-argument-type] src/blib2to3/pgen2/parse.py:34:17: Argument to bound method `__init__` is incorrect: Expected `int`, found `Unknown | int | str | None | tuple[str, tuple[int, int]] | list[@Todo(Inference of subscript on special form)]`
- error[invalid-argument-type] src/blib2to3/pgen2/parse.py:34:31: Argument to bound method `__init__` is incorrect: Expected `list[@Todo(Inference of subscript on special form)]`, found `(Unknown & ~None) | int | str | tuple[str, tuple[int, int]] | list[@Todo(Inference of subscript on special form)]`
- Found 71 diagnostics
+ Found 68 diagnostics

kornia (https://github.com/kornia/kornia)
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/affine.py:135:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/affine.py:135:13: Cannot subscript object of type `None` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/center_crop.py:126:24: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/center_crop.py:149:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/center_crop.py:94:26: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/center_crop.py:94:35: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/center_crop.py:126:24: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/center_crop.py:149:13: Cannot subscript object of type `None` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/crop.py:246:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/crop.py:246:13: Cannot subscript object of type `None` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/resize.py:112:20: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/resized_crop.py:155:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/rotation.py:114:30: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/rotation.py:216:30: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/shear.py:112:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/resize.py:112:20: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/resized_crop.py:155:13: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/rotation.py:114:30: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/rotation.py:216:30: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/shear.py:112:13: Cannot subscript object of type `None` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/augmentation/_2d/geometric/translate.py:108:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] kornia/augmentation/_2d/geometric/translate.py:108:13: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:97:25: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:97:34: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:114:27: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:114:38: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:131:26: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:131:36: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:148:25: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/gaussian_illumination.py:148:34: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:89:25: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:89:34: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:106:25: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:106:34: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:196:25: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:196:34: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:213:25: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/linear_illumination.py:213:34: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:94:35: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:94:54: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:112:27: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:112:38: Cannot subscript object of type `int` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/augmentation/_3d/geometric/affine.py:168:13: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] kornia/augmentation/_3d/geometric/affine.py:168:13: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_3d/geometric/center_crop.py:91:26: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_3d/geometric/center_crop.py:91:35: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/_3d/geometric/center_crop.py:91:44: Cannot subscript object of type `int` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/augmentation/_3d/geometric/rotation.py:131:32: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] kornia/augmentation/_3d/geometric/rotation.py:131:32: Cannot subscript object of type `None` with no `__getitem__` method
+ error[call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> ParamItem, (s: slice[Any, Any, Any], /) -> list[ParamItem]]` is not callable on object of type `list[ParamItem]`
+ error[call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> ParamItem, (s: slice[Any, Any, Any], /) -> list[ParamItem]]` is not callable on object of type `list[ParamItem]`
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:146:35: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:147:35: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:148:35: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:149:56: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:149:75: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:150:56: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:150:75: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:151:56: Method `__getitem__` of type `Unknown | None` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/augmentation/random_generator/_3d/affine.py:151:75: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:146:35: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:147:35: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:148:35: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:149:56: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:149:75: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:150:56: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:150:75: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:151:56: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/augmentation/random_generator/_3d/affine.py:151:75: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] kornia/contrib/models/tiny_vit.py:129:62: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/contrib/models/tiny_vit.py:129:62: Cannot subscript object of type `float` with no `__getitem__` method
- error[invalid-argument-type] kornia/contrib/models/tiny_vit.py:307:21: Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | (int & ~list[Unknown]) | (float & ~list[Unknown]) | (list[int | float] & ~list[Unknown])`
+ error[invalid-argument-type] kornia/contrib/models/tiny_vit.py:307:21: Argument to bound method `__init__` is incorrect: Expected `int | float`, found `Unknown | (int & Unknown) | (float & Unknown) | (int & ~list[Unknown]) | (float & ~list[Unknown]) | (list[int | float] & ~list[Unknown])`
+ error[non-subscriptable] kornia/contrib/models/tiny_vit.py:307:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/contrib/models/tiny_vit.py:307:21: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/adjust.py:53:18: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/adjust.py:111:18: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/adjust.py:368:18: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/adjust.py:423:18: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/adjust.py:496:18: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/adjust.py:549:18: Cannot subscript object of type `int` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/enhance/adjust.py:875:57: Method `__getitem__` of type `int | float | Unknown` is possibly unbound
+ error[non-subscriptable] kornia/enhance/adjust.py:875:57: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/adjust.py:875:57: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/enhance/normalize.py:261:12: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
- warning[possibly-unbound-implicit-call] kornia/enhance/normalize.py:262:11: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ error[non-subscriptable] kornia/enhance/normalize.py:261:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/normalize.py:261:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/normalize.py:262:11: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/enhance/normalize.py:262:11: Cannot subscript object of type `float` with no `__getitem__` method
+ error[call-non-callable] kornia/filters/kernels.py:685:24: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:685:24: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:685:24: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:685:43: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:685:43: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:685:43: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:33: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:52: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels.py:744:71: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- warning[possibly-unbound-implicit-call] kornia/filters/kernels_geometry.py:73:17: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ error[non-subscriptable] kornia/filters/kernels_geometry.py:73:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/filters/kernels_geometry.py:73:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] kornia/filters/kernels_geometry.py:81:21: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ error[non-subscriptable] kornia/filters/kernels_geometry.py:81:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/filters/kernels_geometry.py:81:21: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- warning[possibly-unbound-implicit-call] kornia/filters/kernels_geometry.py:180:21: Method `__getitem__` of type `Unknown | int | float` is possibly unbound
+ error[non-subscriptable] kornia/filters/kernels_geometry.py:180:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/filters/kernels_geometry.py:180:21: Cannot subscript object of type `float` with no `__getitem__` method
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
- error[call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int | float, (key: slice[Any, Any, Any], /) -> tuple[int | float, ...]]` is not callable on object of type `tuple[int | float, int | float, int | float]`
+ error[non-subscriptable] kornia/filters/motion.py:129:27: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/filters/motion.py:129:27: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/filters/motion.py:129:37: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/filters/motion.py:129:37: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/filters/motion.py:129:47: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] kornia/filters/motion.py:129:47: Cannot subscript object of type `int` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]]` is not callable on object of type `tuple[int, int]`
+ error[non-subscriptable] kornia/geometry/boxes.py:349:21: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]]` is not callable on object of type `tuple[int, int]`
+ error[non-subscriptable] kornia/geometry/boxes.py:352:21: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]]` is not callable on object of type `tuple[int, int]`
+ error[non-subscriptable] kornia/geometry/boxes.py:355:22: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> tuple[int, ...]]` is not callable on object of type `tuple[int, int]`
+ error[non-subscriptable] kornia/geometry/boxes.py:358:22: Cannot subscript object of type `None` with no `__getitem__` method
- error[call-non-callable] kornia/geometry/quaternion.py:156:43: Method `__getitem__` of type `Unknown | (bound method Quaternion.__getitem__(idx: int | slice[Any, Any, Any]) -> Quaternion)` is not callable on object of type `Unknown | Quaternion`
+ error[call-non-callable] kornia/geometry/quaternion.py:156:43: Method `__getitem__` of type `bound method Quaternion.__getitem__(idx: int | slice[Any, Any, Any]) -> Quaternion` is not callable on object of type `Quaternion`
- Found 936 diagnostics
+ Found 1007 diagnostics

kopf (https://github.com/nolar/kopf)
+ error[non-subscriptable] kopf/_cogs/structs/dicts.py:63:26: Cannot subscript object of type `KubernetesModel` with no `__getitem__` method
- Found 131 diagnostics
+ Found 132 diagnostics

alerta (https://github.com/alerta/alerta)
- warning[possibly-unbound-implicit-call] alerta/plugins/escalate.py:47:34: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] alerta/plugins/escalate.py:47:34: Cannot subscript object of type `None` with no `__getitem__` method

porcupine (https://github.com/Akuli/porcupine)
+ error[invalid-argument-type] porcupine/plugins/editorconfig.py:152:49: Argument to function `__new__` is incorrect: Expected `(str, /) -> Unknown`, found `def escape(pattern: AnyStr) -> AnyStr`
- Found 49 diagnostics
+ Found 50 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ error[call-non-callable] strawberry/file_uploads/utils.py:24:37: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
+ error[call-non-callable] strawberry/file_uploads/utils.py:29:17: Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` is not callable on object of type `dict[str, Any]`
- Found 375 diagnostics
+ Found 377 diagnostics

ignite (https://github.com/pytorch/ignite)
+ error[non-subscriptable] ignite/handlers/time_profilers.py:407:27: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] ignite/handlers/time_profilers.py:407:27: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] ignite/handlers/time_profilers.py:407:38: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] ignite/handlers/time_profilers.py:407:38: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/conftest.py:125:34: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/conftest.py:125:34: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/conftest.py:125:34: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/test_launcher.py:281:44: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] tests/ignite/distributed/test_launcher.py:281:44: Cannot subscript object of type `None` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/test_launcher.py:282:42: Method `__getitem__` of type `Unknown | None` is possibly unbound
+ error[non-subscriptable] tests/ignite/distributed/test_launcher.py:282:42: Cannot subscript object of type `None` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/distributed/utils/__init__.py:457:24: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/distributed/utils/__init__.py:457:24: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:457:24: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/distributed/utils/__init__.py:459:24: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/distributed/utils/__init__.py:459:24: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:459:24: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/distributed/utils/__init__.py:482:24: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/distributed/utils/__init__.py:482:24: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:482:24: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/distributed/utils/__init__.py:484:24: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/distributed/utils/__init__.py:484:24: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/distributed/utils/__init__.py:484:24: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:191:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:193:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:243:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:245:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:191:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:191:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:193:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:193:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:243:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:243:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:245:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:245:12: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:381:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:382:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:381:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:381:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:382:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:382:12: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:431:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:432:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:431:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:431:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:432:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:432:12: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:481:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:482:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:481:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:481:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:482:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:482:12: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:531:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:532:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:531:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:531:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:532:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:532:12: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:581:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:582:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:581:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:581:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:582:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:582:12: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:631:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:632:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:631:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:631:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:632:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:632:12: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:712:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/handlers/test_time_profilers.py:713:12: Method `__getitem__` of type `str | int | float | tuple[str | int | float, str | int | float]` is possibly unbound
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:712:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:712:12: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:713:12: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/handlers/test_time_profilers.py:713:12: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:174:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:174:21: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:175:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_calinski_harabasz_score.py:175:21: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:174:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:174:21: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:175:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_davies_bouldin_score.py:175:21: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_silhouette_score.py:174:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_silhouette_score.py:174:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_silhouette_score.py:174:21: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/clustering/test_silhouette_score.py:175:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_silhouette_score.py:175:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/clustering/test_silhouette_score.py:175:21: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_canberra_metric.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_canberra_metric.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_canberra_metric.py:138:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_canberra_metric.py:138:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_canberra_metric.py:139:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_canberra_metric.py:139:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_absolute_error.py:135:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_absolute_error.py:136:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_fractional_absolute_error.py:135:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_fractional_absolute_error.py:135:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_fractional_absolute_error.py:136:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_fractional_absolute_error.py:136:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_bias.py:158:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_fractional_bias.py:159:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_fractional_bias.py:158:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_fractional_bias.py:158:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_fractional_bias.py:159:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_fractional_bias.py:159:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:140:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:139:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:139:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:140:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_geometric_mean_absolute_error.py:140:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:124:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:125:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:124:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:124:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:125:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_geometric_mean_relative_absolute_error.py:125:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_kendall_correlation.py:176:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_kendall_correlation.py:176:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_kendall_correlation.py:176:21: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_kendall_correlation.py:177:21: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_kendall_correlation.py:177:21: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_kendall_correlation.py:177:21: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_manhattan_distance.py:131:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_manhattan_distance.py:132:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_manhattan_distance.py:131:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_manhattan_distance.py:131:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_manhattan_distance.py:132:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_manhattan_distance.py:132:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_maximum_absolute_error.py:125:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_maximum_absolute_error.py:126:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_maximum_absolute_error.py:125:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_maximum_absolute_error.py:125:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_maximum_absolute_error.py:126:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_maximum_absolute_error.py:126:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:162:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:163:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:162:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:162:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:163:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_absolute_relative_error.py:163:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_error.py:116:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_error.py:117:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_error.py:116:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_error.py:116:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_error.py:117:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_error.py:117:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_normalized_bias.py:138:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_mean_normalized_bias.py:139:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_normalized_bias.py:138:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_normalized_bias.py:138:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_normalized_bias.py:139:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_mean_normalized_bias.py:139:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_absolute_error.py:145:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_absolute_error.py:146:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_median_absolute_error.py:145:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_median_absolute_error.py:145:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_median_absolute_error.py:146:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_median_absolute_error.py:146:17: Cannot subscript object of type `float` with no `__getitem__` method
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:165:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
- warning[possibly-unbound-implicit-call] tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:166:17: Method `__getitem__` of type `Unknown | int | float | list[int | float] | list[str] | list[Any]` is possibly unbound
+ error[non-subscriptable] tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:165:17: Cannot subscript object of type `int` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_median_absolute_percentage_error.py:165:17: Cannot subscript object of type `float` with no `__getitem__` method
+ error[non-subscriptable] tests/ignite/metrics/regression/test_m...*[Comment body truncated]*

---

_Label `ty` added by @ntBre on 2025-06-21 13:53_

---

_Marked ready for review by @suneettipirneni on 2025-06-21 20:58_

---

_Review requested from @carljm by @suneettipirneni on 2025-06-21 20:58_

---

_Review requested from @AlexWaygood by @suneettipirneni on 2025-06-21 20:58_

---

_Review requested from @sharkdp by @suneettipirneni on 2025-06-21 20:58_

---

_Review requested from @dcreager by @suneettipirneni on 2025-06-21 20:58_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-06-23 07:11_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:8153 on 2025-06-23 13:34_

You don't need the `collect` call here, because `UnionType::from_elements` can take in the iterator directly

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/infer.rs`:8171 on 2025-06-23 13:36_

The `clone` here isn't needed; we want to consume/replace the builder each time through. There's also a `fold` pattern that we've [used elsewhere](https://github.com/astral-sh/ty/issues/625) that will simplify this a bit:

```suggestion
                value_types
                    .iter()
                    .copied()
                    .map(|ty| self.infer_subscript_expression_types(value_node, ty, slice_ty))
                    .fold(
                        IntersectionBuilder::new(db),
                        IntersectionBuilder::add_positive,
                    )
                    .build()
```


---

_@dcreager reviewed on 2025-06-23 13:49_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/subscript/class.md`:67 on 2025-06-23 16:15_

This is wrong; the definition of `Spam` that lacks a `__class_getitem__` should not resolve to `<class 'Spam'>` but to `Unknown`.

```suggestion
    # revealed: str | Unknown
```

It looks like this is due to an old workaround that predates our support for generic types. We can just remove that workaround and fix this:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index 1580058159..b073460207 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -8475,19 +8475,7 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                     );
                 }

-                match value_ty {
-                    Type::ClassLiteral(_) => {
-                        // TODO: proper support for generic classes
-                        // For now, just infer `Sequence`, if we see something like `Sequence[str]`. This allows us
-                        // to look up attributes on generic base classes, even if we don't understand generics yet.
-                        // Note that this isn't handled by the clause up above for generic classes
-                        // that use legacy type variables and an explicit `Generic` base class.
-                        // Once we handle legacy typevars, this special case will be removed in
-                        // favor of the specialization logic above.
-                        value_ty
-                    }
-                    _ => Type::unknown(),
-                }
+                Type::unknown()
             }
         }
     }
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/subscript/class.md`:83 on 2025-06-23 16:21_

I don't think we need a TODO here (and I also think we should remove the TODO from the title of this section, as it's fixed by this PR.)

I think the concise error currently shown here (in the next line) is actually the one we want as the primary diagnostic message, thus a TODO here doesn't make sense.

To improve these diagnostics, what we could do is _also_ show the full union type as an additional "info" sub-diagnostic. (This is similar to how we handle diagnostics when calling a union of callables, some of which error.) I don't think that would be too hard (it would require passing a new argument to `infer_subscript_expression_types`, which is the full "outer" type, and then passing that along to the diagnostic reporting functions), but it should probably be in a separate PR.
```suggestion
```

---

_@carljm reviewed on 2025-06-23 16:22_

Thanks for this PR!

---

_Comment by @carljm on 2025-06-23 16:23_

All the changes mentioned here are pretty minor -- going to just make those changes and land this. Thank you!

---

_@carljm reviewed on 2025-06-23 16:28_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/infer.rs`:8153 on 2025-06-23 16:28_

This actually turns out to be a little harder than that, due to `self` being captured by the closure. But it's easily fixed by making `infer_subscript_expression_types` (and one other method that it calls) take `&self` instead of `&mut self` -- they don't actually need to store anything in the `TypeInferenceBuilder`.

---

_Comment by @suneettipirneni on 2025-06-23 16:29_

> All the changes mentioned here are pretty minor -- going to just make those changes and land this. Thank you!

Ok thanks

---

_Merged by @carljm on 2025-06-23 16:38_

---

_Closed by @carljm on 2025-06-23 16:38_

---
