```yaml
number: 21208
title: "[ty] don't union in default type for annotated parameters"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/nounionany
created_at: 2025-11-02T17:37:40Z
updated_at: 2025-11-02T23:21:56Z
url: https://github.com/astral-sh/ruff/pull/21208
synced_at: 2026-01-12T15:57:18Z
```

# [ty] don't union in default type for annotated parameters

---

_@carljm_

Fixes https://github.com/astral-sh/ty/issues/1013

## Summary

When a function parameter has both an annotation and a default value, we shouldn't union in the type of the default value to the inferred type of the parameter.

If the default value type is not assignable to the annotated type (e.g. `x: int = "foo"`), we emit a diagnostic about that, and ignore the default value type. This matches what we (and other type checkers) do for invalid assignments. The annotation is the explicit declaration of intent and we respect it. (This PR doesn't change anything here.)

If the default is assignable to the annotated type, and both are fully static types (e.g. `x: int = 1`), unioning in the default has no effect, so this PR again changes nothing.

If the annotated type is a static type and the default is not (e.g. `x: int = returns_any()`), it's definitely more useful to have `int` rather than `int | Any` as the inferred type for `x`. Fixing this case is the main point of this PR.

If the annotated type is not fully static (e.g. `x: Any = 1`), this is perhaps the most debatable scenario. A case could be made for continuing to infer `Any | Literal[1]` here, since `Literal[1]` is a definitely-known lower bound. But it's difficult to distinguish all the variants of this case from the previous two cases in a principled way, and a case can also be made that even here, the `Any` annotation is opting out of type checking, and we shouldn't necessarily try to be too smart. All other type checkers infer just `Any` here. So this PR chooses to do that also.

Of course, if there is no annotated type (e.g. `x = 1`) we continue to infer `Unknown | Literal[1]`.

## Test Plan

Adjusted mdtests.


---

_Label `ty` added by @carljm on 2025-11-02 17:37_

---

_Comment by @github-actions[bot] on 2025-11-02 17:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
python-chess (https://github.com/niklasf/python-chess)
+ chess/__init__.py:4119:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 23 diagnostics
+ Found 24 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/feature/loftr/loftr.py:105:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["d_model"]` on object of type `str`
- kornia/feature/loftr/loftr.py:105:13: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal["d_model"]` on object of type `tuple[int, int]`
- kornia/feature/loftr/loftr.py:105:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `Any | int | list[Unknown | int] | ... omitted 3 union elements`
- kornia/feature/loftr/loftr.py:105:13: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- kornia/feature/loftr/loftr.py:105:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Any | int | list[Unknown | int] | ... omitted 3 union elements`
- kornia/feature/loftr/loftr.py:105:55: error[invalid-argument-type] Method `__getitem__` of type `Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]` cannot be called with key of type `Literal["temp_bug_fix"]` on object of type `str`
- kornia/feature/loftr/loftr.py:105:55: error[invalid-argument-type] Method `__getitem__` of type `Overload[(index: SupportsIndex, /) -> int, (index: slice[Any, Any, Any], /) -> tuple[int, ...]]` cannot be called with key of type `Literal["temp_bug_fix"]` on object of type `tuple[int, int]`
- kornia/feature/loftr/loftr.py:105:55: error[non-subscriptable] Cannot subscript object of type `int` with no `__getitem__` method
- kornia/feature/loftr/loftr.py:107:53: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | ... omitted 4 union elements`
- kornia/feature/loftr/loftr.py:108:47: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | ... omitted 4 union elements`
- kornia/feature/loftr/loftr.py:110:51: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `Any | str | tuple[int, int] | ... omitted 4 union elements`
- Found 792 diagnostics
+ Found 781 diagnostics

beartype (https://github.com/beartype/beartype)
+ beartype/_check/metadata/hint/hintsmeta.py:632:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 516 diagnostics
+ Found 517 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- sockeye/beam_search.py:708:30: warning[possibly-missing-attribute] Attribute `max` may be missing on object of type `Unknown | None`
- Found 407 diagnostics
+ Found 406 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/AuthenticatedUser.py:720:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/AuthenticatedUser.py:747:73: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/AuthenticatedUser.py:753:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/AuthenticatedUser.py:780:73: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/AuthenticatedUser.py:786:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/AuthenticatedUser.py:836:31: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/AuthenticatedUser.py:838:32: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Branch.py:211:106: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Branch.py:222:34: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Branch.py:225:74: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Branch.py:378:106: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Branch.py:384:30: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Branch.py:387:70: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/CheckRun.py:253:45: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/CheckRun.py:255:47: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Gist.py:257:107: warning[possibly-missing-attribute] Attribute `items` may be missing on object of type `@Todo | _NotSetType`
- github/Issue.py:460:32: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/MainClass.py:548:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | (@Todo & datetime)`
- github/MainClass.py:640:27: error[no-matching-overload] No overload of bound method `join` matches arguments
- github/MainClass.py:910:42: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `_NotSetType | @Todo`
- github/Milestone.py:202:41: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | (@Todo & date)`
- github/NamedUser.py:444:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `_NotSetType | (@Todo & datetime)`
- github/Organization.py:916:88: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Organization.py:918:93: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Organization.py:1011:79: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Organization.py:1046:85: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Organization.py:1236:73: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/Organization.py:1238:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Organization.py:1440:40: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `@Todo | _NotSetType`
- github/Organization.py:1442:53: error[not-iterable] Object of type `@Todo | _NotSetType` may not be iterable
- github/PullRequest.py:562:30: warning[possibly-missing-attribute] Attribute `sha` may be missing on object of type `@Todo | _NotSetType`
- github/PullRequest.py:686:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
+ github/Repository.py:1712:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ github/Repository.py:1719:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ github/Repository.py:3220:103: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/Repository.py:1446:41: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:1448:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:1627:41: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:1644:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:1715:44: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:1798:45: warning[possibly-missing-attribute] Attribute `isoformat` may be missing on object of type `(@Todo & ~date) | (_NotSetType & ~date)`
- github/Repository.py:1856:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:2290:19: error[no-matching-overload] No overload of function `quote` matches arguments
- github/Repository.py:2435:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:2437:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:2777:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:2779:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:2862:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:2864:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:2913:40: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:2915:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:3217:43: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:3227:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:3232:45: warning[possibly-missing-attribute] Attribute `_identity` may be missing on object of type `(@Todo & ~str) | (_NotSetType & ~str)`
- github/Repository.py:3257:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:3500:39: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:3905:31: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:3907:32: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:4230:45: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- github/Repository.py:4232:47: warning[possibly-missing-attribute] Attribute `strftime` may be missing on object of type `@Todo | _NotSetType`
- Found 339 diagnostics
+ Found 285 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- tests/pyspark/test_pyspark_decorators.py:44:24: warning[possibly-missing-attribute] Attribute `columns` may be missing on object of type `Unknown | None`
- Found 1638 diagnostics
+ Found 1637 diagnostics

mypy (https://github.com/python/mypy)
- mypy/types.py:680:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TypeVarId`, found `object`
- mypy/types.py:681:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[Type]`, found `object`
- mypy/types.py:682:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Type`, found `object`
- mypy/types.py:683:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Type`, found `object`
- mypy/types.py:830:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TypeVarId`, found `object`
- mypy/types.py:833:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Type`, found `object`
- mypy/types.py:836:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Parameters | None`, found `object`
- mypy/types.py:1020:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `TypeVarId`, found `object`
- mypy/types.py:1021:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Type`, found `object`
- mypy/types.py:1023:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Type`, found `object`
- mypy/types.py:1026:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int`, found `object`
- mypy/types.py:1083:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Type] | None`, found `object`
- mypy/types.py:1323:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `AnyType | None`, found `object`
- mypy/types.py:1324:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `object`
- mypy/types.py:1815:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Type]`, found `object`
- mypy/types.py:1818:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `LiteralType | None`, found `object`
- mypy/types.py:1977:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Type]`, found `object`
- mypy/types.py:1978:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ArgKind]`, found `object`
- mypy/types.py:1979:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[str | None]`, found `object`
- mypy/types.py:1980:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- mypy/types.py:1983:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[TypeVarLikeType] | None`, found `object`
- mypy/types.py:1984:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- mypy/types.py:2261:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[Type]`, found `object`
- mypy/types.py:2262:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ArgKind]`, found `object`
- mypy/types.py:2263:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[str | None]`, found `object`
- mypy/types.py:2264:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Type`, found `object`
- mypy/types.py:2265:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Instance`, found `object`
- mypy/types.py:2266:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `object`
- mypy/types.py:2267:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SymbolNode | None`, found `object`
- mypy/types.py:2268:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Sequence[TypeVarLikeType] | None`, found `object`
- mypy/types.py:2271:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- mypy/types.py:2274:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- mypy/types.py:2275:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `object`
- mypy/types.py:2276:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- mypy/types.py:2277:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- mypy/types.py:2278:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Type | None`, found `object`
- mypy/types.py:2279:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Type | None`, found `object`
- mypy/types.py:2280:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- mypy/types.py:2283:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- mypy/types.py:2288:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `object`
- Found 1873 diagnostics
+ Found 1833 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/ref_resolver.py:78:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | None`
- schema_salad/ref_resolver.py:80:40: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- schema_salad/ref_resolver.py:81:18: warning[possibly-missing-attribute] Attribute `line` may be missing on object of type `Unknown | None`
- schema_salad/ref_resolver.py:81:33: warning[possibly-missing-attribute] Attribute `column` may be missing on object of type `Unknown | None`
- Found 192 diagnostics
+ Found 188 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/transforms/_functional_pil.py:317:30: error[invalid-argument-type] Argument to bound method `rotate` is incorrect: Expected `Resampling`, found `int | Unknown`
+ torchvision/transforms/_functional_pil.py:317:30: error[invalid-argument-type] Argument to bound method `rotate` is incorrect: Expected `Resampling`, found `int`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/torch/PyTorchModelTrainer.py:97:17: warning[possibly-missing-attribute] Attribute `log_scalar` may be missing on object of type `Unknown | None`
- freqtrade/freqai/torch/PyTorchModelTrainer.py:118:13: warning[possibly-missing-attribute] Attribute `log_scalar` may be missing on object of type `Unknown | None`
- Found 462 diagnostics
+ Found 460 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo | None | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo | dict[Unknown, Unknown]`
- xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo | None | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool | None`, found `@Todo | dict[Unknown, Unknown]`
- xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `Literal["coordinates", "all"] | bool | None`, found `@Todo | None | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `Literal["coordinates", "all"] | bool | None`, found `@Todo | dict[Unknown, Unknown]`
- xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo | None | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo | dict[Unknown, Unknown]`
- xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo | None | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `bool`, found `@Todo | dict[Unknown, Unknown]`
- xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `str | None`, found `@Todo | None | dict[Unknown, Unknown]`
+ xarray/backends/api.py:1634:27: error[invalid-argument-type] Argument to function `open_dataset` is incorrect: Expected `str | None`, found `@Todo | dict[Unknown, Unknown]`
- xarray/core/dataarray.py:451:21: error[invalid-assignment] Object of type `list[Unknown | (ReprObject & ~Top[Series[Any]] & ~DataFrame)]` is not assignable to `Sequence[Sequence[Unknown] | Index[Any] | DataArray | Variable | ndarray[@Todo, Unknown]] | Mapping[Unknown, Unknown] | None`
- Found 1736 diagnostics
+ Found 1735 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/domains/cpp/_ast.py:4566:13: warning[possibly-missing-attribute] Attribute `clone` may be missing on object of type `Unknown | None`
- sphinx/domains/cpp/_ast.py:4573:16: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | None`
- sphinx/domains/cpp/_ast.py:4579:16: warning[possibly-missing-attribute] Attribute `function_params` may be missing on object of type `Unknown | None`
- sphinx/domains/cpp/_ast.py:4587:20: warning[possibly-missing-attribute] Attribute `get_id` may be missing on object of type `Unknown | None`
- sphinx/domains/cpp/_ast.py:4625:20: warning[possibly-missing-attribute] Attribute `get_id` may be missing on object of type `Unknown | None`
- sphinx/domains/cpp/_ast.py:4678:22: warning[possibly-missing-attribute] Attribute `get_type_declaration_prefix` may be missing on object of type `Unknown | None`
- sphinx/domains/cpp/_ast.py:4711:9: warning[possibly-missing-attribute] Attribute `describe_signature` may be missing on object of type `Unknown | None`
- Found 526 diagnostics
+ Found 519 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/interpreter/interpreter.py:2259:16: error[invalid-return-type] Return type does not match returned value: expected `TestClass@make_test`, found `@Todo | Test`
- Found 1659 diagnostics
+ Found 1658 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
+ src/hydra_zen/third_party/pydantic.py:122:16: warning[redundant-cast] Value is already of type `_T@pydantic_parser`
- src/hydra_zen/wrapper/_implementations.py:946:16: error[invalid-return-type] Return type does not match returned value: expected `DataClass_ | type[@Todo] | ListConfig | DictConfig`, found `@Todo | (((...) -> Any) & Top[dict[Unknown, Unknown]]) | DataClass_ | ... omitted 6 union elements`

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/plugins/worksearch/code.py:301:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, Unknown | int] | tuple[str, @Todo | str]]`
+ openlibrary/plugins/worksearch/code.py:301:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, Unknown | int]]`

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/cli/deploy/_commands.py:290:17: error[invalid-argument-type] Argument is incorrect: Expected `ConcurrencyLimitStrategy`, found `str | (Unknown & ~None)`
+ src/prefect/cli/deploy/_commands.py:290:17: error[invalid-argument-type] Argument is incorrect: Expected `ConcurrencyLimitStrategy`, found `str`
- src/prefect/cli/flow_run.py:321:25: error[unsupported-operator] Operator `-` is unsupported between objects of type `(int & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy) | None` and `Literal[200]`
+ src/prefect/cli/flow_run.py:321:25: error[unsupported-operator] Operator `-` is unsupported between objects of type `(int & ~AlwaysFalsy) | None` and `Literal[200]`
- src/prefect/cli/flow_run.py:412:35: error[invalid-argument-type] Argument to bound method `execute_flow_run` is incorrect: Expected `UUID`, found `UUID | None | Unknown`
+ src/prefect/cli/flow_run.py:412:35: error[invalid-argument-type] Argument to bound method `execute_flow_run` is incorrect: Expected `UUID`, found `UUID | None`
- src/prefect/deployments/steps/core.py:175:29: error[no-matching-overload] No overload of function `print` matches arguments
- src/prefect/events/cli/automations.py:359:44: error[invalid-argument-type] Argument to bound method `delete_automation` is incorrect: Expected `UUID`, found `(str & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)`
+ src/prefect/events/cli/automations.py:359:44: error[invalid-argument-type] Argument to bound method `delete_automation` is incorrect: Expected `UUID`, found `str & ~AlwaysFalsy`
- src/prefect/server/api/admin.py:43:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `status_code` on type `Unknown | None`
+ src/prefect/server/api/admin.py:47:31: error[unresolved-attribute] Object of type `FromClause` has no attribute `delete`
- src/prefect/server/api/admin.py:47:31: warning[possibly-missing-attribute] Attribute `delete` may be missing on object of type `FromClause | Unknown`
- src/prefect/server/api/admin.py:64:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `status_code` on type `Unknown | None`
- src/prefect/server/api/admin.py:82:9: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `status_code` on type `Unknown | None`
- src/prefect/server/api/deployments.py:78:26: warning[possibly-missing-attribute] Attribute `model_dump` may be missing on object of type `UpdatedBy | None | Unknown`
+ src/prefect/server/api/deployments.py:78:26: warning[possibly-missing-attribute] Attribute `model_dump` may be missing on object of type `UpdatedBy | None`
- src/prefect/server/api/deployments.py:838:13: error[invalid-assignment] Object of type `int` is not assignable to attribute `status_code` on type `Unknown | None`
- src/prefect/server/api/flow_runs.py:122:13: error[invalid-assignment] Object of type `int` is not assignable to attribute `status_code` on type `Unknown | None`
- src/prefect/server/api/task_runs.py:158:8: error[unsupported-operator] Operator `<` is not supported for types `int` and `timedelta`, in comparing `int | (Unknown & ~float) | float` with `timedelta`
+ src/prefect/server/api/task_runs.py:158:8: error[unsupported-operator] Operator `<` is not supported for types `int` and `timedelta`, in comparing `int | float` with `timedelta`
- src/prefect/server/api/task_runs.py:338:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `status_code` on type `Unknown | None`
- src/prefect/server/api/task_runs.py:340:9: error[invalid-assignment] Object of type `int` is not assignable to attribute `status_code` on type `Unknown | None`
- src/prefect/server/schemas/states.py:334:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Scheduled`, found `@Todo | State`
- src/prefect/server/schemas/states.py:343:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Completed`, found `@Todo | State`
- src/prefect/server/schemas/states.py:352:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Running`, found `@Todo | State`
- src/prefect/server/schemas/states.py:361:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Failed`, found `@Todo | State`
- src/prefect/server/schemas/states.py:370:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Crashed`, found `@Todo | State`
- src/prefect/server/schemas/states.py:379:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Cancelling`, found `@Todo | State`
- src/prefect/server/schemas/states.py:388:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Cancelled`, found `@Todo | State`
- src/prefect/server/schemas/states.py:397:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Pending`, found `@Todo | State`
- src/prefect/server/schemas/states.py:431:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Paused`, found `@Todo | State`
- src/prefect/server/schemas/states.py:496:12: error[invalid-return-type] Return type does not match returned value: expected `_State@Retrying`, found `@Todo | State`
- Found 3319 diagnostics
+ Found 3301 diagnostics

altair (https://github.com/vega/altair)
- altair/utils/schemapi.py:1623:20: error[invalid-return-type] Return type does not match returned value: expected `SchemaBase`, found `@Todo | dict[str, Any]`
- altair/utils/schemapi.py:1626:20: error[invalid-return-type] Return type does not match returned value: expected `SchemaBase`, found `@Todo | dict[str, Any]`
- altair/utils/schemapi.py:1629:20: error[invalid-return-type] Return type does not match returned value: expected `SchemaBase`, found `@Todo | dict[str, Any]`
- altair/vegalite/v6/api.py:444:13: error[invalid-assignment] Object of type `str` is not assignable to attribute `name` on type `(@Todo & ~None) | UndefinedType`
- altair/vegalite/v6/api.py:569:38: warning[possibly-missing-attribute] Attribute `select` may be missing on object of type `(Unknown & ~VariableParameter) | (UndefinedType & ~VariableParameter)`
- altair/vegalite/v6/api.py:2498:13: error[invalid-assignment] Object of type `(@Todo & Top[list[Unknown]]) | (UndefinedType & Top[list[Unknown]])` is not assignable to `list[str] | LayerRepeatMapping | RepeatMapping`
- altair/vegalite/v6/api.py:2761:30: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `AggregatedFieldDef`
- altair/vegalite/v6/api.py:3094:34: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `JoinAggregateFieldDef`
- altair/vegalite/v6/api.py:3782:26: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Never`, found `WindowFieldDef`
- altair/vegalite/v6/api.py:3990:13: error[invalid-assignment] Object of type `Facet | (@Todo & ~str) | (UndefinedType & ~str)` is not assignable to `Facet | FacetMapping`
- altair/vegalite/v6/api.py:4358:44: error[invalid-argument-type] Argument to function `_repeat_names` is incorrect: Expected `list[str] | LayerRepeatMapping | RepeatMapping`, found `@Todo | UndefinedType`
- tests/vegalite/v6/test_params.py:105:12: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | UndefinedType`
- tests/vegalite/v6/test_params.py:160:18: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | UndefinedType`
- Found 1050 diagnostics
+ Found 1037 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/appsec/track_user_sdk.py:101:9: error[invalid-argument-type] Argument to function `set_user` is incorrect: Expected `str`, found `Any | None`
- tests/appsec/integrations/fastapi_tests/app.py:114:19: warning[possibly-missing-attribute] Attribute `decode` may be missing on object of type `str | Unknown`
+ tests/appsec/integrations/fastapi_tests/app.py:114:19: error[unresolved-attribute] Object of type `str` has no attribute `decode`
- Found 8152 diagnostics
+ Found 8151 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/core/has_props.py:812:27: error[call-non-callable] Object of type `IntrinsicType` is not callable
- src/bokeh/core/property/bases.py:198:35: error[invalid-argument-type] Argument to bound method `_copy_default` is incorrect: Expected `(() -> T@Property) | T@Property`, found `Unknown | IntrinsicType | UndefinedType`
+ src/bokeh/core/property/bases.py:198:35: error[invalid-argument-type] Argument to bound method `_copy_default` is incorrect: Expected `(() -> T@Property) | T@Property`, found `Unknown | UndefinedType`
- Found 613 diagnostics
+ Found 612 diagnostics

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/indexes/datetimes.py:513:20: warning[possibly-missing-attribute] Attribute `is_on_offset` may be missing on object of type `@Todo | Literal["S"]`
- pandas/core/indexes/datetimes.py:514:22: warning[possibly-missing-attribute] Attribute `rollback` may be missing on object of type `@Todo | Literal["S"]`
- pandas/core/indexes/datetimes.py:515:22: warning[possibly-missing-attribute] Attribute `rollforward` may be missing on object of type `@Todo | Literal["S"]`
+ pandas/core/indexes/multi.py:2946:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas/core/resample.py:2440:16: warning[possibly-missing-attribute] Attribute `rule_code` may be missing on object of type `@Todo | Literal["Min"]`
- pandas/io/json/_json.py:1029:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None | _NoDefault`
+ pandas/io/json/_json.py:1029:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None`
- pandas/io/json/_json.py:1029:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | list[str]`, found `Unknown | bool | None | _NoDefault`
+ pandas/io/json/_json.py:1029:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | list[str]`, found `Unknown | bool | None`
- pandas/io/json/_json.py:1029:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None | _NoDefault`
+ pandas/io/json/_json.py:1029:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None`
- pandas/io/json/_json.py:1029:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None | _NoDefault`
+ pandas/io/json/_json.py:1029:38: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None`
- pandas/io/json/_json.py:1033:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None | _NoDefault`
+ pandas/io/json/_json.py:1033:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None`
- pandas/io/json/_json.py:1033:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | list[str]`, found `Unknown | bool | None | _NoDefault`
+ pandas/io/json/_json.py:1033:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool | list[str]`, found `Unknown | bool | None`
- pandas/io/json/_json.py:1033:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None | _NoDefault`
+ pandas/io/json/_json.py:1033:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None`
- pandas/io/json/_json.py:1033:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None | _NoDefault`
+ pandas/io/json/_json.py:1033:39: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | bool | None`
- Found 3384 diagnostics
+ Found 3381 diagnostics

