```yaml
number: 20593
title: "[ty] Simplify `Any | (Any & T)` to `Any`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/dynamic-intersection-unions
created_at: 2025-09-26T13:10:53Z
updated_at: 2025-09-26T16:00:13Z
url: https://github.com/astral-sh/ruff/pull/20593
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Simplify `Any | (Any & T)` to `Any`

---

_Pull request opened by @AlexWaygood on 2025-09-26 13:10_

I think this may help reduce some of the fallout from https://github.com/astral-sh/ruff/pull/20368 by creating simpler types when we narrow unions

## Test plan

- Added mdtests
- Ran `QUICKCHECK_TESTS=1000000 cargo test --release -p ty_python_semantic -- --ignored types::property_tests::stable`

---

_Label `ty` added by @AlexWaygood on 2025-09-26 13:10_

---

_Comment by @github-actions[bot] on 2025-09-26 13:14_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-26 13:15_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- more_itertools/more.py:3486:31: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | None | (Unknown & ~None) | Literal[0]` with `int`
+ more_itertools/more.py:3486:31: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | None | Literal[0]` with `int`
- more_itertools/more.py:3486:43: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `int` with `Unknown | None | (Unknown & ~None) | int`
+ more_itertools/more.py:3486:43: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `int` with `Unknown | None | int`
- more_itertools/more.py:3491:27: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | None | (Unknown & ~None) | Literal[0]` with `int`
+ more_itertools/more.py:3491:27: error[unsupported-operator] Operator `<=` is not supported for types `None` and `int`, in comparing `Unknown | None | Literal[0]` with `int`
- more_itertools/more.py:3491:39: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `int` with `Unknown | None | (Unknown & ~None) | int`
+ more_itertools/more.py:3491:39: error[unsupported-operator] Operator `<=` is not supported for types `int` and `None`, in comparing `int` with `Unknown | None | int`

