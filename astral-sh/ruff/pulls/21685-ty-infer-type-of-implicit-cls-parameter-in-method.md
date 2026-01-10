```yaml
number: 21685
title: "[ty] Infer type of implicit `cls` parameter in method bodies"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/implicit-cls-body
created_at: 2025-11-28T22:04:58Z
updated_at: 2025-12-10T09:31:30Z
url: https://github.com/astral-sh/ruff/pull/21685
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Infer type of implicit `cls` parameter in method bodies

---

_Pull request opened by @ibraheemdev on 2025-11-28 22:04_

## Summary

Extends https://github.com/astral-sh/ruff/pull/20922 to infer unannotated `cls` parameters as `type[Self]` in method bodies.

Part of https://github.com/astral-sh/ty/issues/159.

---

_Review requested from @carljm by @ibraheemdev on 2025-11-28 22:04_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-28 22:04_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-28 22:04_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-28 22:04_

---

_Label `ty` added by @ibraheemdev on 2025-11-28 22:04_

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 22:07_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-12-03 02:01:47.774206207 +0000
+++ new-output.txt	2025-12-03 02:01:51.357221036 +0000
@@ -513,15 +513,13 @@
 generics_self_advanced.py:36:9: error[type-assertion-failure] Type `list[Self@method2]` does not match asserted type `list[typing.Self]`
 generics_self_advanced.py:37:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `typing.Self`
 generics_self_advanced.py:38:9: error[type-assertion-failure] Type `Self@method2` does not match asserted type `Unknown`
-generics_self_advanced.py:42:9: error[type-assertion-failure] Type `type[Self@method3]` does not match asserted type `Unknown`
-generics_self_advanced.py:43:9: error[type-assertion-failure] Type `list[Self@method3]` does not match asserted type `Unknown`
-generics_self_advanced.py:44:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
+generics_self_advanced.py:43:9: error[type-assertion-failure] Type `list[Self@method3]` does not match asserted type `list[typing.Self]`
+generics_self_advanced.py:44:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `typing.Self`
 generics_self_advanced.py:45:9: error[type-assertion-failure] Type `Self@method3` does not match asserted type `Unknown`
 generics_self_attributes.py:26:33: error[invalid-argument-type] Argument is incorrect: Expected `typing.Self | None`, found `LinkedList[int]`
 generics_self_attributes.py:29:5: error[invalid-assignment] Object of type `OrdinalLinkedList` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_attributes.py:32:5: error[invalid-assignment] Object of type `LinkedList[int]` is not assignable to attribute `next` of type `typing.Self | None`
 generics_self_basic.py:20:16: error[invalid-return-type] Return type does not match returned value: expected `Self@method2`, found `Shape`
-generics_self_basic.py:27:9: error[type-assertion-failure] Type `type[Self@from_config]` does not match asserted type `Unknown`
 generics_self_basic.py:33:16: error[invalid-return-type] Return type does not match returned value: expected `Self@cls_method2`, found `Shape`
 generics_self_basic.py:54:1: error[type-assertion-failure] Type `Shape` does not match asserted type `Unknown`
 generics_self_basic.py:55:1: error[type-assertion-failure] Type `Circle` does not match asserted type `Unknown`
@@ -1029,4 +1027,4 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Unknown key "title" for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions
-Found 1031 diagnostics
+Found 1029 diagnostics

```

</details>




---

