```yaml
number: 21267
title: "[ty] Avoid expression reinference for diagnostics"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: ibraheem/bidi-diagnostics
created_at: 2025-11-03T21:40:44Z
updated_at: 2025-11-25T17:24:02Z
url: https://github.com/astral-sh/ruff/pull/21267
synced_at: 2026-01-12T15:57:20Z
```

# [ty] Avoid expression reinference for diagnostics

---

_@ibraheemdev_

## Summary

We now use the type context for a lot of things, so re-inferring without type context actually makes diagnostics more confusing (in most cases).

---

_Review requested from @carljm by @ibraheemdev on 2025-11-03 21:40_

---

_Label `ty` added by @ibraheemdev on 2025-11-03 21:40_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-11-03 21:40_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-11-03 21:40_

---

_Review requested from @dcreager by @ibraheemdev on 2025-11-03 21:40_

---

_Comment by @github-actions[bot] on 2025-11-03 21:43_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @github-actions[bot] on 2025-11-03 21:44_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
aioredis (https://github.com/aio-libs/aioredis)
- aioredis/client.py:3430:34: error[invalid-assignment] Object of type `tuple[KeysView[object], ValuesView[object]]` is not assignable to `Sequence[bytes | str | memoryview[int]] | AbstractSet[AnyKeyT@_zaggregate]`
+ aioredis/client.py:3430:34: error[invalid-assignment] Object of type `KeysView[object]` is not assignable to `Sequence[bytes | str | memoryview[int]] | AbstractSet[AnyKeyT@_zaggregate]`
- aioredis/client.py:3430:34: error[invalid-assignment] Object of type `tuple[KeysView[object], ValuesView[object]]` is not assignable to `ValuesView[int | float] | None`
+ aioredis/client.py:3430:34: error[invalid-assignment] Object of type `ValuesView[object]` is not assignable to `ValuesView[int | float] | None`
- aioredis/client.py:4114:55: error[invalid-assignment] Object of type `dict[Unknown, Any | None]` is not assignable to `dict[bytes | str | memoryview[int], (dict[str, str], /) -> Awaitable[None]]`
+ aioredis/client.py:4114:55: error[invalid-assignment] Object of type `dict[bytes | str | memoryview[int], Any | None]` is not assignable to `dict[bytes | str | memoryview[int], (dict[str, str], /) -> Awaitable[None]]`

pip (https://github.com/pypa/pip)
- src/pip/_vendor/pkg_resources/__init__.py:1120:33: error[invalid-argument-type] Argument to bound method `evaluate` is incorrect: Expected `dict[str, str] | None`, found `dict[Unknown | str, Unknown | str | None]`
+ src/pip/_vendor/pkg_resources/__init__.py:1120:33: error[invalid-argument-type] Argument to bound method `evaluate` is incorrect: Expected `dict[str, str] | None`, found `dict[str, str | None]`
- src/pip/_vendor/urllib3/util/ssl_.py:86:32: error[invalid-assignment] Object of type `tuple[Literal[16777216], Literal[33554432]]` is not assignable to `Literal[Options.OP_NO_SSLv2]`
+ src/pip/_vendor/urllib3/util/ssl_.py:86:32: error[invalid-assignment] Object of type `Literal[16777216]` is not assignable to `Literal[Options.OP_NO_SSLv2]`
- src/pip/_vendor/urllib3/util/ssl_.py:86:32: error[invalid-assignment] Object of type `tuple[Literal[16777216], Literal[33554432]]` is not assignable to `Literal[Options.OP_NO_SSLv3]`
+ src/pip/_vendor/urllib3/util/ssl_.py:86:32: error[invalid-assignment] Object of type `Literal[33554432]` is not assignable to `Literal[Options.OP_NO_SSLv3]`

spack (https://github.com/spack/spack)
- lib/spack/spack/bootstrap/core.py:233:25: error[invalid-assignment] Object of type `tuple[partial[Unknown], dict[Unknown, Unknown]]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:233:25: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`
- lib/spack/spack/bootstrap/core.py:246:25: error[invalid-assignment] Object of type `tuple[partial[Unknown], dict[Unknown, Unknown]]` is not assignable to `QueryInfo`
+ lib/spack/spack/bootstrap/core.py:246:25: error[invalid-assignment] Object of type `dict[Unknown, Unknown]` is not assignable to `QueryInfo`

aiortc (https://github.com/aiortc/aiortc)
- src/aiortc/contrib/signaling.py:44:19: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | str | None | int]` is not assignable to `dict[str, int | str]`
+ src/aiortc/contrib/signaling.py:44:19: error[invalid-assignment] Object of type `dict[str, int | str | None]` is not assignable to `dict[str, int | str]`

graphql-core (https://github.com/graphql-python/graphql-core)
- src/graphql/utilities/build_client_schema.py:262:81: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | ((scalar_introspection: IntrospectionScalarType) -> GraphQLScalarType) | ((object_introspection: IntrospectionObjectType) -> GraphQLObjectType) | ... omitted 4 union elements]` is not assignable to `dict[str, (IntrospectionScalarType | IntrospectionObjectType | IntrospectionInterfaceType | ... omitted 3 union elements, /) -> GraphQLNamedType]`
+ src/graphql/utilities/build_client_schema.py:262:81: error[invalid-assignment] Object of type `dict[str, ((IntrospectionScalarType | IntrospectionObjectType | IntrospectionInterfaceType | ... omitted 3 union elements, /) -> GraphQLNamedType) | ((scalar_introspection: IntrospectionScalarType) -> GraphQLScalarType) | ((object_introspection: IntrospectionObjectType) -> GraphQLObjectType) | ... omitted 4 union elements]` is not assignable to `dict[str, (IntrospectionScalarType | IntrospectionObjectType | IntrospectionInterfaceType | ... omitted 3 union elements, /) -> GraphQLNamedType]`
- tests/pyutils/test_boxed_awaitabe_or_value.py:29:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[Future[Unknown]]` is not assignable to `BoxedAwaitableOrValue[int]`
+ tests/pyutils/test_boxed_awaitabe_or_value.py:29:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[int | Future[Unknown]]` is not assignable to `BoxedAwaitableOrValue[int]`
- tests/pyutils/test_boxed_awaitabe_or_value.py:36:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, int]]` is not assignable to `BoxedAwaitableOrValue[int]`
+ tests/pyutils/test_boxed_awaitabe_or_value.py:36:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[int | CoroutineType[Any, Any, int]]` is not assignable to `BoxedAwaitableOrValue[int]`
- tests/pyutils/test_boxed_awaitabe_or_value.py:41:51: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[Future[Unknown]]` is not assignable to `BoxedAwaitableOrValue[list[int]]`
+ tests/pyutils/test_boxed_awaitabe_or_value.py:41:51: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[list[int] | Future[Unknown]]` is not assignable to `BoxedAwaitableOrValue[list[int]]`
- tests/pyutils/test_boxed_awaitabe_or_value.py:56:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[CoroutineType[Any, Any, int]]` is not assignable to `BoxedAwaitableOrValue[int]`
+ tests/pyutils/test_boxed_awaitabe_or_value.py:56:45: error[invalid-assignment] Object of type `BoxedAwaitableOrValue[int | CoroutineType[Any, Any, int]]` is not assignable to `BoxedAwaitableOrValue[int]`
- tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[Unknown | str, Unknown | str]`
+ tests/type/test_definition.py:163:72: error[invalid-argument-type] Argument to function `parse_literal` is incorrect: Expected `ValueNode`, found `dict[str, Any] & dict[Unknown | str, Unknown | str]`

paasta (https://github.com/yelp/paasta)
- paasta_tools/utils.py:595:45: error[invalid-assignment] Object of type `list[Unknown | dict[Unknown | str, Unknown | str]]` is not assignable to `list[DockerParameter]`
+ paasta_tools/utils.py:595:45: error[invalid-assignment] Object of type `list[DockerParameter | dict[Unknown | str, Unknown | str]]` is not assignable to `list[DockerParameter]`

pylint (https://github.com/pycqa/pylint)
- pylint/checkers/classes/class_checker.py:493:43: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/classes/class_checker.py:493:43: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/deprecated.py:37:71: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str, dict[Unknown | str, Unknown | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/deprecated.py:46:68: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/deprecated.py:55:68: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/deprecated.py:64:70: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/deprecated.py:73:67: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/deprecated.py:82:71: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/deprecated.py:37:71: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/deprecated.py:46:68: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/deprecated.py:55:68: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/deprecated.py:64:70: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/deprecated.py:73:67: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/deprecated.py:82:71: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]] | bool]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/exceptions.py:62:43: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/exceptions.py:62:43: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/format.py:54:43: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str] | tuple[str, str, str, dict[Unknown | str, Unknown | str]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/format.py:54:43: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | str]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/imports.py:227:43: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]] | tuple[str, str, str]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/imports.py:227:43: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/stdlib.py:488:47: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | tuple[int, int]]] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/stdlib.py:488:47: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | tuple[int, int]]] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/typecheck.py:220:43: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]] | tuple[str, str, str]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/typecheck.py:220:43: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
- pylint/checkers/variables.py:351:43: error[invalid-assignment] Object of type `dict[Unknown | str, Unknown | tuple[str, str, str] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`
+ pylint/checkers/variables.py:351:43: error[invalid-assignment] Object of type `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions] | tuple[str, str, str, dict[Unknown | str, Unknown | list[Unknown | tuple[str, str]]]]]` is not assignable to `dict[str, tuple[str, str, str] | tuple[str, str, str, ExtraMessageOptions]]`