kornia (https://github.com/kornia/kornia)
- kornia/enhance/adjust.py:180:14: warning[possibly-missing-attribute] Attribute `to` on type `int | (Unknown & ~float) | Unknown` may be missing
+ kornia/enhance/adjust.py:180:14: warning[possibly-missing-attribute] Attribute `to` on type `int | Unknown` may be missing
- kornia/enhance/normalize.py:249:12: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:249:12: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:249:27: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:249:27: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:250:16: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:250:16: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:250:52: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:250:52: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:251:90: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:251:90: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:254:12: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:254:12: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:254:26: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:254:26: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:255:16: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:255:16: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:255:51: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:255:51: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/enhance/normalize.py:256:89: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/enhance/normalize.py:256:89: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:106:18: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:106:18: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:110:40: warning[possibly-missing-attribute] Attribute `device` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:110:40: warning[possibly-missing-attribute] Attribute `device` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:110:60: warning[possibly-missing-attribute] Attribute `dtype` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:110:60: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:115:43: warning[possibly-missing-attribute] Attribute `device` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:115:43: warning[possibly-missing-attribute] Attribute `device` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:115:63: warning[possibly-missing-attribute] Attribute `dtype` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:115:63: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:120:42: warning[possibly-missing-attribute] Attribute `pow` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:120:42: warning[possibly-missing-attribute] Attribute `pow` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:146:18: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:146:18: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:148:43: warning[possibly-missing-attribute] Attribute `device` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:148:43: warning[possibly-missing-attribute] Attribute `device` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:148:63: warning[possibly-missing-attribute] Attribute `dtype` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:148:63: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | int` may be missing
- kornia/filters/kernels.py:150:22: warning[possibly-missing-attribute] Attribute `abs` on type `(Unknown & ~float) | int | Unknown` may be missing
+ kornia/filters/kernels.py:150:22: warning[possibly-missing-attribute] Attribute `abs` on type `Unknown | int` may be missing

spack (https://github.com/spack/spack)
- lib/spack/spack/ci/gitlab.py:408:61: error[invalid-argument-type] Argument to function `ensure_expected_target_path` is incorrect: Expected `str`, found `Unknown | str | bytes | (Unknown & ~AlwaysFalsy)`
+ lib/spack/spack/ci/gitlab.py:408:61: error[invalid-argument-type] Argument to function `ensure_expected_target_path` is incorrect: Expected `str`, found `Unknown | str | bytes`
- lib/spack/spack/solver/requirements.py:207:52: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (list[Unknown | (Unknown & str)] & ~AlwaysFalsy)`
+ lib/spack/spack/solver/requirements.py:207:52: error[invalid-argument-type] Argument to function `parse_spec_from_yaml_string` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | (list[Unknown] & ~AlwaysFalsy)`
- lib/spack/spack/solver/requirements.py:223:25: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `Unknown | list[Unknown | (Unknown & str)] | None`
+ lib/spack/spack/solver/requirements.py:223:25: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `Unknown | list[Unknown] | None`
- lib/spack/spack/spec.py:1145:12: warning[possibly-missing-attribute] Attribute `replace` on type `Unknown | None | (Unknown & ~AlwaysFalsy)` may be missing
+ lib/spack/spack/spec.py:1145:12: warning[possibly-missing-attribute] Attribute `replace` on type `Unknown | None` may be missing
+ lib/spack/spack/vendor/ruamel/yaml/main.py:504:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 7515 diagnostics
+ Found 7516 diagnostics

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/variables.py:2577:13: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `(Unknown & ~AlwaysTruthy) | None | Unknown`
+ pylint/checkers/variables.py:2577:13: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Unknown | None`

PyGithub (https://github.com/PyGithub/PyGithub)
- github/MainClass.py:548:39: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType | (@Todo & datetime)` may be missing
+ github/MainClass.py:548:39: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType` may be missing
- github/Milestone.py:202:41: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType | (@Todo & date)` may be missing
+ github/Milestone.py:202:41: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType` may be missing
- github/NamedUser.py:444:39: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType | (@Todo & datetime)` may be missing
+ github/NamedUser.py:444:39: warning[possibly-missing-attribute] Attribute `strftime` on type `@Todo | _NotSetType` may be missing

alerta (https://github.com/alerta/alerta)
- alerta/models/group.py:34:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `(Any & ~AlwaysFalsy) | Any | None`
+ alerta/models/group.py:34:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Any | None`
- alerta/tasks.py:37:61: error[invalid-argument-type] Argument to function `process_status` is incorrect: Expected `str`, found `Unknown | (Unknown & ~AlwaysFalsy) | None`
+ alerta/tasks.py:37:61: error[invalid-argument-type] Argument to function `process_status` is incorrect: Expected `str`, found `Unknown | None`
- alerta/webhooks/slack.py:41:52: warning[possibly-missing-attribute] Attribute `capitalize` on type `Unknown | (Unknown & ~AlwaysFalsy) | None` may be missing
+ alerta/webhooks/slack.py:41:52: warning[possibly-missing-attribute] Attribute `capitalize` on type `Unknown | None` may be missing
- alerta/webhooks/telegram.py:52:61: warning[possibly-missing-attribute] Attribute `capitalize` on type `Unknown | (Unknown & ~AlwaysFalsy) | None` may be missing
+ alerta/webhooks/telegram.py:52:61: warning[possibly-missing-attribute] Attribute `capitalize` on type `Unknown | None` may be missing

cki-lib (https://gitlab.com/cki-project/cki-lib)
- tests/test_s3bucket.py:49:26: warning[possibly-missing-attribute] Attribute `signature_version` on type `Unknown | (Unknown & ~AlwaysFalsy) | None` may be missing
+ tests/test_s3bucket.py:49:26: warning[possibly-missing-attribute] Attribute `signature_version` on type `Unknown | None` may be missing
- tests/test_s3bucket.py:57:27: warning[possibly-missing-attribute] Attribute `signature_version` on type `Unknown | (Unknown & ~AlwaysFalsy) | None` may be missing
+ tests/test_s3bucket.py:57:27: warning[possibly-missing-attribute] Attribute `signature_version` on type `Unknown | None` may be missing
- tests/test_s3bucket.py:65:26: warning[possibly-missing-attribute] Attribute `signature_version` on type `Unknown | (Unknown & ~AlwaysFalsy) | None` may be missing
+ tests/test_s3bucket.py:65:26: warning[possibly-missing-attribute] Attribute `signature_version` on type `Unknown | None` may be missing

discord.py (https://github.com/Rapptz/discord.py)
- discord/audit_logs.py:550:56: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | (Unknown & ~None) | Literal[3, 4, 1, 5, -1]`
+ discord/audit_logs.py:550:56: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | Literal[3, 4, 1, 5, -1]`
- discord/audit_logs.py:551:55: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | (Unknown & ~None) | Literal[3, 4, 1, 5, -1]`
+ discord/audit_logs.py:551:55: error[invalid-argument-type] Argument to bound method `from_data` is incorrect: Expected `int`, found `None | Unknown | Literal[3, 4, 1, 5, -1]`
- discord/ext/commands/cog.py:543:17: error[invalid-assignment] Object of type `list[Unknown | (str & ~AlwaysFalsy) | (Any & ~AlwaysFalsy)]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`
+ discord/ext/commands/cog.py:543:17: error[invalid-assignment] Object of type `list[Unknown | (Any & ~AlwaysFalsy) | (str & ~AlwaysFalsy)]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT@listener & ~staticmethod) | ((...) -> Unknown)`

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- pymongo/asynchronous/encryption.py:196:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SSLContext | None`, found `(Unknown & ~None) | SSLContext | SSLContext | Unknown`
+ pymongo/asynchronous/encryption.py:196:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SSLContext | None`, found `Unknown | SSLContext | SSLContext`
- pymongo/synchronous/encryption.py:195:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SSLContext | None`, found `(Unknown & ~None) | SSLContext | SSLContext | Unknown`
+ pymongo/synchronous/encryption.py:195:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `SSLContext | None`, found `Unknown | SSLContext | SSLContext`

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/domains/cpp/_symbol.py:1183:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[Symbol] | None, str]`, found `tuple[list[Unknown | (Unknown & ~None)], None]`
+ sphinx/domains/cpp/_symbol.py:1183:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[list[Symbol] | None, str]`, found `tuple[list[Unknown], None]`

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/util.py:2016:26: error[not-iterable] Object of type `list[Unknown | (Unknown & str)] | list[Unknown] | None | list[Unknown | str]` may not be iterable
+ cloudinit/util.py:2016:26: error[not-iterable] Object of type `list[Unknown] | None | list[Unknown | str]` may not be iterable
- tests/unittests/helpers.py:421:5: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]) | (bound method dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None]].__setitem__(key: Unknown | str, value: Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None], /) -> None) | (bound method dict[Unknown | str, Unknown | bool | list[Unknown]].__setitem__(key: Unknown | str, value: Unknown | bool | list[Unknown], /) -> None)` cannot be called with a key of type `Literal["distro"]` and a value of type `Unknown` on object of type `(Unknown & ~None) | Unknown | list[Unknown | str] | str | list[Unknown] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None]] | dict[Unknown | str, Unknown | bool | list[Unknown]]`
+ tests/unittests/helpers.py:421:5: error[invalid-assignment] Method `__setitem__` of type `Unknown | (Overload[(key: SupportsIndex, value: Unknown | str, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown | str], /) -> None]) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None]) | (bound method dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None]].__setitem__(key: Unknown | str, value: Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None], /) -> None) | (bound method dict[Unknown | str, Unknown | bool | list[Unknown]].__setitem__(key: Unknown | str, value: Unknown | bool | list[Unknown], /) -> None)` cannot be called with a key of type `Literal["distro"]` and a value of type `Unknown` on object of type `Unknown | list[Unknown | str] | str | list[Unknown] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None]] | dict[Unknown | str, Unknown | bool | list[Unknown]]`
- tests/unittests/helpers.py:423:9: error[invalid-assignment] Method `__setitem__` of type `@Todo | (bound method dict[Unknown | str, Unknown | str].__setitem__(key: Unknown | str, value: Unknown | str, /) -> None) | (bound method dict[Unknown | str, Unknown | None].__setitem__(key: Unknown | str, value: Unknown | None, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["renderers"]` and a value of type `Unknown & ~AlwaysFalsy` on object of type `@Todo | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None] | bool | list[Unknown]`
+ tests/unittests/helpers.py:423:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[Unknown | str, Unknown | str].__setitem__(key: Unknown | str, value: Unknown | str, /) -> None) | (bound method dict[Unknown | str, Unknown | None].__setitem__(key: Unknown | str, value: Unknown | None, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["renderers"]` and a value of type `Unknown & ~AlwaysFalsy` on object of type `Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None] | bool | list[Unknown]`
- tests/unittests/helpers.py:425:9: error[invalid-assignment] Method `__setitem__` of type `@Todo | (bound method dict[Unknown | str, Unknown | str].__setitem__(key: Unknown | str, value: Unknown | str, /) -> None) | (bound method dict[Unknown | str, Unknown | None].__setitem__(key: Unknown | str, value: Unknown | None, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["activators"]` and a value of type `Unknown & ~AlwaysFalsy` on object of type `@Todo | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None] | bool | list[Unknown]`
+ tests/unittests/helpers.py:425:9: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method dict[Unknown | str, Unknown | str].__setitem__(key: Unknown | str, value: Unknown | str, /) -> None) | (bound method dict[Unknown | str, Unknown | None].__setitem__(key: Unknown | str, value: Unknown | None, /) -> None) | (Overload[(key: SupportsIndex, value: Unknown, /) -> None, (key: slice[Any, Any, Any], value: Iterable[Unknown], /) -> None])` cannot be called with a key of type `Literal["activators"]` and a value of type `Unknown & ~AlwaysFalsy` on object of type `Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None] | bool | list[Unknown]`
- tests/unittests/helpers.py:426:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[Unknown, Unknown]`, found `@Todo | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None] | bool | list[Unknown]`
+ tests/unittests/helpers.py:426:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[Unknown, Unknown]`, found `Unknown | dict[Unknown | str, Unknown | str] | str | dict[Unknown | str, Unknown | None] | bool | list[Unknown]`

xarray (https://github.com/pydata/xarray)
- xarray/core/variable.py:1457:27: warning[possibly-missing-attribute] Attribute `values` on type `(Unknown & ~str) | list[Unknown | (Unknown & str)]` may be missing
+ xarray/core/variable.py:1457:27: warning[possibly-missing-attribute] Attribute `values` on type `(Unknown & ~str) | list[Unknown]` may be missing
- xarray/plot/utils.py:1097:5: warning[possibly-missing-attribute] Attribute `create_dummy_axis` on type `(Unknown & ~str) | None | Unknown` may be missing
+ xarray/plot/utils.py:1097:5: warning[possibly-missing-attribute] Attribute `create_dummy_axis` on type `Unknown | None` may be missing
- xarray/plot/utils.py:1107:9: warning[possibly-missing-attribute] Attribute `axis` on type `(Unknown & ~str) | None | Unknown` may be missing
+ xarray/plot/utils.py:1107:9: warning[possibly-missing-attribute] Attribute `axis` on type `Unknown | None` may be missing
- xarray/plot/utils.py:1108:9: warning[possibly-missing-attribute] Attribute `axis` on type `(Unknown & ~str) | None | Unknown` may be missing
+ xarray/plot/utils.py:1108:9: warning[possibly-missing-attribute] Attribute `axis` on type `Unknown | None` may be missing

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/generation/coverage.py:938:23: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | (Unknown & ~None) | str` and `(Unknown & ~None) | str`
+ src/schemathesis/generation/coverage.py:938:23: error[unsupported-operator] Operator `-` is unsupported between objects of type `Unknown | str` and `(Unknown & ~None) | str`

