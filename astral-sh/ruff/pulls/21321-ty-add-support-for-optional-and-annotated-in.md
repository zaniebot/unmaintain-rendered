```yaml
number: 21321
title: "[ty] Add support for `Optional` and `Annotated` in implicit type aliases"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/implicit-type-aliases-optional-annotated
created_at: 2025-11-07T17:58:38Z
updated_at: 2025-11-10T09:24:40Z
url: https://github.com/astral-sh/ruff/pull/21321
synced_at: 2026-01-12T15:57:21Z
```

# [ty] Add support for `Optional` and `Annotated` in implicit type aliases

---

_@sharkdp_

## Summary

Add support for `Optional` and `Annotated` in implicit type aliases

part of https://github.com/astral-sh/ty/issues/221

## Typing conformance changes

New expected diagnostics.

## Ecosystem

A lot of true positives, some known limitations unrelated to this PR.

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-11-07 17:58_

---

_Comment by @github-actions[bot] on 2025-11-07 18:00_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)


<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-11-10 09:08:35.361270274 +0000
+++ new-output.txt	2025-11-10 09:08:38.671305041 +0000
@@ -1,4 +1,4 @@
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18b71)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/05a9af7/src/function/execute.rs:451:17 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(18f71)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
@@ -856,7 +856,11 @@
 qualifiers_annotated.py:48:18: error[invalid-type-form] Boolean operations are not allowed in type expressions
 qualifiers_annotated.py:49:18: error[fstring-type-annotation] Type expressions cannot use f-strings
 qualifiers_annotated.py:59:8: error[invalid-type-form] Special form `typing.Annotated` expected at least 2 arguments (one type and at least one metadata element)
+qualifiers_annotated.py:71:1: error[invalid-assignment] Object of type `<typing.Annotated special form>` is not assignable to `type[Any]`
+qualifiers_annotated.py:72:1: error[invalid-assignment] Object of type `<typing.Annotated special form>` is not assignable to `type[Any]`
 qualifiers_annotated.py:86:1: error[call-non-callable] Object of type `typing.Annotated` is not callable
+qualifiers_annotated.py:87:1: error[call-non-callable] Object of type `GenericAlias` is not callable
+qualifiers_annotated.py:88:1: error[call-non-callable] Object of type `GenericAlias` is not callable
 qualifiers_final_annotation.py:18:7: error[invalid-type-form] Type qualifier `typing.Final` expected exactly 1 argument, got 2
 qualifiers_final_annotation.py:52:9: error[invalid-assignment] Cannot assign to final attribute `ID4` on type `Self@__init__`
 qualifiers_final_annotation.py:54:9: error[invalid-assignment] Cannot assign to final attribute `ID5` on type `Self@__init__`
@@ -1001,5 +1005,5 @@
 typeddicts_usage.py:28:17: error[missing-typed-dict-key] Missing required key 'name' in TypedDict `Movie` constructor
 typeddicts_usage.py:28:18: error[invalid-key] Invalid key for TypedDict `Movie`: Unknown key "title"
 typeddicts_usage.py:40:24: error[invalid-type-form] The special form `typing.TypedDict` is not allowed in type expressions. Did you mean to use a concrete TypedDict or `collections.abc.Mapping[str, object]` instead?
-Found 1003 diagnostics
+Found 1007 diagnostics
 WARN A fatal error occurred while checking some files. Not all project files were analyzed. See the diagnostics list above for details.