jax (https://github.com/google/jax)
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `int | Sequence[int] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `int | Sequence[int] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Iterable[str] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Iterable[str] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `int | Sequence[int] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `int | Sequence[int] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Iterable[str] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Iterable[str] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Device | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Device | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `str | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `str | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `dict[str, Any] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `dict[str, Any] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:356:43: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Iterable[str] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Iterable[str] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `int | Sequence[int] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `int | Sequence[int] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Iterable[str] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Iterable[str] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Device | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `Device | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `str | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `str | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `dict[str, Any] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `dict[str, Any] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `bool`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `int | Sequence[int] | None`, found `Any | UnspecifiedValue | int | ... omitted 4 union elements`
+ jax/_src/api.py:358:31: error[invalid-argument-type] Argument to function `make_jit` is incorrect: Expected `int | Sequence[int] | None`, found `Any | int | Sequence[int] | ... omitted 3 union elements`
- jax/_src/array.py:759:3: error[invalid-assignment] Object of type `Unknown | None | (Sharding & ~Format)` is not assignable to `Sharding | Format`
+ jax/_src/numpy/lax_numpy.py:959:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ jax/_src/numpy/lax_numpy.py:1044:27: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ jax/_src/numpy/lax_numpy.py:1051:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- jax/_src/pallas/mosaic/sc_core.py:92:9: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `MemorySpace`, found `Unknown | None`
- jax/_src/pallas/mosaic/sc_core.py:369:13: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `object`
- jax/_src/pallas/mosaic_gpu/core.py:285:13: error[invalid-argument-type] Argument expression after ** must be a mapping with `str` key type: Found `object`
- Found 2550 diagnostics
+ Found 2549 diagnostics

static-frame (https://github.com/static-frame/static-frame)
+ static_frame/core/container_util.py:1832:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:429:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:3494:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:3497:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:3499:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:7515:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/frame.py:8264:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/type_blocks.py:1881:72: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ static_frame/core/type_blocks.py:1883:70: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1991 diagnostics
+ Found 2000 diagnostics

zulip (https://github.com/zulip/zulip)
- zerver/actions/invites.py:294:23: warning[possibly-missing-attribute] Attribute `id` may be missing on object of type `NamedUserGroup | Unknown`
+ zerver/actions/invites.py:294:23: error[unresolved-attribute] Object of type `NamedUserGroup` has no attribute `id`
- zerver/lib/validator.py:250:34: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str | Unknown` on object of type `Top[dict[Unknown, Unknown]]`
+ zerver/lib/validator.py:250:34: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[dict[Unknown, Unknown]]`
- zerver/lib/validator.py:255:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str | Unknown` on object of type `Top[dict[Unknown, Unknown]]`
+ zerver/lib/validator.py:255:38: error[invalid-argument-type] Method `__getitem__` of type `bound method Top[dict[Unknown, Unknown]].__getitem__(key: Never, /) -> object` cannot be called with key of type `str` on object of type `Top[dict[Unknown, Unknown]]`

core (https://github.com/home-assistant/core)
- homeassistant/components/fujitsu_fglair/entity.py:38:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown` cannot be called with key of type `Unknown | None` on object of type `dict[str, Unknown]`
- homeassistant/components/fully_kiosk/config_flow.py:67:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `dict[str, str] | Any | None` may be missing
- homeassistant/components/fully_kiosk/config_flow.py:72:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `dict[str, str] | Any | None` may be missing
- Found 14274 diagnostics
+ Found 14271 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-11-02 17:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Marked ready for review by @carljm on 2025-11-02 17:52_

---

_Review requested from @AlexWaygood by @carljm on 2025-11-02 17:52_

---

_Review requested from @sharkdp by @carljm on 2025-11-02 17:52_

---

_Review requested from @dcreager by @carljm on 2025-11-02 17:52_

---

_@sharkdp approved on 2025-11-02 18:04_

---

_Comment by @codspeed-hq[bot] on 2025-11-02 18:17_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnounionany?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21208 will **improve performances by 5.29%**

<sub>Comparing <code>cjm/nounionany</code> (fbd5536) with <code>main</code> (566d1d6)</sub>



### Summary

`⚡ 1` improvement  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnounionany?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 804.8 ms | 764.3 ms | +5.29% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/cjm%2Fnounionany?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Merged by @carljm on 2025-11-02 23:21_

---

_Closed by @carljm on 2025-11-02 23:21_

---

_Branch deleted on 2025-11-02 23:21_

---