_Comment by @astral-sh-bot[bot] on 2025-11-28 22:08_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
attrs (https://github.com/python-attrs/attrs)
+ src/attr/_version_info.py:48:13: error[unknown-argument] Argument `year` does not match any known parameter of bound method `__init__`
+ src/attr/_version_info.py:48:29: error[unknown-argument] Argument `minor` does not match any known parameter of bound method `__init__`
+ src/attr/_version_info.py:48:46: error[unknown-argument] Argument `micro` does not match any known parameter of bound method `__init__`
+ src/attr/_version_info.py:48:63: error[unknown-argument] Argument `releaselevel` does not match any known parameter of bound method `__init__`
- Found 606 diagnostics
+ Found 610 diagnostics

parso (https://github.com/davidhalter/parso)
+ parso/normalizer.py:100:17: error[unresolved-attribute] Object of type `type[Self@register_rule]` has no attribute `rule_value_classes`
+ parso/normalizer.py:102:17: error[unresolved-attribute] Object of type `type[Self@register_rule]` has no attribute `rule_type_classes`
- Found 197 diagnostics
+ Found 199 diagnostics

kornia (https://github.com/kornia/kornia)
- kornia/feature/dedode/dedode.py:232:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- kornia/feature/dedode/dedode.py:233:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 765 diagnostics
+ Found 763 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/rich/text.py:397:24: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Text | str`, found `str | Style`
+ src/pip/_vendor/urllib3/util/retry.py:346:59: error[unresolved-attribute] Object of type `type[Self@from_int]` has no attribute `DEFAULT`
- Found 601 diagnostics
+ Found 603 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/spec.py:5233:22: error[unresolved-attribute] Object of type `type[Self@from_node_dict]` has no attribute `name_and_data`
+ lib/spack/spack/spec.py:5290:47: error[unresolved-attribute] Object of type `type[Self@from_node_dict]` has no attribute `SPEC_VERSION`
+ lib/spack/spack/spec.py:5326:46: error[unresolved-attribute] Object of type `type[Self@_load]` has no attribute `dependencies_from_node_dict`
+ lib/spack/spack/spec.py:5357:57: error[unresolved-attribute] Object of type `type[Self@_load]` has no attribute `dependencies_from_node_dict`
+ lib/spack/spack/spec.py:5365:31: error[unresolved-attribute] Object of type `type[Self@_load]` has no attribute `extract_build_spec_info_from_node_dict`
+ lib/spack/spack/vendor/attr/_version_info.py:48:13: error[unknown-argument] Argument `year` does not match any known parameter of bound method `__init__`
+ lib/spack/spack/vendor/attr/_version_info.py:48:29: error[unknown-argument] Argument `minor` does not match any known parameter of bound method `__init__`
+ lib/spack/spack/vendor/attr/_version_info.py:48:46: error[unknown-argument] Argument `micro` does not match any known parameter of bound method `__init__`
+ lib/spack/spack/vendor/attr/_version_info.py:48:63: error[unknown-argument] Argument `releaselevel` does not match any known parameter of bound method `__init__`
+ lib/spack/spack/vendor/jsonschema/validators.py:682:61: error[parameter-already-assigned] Multiple values provided for parameter 2 (`base_uri`) of bound method `__init__`
+ lib/spack/spack/vendor/jsonschema/validators.py:682:61: error[parameter-already-assigned] Multiple values provided for parameter 3 (`referrer`) of bound method `__init__`
+ lib/spack/spack/vendor/ruamel/yaml/comments.py:1128:32: error[no-matching-overload] No overload of bound method `fromkeys` matches arguments
+ var/spack/test_repos/spack_repo/builtin_mock/packages/llvm/package.py:70:24: error[no-matching-overload] No overload of bound method `group` matches arguments
- Found 7947 diagnostics
+ Found 7960 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, str] | str | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[bytes] | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `IO[str] | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bool`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Iterable[tuple[str, str]] | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | IO[bytes] | str | bytes | Mapping[str, Any]`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, Any] | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, Any] | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `str | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Mapping[str, Any] | None`, found `Unknown | str | None | Headers`
+ src/werkzeug/test.py:427:20: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Authorization | tuple[str, str] | None`, found `Unknown | str | None | Headers`
- Found 367 diagnostics
+ Found 385 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/core/downloader/contextfactory.py:90:13: error[parameter-already-assigned] Multiple values provided for parameter 2 (`method`) of bound method `__init__`
+ scrapy/core/downloader/contextfactory.py:90:13: error[parameter-already-assigned] Multiple values provided for parameter 3 (`tls_verbose_logging`) of bound method `__init__`
+ scrapy/core/downloader/contextfactory.py:90:13: error[parameter-already-assigned] Multiple values provided for parameter 4 (`tls_ciphers`) of bound method `__init__`
- scrapy/utils/datatypes.py:96:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1737 diagnostics
+ Found 1739 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/mark/structures.py:163:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 433 diagnostics
+ Found 432 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/language/ast.py:424:37: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 428 diagnostics
+ Found 427 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/text.py:397:24: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `Text | str`, found `str | Style`
- Found 347 diagnostics
+ Found 348 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/inspection/info.py:466:9: error[invalid-assignment] Object of type `Literal["directory"]` is not assignable to attribute `_source_type` on type `PackageInfo | None | Unknown`
+ src/poetry/inspection/info.py:467:9: error[invalid-assignment] Object of type `str` is not assignable to attribute `_source_url` on type `PackageInfo | None | Unknown`
+ src/poetry/inspection/info.py:469:16: error[invalid-return-type] Return type does not match returned value: expected `PackageInfo`, found `PackageInfo | None | Unknown`
+ src/poetry/publishing/uploader.py:150:25: error[no-matching-overload] No overload of bound method `join` matches arguments
+ src/poetry/utils/env/python/providers.py:93:20: error[too-many-positional-arguments] Too many positional arguments: expected 0, got 1
- Found 964 diagnostics
+ Found 969 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/config/_diff_base.py:75:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/schemathesis/config/_diff_base.py:94:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 290 diagnostics
+ Found 288 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
+ dummyserver/testcase.py:323:56: error[invalid-argument-type] Argument to bound method `_get_socket_mark` is incorrect: Expected `socket`, found `socket | Any | None`
- Found 264 diagnostics
+ Found 265 diagnostics

artigraph (https://github.com/artigraph/artigraph)
+ src/arti/internal/mappings.py:253:64: error[invalid-type-form] Variable of type `type[V@TypedBox]` is not allowed in a type expression
- src/arti/types/python.py:80:79: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/arti/views/__init__.py:107:16: error[invalid-return-type] Return type does not match returned value: expected `type[Self@get_class_for]`, found `type[View] | (Unknown & ~None)`
- Found 147 diagnostics
+ Found 148 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/proxy/mode_servers.py:103:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2134 diagnostics
+ Found 2133 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg/psycopg/_adapters_map.py:279:20: error[invalid-return-type] Return type does not match returned value: expected `type[RV@_get_optimised]`, found `type`
- Found 673 diagnostics
+ Found 674 diagnostics

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/annotations.py:451:17: error[invalid-argument-type] Argument is incorrect: Expected `Sequence[(...) -> Coroutine[Any, Any, Any] | Any]`, found `(((...) -> Coroutine[Any, Any, Any] | Any) & Sequence[object]) | Sequence[(...) -> Coroutine[Any, Any, Any] | Any]`
- Found 295 diagnostics
+ Found 296 diagnostics

comtypes (https://github.com/enthought/comtypes)
- comtypes/_post_coinit/unknwn.py:338:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ comtypes/safearray.py:159:27: error[invalid-argument-type] Argument to function `cast` is incorrect: Argument type `Self@create` does not satisfy upper bound `_CanCastTo` of type variable `_CastT`
+ comtypes/safearray.py:215:27: error[invalid-argument-type] Argument to function `cast` is incorrect: Argument type `Self@create_from_ndarray` does not satisfy upper bound `_CanCastTo` of type variable `_CastT`
+ comtypes/safearray.py:384:34: error[unresolved-attribute] Object of type `type[Self@from_param]` has no attribute `_type_`
+ comtypes/safearray.py:386:26: error[unresolved-attribute] Object of type `type[Self@from_param]` has no attribute `_type_`
- Found 373 diagnostics
+ Found 376 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/rpc/api_server/webserver.py:103:9: error[invalid-assignment] Object of type `None` is not assignable to attribute `_rpc` of type `RPC`
- Found 695 diagnostics
+ Found 696 diagnostics

vision (https://github.com/pytorch/vision)
+ test/datasets_utils.py:410:21: warning[possibly-missing-attribute] Attribute `__mro__` may be missing on object of type `Unknown | None`
+ test/datasets_utils.py:475:25: warning[possibly-missing-attribute] Attribute `__name__` may be missing on object of type `Unknown | None`
- Found 1471 diagnostics
+ Found 1473 diagnostics

pandera (https://github.com/pandera-dev/pandera)
+ pandera/api/dataframe/model.py:247:13: error[invalid-assignment] Object of type `type` is not assignable to attribute `Config` of type `type[BaseConfig]`
- pandera/api/dataframe/model.py:457:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[type[BaseConfig], dict[str, Any]]`, found `tuple[type, Unknown]`
+ pandera/api/dataframe/model.py:457:16: error[invalid-return-type] Return type does not match returned value: expected `tuple[type[BaseConfig], dict[str, Any]]`, found `tuple[type, dict[str, Any]]`
+ pandera/api/pandas/model.py:209:30: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `ExtensionArray | ndarray[tuple[Any, ...], dtype[Any]] | Index[Any] | ... omitted 4 union elements`, found `dict_keys[Any, Any]`
- pandera/api/pandas/model.py:210:13: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 150 literals] | ExtensionDtype | ... omitted 3 union elements`, found `dict[Unknown, Unknown | None]`
+ pandera/api/pandas/model.py:210:13: error[invalid-argument-type] Argument to bound method `astype` is incorrect: Expected `type | Literal["bool", "boolean", "?", "b1", "bool_", ... omitted 150 literals] | ExtensionDtype | ... omitted 3 union elements`, found `dict[str | Unknown, Any | None]`
+ pandera/api/pyspark/model.py:148:13: error[invalid-assignment] Object of type `type` is not assignable to attribute `Config` of type `type[BaseConfig]`
- pandera/api/pyspark/model.py:249:24: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:669:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:931:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1149:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1200:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/engines/pandas_engine.py:1179:13: error[unknown-argument] Argument `fill_value` does not match any known parameter of bound method `__init__`
- pandera/engines/pandas_engine.py:1754:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/engines/pandas_engine.py:1753:17: error[unknown-argument] Argument `precision` does not match any known parameter of bound method `__init__`
+ pandera/engines/pandas_engine.py:1753:52: error[unknown-argument] Argument `scale` does not match any known parameter of bound method `__init__`
- pandera/engines/pandas_engine.py:1775:71: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/engines/pandas_engine.py:1805:20: error[missing-argument] No argument provided for required parameter `dtype` of bound method `__init__`
- pandera/engines/pandas_engine.py:1806:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1807:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1808:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/engines/pandas_engine.py:1842:26: error[missing-argument] No argument provided for required parameter `dtype` of bound method `__init__`
- pandera/engines/pandas_engine.py:1843:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1844:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1847:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1876:20: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/engines/pandas_engine.py:1872:20: error[missing-argument] No argument provided for required parameter `dtype` of bound method `__init__`
- pandera/engines/pandas_engine.py:1936:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1954:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:1980:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/engines/pandas_engine.py:2014:20: error[missing-argument] No argument provided for required parameter `dtype` of bound method `__init__`
- pandera/engines/pandas_engine.py:2015:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:2016:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:2017:57: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:2049:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pandas_engine.py:2051:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/engines/pyarrow_engine.py:243:13: error[unknown-argument] Argument `precision` does not match any known parameter of bound method `__init__`
+ pandera/engines/pyarrow_engine.py:243:48: error[unknown-argument] Argument `scale` does not match any known parameter of bound method `__init__`
+ pandera/engines/pyarrow_engine.py:289:16: error[missing-argument] No argument provided for required parameter `dtype` of bound method `__init__`
+ pandera/engines/pyarrow_engine.py:323:22: error[missing-argument] No argument provided for required parameter `dtype` of bound method `__init__`
+ pandera/engines/pyarrow_engine.py:352:16: error[missing-argument] No argument provided for required parameter `dtype` of bound method `__init__`
- pandera/engines/pyarrow_engine.py:244:12: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:262:67: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:290:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:291:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:292:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:324:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:325:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:328:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:355:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:415:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:432:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:457:46: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/engines/pyarrow_engine.py:490:16: error[missing-argument] No argument provided for required parameter `dtype` of bound method `__init__`
- pandera/engines/pyarrow_engine.py:491:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:492:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:493:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:522:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/engines/pyarrow_engine.py:524:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1639 diagnostics
+ Found 1616 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/gateway.py:402:13: error[invalid-assignment] Implicit shadowing of function `send`
+ discord/gateway.py:403:13: error[invalid-assignment] Implicit shadowing of function `log_receive`
- discord/guild.py:575:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/invite.py:475:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/member.py:362:75: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/member.py:371:58: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/member.py:391:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/webhook/async_.py:1323:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/webhook/sync.py:702:53: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- discord/welcome_screen.py:92:76: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 566 diagnostics
+ Found 560 diagnostics

django-test-migrations (https://github.com/wemake-services/django-test-migrations)
- django_test_migrations/contrib/unittest_case.py:84:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 3 diagnostics
+ Found 2 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/accounts/model.py:482:13: warning[possibly-missing-attribute] Attribute `get_user` may be missing on object of type `OpenLibraryAccount | None`
+ openlibrary/core/db.py:44:17: error[unresolved-attribute] Object of type `type[Self@update_work_id]` has no attribute `TABLENAME`
+ openlibrary/core/db.py:75:17: error[unresolved-attribute] Object of type `type[Self@update_work_ids_individually]` has no attribute `TABLENAME`
+ openlibrary/core/db.py:82:63: error[unresolved-attribute] Object of type `type[Self@update_work_ids_individually]` has no attribute `PRIMARY_KEY`
+ openlibrary/core/db.py:88:31: error[unresolved-attribute] Object of type `type[Self@update_work_ids_individually]` has no attribute `TABLENAME`
+ openlibrary/core/db.py:95:43: error[unresolved-attribute] Object of type `type[Self@update_work_ids_individually]` has no attribute `TABLENAME`
+ openlibrary/core/db.py:97:33: error[unresolved-attribute] Object of type `type[Self@update_work_ids_individually]` has no attribute `ALLOW_DELETE_ON_CONFLICT`
+ openlibrary/core/db.py:102:24: error[unresolved-attribute] Object of type `type[Self@update_work_ids_individually]` has no attribute `ALLOW_DELETE_ON_CONFLICT`
+ openlibrary/core/db.py:112:17: error[unresolved-attribute] Object of type `type[Self@select_all_by_username]` has no attribute `TABLENAME`
+ openlibrary/core/db.py:123:17: error[unresolved-attribute] Object of type `type[Self@update_username]` has no attribute `TABLENAME`
+ openlibrary/core/db.py:143:17: error[unresolved-attribute] Object of type `type[Self@delete_all_by_username]` has no attribute `TABLENAME`
+ openlibrary/core/edits.py:179:28: error[invalid-argument-type] Argument to bound method `submit_request` is incorrect: Expected `str`, found `type[Self@submit_delete_request]`
+ openlibrary/core/edits.py:179:38: error[parameter-already-assigned] Multiple values provided for parameter `submitter` of bound method `submit_request`
- Found 1122 diagnostics
+ Found 1135 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/_internal/concurrency/services.py:284:32: error[unresolved-attribute] Object of type `typing.Self` has no attribute `_drain`
+ src/prefect/_internal/concurrency/services.py:315:20: error[invalid-return-type] Return type does not match returned value: expected `Self@instance`, found `typing.Self`
+ src/prefect/_waiters.py:243:20: error[invalid-return-type] Return type does not match returned value: expected `Self@instance`, found `typing.Self | Unknown`
+ src/prefect/cli/transfer/_migratable_resources/automations.py:52:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/automations.py:54:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/automations.py:62:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[Automation] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:48:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:50:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:58:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[BlockType] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:102:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:104:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:112:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[BlockSchema] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:242:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:244:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/blocks.py:252:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[BlockDocument] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/concurrency_limits.py:50:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/concurrency_limits.py:52:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/concurrency_limits.py:60:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[GlobalConcurrencyLimitResponse] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/deployments.py:45:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/deployments.py:47:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/deployments.py:55:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[DeploymentResponse] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/flows.py:39:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/flows.py:41:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/flows.py:47:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[Flow] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/variables.py:42:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/variables.py:44:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/variables.py:50:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[Variable] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/work_pools.py:48:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/work_pools.py:52:13: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/work_pools.py:58:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[WorkPool] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/work_pools.py:66:16: error[unresolved-attribute] Object of type `typing.Self` has no attribute `source_work_pool`
+ src/prefect/cli/transfer/_migratable_resources/work_pools.py:67:24: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[WorkPool] | None`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/work_queues.py:43:20: error[invalid-return-type] Return type does not match returned value: expected `Self@construct`, found `typing.Self`
+ src/prefect/cli/transfer/_migratable_resources/work_queues.py:45:9: error[invalid-assignment] Invalid subscript assignment with key of type `UUID` and value of type `Self@construct` on object of type `dict[UUID, typing.Self]`
+ src/prefect/cli/transfer/_migratable_resources/work_queues.py:53:20: error[invalid-return-type] Return type does not match returned value: expected `MigratableResource[WorkQueue] | None`, found `typing.Self`
+ src/prefect/context.py:568:13: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `object`
+ src/prefect/context.py:725:16: error[invalid-return-type] Return type does not match returned value: expected `Self@get`, found `Self@get | typing.Self`
+ src/prefect/deployments/runner.py:722:13: error[invalid-argument-type] Argument is incorrect: Expected `list[DeploymentScheduleCreate | DeploymentScheduleUpdate] | None`, found `Sequence[DeploymentScheduleCreate | dict[str, Any] | IntervalSchedule | ... omitted 4 union elements]`
+ src/prefect/deployments/runner.py:724:13: error[invalid-argument-type] Argument is incorrect: Expected `ConcurrencyOptions | None`, found `dict[Unknown | str, Unknown | ConcurrencyLimitStrategy] | None`
+ src/prefect/deployments/runner.py:866:13: error[invalid-argument-type] Argument is incorrect: Expected `list[DeploymentScheduleCreate | DeploymentScheduleUpdate] | None`, found `Sequence[DeploymentScheduleCreate | dict[str, Any] | IntervalSchedule | ... omitted 4 union elements]`
+ src/prefect/deployments/runner.py:868:13: error[invalid-argument-type] Argument is incorrect: Expected `ConcurrencyOptions | None`, found `dict[Unknown | str, Unknown | ConcurrencyLimitStrategy] | None`
+ src/prefect/deployments/runner.py:987:13: error[invalid-argument-type] Argument is incorrect: Expected `list[DeploymentScheduleCreate | DeploymentScheduleUpdate] | None`, found `Sequence[DeploymentScheduleCreate | dict[str, Any] | IntervalSchedule | ... omitted 4 union elements]`
+ src/prefect/deployments/runner.py:989:13: error[invalid-argument-type] Argument is incorrect: Expected `ConcurrencyOptions | None`, found `dict[Unknown | str, Unknown | ConcurrencyLimitStrategy] | None`
+ src/prefect/deployments/runner.py:1111:13: error[invalid-argument-type] Argument is incorrect: Expected `list[DeploymentScheduleCreate | DeploymentScheduleUpdate] | None`, found `Sequence[DeploymentScheduleCreate | dict[str, Any] | IntervalSchedule | ... omitted 4 union elements]`
+ src/prefect/deployments/runner.py:1113:13: error[invalid-argument-type] Argument is incorrect: Expected `ConcurrencyOptions | None`, found `dict[Unknown | str, Unknown | ConcurrencyLimitStrategy] | None`
+ src/prefect/server/task_queue.py:52:13: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `Self@for_key` on object of type `dict[str, typing.Self]`
+ src/prefect/server/task_queue.py:53:16: error[invalid-return-type] Return type does not match returned value: expected `Self@for_key`, found `typing.Self`
+ src/prefect/task_runs.py:258:20: error[invalid-return-type] Return type does not match returned value: expected `Self@instance`, found `typing.Self | Unknown`
- Found 3396 diagnostics
+ Found 3444 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
+ cloudinit/distros/__init__.py:1347:30: error[invalid-argument-type] Argument to function `subp` is incorrect: Expected `str | bytes | list[str] | list[bytes]`, found `list[Unknown | list[str] | str]`
- Found 1179 diagnostics
+ Found 1180 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:1325:20: error[invalid-return-type] Return type does not match returned value: expected `_T@_sanitize_collection`, found `dict[Unknown, Unknown]`
+ src/hydra_zen/structured_configs/_implementations.py:1325:20: error[invalid-return-type] Return type does not match returned value: expected `_T@_sanitize_collection`, found `dict[int | None | float | ... omitted 13 union elements, int | None | float | ... omitted 13 union elements]`
+ src/hydra_zen/structured_configs/_implementations.py:3242:9: error[no-matching-overload] No overload of bound method `builds` matches arguments
- Found 539 diagnostics
+ Found 540 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ pkg_resources/tests/test_pkg_resources.py:45:9: error[invalid-assignment] Object of type `Unknown | struct_time` is not assignable to attribute `date_time` of type `tuple[int, int, int, int, int, int]`
+ pkg_resources/tests/test_pkg_resources.py:49:9: error[invalid-assignment] Object of type `Unknown | struct_time` is not assignable to attribute `date_time` of type `tuple[int, int, int, int, int, int]`
+ pkg_resources/tests/test_pkg_resources.py:53:9: error[invalid-assignment] Object of type `Unknown | struct_time` is not assignable to attribute `date_time` of type `tuple[int, int, int, int, int, int]`
+ pkg_resources/tests/test_pkg_resources.py:57:9: error[invalid-assignment] Object of type `Unknown | struct_time` is not assignable to attribute `date_time` of type `tuple[int, int, int, int, int, int]`
+ setuptools/_distutils/compilers/C/base.py:1048:35: error[invalid-argument-type] Argument to function `_make_relative` is incorrect: Expected `Path`, found `PurePath`
+ setuptools/_distutils/dir_util.py:28:20: error[unresolved-attribute] Object of type `type[Self@clear]` has no attribute `instance`
+ setuptools/_vendor/backports/tarfile/__init__.py:1300:13: error[unresolved-attribute] Unresolved attribute `_sparse_structs` on type `Self@frombuf`.
+ setuptools/_vendor/typing_extensions.py:712:17: error[invalid-assignment] Implicit shadowing of function `__subclasshook__`
+ setuptools/_vendor/typing_extensions.py:716:17: error[invalid-assignment] Implicit shadowing of function `__init__`
+ setuptools/tests/test_windows_wrappers.py:44:34: error[unresolved-attribute] Object of type `type[Self@create_script]` has no attribute `script_tmpl`
+ setuptools/tests/test_windows_wrappers.py:46:24: error[unresolved-attribute] Object of type `type[Self@create_script]` has no attribute `script_name`
+ setuptools/tests/test_windows_wrappers.py:50:24: error[unresolved-attribute] Object of type `type[Self@create_script]` has no attribute `wrapper_name`
+ setuptools/tests/test_windows_wrappers.py:51:56: error[unresolved-attribute] Object of type `type[Self@create_script]` has no attribute `wrapper_source`
- Found 1262 diagnostics
+ Found 1275 diagnostics