pandera (https://github.com/pandera-dev/pandera)
- pandera/strategies/pandas_strategies.py:453:12: warning[possibly-missing-attribute] Attribute `filter` on type `(Unknown & ~None) | SearchStrategy[Unknown] | Unknown` may be missing
+ pandera/strategies/pandas_strategies.py:453:12: warning[possibly-missing-attribute] Attribute `filter` on type `Unknown | SearchStrategy[Unknown]` may be missing
- pandera/strategies/pandas_strategies.py:476:12: warning[possibly-missing-attribute] Attribute `filter` on type `(Unknown & ~None) | SearchStrategy[Unknown] | Unknown` may be missing
+ pandera/strategies/pandas_strategies.py:476:12: warning[possibly-missing-attribute] Attribute `filter` on type `Unknown | SearchStrategy[Unknown]` may be missing
- pandera/strategies/pandas_strategies.py:522:12: warning[possibly-missing-attribute] Attribute `filter` on type `(Unknown & ~None) | SearchStrategy[Unknown] | Unknown` may be missing
+ pandera/strategies/pandas_strategies.py:522:12: warning[possibly-missing-attribute] Attribute `filter` on type `Unknown | SearchStrategy[Unknown]` may be missing
- pandera/strategies/pandas_strategies.py:620:12: warning[possibly-missing-attribute] Attribute `filter` on type `(Unknown & ~None) | SearchStrategy[Unknown] | Unknown` may be missing
+ pandera/strategies/pandas_strategies.py:620:12: warning[possibly-missing-attribute] Attribute `filter` on type `Unknown | SearchStrategy[Unknown]` may be missing
- pandera/strategies/pandas_strategies.py:775:16: warning[possibly-missing-attribute] Attribute `filter` on type `Unknown | SearchStrategy[Unknown] | (Unknown & ~None)` may be missing
+ pandera/strategies/pandas_strategies.py:775:16: warning[possibly-missing-attribute] Attribute `filter` on type `Unknown | SearchStrategy[Unknown]` may be missing

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/experimental/pydantic/conversion.py:47:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~None) | Unknown | None`
+ strawberry/experimental/pydantic/conversion.py:47:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `Unknown | None`
- strawberry/experimental/pydantic/conversion.py:63:21: warning[possibly-missing-attribute] Attribute `_strawberry_type` on type `type[@Todo] | <class 'NoneType'>` may be missing
+ strawberry/experimental/pydantic/conversion.py:63:21: warning[possibly-missing-attribute] Attribute `_strawberry_type` on type `type[Unknown] | <class 'NoneType'>` may be missing

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/accounts/model.py:735:34: error[invalid-argument-type] Argument to function `post` is incorrect: Expected `str | bytes`, found `(Unknown & ~AlwaysFalsy) | Unknown | None`
+ openlibrary/accounts/model.py:735:34: error[invalid-argument-type] Argument to function `post` is incorrect: Expected `str | bytes`, found `Unknown | None`
- openlibrary/accounts/model.py:844:9: warning[possibly-missing-attribute] Attribute `get` on type `Unknown | bool | dict[Unknown | str, Unknown | (Unknown & ~AlwaysFalsy)]` may be missing
+ openlibrary/accounts/model.py:844:9: warning[possibly-missing-attribute] Attribute `get` on type `Unknown | bool | dict[Unknown | str, Unknown]` may be missing
- openlibrary/accounts/model.py:851:12: warning[possibly-missing-attribute] Attribute `get` on type `Unknown | bool | dict[Unknown | str, Unknown | (Unknown & ~AlwaysFalsy)]` may be missing
+ openlibrary/accounts/model.py:851:12: warning[possibly-missing-attribute] Attribute `get` on type `Unknown | bool | dict[Unknown | str, Unknown]` may be missing
- openlibrary/accounts/model.py:852:30: warning[possibly-missing-attribute] Attribute `get` on type `Unknown | bool | dict[Unknown | str, Unknown | (Unknown & ~AlwaysFalsy)]` may be missing
+ openlibrary/accounts/model.py:852:30: warning[possibly-missing-attribute] Attribute `get` on type `Unknown | bool | dict[Unknown | str, Unknown]` may be missing
- openlibrary/accounts/model.py:939:23: warning[possibly-missing-attribute] Attribute `pop` on type `Unknown | bool | dict[Unknown | str, Unknown | (Unknown & ~AlwaysFalsy)]` may be missing
+ openlibrary/accounts/model.py:939:23: warning[possibly-missing-attribute] Attribute `pop` on type `Unknown | bool | dict[Unknown | str, Unknown]` may be missing
- openlibrary/accounts/model.py:940:23: warning[possibly-missing-attribute] Attribute `pop` on type `Unknown | bool | dict[Unknown | str, Unknown | (Unknown & ~AlwaysFalsy)]` may be missing
+ openlibrary/accounts/model.py:940:23: warning[possibly-missing-attribute] Attribute `pop` on type `Unknown | bool | dict[Unknown | str, Unknown]` may be missing
- openlibrary/plugins/upstream/models.py:586:28: error[invalid-return-type] Return type does not match returned value: expected `list[Image]`, found `list[Unknown | (Edition & ~AlwaysTruthy & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)]`
+ openlibrary/plugins/upstream/models.py:586:28: error[invalid-return-type] Return type does not match returned value: expected `list[Image]`, found `list[Unknown | (Edition & ~AlwaysTruthy & ~AlwaysFalsy)]`
- openlibrary/plugins/worksearch/code.py:315:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, (Unknown & ~None) | Unknown | int] | tuple[str, Unknown | int] | tuple[str, @Todo | str] | tuple[str, Unknown]] | Unknown`
+ openlibrary/plugins/worksearch/code.py:315:59: error[invalid-argument-type] Argument to bound method `q_to_solr_params` is incorrect: Expected `list[tuple[str, str]]`, found `list[Unknown | tuple[str, Unknown | int] | tuple[str, @Todo | str] | tuple[str, Unknown]] | Unknown`