```

</details>




---

_Comment by @github-actions[bot] on 2025-11-07 18:02_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/_util/hint/pep/proposal/pep484/pep484typevar.py:158:6: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
+ beartype/_util/hint/pep/proposal/pep484585/generic/pep484585genget.py:67:23: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
+ beartype/_util/hint/pep/utilpepget.py:561:48: error[invalid-type-form] Variable of type `_SpecialForm` is not allowed in a type expression
- Found 497 diagnostics
+ Found 500 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/utils.py:395:32: error[not-iterable] Object of type `Sequence[Sequence[str]] | None` may not be iterable
+ paasta_tools/utils.py:400:41: error[not-iterable] Object of type `None` is not iterable
- Found 987 diagnostics
+ Found 989 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/experimental/pipeline.py:403:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pydantic/functional_serializers.py:438:36: error[call-non-callable] Object of type `GenericAlias` is not callable
+ pydantic/functional_validators.py:785:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<typing.Annotated special form>`
+ pydantic/functional_validators.py:828:36: error[call-non-callable] Object of type `GenericAlias` is not callable
+ pydantic/json_schema.py:2827:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<typing.Annotated special form>`
+ pydantic/types.py:230:12: error[invalid-return-type] Return type does not match returned value: expected `type[int]`, found `<typing.Annotated special form>`
+ pydantic/types.py:491:12: error[invalid-return-type] Return type does not match returned value: expected `type[float]`, found `<typing.Annotated special form>`
+ pydantic/types.py:679:12: error[invalid-return-type] Return type does not match returned value: expected `type[bytes]`, found `<typing.Annotated special form>`
+ pydantic/types.py:817:12: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `<typing.Annotated special form>`
+ pydantic/types.py:995:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<typing.Annotated special form>`
+ pydantic/types.py:1124:12: error[invalid-return-type] Return type does not match returned value: expected `type[Decimal]`, found `<typing.Annotated special form>`
+ pydantic/types.py:1511:20: error[invalid-return-type] Return type does not match returned value: expected `AnyType@__class_getitem__`, found `<typing.Annotated special form>`
+ pydantic/types.py:2257:12: error[invalid-return-type] Return type does not match returned value: expected `type[date]`, found `<typing.Annotated special form>`
+ pydantic/v1/fields.py:586:45: error[invalid-argument-type] Argument to function `lenient_issubclass` is incorrect: Expected `type[Any] | tuple[type[Any], ...] | None`, found `<typing.Annotated special form>`
+ pydantic/v1/types.py:832:20: error[invalid-return-type] Return type does not match returned value: expected `type[JsonWrapper]`, found `<typing.Annotated special form>`
- Found 763 diagnostics
+ Found 776 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ examples/contrib/ntlm_upstream_proxy.py:38:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `types.UnionType`
+ examples/contrib/ntlm_upstream_proxy.py:47:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `types.UnionType`
+ examples/contrib/ntlm_upstream_proxy.py:55:13: error[invalid-argument-type] Argument to bound method `add_option` is incorrect: Expected `type`, found `types.UnionType`
- Found 1890 diagnostics
+ Found 1893 diagnostics