alerta (https://github.com/alerta/alerta)
- alerta/auth/basic.py:31:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/basic.py:31:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
- alerta/auth/basic.py:85:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/basic.py:85:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
- alerta/auth/decorators.py:142:60: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/decorators.py:142:60: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
- alerta/auth/decorators.py:147:57: error[invalid-argument-type] Argument to function `get_customers` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/decorators.py:147:57: error[invalid-argument-type] Argument to function `get_customers` is incorrect: Expected `list[str]`, found `list[str | None]`
- alerta/auth/github.py:85:115: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/github.py:85:115: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
- alerta/auth/oidc.py:192:95: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/oidc.py:192:95: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
- alerta/auth/saml.py:100:98: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/auth/saml.py:100:98: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
- alerta/views/users.py:33:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[Unknown | str | None]`
+ alerta/views/users.py:33:48: error[invalid-argument-type] Argument to function `not_authorized` is incorrect: Expected `list[str]`, found `list[str | None]`
- tests/test_scopes.py:81:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:81:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:82:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:82:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:83:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:83:71: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:85:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:85:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:86:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:86:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:87:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:87:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:89:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:89:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:90:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:90:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:92:59: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:92:59: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:93:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:93:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:94:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:94:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:95:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:95:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:96:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:96:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:97:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:97:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:99:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:99:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:100:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:100:62: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`
- tests/test_scopes.py:101:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[Unknown | str]`
+ tests/test_scopes.py:101:72: error[invalid-argument-type] Argument to bound method `is_in_scope` is incorrect: Expected `list[Scope]`, found `list[str | Unknown]`

scrapy (https://github.com/scrapy/scrapy)
- tests/test_downloadermiddleware_cookies.py:363:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | bytes]`
+ tests/test_downloadermiddleware_cookies.py:363:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | bytes] & dict[Unknown | str, Unknown | bytes]`
- tests/test_downloadermiddleware_cookies.py:368:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | bytes]`
+ tests/test_downloadermiddleware_cookies.py:368:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | bytes] & dict[Unknown | str, Unknown | bytes]`
- tests/test_downloadermiddleware_cookies.py:438:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | bool]`
+ tests/test_downloadermiddleware_cookies.py:438:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | bool] & dict[Unknown | str, Unknown | bool]`
- tests/test_downloadermiddleware_cookies.py:443:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | float]`
+ tests/test_downloadermiddleware_cookies.py:443:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | float] & dict[Unknown | str, Unknown | float]`
- tests/test_downloadermiddleware_cookies.py:448:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[Unknown | str, Unknown | int]`
+ tests/test_downloadermiddleware_cookies.py:448:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `dict[str, str | int] & dict[Unknown | str, Unknown | int]`
- tests/test_http_request.py:667:58: error[invalid-argument-type] Argument to bound method `from_response` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[Unknown | str, Unknown | None]`
+ tests/test_http_request.py:667:58: error[invalid-argument-type] Argument to bound method `from_response` is incorrect: Expected `dict[str, Iterable[str]] | list[tuple[str, Iterable[str]]] | None`, found `dict[str, Iterable[str] | None] & dict[Unknown | str, Unknown | None]`
- tests/test_pipeline_files.py:368:28: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[str]`
+ tests/test_pipeline_files.py:368:28: error[invalid-assignment] Object of type `dataclasses.Field[list[str] | <class 'list'>]` is not assignable to `list[str]`
- tests/test_pipeline_files.py:369:35: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_files.py:369:35: error[invalid-assignment] Object of type `dataclasses.Field[list[dict[str, str]] | <class 'list'>]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_files.py:371:35: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[str]`
+ tests/test_pipeline_files.py:371:35: error[invalid-assignment] Object of type `dataclasses.Field[list[str] | <class 'list'>]` is not assignable to `list[str]`
- tests/test_pipeline_files.py:372:42: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_files.py:372:42: error[invalid-assignment] Object of type `dataclasses.Field[list[dict[str, str]] | <class 'list'>]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_images.py:332:29: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[str]`
+ tests/test_pipeline_images.py:332:29: error[invalid-assignment] Object of type `dataclasses.Field[list[str] | <class 'list'>]` is not assignable to `list[str]`
- tests/test_pipeline_images.py:333:36: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_images.py:333:36: error[invalid-assignment] Object of type `dataclasses.Field[list[dict[str, str]] | <class 'list'>]` is not assignable to `list[dict[str, str]]`
- tests/test_pipeline_images.py:335:36: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[str]`
+ tests/test_pipeline_images.py:335:36: error[invalid-assignment] Object of type `dataclasses.Field[list[str] | <class 'list'>]` is not assignable to `list[str]`
- tests/test_pipeline_images.py:336:43: error[invalid-assignment] Object of type `<class 'list'>` is not assignable to `list[dict[str, str]]`
+ tests/test_pipeline_images.py:336:43: error[invalid-assignment] Object of type `dataclasses.Field[list[dict[str, str]] | <class 'list'>]` is not assignable to `list[dict[str, str]]`
- tests/test_utils_request.py:377:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `list[Unknown | dict[Unknown | str, Unknown | str]]`
+ tests/test_utils_request.py:377:13: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `dict[str, str] | list[VerboseCookie] | None`, found `list[VerboseCookie | dict[Unknown | str, Unknown | str]] & list[Unknown | dict[Unknown | str, Unknown | str]]`

sockeye (https://github.com/awslabs/sockeye)
- test/unit/test_output_handler.py:46:43: error[invalid-argument-type] Argument is incorrect: Expected `list[list[int | float]] | None`, found `list[Unknown | float]`
+ test/unit/test_output_handler.py:46:43: error[invalid-argument-type] Argument is incorrect: Expected `list[list[int | float]] | None`, found `list[list[int | float] | float]`

ignite (https://github.com/pytorch/ignite)
- tests/ignite/handlers/test_base_logger.py:265:5: error[invalid-assignment] Object of type `list[CallableEventWithFilter | Literal["unknown"]]` is not assignable to attribute `_events` of type `list[CallableEventWithFilter]`
+ tests/ignite/handlers/test_base_logger.py:265:5: error[invalid-assignment] Object of type `list[CallableEventWithFilter | str]` is not assignable to attribute `_events` of type `list[CallableEventWithFilter]`
- tests/ignite/handlers/test_param_scheduler.py:349:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[Unknown | LinearCyclicalScheduler | int]`
+ tests/ignite/handlers/test_param_scheduler.py:349:25: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | int]`
- tests/ignite/handlers/test_param_scheduler.py:355:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `list[Unknown | int | float]`
+ tests/ignite/handlers/test_param_scheduler.py:355:77: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[int]`, found `list[int | float]`
- tests/ignite/handlers/test_param_scheduler.py:367:84: error[invalid-argument-type] Argument to bound method `simulate_values` is incorrect: Expected `list[str] | tuple[str] | None`, found `list[Unknown | int]`
+ tests/ignite/handlers/test_param_scheduler.py:367:84: error[invalid-argument-type] Argument to bound method `simulate_values` is incorrect: Expected `list[str] | tuple[str] | None`, found `list[str | int]`
- tests/ignite/handlers/test_param_scheduler.py:768:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[float]]`
+ tests/ignite/handlers/test_param_scheduler.py:768:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, int | float] | tuple[float]]`
- tests/ignite/handlers/test_param_scheduler.py:771:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[int, float] | tuple[float]]`
+ tests/ignite/handlers/test_param_scheduler.py:771:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, int | float] | tuple[float]]`
- tests/ignite/handlers/test_param_scheduler.py:777:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[float, int]]`
+ tests/ignite/handlers/test_param_scheduler.py:777:42: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, int | float] | tuple[float, int]]`
- tests/ignite/handlers/test_param_scheduler.py:1171:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[Unknown | int]`
+ tests/ignite/handlers/test_param_scheduler.py:1171:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | int]`
- tests/ignite/handlers/test_param_scheduler.py:1174:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[Unknown | LinearCyclicalScheduler | str]`
+ tests/ignite/handlers/test_param_scheduler.py:1174:29: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[ParamScheduler]`, found `list[ParamScheduler | str]`
- tests/ignite/handlers/test_param_scheduler.py:1180:72: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `list[Unknown | int]`
+ tests/ignite/handlers/test_param_scheduler.py:1180:72: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[str] | None`, found `list[str | int]`
- tests/ignite/handlers/test_state_param_scheduler.py:151:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[float]]`
+ tests/ignite/handlers/test_state_param_scheduler.py:151:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, int | float] | tuple[float]]`
- tests/ignite/handlers/test_state_param_scheduler.py:154:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[int, float] | tuple[float]]`
+ tests/ignite/handlers/test_state_param_scheduler.py:154:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, int | float] | tuple[float]]`
- tests/ignite/handlers/test_state_param_scheduler.py:160:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[Unknown | tuple[float, int]]`
+ tests/ignite/handlers/test_state_param_scheduler.py:160:76: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `list[tuple[int, int | float]]`, found `list[tuple[int, int | float] | tuple[float, int]]`