meson (https://github.com/mesonbuild/meson)
- mesonbuild/cmake/fileapi.py:218:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Path` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)]]`
+ mesonbuild/cmake/fileapi.py:218:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Path` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy)]]`
- mesonbuild/cmake/fileapi.py:218:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `bool` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)]]`
+ mesonbuild/cmake/fileapi.py:218:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `bool` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy)]]`
- mesonbuild/cmake/fileapi.py:223:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Path` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)]]`
+ mesonbuild/cmake/fileapi.py:223:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `Path` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy)]]`
- mesonbuild/cmake/fileapi.py:223:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `bool` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)]]`
+ mesonbuild/cmake/fileapi.py:223:17: error[unsupported-operator] Operator `+=` is unsupported between objects of type `bool` and `list[Unknown | dict[Unknown | str, Unknown | bool | (list[Unknown] & ~AlwaysFalsy)]]`
- mesonbuild/wrap/wrap.py:934:36: error[invalid-argument-type] Argument to function `Popen_safe` is incorrect: Expected `list[str]`, found `list[Unknown | (Unknown & ~AlwaysFalsy) | str] | list[Unknown | (Unknown & ~AlwaysFalsy) | str | bytes]`
+ mesonbuild/wrap/wrap.py:934:36: error[invalid-argument-type] Argument to function `Popen_safe` is incorrect: Expected `list[str]`, found `list[Unknown | str] | list[Unknown | str | bytes]`