mypy (https://github.com/python/mypy)
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:223:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/_typeshed/__init__.pyi:223:27: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/_typeshed/__init__.pyi:252:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/_typeshed/__init__.pyi:252:29: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/array.pyi:14:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/array.pyi:14:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/builtins.pyi:249:19: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/builtins.pyi:249:19: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/math.pyi:98:19: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `typing.Literal`
+ mypy/typeshed/stdlib/math.pyi:98:19: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<typing.Literal special form>`
- mypy/typeshed/stdlib/sqlite3/__init__.pyi:224:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `None`
+ mypy/typeshed/stdlib/sqlite3/__init__.pyi:224:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `None`
- mypy/typeshed/stdlib/sys/__init__.pyi:306:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `None`
+ mypy/typeshed/stdlib/sys/__init__.pyi:306:30: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `None`
- mypy/typeshed/stdlib/xml/sax/expatreader.pyi:12:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `typing.Literal` and `<class 'bool'>`
+ mypy/typeshed/stdlib/xml/sax/expatreader.pyi:12:24: error[unsupported-operator] Operator `|` is unsupported between objects of type `<typing.Literal special form>` and `<class 'bool'>`

Tanjun (https://github.com/FasterSpeeding/Tanjun)
+ tanjun/annotations.py:1855:16: error[invalid-return-type] Return type does not match returned value: expected `type[str]`, found `<typing.Annotated special form>`
+ tanjun/annotations.py:2110:86: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `int | float`, found `_NumberT@__getitem__ & ~float`
+ tanjun/annotations.py:2268:16: error[invalid-return-type] Return type does not match returned value: expected `type[PartialChannel]`, found `<typing.Annotated special form>`
- Found 150 diagnostics
+ Found 153 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/experimental/pydantic/error_type.py:62:26: error[invalid-type-form] Variable of type `list[Any]` is not allowed in a type expression
- Found 376 diagnostics
+ Found 377 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/prefect/cli/variable.py:163:57: error[invalid-argument-type] Argument is incorrect: Expected `str | None`, found `Unknown | str | int | ... omitted 5 union elements`
+ src/prefect/cli/variable.py:165:57: error[invalid-argument-type] Argument is incorrect: Expected `str`, found `Unknown | str | int | ... omitted 5 union elements`
+ src/prefect/client/schemas/objects.py:230:13: error[invalid-type-form] Variable of type `Literal["ResultRecordMetadata"]` is not allowed in a type expression
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `VersionInfo | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `VersionInfo | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `list[DeploymentScheduleCreate] | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `list[DeploymentScheduleCreate] | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `int | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `int | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `ConcurrencyOptions | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `ConcurrencyOptions | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `dict[str, Any] | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `dict[str, Any] | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `list[str] | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `list[str] | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `dict[str, Any] | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `dict[str, Any] | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `bool | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `bool | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `list[dict[str, Any]] | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `list[dict[str, Any]] | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `bool | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `bool | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `dict[str, Any] | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `dict[str, Any] | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `str | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID | None`, found `UUID | str | None | ... omitted 6 union elements`
+ src/prefect/deployments/runner.py:396:64: error[invalid-argument-type] Argument to bound method `create_deployment` is incorrect: Expected `UUID | None`, found `UUID | str | None | ... omitted 7 union elements`
- src/prefect/server/schemas/schedules.py:650:68: error[invalid-argument-type] Argument to function `_prepare_scheduling_start_and_end` is incorrect: Expected `str`, found `@Todo | None`
+ src/prefect/server/schemas/schedules.py:650:68: error[invalid-argument-type] Argument to function `_prepare_scheduling_start_and_end` is incorrect: Expected `str`, found `str | None`
- src/prefect/server/schemas/schedules.py:667:26: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str`, found `@Todo | None`
+ src/prefect/server/schemas/schedules.py:667:26: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str`, found `str | None`
- src/prefect/states.py:515:22: error[not-iterable] Object of type `~None & ~BaseException & ~str & ~Top[State[Any]]` is not iterable
- src/prefect/states.py:616:22: error[not-iterable] Object of type `~None & ~BaseException & ~str & ~Top[State[Any]]` is not iterable
- src/prefect/testing/utilities.py:241:9: error[invalid-argument-type] Argument to bound method `aread` is incorrect: Expected `str`, found `str | None`
- src/prefect/testing/utilities.py:258:50: error[invalid-argument-type] Argument to bound method `read_block_document` is incorrect: Expected `UUID`, found `UUID | None`
- src/prefect/testing/utilities.py:269:50: error[invalid-argument-type] Argument to bound method `read_block_document` is incorrect: Expected `UUID`, found `UUID | None`
- Found 3389 diagnostics
+ Found 3387 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/contrib/internal/pytest/_plugin_v2.py:1025:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[str, str, tuple[str, Mapping[str, bool]]] | None`, found `tuple[Literal[""], Literal[""], Literal[""]]`
- Found 8206 diagnostics
+ Found 8207 diagnostics

ibis (https://github.com/ibis-project/ibis)
+ ibis/backends/sql/compilers/base.py:590:13: error[invalid-assignment] Object of type `int | None` is not assignable to `str | None`
- ibis/backends/sql/compilers/base.py:592:43: error[invalid-argument-type] Argument to bound method `limit` is incorrect: Expected `int | None`, found `(str & ~Literal["default"]) | (@Todo & ~None)`
+ ibis/backends/sql/compilers/base.py:592:43: error[invalid-argument-type] Argument to bound method `limit` is incorrect: Expected `int | None`, found `str`
+ ibis/expr/datatypes/tests/test_core.py:719:31: error[invalid-argument-type] Argument to bound method `from_typehint` is incorrect: Expected `type`, found `<typing.Annotated special form>`
- Found 4238 diagnostics
+ Found 4240 diagnostics

static-frame (https://github.com/static-frame/static-frame)
- static_frame/core/container_util.py:196:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/container_util.py:354:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/container_util.py:524:40: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/container_util.py:594:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/frame.py:429:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/core/index_hierarchy_set_utils.py:126:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- static_frame/test/unit/test_store.py:249:50: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2007 diagnostics
+ Found 2000 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`

trio (https://github.com/python-trio/trio)
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`

sphinx (https://github.com/sphinx-doc/sphinx)
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`

prefect (https://github.com/PrefectHQ/prefect)
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`
- WARN expected `heap_size` to be provided by Salsa struct `TypeList`


```

</details>




---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-07 21:58_

---

_Comment by @github-actions[bot] on 2025-11-07 22:04_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 31 | 0 | 27 |
| `invalid-return-type` | 14 | 0 | 0 |
| `unused-ignore-comment` | 0 | 8 | 0 |
| `invalid-type-form` | 4 | 0 | 0 |
| `call-non-callable` | 2 | 0 | 0 |
| `not-iterable` | 2 | 0 | 0 |
| `invalid-assignment` | 1 | 0 | 0 |
| **Total** | **54** | **8** | **27** |

**[Full report with detailed diff](https://david-implicit-type-aliases-3fb2.ecosystem-663.pages.dev/diff)** ([timing results](https://david-implicit-type-aliases-3fb2.ecosystem-663.pages.dev/timing))




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-11-08 08:30_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-11-08 08:30_

---

_Marked ready for review by @sharkdp on 2025-11-08 09:01_

---

_Review requested from @carljm by @sharkdp on 2025-11-08 09:01_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-11-08 09:01_

---

_Review requested from @dcreager by @sharkdp on 2025-11-08 09:01_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:7865 on 2025-11-08 18:25_

I'm not wild about the displays for these two, because `int` as a type display means "instance of `int`", and `list[int]` means "inhabitant of the type `list[int]`"... but `KnownInstance` types here that would be displayed as `typing.Literal` or `typing.Annotated` aren't actually instances/inhabitants of `typing.Literal` or `typing.Annotated`. Neither of those symbols are classes at runtime, so it would be impossible to create an "instance of `typing.Literal`".

What about something like `<subscripted typing.Literal>` or `<typing.Literal special form>`?

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:9968 on 2025-11-08 19:06_

Now that you've added this logic to `infer/builder.rs`, I think it's possible to remove a bunch of logic from `infer/builder/type_expression.rs`. E.g. no tests fail with this diff applied to your branch:

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index f581038593..7c588b2364 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -8059,7 +8059,7 @@ impl<'db> InvalidTypeExpressionError<'db> {
     fn into_fallback_type(
         self,
         context: &InferContext,
-        node: &ast::Expr,
+        node: &impl Ranged,
         is_reachable: bool,
     ) -> Type<'db> {
         let InvalidTypeExpressionError {
diff --git a/crates/ty_python_semantic/src/types/infer/builder.rs b/crates/ty_python_semantic/src/types/infer/builder.rs
index 8887179930..37cb64ff0b 100644
--- a/crates/ty_python_semantic/src/types/infer/builder.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder.rs
@@ -9867,14 +9867,23 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
     }
 
     fn infer_subscript_load(&mut self, subscript: &ast::ExprSubscript) -> Type<'db> {
+        let value_ty = self.infer_expression(&subscript.value, TypeContext::default());
+        self.infer_subscript_load_impl(value_ty, subscript)
+    }
+
+    fn infer_subscript_load_impl(
+        &mut self,
+        value_ty: Type<'db>,
+        subscript: &ast::ExprSubscript,
+    ) -> Type<'db> {
         let ast::ExprSubscript {
             range: _,
             node_index: _,
-            value,
+            value: _,
             slice,
             ctx,
         } = subscript;
-        let value_ty = self.infer_expression(value, TypeContext::default());
+
         let mut constraint_keys = vec![];
 
         // If `value` is a valid reference, we attempt type narrowing by assignment.
diff --git a/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs b/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs
index 89acc81e44..7694839c15 100644
--- a/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs
+++ b/crates/ty_python_semantic/src/types/infer/builder/type_expression.rs
@@ -4,7 +4,7 @@ use ruff_python_ast as ast;
 use super::{DeferredExpressionState, TypeInferenceBuilder};
 use crate::types::diagnostic::{
     self, INVALID_TYPE_FORM, NON_SUBSCRIPTABLE, report_invalid_argument_number_to_special_form,
-    report_invalid_arguments_to_annotated, report_invalid_arguments_to_callable,
+    report_invalid_arguments_to_callable,
 };
 use crate::types::signatures::Signature;
 use crate::types::string_annotation::parse_string_annotation;
@@ -913,36 +913,13 @@ impl<'db> TypeInferenceBuilder<'db, '_> {
         let arguments_slice = &*subscript.slice;
         match special_form {
             SpecialFormType::Annotated => {
-                let ast::Expr::Tuple(ast::ExprTuple {
-                    elts: arguments, ..
-                }) = arguments_slice
-                else {
-                    report_invalid_arguments_to_annotated(&self.context, subscript);
-
-                    // `Annotated[]` with less than two arguments is an error at runtime.
-                    // However, we still treat `Annotated[T]` as `T` here for the purpose of
-                    // giving better diagnostics later on.
-                    // Pyright also does this. Mypy doesn't; it falls back to `Any` instead.
-                    return self.infer_type_expression(arguments_slice);
-                };
-
-                if arguments.len() < 2 {
-                    report_invalid_arguments_to_annotated(&self.context, subscript);
-                }
-
-                let [type_expr, metadata @ ..] = &arguments[..] else {
-                    for argument in arguments {
-                        self.infer_expression(argument, TypeContext::default());
-                    }
-                    self.store_expression_type(arguments_slice, Type::unknown());
-                    return Type::unknown();
-                };
-
-                for element in metadata {
-                    self.infer_expression(element, TypeContext::default());
-                }
-
-                let ty = self.infer_type_expression(type_expr);
+                let ty = self
+                    .infer_subscript_load_impl(
+                        Type::SpecialForm(SpecialFormType::Annotated),
+                        subscript,
+                    )
+                    .in_type_expression(db, self.scope(), None)
+                    .unwrap_or_else(|err| err.into_fallback_type(&self.context, subscript, true));
                 self.store_expression_type(arguments_slice, ty);
                 ty
             }
```

---

_@AlexWaygood approved on 2025-11-08 19:06_

Nice!

---

_@sharkdp reviewed on 2025-11-10 08:37_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer/builder.rs`:9968 on 2025-11-10 08:37_

Thank you for spotting this. I think my initial version here was not compatible, which is why I thought the duplication was necessary.

---

_@sharkdp reviewed on 2025-11-10 08:52_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:7865 on 2025-11-10 08:52_

`typing.Never` is also not an "instance of (a class) `Never`". I don't really see what's different for `Literal` and `Annotated`, when compared to some other special form / known instance types, but I changed it to your suggestion for now.

---

_Merged by @sharkdp on 2025-11-10 09:24_

---

_Closed by @sharkdp on 2025-11-10 09:24_

---

_Branch deleted on 2025-11-10 09:24_

---
