```yaml
number: 22292
title: "[ty] Try eliminating `~AlwaysFalsy` and `~AlwaysTruthy` from intersections"
type: pull_request
state: open
author: charliermarsh
labels:
  - ty
assignees: []
draft: true
base: main
head: charlie/truthy
created_at: 2025-12-29T22:52:18Z
updated_at: 2025-12-29T23:07:12Z
url: https://github.com/astral-sh/ruff/pull/22292
synced_at: 2026-01-10T16:36:19Z
```

# [ty] Try eliminating `~AlwaysFalsy` and `~AlwaysTruthy` from intersections

---

_Pull request opened by @charliermarsh on 2025-12-29 22:52_

## Summary

Following the instructions in https://github.com/astral-sh/ty/issues/2233, mostly to see the ecosystem impact.


---

_Label `ty` added by @charliermarsh on 2025-12-29 22:52_

---

_Comment by @astral-sh-bot[bot] on 2025-12-29 22:54_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-29 22:59_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyp (https://github.com/hauntsaninja/pyp)
+ pyp.py:429:34: error[unresolved-attribute] Object of type `stmt` has no attribute `body`
+ pyp.py:432:27: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
+ pyp.py:433:26: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
+ pyp.py:439:75: error[unresolved-attribute] Object of type `stmt` has no attribute `value`
- Found 5 diagnostics
+ Found 9 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `(@Todo & ~bytes) | int | list[bytes | memoryview[int] | str | ... omitted 5 union elements] | ... omitted 4 union elements`
+ aioredis/connection.py:441:16: error[invalid-return-type] Return type does not match returned value: expected `bytes | memoryview[int] | str | ... omitted 4 union elements`, found `int | list[bytes | memoryview[int] | str | ... omitted 5 union elements] | bytes | ... omitted 3 union elements`

parso (https://github.com/davidhalter/parso)
- parso/python/parser.py:120:50: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `@Todo | None`
+ parso/python/parser.py:120:50: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | None`
- parso/python/parser.py:121:26: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `@Todo | None`
+ parso/python/parser.py:121:26: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | None`
+ parso/python/tokenize.py:447:29: error[invalid-argument-type] Argument is incorrect: Expected `tuple[int, int]`, found `Unknown | None`
+ parso/python/tokenize.py:633:17: error[invalid-argument-type] Argument is incorrect: Expected `tuple[int, int]`, found `Unknown | None`
- Found 199 diagnostics
+ Found 201 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/contrib/extract_patches.py:411:47: error[not-iterable] Object of type `(int & ~AlwaysTruthy & ~AlwaysFalsy) | tuple[int, int, int, int]` may not be iterable
+ kornia/contrib/extract_patches.py:411:47: error[not-iterable] Object of type `int | tuple[int, int, int, int]` may not be iterable
- kornia/contrib/extract_patches.py:416:81: error[not-iterable] Object of type `(int & ~AlwaysTruthy & ~AlwaysFalsy) | tuple[int, int, int, int]` may not be iterable
+ kornia/contrib/extract_patches.py:416:81: error[not-iterable] Object of type `int | tuple[int, int, int, int]` may not be iterable

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ pytest_robotframework/_internal/pytest/robot_file_support.py:57:9: error[unresolved-attribute] Unresolved attribute `error` on type `object`.
+ pytest_robotframework/_internal/pytest/robot_file_support.py:57:118: warning[unused-ignore-comment] Unused `ty: ignore` directive
- Found 172 diagnostics
+ Found 174 diagnostics

pip (https://github.com/pypa/pip)
- src/pip/_internal/operations/check.py:99:37: error[no-matching-overload] No overload of function `sorted` matches arguments
- src/pip/_internal/operations/check.py:101:41: error[no-matching-overload] No overload of function `sorted` matches arguments
- src/pip/_internal/operations/prepare.py:609:12: warning[possibly-missing-attribute] Attribute `is_existing_dir` may be missing on object of type `(Unknown & ~AlwaysFalsy) | Link | None`
+ src/pip/_internal/operations/prepare.py:609:12: warning[possibly-missing-attribute] Attribute `is_existing_dir` may be missing on object of type `Unknown | Link | None`
- src/pip/_internal/operations/prepare.py:611:14: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `(Unknown & ~AlwaysFalsy) | Link | None`
+ src/pip/_internal/operations/prepare.py:611:14: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Unknown | Link | None`
- src/pip/_internal/operations/prepare.py:614:21: error[invalid-argument-type] Argument to function `unpack_url` is incorrect: Expected `Link`, found `(Unknown & ~AlwaysFalsy) | Link | None`
+ src/pip/_internal/operations/prepare.py:614:21: error[invalid-argument-type] Argument to function `unpack_url` is incorrect: Expected `Link`, found `Unknown | Link | None`
- src/pip/_internal/operations/prepare.py:627:42: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `(Unknown & ~AlwaysFalsy) | Link | None`
+ src/pip/_internal/operations/prepare.py:627:42: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Unknown | Link | None`
- src/pip/_internal/operations/prepare.py:637:54: error[invalid-argument-type] Argument to function `direct_url_from_link` is incorrect: Expected `Link`, found `(Unknown & ~AlwaysFalsy) | Link | None`
+ src/pip/_internal/operations/prepare.py:637:54: error[invalid-argument-type] Argument to function `direct_url_from_link` is incorrect: Expected `Link`, found `Unknown | Link | None`
+ src/pip/_vendor/cachecontrol/filewrapper.py:84:29: error[invalid-argument-type] Argument is incorrect: Expected `bytes`, found `Literal[b""] | memoryview[int]`
+ src/pip/_vendor/urllib3/packages/six.py:1070:17: error[unresolved-attribute] Object of type `MetaPathFinderProtocol` has no attribute `name`
- src/pip/_vendor/urllib3/util/retry.py:466:32: error[unsupported-operator] Operator `not in` is not supported between objects of type `Unknown` and `~AlwaysFalsy`
+ src/pip/_vendor/urllib3/util/retry.py:466:32: error[unsupported-operator] Operator `not in` is not supported between objects of type `Unknown` and `object`
- src/pip/_vendor/urllib3/util/ssl_.py:359:13: error[invalid-assignment] Object of type `str & ~AlwaysFalsy` is not assignable to attribute `keylog_filename` on type `(Unknown & <Protocol with members 'keylog_filename'>) | ssl.SSLContext | (pip._vendor.urllib3.util.ssl_.SSLContext & <Protocol with members 'keylog_filename'>)`
+ src/pip/_vendor/urllib3/util/ssl_.py:359:13: error[invalid-assignment] Object of type `str` is not assignable to attribute `keylog_filename` on type `(Unknown & <Protocol with members 'keylog_filename'>) | ssl.SSLContext | (pip._vendor.urllib3.util.ssl_.SSLContext & <Protocol with members 'keylog_filename'>)`

spack (https://github.com/spack/spack)
+ lib/spack/spack/audit.py:1341:86: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
+ lib/spack/spack/audit.py:1348:63: error[invalid-argument-type] Argument to function `sorted` is incorrect: Argument type `Spec` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- lib/spack/spack/cmd/create.py:1059:45: error[invalid-argument-type] Argument to bound method `get_repo` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (str & ~AlwaysFalsy) | Literal[True]`
+ lib/spack/spack/cmd/create.py:1059:45: error[invalid-argument-type] Argument to bound method `get_repo` is incorrect: Expected `str`, found `Unknown | str | Literal[True]`
- lib/spack/spack/cmd/info.py:492:20: error[invalid-return-type] Return type does not match returned value: expected `SupportsRichComparison`, found `tuple[bool & Any, Any & ~AlwaysFalsy]`
+ lib/spack/spack/cmd/info.py:492:20: error[invalid-return-type] Return type does not match returned value: expected `SupportsRichComparison`, found `tuple[Any, Any]`
- lib/spack/spack/cmd/pkg.py:195:40: error[invalid-argument-type] Argument to function `group_arguments` is incorrect: Expected `Sequence[str]`, found `Generator[str, None, None] & ~AlwaysFalsy`
+ lib/spack/spack/cmd/pkg.py:195:40: error[invalid-argument-type] Argument to function `group_arguments` is incorrect: Expected `Sequence[str]`, found `Generator[str, None, None]`
- lib/spack/spack/environment/environment.py:1317:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["include_concrete"]` and value of type `dict[str, dict[str, list[str]]] & ~AlwaysFalsy` on object of type `dict[str, list[str]]`
+ lib/spack/spack/environment/environment.py:1317:17: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["include_concrete"]` and value of type `dict[str, dict[str, list[str]]]` on object of type `dict[str, list[str]]`
- lib/spack/spack/environment/environment.py:1625:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[Spec], list[Spec], list[tuple[Spec, Spec]]]`, found `tuple[list[Unknown] & ~AlwaysFalsy, list[Unknown], list[tuple[Unknown, None] | Unknown]]`
+ lib/spack/spack/environment/environment.py:1625:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[Spec], list[Spec], list[tuple[Spec, Spec]]]`, found `tuple[list[Unknown], list[Unknown], list[tuple[Unknown, None] | Unknown]]`
- lib/spack/spack/externals.py:88:9: error[invalid-assignment] Object of type `Microarchitecture` is not assignable to attribute `target` on type `(Unknown & ~AlwaysTruthy) | None | (ArchSpec & ~AlwaysTruthy)`
+ lib/spack/spack/externals.py:88:9: error[invalid-assignment] Object of type `Microarchitecture` is not assignable to attribute `target` on type `Unknown | None | ArchSpec`
- lib/spack/spack/fetch_strategy.py:313:34: warning[division-by-zero] Cannot divide object of type `Literal[0]` by zero
- lib/spack/spack/fetch_strategy.py:1520:51: error[invalid-argument-type] Argument to function `quote` is incorrect: Expected `list[str]`, found `set[Unknown] & ~AlwaysFalsy`
+ lib/spack/spack/fetch_strategy.py:1520:51: error[invalid-argument-type] Argument to function `quote` is incorrect: Expected `list[str]`, found `set[Unknown]`
+ lib/spack/spack/llnl/util/lock.py:738:24: error[call-non-callable] Object of type `ContextManager[Unknown]` is not callable
- lib/spack/spack/llnl/util/tty/__init__.py:291:77: error[unresolved-attribute] Object of type `dict[str, Unknown] & ~AlwaysFalsy` has no attribute `iterkeys`
+ lib/spack/spack/llnl/util/tty/__init__.py:291:77: error[unresolved-attribute] Object of type `dict[str, Unknown]` has no attribute `iterkeys`
- lib/spack/spack/main.py:1023:13: error[invalid-assignment] Object of type `(ConfigFormatError & ~AlwaysFalsy) | (SpackEnvironmentConfigError & ~AlwaysFalsy)` is not assignable to attribute `_active_environment_error` of type `ConfigFormatError | None`
+ lib/spack/spack/main.py:1023:13: error[invalid-assignment] Object of type `ConfigFormatError | SpackEnvironmentConfigError` is not assignable to attribute `_active_environment_error` of type `ConfigFormatError | None`
- lib/spack/spack/package_base.py:2294:25: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str`, found `~AlwaysFalsy`
+ lib/spack/spack/package_base.py:2294:25: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str`, found `object`
+ lib/spack/spack/package_base.py:2298:25: error[not-subscriptable] Cannot subscript object of type `object` with no `__getitem__` method
- lib/spack/spack/solver/asp.py:3363:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `(Unknown & ~AlwaysFalsy) | (ConcreteVersion & ~AlwaysFalsy)` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
+ lib/spack/spack/solver/asp.py:3363:21: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[GitVersion | StandardVersion, list[Provenance]].__getitem__(key: GitVersion | StandardVersion, /) -> list[Provenance]` cannot be called with key of type `Unknown | ConcreteVersion` on object of type `dict[GitVersion | StandardVersion, list[Provenance]]`
- lib/spack/spack/solver/requirements.py:255:52: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (list[Unknown] & ~AlwaysFalsy)`
+ lib/spack/spack/solver/requirements.py:255:52: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `Unknown | list[Unknown]`
- lib/spack/spack/spec.py:3668:17: error[invalid-assignment] Object of type `Any & ~AlwaysFalsy` is not assignable to attribute `_patches_in_order_of_appearance` on type `Unknown | VariantValue`
+ lib/spack/spack/spec.py:3668:17: error[invalid-assignment] Object of type `Any` is not assignable to attribute `_patches_in_order_of_appearance` on type `Unknown | VariantValue`
- lib/spack/spack/spec_parser.py:223:32: warning[possibly-missing-attribute] Attribute `kind` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (Token & ~AlwaysFalsy) | (<class 'Token'> & ~AlwaysFalsy)`
+ lib/spack/spack/spec_parser.py:223:32: warning[possibly-missing-attribute] Attribute `kind` may be missing on object of type `Unknown | Token | <class 'Token'>`
- lib/spack/spack/spec_parser.py:234:36: warning[possibly-missing-attribute] Attribute `kind` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (Token & ~AlwaysFalsy) | (<class 'Token'> & ~AlwaysFalsy)`
+ lib/spack/spack/spec_parser.py:234:36: warning[possibly-missing-attribute] Attribute `kind` may be missing on object of type `Unknown | Token | <class 'Token'>`
- lib/spack/spack/spec_parser.py:735:32: warning[possibly-missing-attribute] Attribute `start` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (Token & ~AlwaysFalsy) | (<class 'Token'> & ~AlwaysFalsy)`
+ lib/spack/spack/spec_parser.py:735:32: warning[possibly-missing-attribute] Attribute `start` may be missing on object of type `Unknown | Token | <class 'Token'>`
- lib/spack/spack/spec_parser.py:735:60: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (Token & ~AlwaysFalsy) | (<class 'Token'> & ~AlwaysFalsy)`
+ lib/spack/spack/spec_parser.py:735:60: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | Token | <class 'Token'>`
- lib/spack/spack/util/s3.py:61:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[tuple[str, str], Any].__getitem__(key: tuple[str, str], /) -> Any` cannot be called with key of type `tuple[None | Unknown, Unknown | Literal["fetch"]]` on object of type `dict[tuple[str, str], Any]`
+ lib/spack/spack/util/s3.py:61:16: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[tuple[str, str], Any].__getitem__(key: tuple[str, str], /) -> Any` cannot be called with key of type `tuple[None | str | Unknown, Unknown | Literal["fetch"]]` on object of type `dict[tuple[str, str], Any]`
- lib/spack/spack/util/s3.py:75:5: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[None | Unknown, Unknown | Literal["fetch"]]` and value of type `Unknown` on object of type `dict[tuple[str, str], Any]`
+ lib/spack/spack/util/s3.py:75:5: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[None | str | Unknown, Unknown | Literal["fetch"]]` and value of type `Unknown` on object of type `dict[tuple[str, str], Any]`
+ lib/spack/spack/vendor/jinja2/compiler.py:1516:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Any] | Expr`
- lib/spack/spack/vendor/pyrsistent/_plist.py:22:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `rest` on type `(Unknown & ~AlwaysFalsy) | (_EmptyPList & ~AlwaysFalsy)`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:22:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `rest` on type `Unknown | _EmptyPList`
- lib/spack/spack/vendor/pyrsistent/_plist.py:103:34: error[unresolved-attribute] Object of type `Self@reverse & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:103:34: error[unresolved-attribute] Object of type `Self@reverse` has no attribute `first`
- lib/spack/spack/vendor/pyrsistent/_plist.py:104:20: error[unresolved-attribute] Object of type `Self@reverse & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:104:20: error[unresolved-attribute] Object of type `Self@reverse` has no attribute `rest`
- lib/spack/spack/vendor/pyrsistent/_plist.py:121:28: error[unresolved-attribute] Object of type `Self@split & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:121:28: error[unresolved-attribute] Object of type `Self@split` has no attribute `first`
- lib/spack/spack/vendor/pyrsistent/_plist.py:122:26: error[unresolved-attribute] Object of type `Self@split & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:122:26: error[unresolved-attribute] Object of type `Self@split` has no attribute `rest`
- lib/spack/spack/vendor/pyrsistent/_plist.py:134:19: error[unresolved-attribute] Object of type `Self@__iter__ & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:134:19: error[unresolved-attribute] Object of type `Self@__iter__` has no attribute `first`
- lib/spack/spack/vendor/pyrsistent/_plist.py:135:18: error[unresolved-attribute] Object of type `Self@__iter__ & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:135:18: error[unresolved-attribute] Object of type `Self@__iter__` has no attribute `rest`
- lib/spack/spack/vendor/pyrsistent/_plist.py:155:20: error[unresolved-attribute] Object of type `Self@__eq__ & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:155:20: error[unresolved-attribute] Object of type `Self@__eq__` has no attribute `first`
- lib/spack/spack/vendor/pyrsistent/_plist.py:157:25: error[unresolved-attribute] Object of type `Self@__eq__ & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:157:25: error[unresolved-attribute] Object of type `Self@__eq__` has no attribute `rest`
- lib/spack/spack/vendor/pyrsistent/_plist.py:213:16: error[unresolved-attribute] Object of type `Self@remove & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:213:16: error[unresolved-attribute] Object of type `Self@remove` has no attribute `first`
- lib/spack/spack/vendor/pyrsistent/_plist.py:214:45: error[unresolved-attribute] Object of type `Self@remove & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:214:45: error[unresolved-attribute] Object of type `Self@remove` has no attribute `rest`
- lib/spack/spack/vendor/pyrsistent/_plist.py:216:33: error[unresolved-attribute] Object of type `Self@remove & ~AlwaysFalsy` has no attribute `first`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:216:33: error[unresolved-attribute] Object of type `Self@remove` has no attribute `first`
- lib/spack/spack/vendor/pyrsistent/_plist.py:217:20: error[unresolved-attribute] Object of type `Self@remove & ~AlwaysFalsy` has no attribute `rest`
+ lib/spack/spack/vendor/pyrsistent/_plist.py:217:20: error[unresolved-attribute] Object of type `Self@remove` has no attribute `rest`
- lib/spack/spack/vendor/ruamel/yaml/constructor.py:1776:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["delta"]` and value of type `timedelta & ~AlwaysFalsy` on object of type `dict[str, int | None]`
+ lib/spack/spack/vendor/ruamel/yaml/constructor.py:1776:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["delta"]` and value of type `timedelta` on object of type `dict[str, int | None]`
+ lib/spack/spack/vendor/six.py:993:17: error[unresolved-attribute] Object of type `MetaPathFinderProtocol` has no attribute `name`
- lib/spack/spack/vendor/typing_extensions.py:111:49: error[unsupported-operator] Operator `-` is not supported between objects of type `~AlwaysFalsy | int` and `int`
+ lib/spack/spack/vendor/typing_extensions.py:111:49: error[unsupported-operator] Operator `-` is not supported between objects of type `object` and `int`
- lib/spack/spack/vendor/typing_extensions.py:113:42: error[unsupported-operator] Operator `>` is not supported between objects of type `int` and `~AlwaysFalsy | int`
+ lib/spack/spack/vendor/typing_extensions.py:113:42: error[unsupported-operator] Operator `>` is not supported between objects of type `int` and `object`
- Found 4292 diagnostics
+ Found 4297 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/mirror.py:376:47: error[unresolved-attribute] Object of type `dict[Unknown, Unknown] & ~AlwaysFalsy` has no attribute `name`
+ src/bandersnatch/mirror.py:376:47: error[unresolved-attribute] Object of type `dict[Unknown, Unknown]` has no attribute `name`
- src/bandersnatch/mirror.py:376:62: error[unresolved-attribute] Object of type `dict[Unknown, Unknown] & ~AlwaysFalsy` has no attribute `serial`
+ src/bandersnatch/mirror.py:376:62: error[unresolved-attribute] Object of type `dict[Unknown, Unknown]` has no attribute `serial`

stone (https://github.com/dropbox/stone)
- stone/frontend/lexer.py:66:17: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `(Unknown & ~AlwaysTruthy) | None | (list[Unknown] & ~AlwaysTruthy)`
+ stone/frontend/lexer.py:66:17: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `Unknown | None | list[Unknown]`
- stone/frontend/lexer.py:67:35: warning[possibly-missing-attribute] Attribute `pop` may be missing on object of type `(Unknown & ~AlwaysTruthy) | None | (list[Unknown] & ~AlwaysTruthy)`
+ stone/frontend/lexer.py:67:35: warning[possibly-missing-attribute] Attribute `pop` may be missing on object of type `Unknown | None | list[Unknown]`
- stone/frontend/lexer.py:74:25: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `(Unknown & ~AlwaysTruthy) | None | (list[Unknown] & ~AlwaysTruthy)`
+ stone/frontend/lexer.py:74:25: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `Unknown | None | list[Unknown]`
- stone/frontend/lexer.py:78:21: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `(Unknown & ~AlwaysTruthy) | None | (list[Unknown] & ~AlwaysTruthy)`
+ stone/frontend/lexer.py:78:21: warning[possibly-missing-attribute] Attribute `extend` may be missing on object of type `Unknown | None | list[Unknown]`
- stone/frontend/lexer.py:81:39: warning[possibly-missing-attribute] Attribute `pop` may be missing on object of type `(Unknown & ~AlwaysTruthy) | None | (list[Unknown] & ~AlwaysTruthy)`
+ stone/frontend/lexer.py:81:39: warning[possibly-missing-attribute] Attribute `pop` may be missing on object of type `Unknown | None | list[Unknown]`

beartype (https://github.com/beartype/beartype)
- beartype/_check/convert/_reduce/_pep/pep484/redpep484ref.py:178:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_decor/_nontype/decornontype.py:156:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_decor/_nontype/decornontype.py:215:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 492 diagnostics
+ Found 489 diagnostics

jinja (https://github.com/pallets/jinja)
+ src/jinja2/compiler.py:1534:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Any] | Expr`
- Found 180 diagnostics
+ Found 181 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- src/werkzeug/routing/rules.py:510:43: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Buffer]`, found `Mapping[str, Any] & ~AlwaysFalsy`
- src/werkzeug/sansio/response.py:620:13: error[invalid-assignment] Object of type `def on_update(value: WWWAuthenticate) -> None` is not assignable to attribute `_on_update` on type `WWWAuthenticate & ~AlwaysFalsy & ~Top[list[Unknown]]`
+ src/werkzeug/sansio/response.py:620:13: error[invalid-assignment] Object of type `def on_update(value: WWWAuthenticate) -> None` is not assignable to attribute `_on_update` on type `WWWAuthenticate & ~Top[list[Unknown]]`
- tests/test_datastructures.py:863:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal[42]` and `(Unknown & ~AlwaysFalsy) | (EnvironHeaders & ~AlwaysFalsy)`
+ tests/test_datastructures.py:863:16: error[unsupported-operator] Operator `not in` is not supported between objects of type `Literal[42]` and `Unknown | EnvironHeaders`
- Found 386 diagnostics
+ Found 385 diagnostics

black (https://github.com/psf/black)
- src/black/trans.py:466:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
- src/black/trans.py:980:20: error[invalid-return-type] Return type does not match returned value: expected `Ok[list[int]] | Err[CannotTransform]`, found `Ok[list[Unknown] & ~AlwaysFalsy]`
+ src/blib2to3/pgen2/parse.py:392:17: warning[possibly-missing-attribute] Attribute `append` may be missing on object of type `list[Node | Leaf] | None`
- Found 68 diagnostics
+ Found 67 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/execution/execute.py:1456:34: error[invalid-await] `Awaitable[bool] | bool` is not awaitable
- src/graphql/type/definition.py:1715:30: error[unresolved-attribute] Object of type `GraphQLType & ~AlwaysFalsy` has no attribute `of_type`
+ src/graphql/type/definition.py:1715:30: error[unresolved-attribute] Object of type `GraphQLType` has no attribute `of_type`
- src/graphql/type/schema.py:237:30: error[unresolved-attribute] Object of type `GraphQLNamedType & ~AlwaysFalsy` has no attribute `interfaces`
+ src/graphql/type/schema.py:237:30: error[unresolved-attribute] Object of type `GraphQLNamedType` has no attribute `interfaces`
- src/graphql/type/schema.py:246:59: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLInterfaceType`, found `GraphQLNamedType & ~AlwaysFalsy`
+ src/graphql/type/schema.py:246:59: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLInterfaceType`, found `GraphQLNamedType`
- src/graphql/type/schema.py:249:30: error[unresolved-attribute] Object of type `GraphQLNamedType & ~AlwaysFalsy` has no attribute `interfaces`
+ src/graphql/type/schema.py:249:30: error[unresolved-attribute] Object of type `GraphQLNamedType` has no attribute `interfaces`
- src/graphql/type/schema.py:258:56: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLObjectType`, found `GraphQLNamedType & ~AlwaysFalsy`
+ src/graphql/type/schema.py:258:56: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLObjectType`, found `GraphQLNamedType`
- src/graphql/type/validate.py:621:26: error[not-iterable] Object of type `(InputValueDefinitionNode & ~AlwaysTruthy & ~AlwaysFalsy) | (tuple[ConstDirectiveNode, ...] & ~AlwaysFalsy)` may not be iterable
+ src/graphql/type/validate.py:621:26: error[not-iterable] Object of type `(InputValueDefinitionNode & ~AlwaysTruthy) | tuple[ConstDirectiveNode, ...]` may not be iterable
- src/graphql/utilities/type_info.py:92:47: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | ... omitted 3 union elements`, found `GraphQLType & ~AlwaysFalsy`
+ src/graphql/utilities/type_info.py:92:47: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLScalarType | GraphQLEnumType | GraphQLInputObjectType | ... omitted 3 union elements`, found `GraphQLType`
- src/graphql/utilities/type_info.py:94:48: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | None`, found `GraphQLType & ~AlwaysFalsy`
+ src/graphql/utilities/type_info.py:94:48: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLObjectType | GraphQLInterfaceType | GraphQLUnionType | None`, found `GraphQLType`
- src/graphql/utilities/type_info.py:96:41: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 5 union elements`, found `GraphQLType & ~AlwaysFalsy`
+ src/graphql/utilities/type_info.py:96:41: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `GraphQLScalarType | GraphQLObjectType | GraphQLInterfaceType | ... omitted 5 union elements`, found `GraphQLType`
- src/graphql/utilities/value_from_ast.py:137:46: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode & ~AlwaysFalsy & ~VariableNode & ~NullValueNode`
+ src/graphql/utilities/value_from_ast.py:137:46: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode & ~VariableNode & ~NullValueNode`
- src/graphql/utilities/value_from_ast.py:137:58: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & ~AlwaysFalsy`
+ src/graphql/utilities/value_from_ast.py:137:58: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any]`
- src/graphql/utilities/value_from_ast.py:139:46: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode & ~AlwaysFalsy & ~VariableNode & ~NullValueNode`
+ src/graphql/utilities/value_from_ast.py:139:46: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `GraphQLScalarType`, found `ValueNode & ~VariableNode & ~NullValueNode`
- tests/language/test_parser.py:655:16: warning[possibly-missing-attribute] Attribute `kind` may be missing on object of type `(Location & ~AlwaysTruthy & ~AlwaysFalsy) | (Token & ~AlwaysFalsy)`
+ tests/language/test_parser.py:655:16: warning[possibly-missing-attribute] Attribute `kind` may be missing on object of type `(Location & ~AlwaysTruthy) | Token`
- tests/language/test_parser.py:656:16: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `(Location & ~AlwaysTruthy & ~AlwaysFalsy) | (Token & ~AlwaysFalsy)`
+ tests/language/test_parser.py:656:16: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `(Location & ~AlwaysTruthy) | Token`
- tests/pyutils/test_ref_map.py:35:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pyutils/test_ref_map.py:52:19: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/pyutils/test_ref_map.py:72:23: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 423 diagnostics
+ Found 421 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ boostedblob/boost.py:366:35: error[not-subscriptable] Cannot subscript object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` with no `__getitem__` method
- boostedblob/boost.py:368:16: error[unresolved-attribute] Object of type `Collection[Awaitable[T@OrderedMappingBoostable]] & ~AlwaysFalsy` has no attribute `popleft`
+ boostedblob/boost.py:368:16: error[unresolved-attribute] Object of type `Collection[Awaitable[T@OrderedMappingBoostable]]` has no attribute `popleft`
- Found 21 diagnostics
+ Found 22 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- testing/test_monkeypatch.py:171:25: error[invalid-argument-type] Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["y"], Literal[1700]]`, found `dict[str, object] & ~AlwaysTruthy`
- testing/test_monkeypatch.py:174:25: error[invalid-argument-type] Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["x"], Literal[1500]]`, found `dict[str, object] & ~AlwaysTruthy`
- Found 429 diagnostics
+ Found 427 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/aiokits/aiotime.py:20:25: error[invalid-argument-type] Argument to function `min` is incorrect: Argument type `~None | Unknown` does not satisfy upper bound `SupportsDunderLT[Any] | SupportsDunderGT[Any]` of type variable `SupportsRichComparisonT`
- Found 267 diagnostics
+ Found 268 diagnostics

paasta (https://github.com/yelp/paasta)
- paasta_tools/cli/cmds/get_image_version.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `None | @Todo`
+ paasta_tools/cli/cmds/get_image_version.py:132:12: error[invalid-return-type] Return type does not match returned value: expected `str`, found `None | Unknown`

scrapy (https://github.com/scrapy/scrapy)
- scrapy/pipelines/files.py:470:58: error[invalid-argument-type] Argument to bound method `_get_store` is incorrect: Expected `str`, found `(str & ~AlwaysFalsy) | (PathLike[str] & ~AlwaysFalsy)`
+ scrapy/pipelines/files.py:470:58: error[invalid-argument-type] Argument to bound method `_get_store` is incorrect: Expected `str`, found `str | PathLike[str]`

dulwich (https://github.com/dulwich/dulwich)
+ dulwich/dumb.py:522:30: error[invalid-argument-type] Argument to bound method `add` is incorrect: Expected `ObjectID`, found `Unknown | bytes`
- dulwich/object_store.py:2902:12: error[unsupported-operator] Operator `in` is not supported between objects of type `ObjectID` and `Unknown | ((() -> dict[ObjectID, ObjectID]) & ~AlwaysTruthy & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ dulwich/object_store.py:2902:12: error[unsupported-operator] Operator `in` is not supported between objects of type `ObjectID` and `Unknown | ((() -> dict[ObjectID, ObjectID]) & ~AlwaysTruthy & ~AlwaysFalsy) | (dict[ObjectID, ObjectID] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
- dulwich/pack.py:2247:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- dulwich/walk.py:377:17: warning[possibly-missing-attribute] Attribute `add` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (Sequence[bytes] & ~AlwaysTruthy & ~AlwaysFalsy) | (set[Unknown] & ~AlwaysFalsy)`
+ dulwich/walk.py:377:17: warning[possibly-missing-attribute] Attribute `add` may be missing on object of type `Unknown | (Sequence[bytes] & ~AlwaysTruthy & ~AlwaysFalsy) | (set[bytes] & ~AlwaysFalsy)`
- dulwich/walk.py:378:17: warning[possibly-missing-attribute] Attribute `remove` may be missing on object of type `(Unknown & ~AlwaysFalsy) | (Sequence[bytes] & ~AlwaysTruthy & ~AlwaysFalsy) | (set[Unknown] & ~AlwaysFalsy)`
+ dulwich/walk.py:378:17: warning[possibly-missing-attribute] Attribute `remove` may be missing on object of type `Unknown | (Sequence[bytes] & ~AlwaysTruthy & ~AlwaysFalsy) | (set[bytes] & ~AlwaysFalsy)`

mkosi (https://github.com/systemd/mkosi)
- mkosi/__init__.py:1836:17: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion & ~AlwaysFalsy` and `Literal["256"]`
+ mkosi/__init__.py:1836:17: error[unsupported-operator] Operator `>=` is not supported between objects of type `GenericVersion` and `Literal["256"]`
- mkosi/config.py:1566:21: error[unsupported-operator] Operator `>` is not supported between objects of type `GenericVersion` and `str & ~AlwaysFalsy`
+ mkosi/config.py:1566:21: error[unsupported-operator] Operator `>` is not supported between objects of type `GenericVersion` and `str`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ examples/contrib/webscanner_helper/proxyauth_selenium.py:130:23: error[no-matching-overload] No overload of bound method `join` matches arguments
- mitmproxy/addons/cut.py:57:24: error[unresolved-attribute] Object of type `Flow & ~AlwaysFalsy` has no attribute `headers`
+ mitmproxy/addons/cut.py:57:24: error[unresolved-attribute] Object of type `Flow` has no attribute `headers`
- mitmproxy/contentviews/_view_mqtt.py:106:53: error[invalid-argument-type] Argument to function `bytes_to_escaped_str` is incorrect: Expected `bytes`, found `(Unknown & ~AlwaysFalsy) | (dict[Unknown, Unknown] & ~AlwaysFalsy)`
+ mitmproxy/contentviews/_view_mqtt.py:106:53: error[invalid-argument-type] Argument to function `bytes_to_escaped_str` is incorrect: Expected `bytes`, found `Unknown | dict[Unknown, Unknown]`
- mitmproxy/proxy/tunnel.py:195:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- mitmproxy/proxy/tunnel.py:199:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- test/mitmproxy/addons/test_proxyauth.py:107:20: warning[possibly-missing-attribute] Attribute `status_code` may be missing on object of type `(Response & ~AlwaysTruthy) | None`
+ test/mitmproxy/addons/test_proxyauth.py:107:20: warning[possibly-missing-attribute] Attribute `status_code` may be missing on object of type `Response | None`
- test/mitmproxy/addons/test_proxyauth.py:119:20: warning[possibly-missing-attribute] Attribute `status_code` may be missing on object of type `(Response & ~AlwaysTruthy) | None`
+ test/mitmproxy/addons/test_proxyauth.py:119:20: warning[possibly-missing-attribute] Attribute `status_code` may be missing on object of type `Response | None`
- test/mitmproxy/addons/test_proxyauth.py:230:20: warning[possibly-missing-attribute] Attribute `status_code` may be missing on object of type `(Response & ~AlwaysTruthy) | None`
+ test/mitmproxy/addons/test_proxyauth.py:230:20: warning[possibly-missing-attribute] Attribute `status_code` may be missing on object of type `Response | None`
- test/mitmproxy/addons/test_proxyauth.py:236:20: warning[possibly-missing-attribute] Attribute `status_code` may be missing on object of type `(Response & ~AlwaysTruthy) | None`
+ test/mitmproxy/addons/test_proxyauth.py:236:20: warning[possibly-missing-attribute] Attribute `status_code` may be missing on object of type `Response | None`
- test/mitmproxy/addons/test_serverplayback.py:359:16: warning[possibly-missing-attribute] Attribute `data` may be missing on object of type `(Response & ~AlwaysTruthy) | None`
+ test/mitmproxy/addons/test_serverplayback.py:359:16: warning[possibly-missing-attribute] Attribute `data` may be missing on object of type `Response | None`
- Found 2144 diagnostics
+ Found 2143 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/auth.py:387:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["verifier"]` and value of type `str & ~AlwaysFalsy` on object of type `dict[str, bytes]`
+ tornado/auth.py:387:13: error[invalid-assignment] Invalid subscript assignment with key of type `Literal["verifier"]` and value of type `str` on object of type `dict[str, bytes]`
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`

optuna (https://github.com/optuna/optuna)
- optuna/storages/_rdb/storage.py:463:17: error[invalid-assignment] Object of type `Unknown | Column[Unknown]` is not assignable to attribute `number` on type `FrozenTrial & ~AlwaysFalsy`
- optuna/storages/_rdb/storage.py:464:17: error[invalid-assignment] Object of type `Unknown | Column[Unknown]` is not assignable to attribute `datetime_start` on type `FrozenTrial & ~AlwaysFalsy`
+ optuna/storages/_rdb/storage.py:463:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `number` on type `FrozenTrial` with custom `__set__` method
+ optuna/storages/_rdb/storage.py:464:17: error[invalid-assignment] Invalid assignment to data descriptor attribute `datetime_start` on type `FrozenTrial` with custom `__set__` method

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/job_processor/contract_job.py:167:24: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`
+ dragonchain/job_processor/contract_job.py:167:24: warning[possibly-missing-attribute] Attribute `split` may be missing on object of type `Unknown | None`
- dragonchain/job_processor/contract_job.py:198:60: error[not-iterable] Object of type `Unknown | None | (Divergent & ~AlwaysFalsy)` may not be iterable
+ dragonchain/job_processor/contract_job.py:198:60: error[not-iterable] Object of type `Unknown | None` may not be iterable
- dragonchain/job_processor/contract_job.py:250:29: error[invalid-argument-type] Argument to bound method `pull_image` is incorrect: Expected `str`, found `Unknown | None | (Divergent & ~AlwaysFalsy)`
+ dragonchain/job_processor/contract_job.py:250:29: error[invalid-argument-type] Argument to bound method `pull_image` is incorrect: Expected `str`, found `Unknown | None`
- dragonchain/job_processor/contract_job.py:327:23: error[not-iterable] Object of type `Unknown | None | (Divergent & ~AlwaysFalsy)` may not be iterable
+ dragonchain/job_processor/contract_job.py:327:23: error[not-iterable] Object of type `Unknown | None` may not be iterable
- dragonchain/job_processor/contract_job.py:419:13: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`
+ dragonchain/job_processor/contract_job.py:419:13: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None`
- dragonchain/job_processor/contract_job_utest.py:431:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None | (Divergent & ~AlwaysFalsy)`
+ dragonchain/job_processor/contract_job_utest.py:431:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None`

pandera (https://github.com/pandera-dev/pandera)
- pandera/typing/common.py:236:35: error[invalid-argument-type] Argument to function `signature` is incorrect: Expected `(...) -> Any`, found `@Todo | (tuple[Any, ...] & ~AlwaysFalsy & ~AlwaysTruthy) | None`
+ pandera/typing/common.py:236:35: error[invalid-argument-type] Argument to function `signature` is incorrect: Expected `(...) -> Any`, found `@Todo | (tuple[Any, ...] & ~AlwaysFalsy) | None`

urllib3 (https://github.com/urllib3/urllib3)
- test/test_ssltransport.py:308:59: error[invalid-argument-type] Argument to function `select` is incorrect: Expected `Iterable[Never]`, found `list[Unknown | socket] & ~AlwaysFalsy`
- test/test_ssltransport.py:308:75: error[invalid-argument-type] Argument to function `select` is incorrect: Expected `Iterable[Never]`, found `list[Unknown | socket] & ~AlwaysFalsy`
- Found 266 diagnostics
+ Found 264 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/datasets/utils.py:268:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str | None, str | None]`, found `tuple[@Todo, @Todo(StarredExpression)]`
+ torchvision/datasets/utils.py:268:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str | None, str | None]`, found `tuple[str, @Todo(StarredExpression)]`

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
- pydantic/json_schema.py:2857:16: error[invalid-return-type] Return type does not match returned value: expected `PlainSerializerFunctionSerSchema | None`, found `(SimpleSerSchema & ~AlwaysFalsy) | (PlainSerializerFunctionSerSchema & ~AlwaysFalsy) | (WrapSerializerFunctionSerSchema & ~AlwaysFalsy) | ... omitted 5 union elements`
+ pydantic/json_schema.py:2857:16: error[invalid-return-type] Return type does not match returned value: expected `PlainSerializerFunctionSerSchema | None`, found `SimpleSerSchema | PlainSerializerFunctionSerSchema | WrapSerializerFunctionSerSchema | ... omitted 5 union elements`

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/freqai/data_drawer.py:383:17: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `@Todo` on object of type `_iLocIndexerFrame[DataFrame]`
+ freqtrade/freqai/data_drawer.py:383:17: error[invalid-assignment] Invalid subscript assignment with key of type `tuple[Literal[-1], int | slice[Any, Any, Any] | ndarray[tuple[int], dtype[numpy.bool[builtins.bool]]]]` and value of type `Any` on object of type `_iLocIndexerFrame[DataFrame]`
- freqtrade/freqtradebot.py:418:21: error[invalid-argument-type] Argument to bound method `update_trade_state` is incorrect: Expected `Trade`, found `LocalTrade & ~AlwaysFalsy`
+ freqtrade/freqtradebot.py:418:21: error[invalid-argument-type] Argument to bound method `update_trade_state` is incorrect: Expected `Trade`, found `LocalTrade`
+ freqtrade/strategy/informative_decorator.py:157:49: error[missing-argument] No argument provided for required parameter 1
+ freqtrade/strategy/informativ

... (truncated 1049 lines) ...
```

</details>


No memory usage changes detected ✅



---