apprise (https://github.com/caronc/apprise)
- apprise/plugins/discord.py:357:25: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown] | str | (Unknown & ~AlwaysFalsy)` may be missing
+ apprise/plugins/discord.py:357:25: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | dict[Unknown | str, Unknown] | str` may be missing
- apprise/plugins/lametric.py:768:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | int | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown | (Unknown & ~AlwaysFalsy)]]]` may be missing
+ apprise/plugins/lametric.py:768:13: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | int | dict[Unknown | str, Unknown | list[Unknown | dict[Unknown | str, Unknown]]]` may be missing
- apprise/plugins/notifiarr.py:331:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | (Unknown & ~AlwaysFalsy) | NotifyType | dict[Unknown | str, Unknown | bool | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int] | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown]]` may be missing
+ apprise/plugins/notifiarr.py:331:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | NotifyType | dict[Unknown | str, Unknown | bool | str] | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | int] | dict[Unknown | str, Unknown | str] | dict[Unknown | str, Unknown]]` may be missing
- apprise/plugins/pagerduty.py:384:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str | (Unknown & ~AlwaysFalsy)` may be missing
+ apprise/plugins/pagerduty.py:384:17: warning[possibly-missing-implicit-call] Method `__setitem__` of type `Unknown | str` may be missing
- apprise/plugins/sendpulse.py:562:17: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | dict[Unknown | str, Unknown] | list[Unknown] | (Unknown & ~AlwaysFalsy)` may be missing
+ apprise/plugins/sendpulse.py:562:17: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | dict[Unknown | str, Unknown] | list[Unknown]` may be missing
- apprise/plugins/sendpulse.py:573:17: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | dict[Unknown | str, Unknown] | list[Unknown] | (Unknown & ~AlwaysFalsy)` may be missing
+ apprise/plugins/sendpulse.py:573:17: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | dict[Unknown | str, Unknown] | list[Unknown]` may be missing
- apprise/plugins/sparkpost.py:578:21: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | bool | dict[Unknown | str, Unknown | (Unknown & ~AlwaysFalsy)] | str` may be missing
+ apprise/plugins/sparkpost.py:578:21: warning[possibly-missing-attribute] Attribute `append` on type `Unknown | bool | dict[Unknown | str, Unknown] | str` may be missing
- apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy)`
+ apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `None` and `Unknown | None | str | dict[Unknown, Unknown]`
- apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy)`
+ apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `str` and `Unknown | None | str | dict[Unknown, Unknown]`
- apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `dict[Unknown, Unknown]` and `Unknown | None | str | dict[Unknown, Unknown] | (Unknown & ~AlwaysFalsy)`
+ apprise/utils/parse.py:790:5: error[unsupported-operator] Operator `+=` is unsupported between objects of type `dict[Unknown, Unknown]` and `Unknown | None | str | dict[Unknown, Unknown]`

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/ghidra.py:106:45: error[invalid-argument-type] Argument to function `syntax_highlight` is incorrect: Expected `str`, found `(Unknown & ~AlwaysFalsy) | Unknown | str | None`
+ pwndbg/ghidra.py:106:45: error[invalid-argument-type] Argument to function `syntax_highlight` is incorrect: Expected `str`, found `Unknown | str | None`

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/_loss/loss.py:370:12: warning[possibly-missing-attribute] Attribute `ndim` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/_loss/loss.py:370:12: warning[possibly-missing-attribute] Attribute `ndim` on type `Unknown | None` may be missing
- sklearn/_loss/loss.py:370:38: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/_loss/loss.py:370:38: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | None` may be missing
- sklearn/_loss/loss.py:371:27: warning[possibly-missing-attribute] Attribute `squeeze` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/_loss/loss.py:371:27: warning[possibly-missing-attribute] Attribute `squeeze` on type `Unknown | None` may be missing
- sklearn/mixture/tests/test_gaussian_mixture.py:605:16: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/mixture/tests/test_gaussian_mixture.py:605:16: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None` may be missing
- sklearn/mixture/tests/test_gaussian_mixture.py:606:16: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/mixture/tests/test_gaussian_mixture.py:606:16: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None` may be missing
- sklearn/mixture/tests/test_gaussian_mixture.py:671:16: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/mixture/tests/test_gaussian_mixture.py:671:16: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None` may be missing
- sklearn/mixture/tests/test_gaussian_mixture.py:971:12: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/mixture/tests/test_gaussian_mixture.py:971:12: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None` may be missing
- sklearn/mixture/tests/test_gaussian_mixture.py:1289:12: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/mixture/tests/test_gaussian_mixture.py:1289:12: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | None` may be missing
- sklearn/mixture/tests/test_gaussian_mixture.py:1293:12: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/mixture/tests/test_gaussian_mixture.py:1293:12: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None` may be missing
- sklearn/mixture/tests/test_gaussian_mixture.py:1295:12: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None | (Unknown & ~None)` may be missing
+ sklearn/mixture/tests/test_gaussian_mixture.py:1295:12: warning[possibly-missing-attribute] Attribute `dtype` on type `Unknown | None` may be missing

prefect (https://github.com/PrefectHQ/prefect)
- src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:218:42: error[invalid-argument-type] Argument to function `create_async_engine` is incorrect: Expected `str | URL`, found `Unknown | (Unknown & ~AlwaysFalsy) | dict[Unknown, Unknown] | dict[str, Any]`
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:218:42: error[invalid-argument-type] Argument to function `create_async_engine` is incorrect: Expected `str | URL`, found `Unknown | dict[Unknown, Unknown] | dict[str, Any]`
- src/prefect/cli/deploy/_actions.py:193:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[str, Any], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[str, Any]], /) -> None]` cannot be called with a key of type `Literal["push"]` and a value of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str | (Unknown & ~AlwaysFalsy) | None]]]` on object of type `list[dict[str, Any]]`
+ src/prefect/cli/deploy/_actions.py:193:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[str, Any], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[str, Any]], /) -> None]` cannot be called with a key of type `Literal["push"]` and a value of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str | None]]]` on object of type `list[dict[str, Any]]`
- src/prefect/cli/deploy/_actions.py:201:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[str, Any], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[str, Any]], /) -> None]` cannot be called with a key of type `Literal["pull"]` and a value of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str | (Unknown & ~AlwaysFalsy) | None]]]` on object of type `list[dict[str, Any]]`
+ src/prefect/cli/deploy/_actions.py:201:9: error[invalid-assignment] Method `__setitem__` of type `Overload[(key: SupportsIndex, value: dict[str, Any], /) -> None, (key: slice[Any, Any, Any], value: Iterable[dict[str, Any]], /) -> None]` cannot be called with a key of type `Literal["pull"]` and a value of type `list[Unknown | dict[Unknown | str, Unknown | dict[Unknown | str, Unknown | str | None]]]` on object of type `list[dict[str, Any]]`
- src/prefect/cli/flow_run.py:183:36: error[invalid-argument-type] Argument is incorrect: Expected `FlowFilterName | None`, found `dict[Unknown | str, Unknown | (list[str] & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy)]`
+ src/prefect/cli/flow_run.py:183:36: error[invalid-argument-type] Argument is incorrect: Expected `FlowFilterName | None`, found `dict[Unknown | str, Unknown | (list[str] & ~AlwaysFalsy)]`
- src/prefect/cli/flow_run.py:321:25: error[unsupported-operator] Operator `-` is unsupported between objects of type `(int & ~AlwaysFalsy) | (Unknown & ~AlwaysFalsy) | Unknown | None` and `Unknown | Literal[200]`
+ src/prefect/cli/flow_run.py:321:25: error[unsupported-operator] Operator `-` is unsupported between objects of type `(int & ~AlwaysFalsy) | Unknown | None` and `Unknown | Literal[200]`
- src/prefect/server/orchestration/global_policy.py:460:13: warning[possibly-missing-attribute] Attribute `state_details` on type `Unknown | None | (State & @Todo)` may be missing
+ src/prefect/server/orchestration/global_policy.py:460:13: warning[possibly-missing-attribute] Attribute `state_details` on type `Unknown | (State & @Todo) | None` may be missing