xarray (https://github.com/pydata/xarray)
+ xarray/core/common.py:281:13: error[invalid-assignment] Implicit shadowing of function `__setattr__`
- Found 1733 diagnostics
+ Found 1734 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/heap/mallocng.py:559:13: error[unresolved-attribute] Unresolved attribute `_sn3` on type `Self@from_start`.
+ pwndbg/aglib/heap/mallocng.py:563:13: error[unresolved-attribute] Unresolved attribute `_sn3` on type `Self@from_start`.
- Found 2815 diagnostics
+ Found 2817 diagnostics

altair (https://github.com/vega/altair)
- altair/utils/schemapi.py:1342:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1107 diagnostics
+ Found 1106 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/appsec/_iast/taint_sinks/weak_randomness.py:10:43: error[invalid-argument-type] Argument to bound method `report` is incorrect: Expected `str | bytes | bytearray`, found `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:721:14: warning[possibly-missing-attribute] Attribute `_dne_client` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:736:14: warning[possibly-missing-attribute] Attribute `_dne_client` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:805:14: warning[possibly-missing-attribute] Attribute `_dne_client` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:809:13: warning[possibly-missing-attribute] Attribute `_dne_client` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:814:16: warning[possibly-missing-attribute] Attribute `_dne_client` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:908:9: error[invalid-assignment] Object of type `((LLMObsSpan, /) -> LLMObsSpan | None) | None` is not assignable to attribute `_user_span_processor` on type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:924:9: warning[possibly-missing-attribute] Attribute `stop` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:978:27: warning[possibly-missing-attribute] Attribute `tracer` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:984:17: warning[possibly-missing-attribute] Attribute `tracer` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:992:18: warning[possibly-missing-attribute] Attribute `_annotation_context_lock` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:994:17: warning[possibly-missing-attribute] Attribute `_annotations` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:999:18: warning[possibly-missing-attribute] Attribute `_annotation_context_lock` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1000:49: warning[possibly-missing-attribute] Attribute `_annotations` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1002:25: warning[possibly-missing-attribute] Attribute `_annotations` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1020:13: warning[possibly-missing-attribute] Attribute `_evaluator_runner` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1026:13: warning[possibly-missing-attribute] Attribute `_llmobs_span_writer` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1027:13: warning[possibly-missing-attribute] Attribute `_llmobs_eval_metric_writer` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1062:20: warning[possibly-missing-attribute] Attribute `_current_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1187:16: warning[possibly-missing-attribute] Attribute `_start_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1217:16: warning[possibly-missing-attribute] Attribute `_start_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1239:16: warning[possibly-missing-attribute] Attribute `_start_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1261:16: warning[possibly-missing-attribute] Attribute `_start_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1285:16: warning[possibly-missing-attribute] Attribute `_start_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1319:16: warning[possibly-missing-attribute] Attribute `_start_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1349:16: warning[possibly-missing-attribute] Attribute `_start_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1374:16: warning[possibly-missing-attribute] Attribute `_start_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1457:24: warning[possibly-missing-attribute] Attribute `_current_span` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1796:13: warning[possibly-missing-attribute] Attribute `_llmobs_eval_metric_writer` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1805:23: warning[possibly-missing-attribute] Attribute `_llmobs_context_provider` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1844:24: warning[possibly-missing-attribute] Attribute `tracer` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1891:17: warning[possibly-missing-attribute] Attribute `_llmobs_context_provider` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1895:13: warning[possibly-missing-attribute] Attribute `_llmobs_context_provider` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1913:9: warning[possibly-missing-attribute] Attribute `tracer` may be missing on object of type `Unknown | None`
+ ddtrace/llmobs/_llmobs.py:1914:9: warning[possibly-missing-attribute] Attribute `_activate_llmobs_distributed_context` may be missing on object of type `Unknown | None`
- ddtrace/testing/internal/test_data.py:140:15: warning[unsupported-base] Unsupported class base with type `<class 'TestItem[Test, Never]'> | <class 'TestItem[Unknown, Unknown]'>`
+ tests/debugging/exploration/debugger.py:206:9: error[invalid-assignment] Object of type `LightProbeRegistry` is not assignable to attribute `_probe_registry` on type `Debugger | None`
+ tests/debugging/exploration/debugger.py:206:60: warning[possibly-missing-attribute] Attribute `_status_logger` may be missing on object of type `Debugger | None`
+ tests/debugging/exploration/debugger.py:208:9: error[invalid-assignment] Object of type `bound method type[Self@enable].on_snapshot(snapshot: Snapshot) -> None` is not assignable to attribute `on_snapshot` on type `Unknown | SignalCollector | None`
+ tests/debugging/exploration/debugger.py:208:9: warning[possibly-missing-attribute] Attribute `__uploader__` may be missing on object of type `Debugger | None`
+ tests/debugging/exploration/debugger.py:218:20: warning[possibly-missing-attribute] Attribute `_probe_registry` may be missing on object of type `Debugger | None`
+ tests/debugging/exploration/debugger.py:244:28: error[unresolved-attribute] Object of type `Probe` has no attribute `probe`
+ tests/debugging/exploration/debugger.py:246:28: error[unresolved-attribute] Object of type `Probe` has no attribute `error_type`
+ tests/debugging/exploration/debugger.py:246:44: error[unresolved-attribute] Object of type `Probe` has no attribute `message`
+ tests/debugging/exploration/debugger.py:258:16: warning[possibly-missing-attribute] Attribute `snapshots` may be missing on object of type `Unknown | SignalCollector | None`
+ tests/debugging/exploration/debugger.py:264:16: warning[possibly-missing-attribute] Attribute `probes` may be missing on object of type `Unknown | SignalCollector | None`
+ tests/debugging/origin/test_span.py:25:33: error[invalid-argument-type] Argument to bound method `instrument_view` is incorrect: Expected `FunctionType | MethodType`, found `(...) -> Unknown`
- Found 8310 diagnostics
+ Found 8355 diagnostics

manticore (https://github.com/trailofbits/manticore)
+ manticore/core/workspace.py:97:16: error[unresolved-attribute] Object of type `type[Self@fromdescriptor]` has no attribute `store_type`
- Found 11133 diagnostics
+ Found 11134 diagnostics

pandas (https://github.com/pandas-dev/pandas)
+ pandas/core/arrays/datetimes.py:534:32: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[datetime64[date | int | None]]]`, found `ndarray[tuple[Any, ...], str]`
+ pandas/core/arrays/interval.py:1147:73: error[invalid-argument-type] Argument to bound method `_ensure_simple_new_inputs` is incorrect: Expected `Literal["left", "right", "both", "neither"] | None`, found `str | Unknown`
+ pandas/core/arrays/masked.py:156:54: error[invalid-argument-type] Argument to bound method `_coerce_to_array` is incorrect: Expected `dtype[Any] | ExtensionDtype`, found `Unknown | None`
- pandas/core/arrays/string_arrow.py:220:42: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandas/core/arrays/timedeltas.py:324:32: error[invalid-argument-type] Argument to bound method `_simple_new` is incorrect: Expected `ndarray[tuple[Any, ...], dtype[timedelta64[timedelta | int | None]]]`, found `ndarray[tuple[Any, ...], str] | Unknown`
- pandas/core/indexes/base.py:684:65: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandas/io/formats/style.py:3733:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 3791 diagnostics
+ Found 3792 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/model/model.py:98:13: error[unresolved-attribute] Unresolved attribute `__signature__` on type `def __init__(self, *args: Any, **kwargs: Any) -> None`.
- Found 856 diagnostics
+ Found 857 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
+ mypy_django_plugin/lib/helpers.py:226:16: error[missing-argument] No argument provided for required parameter `cls`
+ mypy_django_plugin/lib/helpers.py:226:20: error[parameter-already-assigned] Multiple values provided for parameter `cls`
- Found 440 diagnostics
+ Found 442 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
+ win32/Lib/win32timezone.py:848:13: error[invalid-assignment] Object of type `Self@utc` is not assignable to attribute `_tzutc` of type `typing.Self | None`
+ win32/Lib/win32timezone.py:849:16: error[invalid-return-type] Return type does not match returned value: expected `Self@utc`, found `typing.Self | None`
- Found 2654 diagnostics
+ Found 2656 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/backends/__init__.py:1248:31: error[unresolved-attribute] Object of type `type[Self@register_options]` has no attribute `Options`
+ ibis/backends/athena/__init__.py:389:35: error[invalid-argument-type] Argument to bound method `_post_connect` is incorrect: Expected `str`, found `str | None`
- ibis/backends/bigquery/datatypes.py:50:31: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `dict[Unknown, Unknown]`
+ ibis/backends/bigquery/datatypes.py:50:31: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `dict[Unknown, DataType | Unknown]`
- ibis/backends/bigquery/datatypes.py:67:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, Unknown]`
+ ibis/backends/bigquery/datatypes.py:67:27: error[invalid-argument-type] Argument is incorrect: Expected `FrozenOrderedDict[str, DataType]`, found `dict[Unknown, DataType | Unknown]`
+ ibis/backends/databricks/__init__.py:440:35: error[invalid-argument-type] Argument to bound method `_post_connect` is incorrect: Expected `str`, found `str | None`
- ibis/backends/flink/datatypes.py:75:17: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `dict[Unknown, Unknown]`
+ ibis/backends/flink/datatypes.py:75:17: error[invalid-argument-type] Argument is incorrect: Expected `bool`, found `dict[Unknown, DataType | Unknown]`
+ ibis/backends/polars/rewrites.py:22:28: error[invalid-argument-type] Argument is incorrect: Expected `FrozenDict[str, str]`, found `dict[Unknown, str | Unknown]`
+ ibis/backends/sql

... (truncated 491 lines) ...
```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
prefect (https://github.com/PrefectHQ/prefect)
-     memo metadata = ~159MB
+     memo metadata = ~167MB


```

</details>




---

_Comment by @codspeed-hq[bot] on 2025-11-28 22:21_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fimplicit-cls-body?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21685 will **degrade performances by 11.61%**

<sub>Comparing <code>ibraheem/implicit-cls-body</code> (b691fbc) with <code>main</code> (abaa49f)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 51` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/ibraheem%2Fimplicit-cls-body?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 1.1 s | 1.3 s | -11.61% |


---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-11-28 22:59_

---

_Comment by @astral-sh-bot[bot] on 2025-11-28 23:08_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 69 | 18 | 77 |
| `unresolved-attribute` | 85 | 0 | 0 |
| `unused-ignore-comment` | 0 | 80 | 0 |
| `possibly-missing-attribute` | 48 | 0 | 0 |
| `invalid-return-type` | 34 | 1 | 5 |
| `unknown-argument` | 39 | 0 | 0 |
| `invalid-assignment` | 38 | 0 | 0 |
| `missing-argument` | 10 | 0 | 0 |
| `parameter-already-assigned` | 8 | 0 | 0 |
| `no-matching-overload` | 7 | 0 | 0 |
| `too-many-positional-arguments` | 5 | 0 | 0 |
| `unsupported-operator` | 5 | 0 | 0 |
| `not-iterable` | 2 | 0 | 0 |
| `unsupported-base` | 0 | 2 | 0 |
| `deprecated` | 1 | 0 | 0 |
| `invalid-context-manager` | 1 | 0 | 0 |
| `invalid-type-form` | 1 | 0 | 0 |
| **Total** | **353** | **101** | **82** |

**[Full report with detailed diff](https://ibraheem-implicit-cls-body.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-implicit-cls-body.ecosystem-663.pages.dev/timing))




---

_Comment by @astral-sh-bot[bot] on 2025-11-28 23:25_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.





---

_Comment by @AlexWaygood on 2025-11-29 13:58_

Here's a minimal repro of one of the new diagnostics on `artigraph` (looks like an underlying issue to do with `type[T]` rather than an issue with this PR -- it also reproduces on `main`):

```py
from typing import Callable, Any

def needs_a_callable(x: Callable[[Any], Any] | Callable[[Any, object], Any]): ...

class F:
    def __init__(self, arg) -> None: ...

def f[T: F](x: T):
    needs_a_callable(type(x))
```

It looks like we might not be implementing assignability correctly for `type[T]` vs a _union_ of callables?

---

_Comment by @AlexWaygood on 2025-11-29 14:13_

The artigraph diagnostic in `src/arti/views/__init__.py` looks like another problem caused by simplifying `T | <upper bound of T>` to `<upper bound of T>` in some situations (x-ref https://github.com/astral-sh/ty/issues/1666). So that one's a bigger issue that I don't think we should try to fix before landing this PR.

---

_Comment by @AlexWaygood on 2025-11-29 14:30_

The new attrs diagnostics are the same as https://github.com/astral-sh/ty/issues/1425, but for attrs rather than pydantic this time. We don't recognise the assignments in the class body of [`VersionInfo`](https://github.com/python-attrs/attrs/blob/43a868d703f06fffb724d5d03d4f80ac447cb27c/src/attr/_version_info.py#L12) as defining dataclass fields, because the stdlib `dataclasses` module only creates fields for _annotated_ assignments, and the assignments in `VersionInfo` are all unannotated.

---

_Comment by @AlexWaygood on 2025-11-29 14:42_

comtypes looks like it's doing some highly dynamic stuff that I don't think we should be expected to understand right now.

The new diagnostics in `homeassistant/components/recorder/db_schema.py` already all have `type: ignore` comments for mypy, so I don't think we need to worry about them (though we emit these on a different line to mypy, which is maybe slightly unfortunate).

The new diagnostics in `homeassistant/components/yeelight/scanner.py` are https://github.com/astral-sh/ty/issues/1124 -- but they're also using a generic `ClassVar`, which we should probably ban anyway 😄. Anyway, no need to worry about them for this PR.

---

_Comment by @AlexWaygood on 2025-11-29 15:06_

Here's a minimal repro for the diagnostic on `ddtrace/internal/symbol_db/symbols.py`. I actually can't repro this one on `main`, so it may be a problem with this PR?

```py
from typing import Iterable, Iterator, Self

class chain[T]:
    def __new__(cls, *its: Iterable[T]): ...

    def __iter__(self) -> Self:
        return self

    def __next__(self) -> T:
        raise NotImplementedError

class Symbol:
    @classmethod
    def g(cls) -> list[Symbol]:
        return list(chain((cls(),), (cls(),)))  # error here...
                                                # error [invalid-argument-type] "Argument to bound method `__init__` is incorrect: Expected `Iterable[Symbol]`, found `chain[Self@from_code]`"

def f[T: Symbol](x: type[T]) -> list[Symbol]:
    return list(chain((x(),), (x(),)))  # ... but no error here?
```

---

_Comment by @ibraheemdev on 2025-12-01 21:51_

That last one may be an issue with how we treat `Self`, I can reproduce it without `type[T]` as well:
```py
class Symbol:
    @classmethod
    def f(cls) -> list[Symbol]:
        y = chain((cls(),), (cls(),))
        reveal_type(y)  # chain[Self@g]
        return list(y)  # error[invalid-argument-type]

    def g(self) -> list[Symbol]:
        y = chain((self,), (self,))
        reveal_type(y)  # chain[Self@g]
        return list(y)  # error[invalid-argument-type]

def f[T: Symbol](x: type[T]) -> list[Symbol]:
    y = chain((x(),), (x(),))
    reveal_type(y)  # chain[T@g]
    return list(y)  # ok

def g[T: Symbol](x: T) -> list[Symbol]:
    y = chain((x,), (x,))
    reveal_type(y)  # chain[T@g]
    return list(y)  # ok
```

---

_Closed by @ibraheemdev on 2025-12-01 23:20_

---

_Reopened by @AlexWaygood on 2025-12-01 23:21_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-12-01 23:39_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-12-01 23:40_

---

_Label `ecosystem-analyzer` removed by @ibraheemdev on 2025-12-03 02:10_

---

_Label `ecosystem-analyzer` added by @ibraheemdev on 2025-12-03 02:10_

---

_@carljm approved on 2025-12-10 00:39_

This looks good to me. I filed https://github.com/astral-sh/ty/issues/1831 for the assignability-to-union-of-callables issue, but I don't think it should block landing this PR, either.

---

_@sharkdp approved on 2025-12-10 09:30_

Thank you!

---

_Merged by @sharkdp on 2025-12-10 09:31_

---

_Closed by @sharkdp on 2025-12-10 09:31_

---

_Branch deleted on 2025-12-10 09:31_

---