mkosi (https://github.com/systemd/mkosi)
- mkosi/qemu.py:343:33: error[invalid-assignment] Object of type `list[Unknown | Path | None | str]` is not assignable to `list[Path | str]`
+ mkosi/qemu.py:343:33: error[invalid-assignment] Object of type `list[Path | str | None]` is not assignable to `list[Path | str]`
- mkosi/sysupdate.py:70:33: error[invalid-assignment] Object of type `list[Unknown | Path | None | str]` is not assignable to `list[Path | str]`
+ mkosi/sysupdate.py:70:33: error[invalid-assignment] Object of type `list[Path | str | None]` is not assignable to `list[Path | str]`

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- test/mitmproxy/addons/test_next_layer.py:492:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[Unknown | <class 'HttpProxy'> | <class 'ClientTLSLayer'> | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:492:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
- test/mitmproxy/addons/test_next_layer.py:524:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[Unknown | <class 'HttpProxy'> | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:524:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
- test/mitmproxy/addons/test_next_layer.py:536:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[Unknown | <class 'HttpProxy'> | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:536:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
- test/mitmproxy/addons/test_next_layer.py:554:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[Unknown | <class 'HttpProxy'> | partial[Unknown] | <class 'ServerTLSLayer'> | <class 'ClientTLSLayer'>]`
+ test/mitmproxy/addons/test_next_layer.py:554:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
- test/mitmproxy/addons/test_next_layer.py:575:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[Unknown | <class 'HttpProxy'> | partial[Unknown]]`
+ test/mitmproxy/addons/test_next_layer.py:575:13: error[invalid-argument-type] Argument is incorrect: Expected `list[type[Layer]]`, found `list[type[Layer] | partial[Unknown]]`
- test/mitmproxy/addons/test_tlsconfig.py:476:13: error[invalid-assignment] Object of type `list[Layer | Literal[123]]` is not assignable to attribute `layers` of type `list[Layer]`
+ test/mitmproxy/addons/test_tlsconfig.py:476:13: error[invalid-assignment] Object of type `list[Layer | int]` is not assignable to attribute `layers` of type `list[Layer]`
- test/mitmproxy/proxy/layers/http/test_http.py:84:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:84:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:92:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:92:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:98:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:98:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:150:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:150:33: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Connection | _Placeholder[Connection]`
- test/mitmproxy/proxy/layers/http/test_http.py:154:34: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:154:34: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:198:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:198:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:212:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:212:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:272:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:272:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:314:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:314:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:318:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:318:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:322:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:322:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:846:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:846:36: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:965:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:965:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:1029:37: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1029:37: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:1296:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1296:32: error[invalid-argument-type] Argument is incorrect: Expected `NextLayer`, found `(NextLayer & Unknown) | (_Placeholder[NextLayer] & Unknown) | (NextLayer & _Placeholder[Unknown]) | (_Placeholder[NextLayer] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:1454:41: error[invalid-argument-type] Argument is incorrect: Expected `TCPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1454:41: error[invalid-argument-type] Argument is incorrect: Expected `TCPFlow`, found `(TCPFlow & Unknown) | (_Placeholder[TCPFlow] & Unknown) | (TCPFlow & _Placeholder[Unknown]) | (_Placeholder[TCPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:1641:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1641:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Connection | _Placeholder[Connection]`
- test/mitmproxy/proxy/layers/http/test_http.py:1749:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1749:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:1753:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1753:35: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:1794:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1794:33: error[invalid-argument-type] Argument is incorrect: Expected `HTTPFlow`, found `(HTTPFlow & Unknown) | (_Placeholder[HTTPFlow] & Unknown) | (HTTPFlow & _Placeholder[Unknown]) | (_Placeholder[HTTPFlow] & _Placeholder[Unknown])`
- test/mitmproxy/proxy/layers/http/test_http.py:1796:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1796:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Connection | _Placeholder[Connection]`
- test/mitmproxy/proxy/layers/http/test_http.py:1838:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http.py:1838:27: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Connection`, found `Connection | _Placeholder[Connection]`
- test/mitmproxy/proxy/layers/http/test_http1.py:59:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http1.py:59:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
- test/mitmproxy/proxy/layers/http/test_http1.py:81:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http1.py:81:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
- test/mitmproxy/proxy/layers/http/test_http1.py:104:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `Unknown | _Placeholder[Unknown]`
+ test/mitmproxy/proxy/layers/http/test_http1.py:104:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `HttpEvent | _Placeholder[HttpEvent]`
- test/mitmproxy/proxy/layers/http/test_http1.py:110:28: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `HttpEvent`, found `Unknown | _P

... (truncated 560 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Comment by @ibraheemdev on 2025-11-03 21:50_

I may have underestimated the size of those intersection types...

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-03 21:56_

---

_Comment by @github-actions[bot] on 2025-11-03 22:02_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 0 | 211 |
| `invalid-assignment` | 0 | 0 | 37 |
| **Total** | **0** | **0** | **248** |

**[Full report with detailed diff](https://ibraheem-bidi-diagnostics.ecosystem-663.pages.dev/diff)** ([timing results](https://ibraheem-bidi-diagnostics.ecosystem-663.pages.dev/timing))


---

_Comment by @MichaReiser on 2025-11-10 12:35_

Can you take a look at the failing mdtest

---

_Comment by @carljm on 2025-11-21 21:56_

@ibraheemdev Not sure how much work is required to get this ready -- it has some failing mdtests (and now some conflicts), but if it's not a lot, it would make sense to get it landed for the beta

---

_Comment by @ibraheemdev on 2025-11-21 22:07_

CI was failing spuriously because of an apparent non-determinism in intersection type ordering. I suspect that was because of the ad-hoc `TypedDict` assignability special cases, which should be resolved now after a rebase.

---

_Comment by @carljm on 2025-11-22 00:00_

> which should be resolved now after a rebase

Not sure if you did a rebase, but doesn't look like it was pushed?

---

_Comment by @ibraheemdev on 2025-11-22 01:50_

Hmm, looks like the problem is still there. It seems perhaps to not me non-deterministic but just platform specific? The tests pass locally on my machine.

---

_Comment by @carljm on 2025-11-24 22:41_

Hmm, that's unfortunate! I'm assuming this is in your court to look further into the cause of those platform-specific failures, when you have a chance, but ping if you could use another pair of eyes on it

---

_Comment by @ibraheemdev on 2025-11-25 02:35_

I rebased again as well as updated my local rustc version, and the test results have changed locally (not sure which of those caused it), so hopefully the issue is resolved.

The diagnostics in cases where we perform multi-inference are not great -- the intersected types can be quite large, despite most of the elements typically being irrelevant. However, our current solution of reinference can lead to an assignable type being outputted as part of an invalid-assignment error, so I think we should still merge this and work on improving those diagnostics over time.

---

_Comment by @carljm on 2025-11-25 02:42_

Yes, the test failures repro for me:

```

failures:

---- mdtest__bidirectional stdout ----

bidirectional.md - Bidirectional type i… - Class constructor pa… (f53c702724d63166)

  crates/ty_python_semantic/resources/mdtest/bidirectional.md:273 unmatched assertion: error: [invalid-argument-type] "Argument to function `__new__` is incorrect: Expected `list[int | str]`, found `list[int | str | list[Unknown]] & list[int | None | list[Unknown]] & list[list[Unknown]]`"
  crates/ty_python_semantic/resources/mdtest/bidirectional.md:273 unmatched assertion: error: [invalid-argument-type] "Argument to bound method `__init__` is incorrect: Expected `list[int | None]`, found `list[int | str | list[Unknown]] & list[int | None | list[Unknown]] & list[list[Unknown]]`"
  crates/ty_python_semantic/resources/mdtest/bidirectional.md:273 unexpected error: 3 [invalid-argument-type] "Argument to function `__new__` is incorrect: Expected `list[int | str]`, found `list[int | None | list[Unknown]] & list[int | str | list[Unknown]] & list[list[Unknown]]`"
  crates/ty_python_semantic/resources/mdtest/bidirectional.md:273 unexpected error: 3 [invalid-argument-type] "Argument to bound method `__init__` is incorrect: Expected `list[int | None]`, found `list[int | None | list[Unknown]] & list[int | str | list[Unknown]] & list[list[Unknown]]`"

To rerun this specific test, set the environment variable: MDTEST_TEST_FILTER='bidirectional.md - Bidirectional type i… - Class constructor pa… (f53c702724d63166)'
MDTEST_TEST_FILTER='bidirectional.md - Bidirectional type i… - Class constructor pa… (f53c702724d63166)' cargo test -p ty_python_semantic --test mdtest -- mdtest__bidirectional
```

---

_Comment by @ibraheemdev on 2025-11-25 02:44_

Sorry, I had forgotten to commit the changes after the last rebase. CI should be passing now.

---

_@carljm approved on 2025-11-25 17:01_

Looks great! Spot-checked the ecosystem and I agree, in the vast majority of cases this gives a better diagnostic. Thank you!

---

_Merged by @carljm on 2025-11-25 17:24_

---

_Closed by @carljm on 2025-11-25 17:24_

---

_Branch deleted on 2025-11-25 17:24_

---