altair (https://github.com/vega/altair)
- altair/jupyter/jupyter_chart.py:275:60: warning[possibly-missing-attribute] Attribute `type` on type `(Any & Top[dict[Unknown, Unknown]]) | (UndefinedType & Top[dict[Unknown, Unknown]]) | Any` may be missing
+ altair/jupyter/jupyter_chart.py:275:60: warning[possibly-missing-attribute] Attribute `type` on type `Any | (UndefinedType & Top[dict[Unknown, Unknown]])` may be missing

arviz (https://github.com/arviz-devs/arviz)
- arviz/plots/backends/bokeh/khatplot.py:96:52: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `Iterable[Unknown]`, found `int | float | Unknown`
- arviz/plots/lmplot.py:229:32: warning[possibly-missing-attribute] Attribute `sizes` on type `(Unknown & ~str) | None | Unknown` may be missing
+ arviz/plots/lmplot.py:229:32: warning[possibly-missing-attribute] Attribute `sizes` on type `Unknown | None` may be missing
- arviz/plots/lmplot.py:229:57: warning[possibly-missing-attribute] Attribute `sizes` on type `(Unknown & ~str) | None | Unknown` may be missing
+ arviz/plots/lmplot.py:229:57: warning[possibly-missing-attribute] Attribute `sizes` on type `Unknown | None` may be missing
- arviz/plots/tsplot.py:231:33: warning[possibly-missing-attribute] Attribute `coords` on type `(Unknown & ~str) | None | Unknown` may be missing
+ arviz/plots/tsplot.py:231:33: warning[possibly-missing-attribute] Attribute `coords` on type `Unknown | None` may be missing
- arviz/plots/tsplot.py:231:52: warning[possibly-missing-attribute] Attribute `dims` on type `(Unknown & ~str) | None | Unknown` may be missing
+ arviz/plots/tsplot.py:231:52: warning[possibly-missing-attribute] Attribute `dims` on type `Unknown | None` may be missing
- arviz/plots/tsplot.py:235:29: warning[possibly-missing-attribute] Attribute `coords` on type `(Unknown & ~str) | None | Unknown` may be missing
+ arviz/plots/tsplot.py:235:29: warning[possibly-missing-attribute] Attribute `coords` on type `Unknown | None` may be missing
- Found 732 diagnostics
+ Found 731 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/_vendor/typing_extensions.py:2911:46: error[unsupported-operator] Operator `>` is not supported for types `int` and `_Sentinel`, in comparing `int` with `(Unknown & ~AlwaysFalsy) | (_Sentinel & ~AlwaysFalsy) | int | Unknown`
+ setuptools/_vendor/typing_extensions.py:2911:46: error[unsupported-operator] Operator `>` is not supported for types `int` and `_Sentinel`, in comparing `int` with `Unknown | (_Sentinel & ~AlwaysFalsy) | int`

jax (https://github.com/google/jax)
- jax/_src/array.py:759:3: error[invalid-assignment] Object of type `Unknown | (Unknown & Sharding) | None | (Unknown & AUTO) | (Sharding & ~Format)` is not assignable to `Sharding | Format`
+ jax/_src/array.py:759:3: error[invalid-assignment] Object of type `Unknown | None | (Sharding & ~Format)` is not assignable to `Sharding | Format`
- jax/_src/checkify.py:374:61: warning[possibly-missing-attribute] Attribute `debug_info` on type `Any | (Any & ~ClosedJaxpr) | (tuple[@Todo, @Todo] & ~ClosedJaxpr)` may be missing
+ jax/_src/checkify.py:374:61: warning[possibly-missing-attribute] Attribute `debug_info` on type `Any | (tuple[@Todo, @Todo] & ~ClosedJaxpr)` may be missing

pywin32 (https://github.com/mhammond/pywin32)
- win32/Demos/win32cred_demo.py:73:13: error[invalid-argument-type] Argument to function `LoadUserProfile` is incorrect: Expected `PyPROFILEINFO`, found `dict[Unknown | str, Unknown | str | int | (Unknown & ~AlwaysFalsy) | None]`
+ win32/Demos/win32cred_demo.py:73:13: error[invalid-argument-type] Argument to function `LoadUserProfile` is incorrect: Expected `PyPROFILEINFO`, found `dict[Unknown | str, Unknown | str | int | None]`
- win32/Demos/win32netdemo.py:167:64: error[invalid-argument-type] Argument to function `NetLocalGroupAddMembers` is incorrect: Expected `tuple[@Todo, @Todo]`, found `list[Unknown | dict[Unknown | str, Unknown | (Unknown & ~None) | str]]`
+ win32/Demos/win32netdemo.py:167:64: error[invalid-argument-type] Argument to function `NetLocalGroupAddMembers` is incorrect: Expected `tuple[@Todo, @Todo]`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`

django-stubs (https://github.com/typeddjango/django-stubs)
- mypy_django_plugin/django/context.py:332:64: warning[possibly-missing-attribute] Attribute `base_field` on type `(Field[Any, Any] & Top[ArrayField[Unknown, Unknown]]) | (ForeignObjectRel & Top[ArrayField[Unknown, Unknown]]) | (Field[Any, Any] & ArrayField) | (ForeignObjectRel & ArrayField) | (AutoField[Unknown, Unknown] & Field[Unknown, Unknown] & Top[ArrayField[Unknown, Unknown]]) | (AutoField[Unknown, Unknown] & Field[Unknown, Unknown] & ArrayField)` may be missing
+ mypy_django_plugin/django/context.py:332:64: warning[possibly-missing-attribute] Attribute `base_field` on type `(Field[Any, Any] & Top[ArrayField[Unknown, Unknown]]) | (ForeignObjectRel & Top[ArrayField[Unknown, Unknown]]) | (Field[Any, Any] & ArrayField) | (ForeignObjectRel & ArrayField)` may be missing

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/elements/widgets/slider.py:805:19: error[unsupported-operator] Operator `-` is unsupported between objects of type `(Any & ~None) | Any | int | str | float | time | timedelta` and `(Any & ~None) | Any | int | str | float | time | timedelta`
+ lib/streamlit/elements/widgets/slider.py:805:19: error[unsupported-operator] Operator `-` is unsupported between objects of type `Any | int | str | float | time | timedelta` and `Any | int | str | float | time | timedelta`

ibis (https://github.com/ibis-project/ibis)
- ibis/backends/impala/__init__.py:1304:69: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `set[Unknown | str | (Unknown & ~None) | dict[Unknown, Unknown]]`
+ ibis/backends/impala/__init__.py:1304:69: error[invalid-argument-type] Argument expression after ** must be a mapping type: Found `set[Unknown | str | dict[Unknown, Unknown]]`

pandas (https://github.com/pandas-dev/pandas)
- pandas/core/arrays/boolean.py:242:24: warning[possibly-missing-attribute] Attribute `shape` on type `(Unknown & ndarray[object, object]) | Unknown | None` may be missing
+ pandas/core/arrays/boolean.py:242:24: warning[possibly-missing-attribute] Attribute `shape` on type `Unknown | None` may be missing
- pandas/core/arrays/boolean.py:245:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[@Todo, Unknown], ndarray[@Todo, Unknown]]`, found `tuple[(Unknown & ndarray[object, object] & ~BooleanArray) | @Todo | ndarray[tuple[int], <class 'bool'>], (Unknown & ndarray[object, object]) | Unknown | None]`
+ pandas/core/arrays/boolean.py:245:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[@Todo, Unknown], ndarray[@Todo, Unknown]]`, found `tuple[(Unknown & ndarray[object, object] & ~BooleanArray) | @Todo | ndarray[tuple[int], <class 'bool'>], Unknown | None]`
- pandas/core/arrays/datetimes.py:2917:29: error[invalid-argument-type] Argument to bound method `tz_localize` is incorrect: Expected `bool | Literal["raise", "NaT"]`, found `Unknown | (Unknown & ~str) | bool | None`
+ pandas/core/arrays/datetimes.py:2917:29: error[invalid-argument-type] Argument to bound method `tz_localize` is incorrect: Expected `bool | Literal["raise", "NaT"]`, found `Unknown | bool | None`
- pandas/core/arrays/interval.py:773:31: warning[possibly-missing-attribute] Attribute `closed` on type `(Unknown & Top[Interval[Unknown]]) | ExtensionArray | Unknown` may be missing
+ pandas/core/arrays/interval.py:773:31: warning[possibly-missing-attribute] Attribute `closed` on type `Unknown | ExtensionArray` may be missing
- pandas/core/generic.py:5411:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | None | (Unknown & ~None)]` is not assignable to `dict[Literal["index", "columns"], Any]`
+ pandas/core/generic.py:5411:9: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | None]` is not assignable to `dict[Literal["index", "columns"], Any]`
- pandas/core/indexes/base.py:1788:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | (Unknown & ~None) | list[Unknown | None] | None`
+ pandas/core/indexes/base.py:1788:16: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | list[Unknown | None] | None`
- pandas/core/indexes/base.py:1790:75: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | (Unknown & ~None) | list[Unknown | None] | None`
+ pandas/core/indexes/base.py:1790:75: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | list[Unknown | None] | None`
- pandas/core/indexes/base.py:1796:16: error[invalid-return-type] Return type does not match returned value: expected `list[Hashable]`, found `Unknown | (Unknown & ~None) | list[Unknown | None] | None`
+ pandas/core/indexes/base.py:1796:16: error[invalid-return-type] Return type does not match returned value: expected `list[Hashable]`, found `Unknown | list[Unknown | None] | None`
- pandas/core/indexes/base.py:1831:16: error[invalid-return-type] Return type does not match returned value: expected `list[Hashable]`, found `(Top[list[Unknown]] & ~AlwaysFalsy) | list[Hashable] | list[Unknown | None] | list[Unknown | (Unknown & ~None)]`
+ pandas/core/indexes/base.py:1831:16: error[invalid-return-type] Return type does not match returned value: expected `list[Hashable]`, found `(Top[list[Unknown]] & ~AlwaysFalsy) | list[Hashable] | list[Unknown | None] | list[Unknown]`
- pandas/core/indexes/interval.py:1268:46: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | None | Literal[1, "D"] | (Unknown & ~None)` and `float`
+ pandas/core/indexes/interval.py:1268:46: error[unsupported-operator] Operator `*` is unsupported between objects of type `Unknown | None | Literal[1, "D"]` and `float`
- pandas/core/internals/construction.py:254:29: error[invalid-argument-type] Argument to function `_ensure_2d` is incorrect: Expected `ndarray[@Todo, Unknown]`, found `Unknown | ExtensionArray | (Unknown & ndarray[@Todo, Unknown]) | ndarray[@Todo, Unknown]`
+ pandas/core/internals/construction.py:254:29: error[invalid-argument-type] Argument to function `_ensure_2d` is incorrect: Expected `ndarray[@Todo, Unknown]`, found `Unknown | ExtensionArray | ndarray[@Todo, Unknown]`
- pandas/plotting/_matplotlib/boxplot.py:338:27: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[Unknown | (Unknown & ~Top[list[Unknown]] & ~tuple[object, ...]) | None]`
+ pandas/plotting/_matplotlib/boxplot.py:338:27: error[invalid-argument-type] Argument to function `len` is incorrect: Expected `Sized`, found `Unknown | None | list[Unknown | None]`
- pandas/tests/reshape/merge/test_merge_index_as_string.py:94:33: error[not-iterable] Object of type `Unknown | None | (Unknown & ~None)` may not be iterable
+ pandas/tests/reshape/merge/test_merge_index_as_string.py:94:33: error[not-iterable] Object of type `Unknown | None` may not be iterable
- pandas/tests/reshape/merge/test_merge_index_as_string.py:97:44: error[unsupported-operator] Operator `not in` is not supported for types `@Todo` and `None`, in comparing `@Todo` with `Unknown | None | (Unknown & ~None)`
+ pandas/tests/reshape/merge/test_merge_index_as_string.py:97:44: error[unsupported-operator] Operator `not in` is not supported for types `@Todo` and `None`, in comparing `@Todo` with `Unknown | None`
- pandas/tests/reshape/merge/test_merge_index_as_string.py:101:46: error[unsupported-operator] Operator `not in` is not supported for types `@Todo` and `None`, in comparing `@Todo` with `Unknown | None | (Unknown & ~None)`
+ pandas/tests/reshape/merge/test_merge_index_as_string.py:101:46: error[unsupported-operator] Operator `not in` is not supported for types `@Todo` and `None`, in comparing `@Todo` with `Unknown | None`
- pandas/tests/reshape/merge/test_merge_index_as_string.py:106:45: error[unsupported-operator] Operator `in` is not supported for types `@Todo` and `None`, in comparing `@Todo` with `Unknown | None | (Unknown & ~None)`
+ pandas/tests/reshape/merge/test_merge_index_as_string.py:106:45: error[unsupported-operator] Operator `in` is not supported for types `@Todo` and `None`, in comparing `@Todo` with `Unknown | None`
- pandas/tests/reshape/merge/test_merge_index_as_string.py:110:47: error[unsupported-operator] Operator `in` is not supported for types `@Todo` and `None`, in comparing `@Todo` with `Unknown | None | (Unknown & ~None)`
+ pandas/tests/reshape/merge/test_merge_index_as_string.py:110:47: error[unsupported-operator] Operator `in` is not supported for types `@Todo` and `None`, in comparing `@Todo` with `Unknown | None`
- pandas/util/_decorators.py:199:41: warning[possibly-missing-attribute] Attribute `get` on type `(Mapping[Any, Any] & ~((...) -> object)) | (((Any, /) -> Any) & <Protocol with members 'get'> & ~((...) -> object)) | (Mapping[Any, Any] & ((...) -> object) & ~((...) -> object)) | (((Any, /) -> Any) & ((...) -> object) & ~((...) -> object))` may be missing
+ pandas/util/_decorators.py:199:41: warning[possibly-missing-attribute] Attribute `get` on type `(Mapping[Any, Any] & ~((...) -> object)) | (((Any, /) -> Any) & <Protocol with members 'get'> & ~((...) -> object)) | (((Any, /) -> Any) & ((...) -> object) & ~((...) -> object))` may be missing

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `type[ContainerBase]`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `type[ContainerBase]`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1537:63: error[invalid-argument-type] Argument to bound method `gen_from_dict_like` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `type[ContainerBase]`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `type[ContainerBase]`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1539:61: error[invalid-argument-type] Argument to bound method `gen_from_display` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `type[ContainerBase]`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `type[ContainerBase]`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1541:60: error[invalid-argument-type] Argument to bound method `gen_from_astype` is incorrect: Expected `int`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1543:61: error[invalid-argument-type] Argument to bound method `gen_from_persist` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1543:61: error[invalid-argument-type] Argument to bound method `gen_from_persist` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | int`
- static_frame/core/interface.py:1543:61: error[invalid-argument-type] Argument to bound method `gen_from_persist` is incorrect: Expected `str`, found `str | type[ContainerBase] | Unknown | (Unknown & ~str) | int`
+ static_frame/core/interface.py:1543:61: error[invalid-argument-type] Argument to bound method `gen_from_persist` is incorrect: Expected `str`, found `str...*[Comment body truncated]*

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-09-26 13:16_

---

_Comment by @github-actions[bot] on 2025-09-26 13:21_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 104 | 1 | 178 |
| `possibly-missing-attribute` | 0 | 1 | 81 |
| `unsupported-operator` | 4 | 0 | 33 |
| `invalid-assignment` | 0 | 0 | 17 |
| `invalid-return-type` | 0 | 0 | 12 |
| `possibly-missing-implicit-call` | 0 | 0 | 12 |
| `not-iterable` | 0 | 0 | 4 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **109** | **2** | **337** |

**[Full report with detailed diff](https://alex-dynamic-intersection-un.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-dynamic-intersection-un.ecosystem-663.pages.dev/timing))


---

_Comment by @AlexWaygood on 2025-09-26 13:29_

All the new ecosystem diagnostics look like new manifestations of https://github.com/astral-sh/ty/issues/1248. I'm not sure why we weren't emitting them before (maybe some `Todo`-type branch we have for intersections somewhere?), but they're what I would expect given our current behaviour for unannotated `dict` literals and unannotated `list` literals.

Overall I think this makes us produce simpler, more user-friendly types.

---

_Marked ready for review by @AlexWaygood on 2025-09-26 13:29_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-26 13:29_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-26 13:29_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-26 13:29_

---

_@AlexWaygood reviewed on 2025-09-26 14:13_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/builder.rs`:509 on 2025-09-26 14:13_

A more generalised solution here (that would also allow us to get rid of the separate `is_equivalent_to` call -- so might be quite a bit faster?) would be to add a third type relation, `UnionSimplification` (not sure what the best name would be really), which would sit in between `TypeRelation::Subtyping` and `TypeRelation::Assignability`. It would mostly behave the same way as subtyping (`Any | int` would not be considered simplifiable to `Any`), but `Any <: Any`, `(Any | int) <: (int | Any)` and `(Any & str) <: Any` would all be considered `true` under this type relation.

---

_Comment by @carljm on 2025-09-26 15:21_

> maybe some `Todo`-type branch we have for intersections somewhere?

We still emit Todo for mapping over an intersection with negated elements, could be that?

---

_@carljm approved on 2025-09-26 15:24_

---

_Merged by @AlexWaygood on 2025-09-26 16:00_

---

_Closed by @AlexWaygood on 2025-09-26 16:00_

---

_Branch deleted on 2025-09-26 16:00_

---
