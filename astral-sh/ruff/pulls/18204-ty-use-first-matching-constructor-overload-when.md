```yaml
number: 18204
title: "[ty] Use first matching constructor overload when inferring specializations"
type: pull_request
state: merged
author: dcreager
labels: []
assignees: []
merged: true
base: main
head: dcreager/single-constructor-overload
created_at: 2025-05-19T17:59:40Z
updated_at: 2025-05-19T19:22:23Z
url: https://github.com/astral-sh/ruff/pull/18204
synced_at: 2026-01-12T15:56:14Z
```

# [ty] Use first matching constructor overload when inferring specializations

---

_@dcreager_

This is a follow-on to #18155.  For the example raised in https://github.com/astral-sh/ty/issues/370:

```py
import tempfile

with tempfile.TemporaryDirectory() as tmp: ...
```

the new logic would notice that both overloads of `TemporaryDirectory` match, and combine their specializations, resulting in an inferred type of `str | bytes`.

This PR updates the logic to match our other handling of other calls, where we only keep the _first_ matching overload. The result for this example then becomes `str`, matching the runtime behavior. (We still do not implement the full [overload resolution algorithm](https://typing.python.org/en/latest/spec/overload.html#overload-call-evaluation) from the spec.)

---

_Review requested from @carljm by @dcreager on 2025-05-19 17:59_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-19 17:59_

---

_Review requested from @sharkdp by @dcreager on 2025-05-19 17:59_

---

_@dcreager reviewed on 2025-05-19 18:00_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:364 on 2025-05-19 18:00_

Without the code changes, this test mimics the `TemporaryDirectory` behavior, returning a union specialization

---

_Comment by @github-actions[bot] on 2025-05-19 18:49_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[invalid-assignment] mypy_primer/main.py:303:5: Object of type `dict[str | bytes, str | bytes]` is not assignable to `dict[str, TypeCheckResult]`
- error[invalid-assignment] mypy_primer/main.py:335:9: Object of type `dict[str | bytes, str | bytes]` is not assignable to `dict[str, TypeCheckResult]`
- Found 19 diagnostics
+ Found 17 diagnostics

com2ann (https://github.com/ilevkivskyi/com2ann)
- error[call-non-callable] src/com2ann.py:646:23: Method `__getitem__` of type `Unknown | (bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown)` is not callable on object of type `Unknown | defaultdict[str, Unknown]`
- Found 13 diagnostics
+ Found 12 diagnostics

paroxython (https://github.com/laowantong/paroxython)
- error[call-non-callable] paroxython/filter_programs.py:441:17: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- Found 16 diagnostics
+ Found 15 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- error[call-non-callable] more_itertools/more.py:814:13: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- Found 51 diagnostics
+ Found 50 diagnostics

alerta (https://github.com/alerta/alerta)
- error[call-non-callable] alerta/database/backends/mongodb/base.py:857:13: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/mongodb/base.py:862:13: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/mongodb/base.py:874:40: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/mongodb/base.py:875:38: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/mongodb/base.py:876:44: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/postgres/base.py:683:17: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/postgres/base.py:685:17: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/postgres/base.py:687:17: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/postgres/base.py:695:40: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/postgres/base.py:696:38: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] alerta/database/backends/postgres/base.py:697:26: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- Found 482 diagnostics
+ Found 471 diagnostics

kopf (https://github.com/nolar/kopf)
- error[invalid-assignment] kopf/_cogs/structs/credentials.py:221:9: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[int, list[tuple[Unknown, VaultItem]]]`
- error[invalid-return-type] kopf/_core/actions/invocation.py:58:16: Return type does not match returned value: expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] kopf/_core/actions/invocation.py:63:16: Return type does not match returned value: expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] kopf/_core/actions/invocation.py:68:16: Return type does not match returned value: expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-assignment] kopf/_core/actions/invocation.py:113:9: Object of type `dict[str | bytes, str | bytes]` is not assignable to `Mapping[str, Any] | None`
- error[invalid-assignment] kopf/_core/actions/invocation.py:116:9: Object of type `dict[str | bytes, str | bytes]` is not assignable to `Mapping[str, Any] | None`
- error[invalid-assignment] kopf/_core/actions/progression.py:180:9: Object of type `dict[str | bytes, str | bytes]` is not assignable to `dict[Unknown, HandlerState]`
- error[invalid-assignment] kopf/_core/actions/progression.py:190:9: Object of type `dict[str | bytes, str | bytes]` is not assignable to `dict[Unknown, HandlerState]`
- error[invalid-return-type] kopf/_core/intents/causes.py:112:16: Return type does not match returned value: expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] kopf/_core/intents/causes.py:166:16: Return type does not match returned value: expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] kopf/_core/intents/causes.py:202:16: Return type does not match returned value: expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] kopf/_core/intents/causes.py:223:16: Return type does not match returned value: expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] kopf/_core/intents/causes.py:256:16: Return type does not match returned value: expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] kopf/_kits/webhooks.py:215:73: Argument to bound method `__call__` is incorrect: Expected `Mapping[str, str] | None`, found `dict[str | bytes, str | bytes]`
- Found 181 diagnostics
+ Found 167 diagnostics

starlette (https://github.com/encode/starlette)
- error[invalid-argument-type] starlette/middleware/cors.py:138:69: Argument to bound method `__init__` is incorrect: Expected `Mapping[str, str] | None`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] starlette/middleware/cors.py:140:57: Argument to bound method `__init__` is incorrect: Expected `Mapping[str, str] | None`, found `dict[str | bytes, str | bytes]`
+ warning[possibly-unbound-attribute] starlette/requests.py:114:20: Attribute `endswith` on type `Unknown | None` is possibly unbound
+ error[unsupported-operator] starlette/requests.py:115:17: Operator `+=` is unsupported between objects of type `None` and `Literal["/"]`
- error[invalid-assignment] starlette/routing.py:259:17: Object of type `dict[str | bytes, str | bytes]` is not assignable to `dict[str, Any]`
- error[invalid-assignment] starlette/routing.py:344:17: Object of type `dict[str | bytes, str | bytes]` is not assignable to `dict[str, Any]`
- error[invalid-assignment] starlette/routing.py:411:17: Object of type `dict[str | bytes, str | bytes]` is not assignable to `dict[str, Any]`
- error[no-matching-overload] starlette/schemas.py:133:9: No overload of bound method `setdefault` matches arguments
- warning[call-possibly-unbound-method] starlette/schemas.py:145:13: Method `__getitem__` of type `str | int` is possibly unbound
- error[invalid-return-type] starlette/schemas.py:147:16: Return type does not match returned value: expected `dict[str, Any]`, found `dict[str | bytes, str | bytes]`
- Found 182 diagnostics
+ Found 176 diagnostics

kornia (https://github.com/kornia/kornia)
- error[invalid-return-type] kornia/utils/image_print.py:316:12: Return type does not match returned value: expected `str`, found `Unknown | str | bytes`
- Found 970 diagnostics
+ Found 969 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[invalid-return-type] strawberry/http/base.py:68:16: Return type does not match returned value: expected `dict[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] strawberry/http/parse_content_type.py:15:12: Return type does not match returned value: expected `tuple[str, dict[str, str]]`, found `tuple[@Todo(map_with_boundness: intersections with negative contributions), dict[str | bytes, str | bytes]]`
- error[invalid-assignment] strawberry/relay/fields.py:128:13: Object of type `defaultdict[str, Unknown]` is not assignable to `defaultdict[type[Node], list[str]]`
- Found 439 diagnostics
+ Found 436 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-assignment] src/trio/_core/_tests/test_run.py:2673:5: Object of type `<class 'RuntimeError'> | RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2] | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not assignable to `type[RuntimeError] | RaisesGroup[RuntimeError]`
+ error[invalid-assignment] src/trio/_core/_tests/test_run.py:2673:5: Object of type `<class 'RuntimeError'> | RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2]]` is not assignable to `type[RuntimeError] | RaisesGroup[RuntimeError]`
- error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:169:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2] | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
+ error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:169:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
- error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:170:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2] | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
+ error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:170:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
- error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:171:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2] | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
+ error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:171:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2]]` is not assignable to `RaisesGroup[ValueError]`
- error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:182:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2] | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not assignable to `RaisesGroup[Exception]`
- error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:183:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2] | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not assignable to `RaisesGroup[Exception]`
- error[invalid-assignment] src/trio/_tests/type_tests/raisesgroup.py:184:5: Object of type `RaisesGroup[ExcT_1 | ExceptionGroup[ExcT_2] | BaseExcT_1 | BaseExceptionGroup[BaseExcT_2]]` is not assignable to `RaisesGroup[Exception]`
- Found 1098 diagnostics
+ Found 1095 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- error[invalid-assignment] src/graphql/pyutils/group_by.py:16:5: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[K, list[T]]`
- error[invalid-assignment] src/graphql/type/validate.py:111:9: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[GraphQLObjectType, list[OperationType]]`
- Found 412 diagnostics
+ Found 410 diagnostics

dedupe (https://github.com/dedupeio/dedupe)
- error[invalid-return-type] dedupe/clustering.py:210:12: Return type does not match returned value: expected `tuple[dict[int, Unknown], @Todo(unknown type subscript), int]`, found `tuple[dict[str | bytes, str | bytes], ndarray[tuple[int], Literal["f4"]], int]`
- error[invalid-assignment] dedupe/clustering.py:241:13: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[int, list[int]]`
- Found 65 diagnostics
+ Found 63 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[no-matching-overload] ignite/distributed/auto.py:269:15: No overload of class `type` matches arguments
- warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:53:19: Attribute `read` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:53:19: Attribute `read` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:53:48: Attribute `read` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:53:48: Attribute `read` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:54:92: Attribute `read` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:54:92: Attribute `read` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:55:16: Attribute `read` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:55:16: Attribute `read` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:55:45: Attribute `read` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/distributed/test_launcher.py:55:45: Attribute `read` on type `IO[bytes] | None` is possibly unbound
- error[call-non-callable] tests/ignite/handlers/test_clearml_logger.py:907:19: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- Found 2231 diagnostics
+ Found 2229 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- warning[possibly-unbound-attribute] setup.py:28:19: Attribute `decode` on type `bytes | str` is possibly unbound
- error[unresolved-attribute] sockeye/model.py:497:24: Type `str | bytes` has no attribute `size`
- error[unresolved-attribute] sockeye/model.py:499:28: Type `str | bytes` has no attribute `size`
- error[unresolved-attribute] sockeye/model.py:500:17: Type `str | bytes` has no attribute `data`
- error[unresolved-attribute] sockeye/utils.py:587:29: Type `str` has no attribute `shape`
- error[call-non-callable] sockeye_contrib/rouge.py:102:17: Method `__getitem__` of type `bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `dict[str, Unknown]`
- error[call-non-callable] sockeye_contrib/rouge.py:104:17: Method `__getitem__` of type `bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `dict[str, Unknown]`
- error[call-non-callable] sockeye_contrib/rouge.py:104:31: Method `__getitem__` of type `bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `dict[str, Unknown]`
- error[call-non-callable] sockeye_contrib/rouge.py:106:17: Method `__getitem__` of type `bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `dict[str, Unknown]`
- error[call-non-callable] sockeye_contrib/rouge.py:106:35: Method `__getitem__` of type `bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `dict[str, Unknown]`
- error[call-non-callable] sockeye_contrib/rouge.py:106:52: Method `__getitem__` of type `bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `dict[str, Unknown]`
- error[invalid-argument-type] test/unit/test_knn.py:104:66: Argument to bound method `__init__` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] test/unit/test_params.py:33:47: Argument to function `cleanup_params_files` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] test/unit/test_params.py:46:47: Argument to function `cleanup_params_files` is incorrect: Expected `str`, found `str | bytes`
- error[unresolved-attribute] test/unit/test_params.py:80:29: Type `str | bytes` has no attribute `data`
- error[invalid-argument-type] test/unit/test_quantize.py:51:27: Argument to bound method `save_config` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] test/unit/test_quantize.py:73:48: Argument to function `load_model` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] test/unit/test_quantize.py:73:48: Argument to function `load_model` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] test/unit/test_quantize.py:73:48: Argument to function `load_model` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] test/unit/test_quantize.py:73:48: Argument to function `load_model` is incorrect: Expected `str`, found `str | bytes`
- Found 369 diagnostics
+ Found 349 diagnostics

comtypes (https://github.com/enthought/comtypes)
- error[unsupported-operator] comtypes/tools/codegenerator/namespaces.py:248:13: Operator `-=` is unsupported between objects of type `str` and `Literal[1]`
- error[unsupported-operator] comtypes/tools/codegenerator/namespaces.py:248:13: Operator `-=` is unsupported between objects of type `bytes` and `Literal[1]`
- Found 603 diagnostics
+ Found 601 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-return-type] pydantic/_internal/_generics.py:225:12: Return type does not match returned value: expected `dict[TypeVar, Any] | None`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] pydantic/_internal/_generics.py:243:12: Return type does not match returned value: expected `dict[TypeVar, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] pydantic/main.py:1525:58: Argument to function `_copy_and_set_values` is incorrect: Expected `dict[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] pydantic/v1/generics.py:166:75: Argument to function `_prepare_model_fields` is incorrect: Expected `Mapping[Any, type]`, found `dict[str | bytes, str | bytes]`
- error[call-non-callable] pydantic/v1/schema.py:961:9: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- Found 767 diagnostics
+ Found 762 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[unsupported-operator] porcupine/plugins/autocomplete.py:296:13: Unary operator `-` is unsupported for type `Unknown | str | bytes`
- error[invalid-argument-type] porcupine/plugins/autocomplete.py:306:13: Argument is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] porcupine/plugins/autocomplete.py:309:13: Argument is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] porcupine/plugins/autocomplete.py:310:13: Argument is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] porcupine/plugins/autocomplete.py:311:13: Argument is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] porcupine/plugins/langserver.py:706:29: Argument to bound method `__init__` is incorrect: Expected `Popen[bytes]`, found `Popen[bytes | str]`
- error[unsupported-operator] porcupine/plugins/longlinemarker.py:60:47: Operator `+` is unsupported between objects of type `Literal["#"]` and `str | int`
- error[invalid-argument-type] porcupine/plugins/run/common.py:68:74: Argument to bound method `startswith` is incorrect: Expected `str | tuple[str, ...]`, found `str | bytes`
- error[invalid-return-type] porcupine/plugins/run/common.py:71:12: Return type does not match returned value: expected `dict[str, str]`, found `dict[str | bytes, str | bytes]`
- Found 90 diagnostics
+ Found 81 diagnostics

nox (https://github.com/wntrblm/nox)
- error[invalid-return-type] nox/_parametrize.py:66:16: Return type does not match returned value: expected `dict[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] nox/popen.py:105:38: Argument to function `shutdown_process` is incorrect: Expected `Popen[bytes]`, found `Popen[bytes | str]`
- error[invalid-argument-type] nox/popen.py:105:38: Argument to function `shutdown_process` is incorrect: Expected `Popen[bytes]`, found `Popen[bytes | str]`
- error[invalid-argument-type] nox/popen.py:105:38: Argument to function `shutdown_process` is incorrect: Expected `Popen[bytes]`, found `Popen[bytes | str]`
- error[invalid-argument-type] nox/popen.py:111:39: Argument to function `decode_output` is incorrect: Expected `bytes`, found `(bytes & ~AlwaysFalsy) | (str & ~AlwaysFalsy)`
- Found 43 diagnostics
+ Found 38 diagnostics

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
- warning[possibly-unbound-attribute] src/pywinctl/_pywinctl_linux.py:236:17: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] src/pywinctl/_pywinctl_linux.py:759:20: Attribute `decode` on type `bytes | str` is possibly unbound
- Found 40 diagnostics
+ Found 38 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[invalid-argument-type] mkosi/config.py:1751:29: Argument to function `key_transformer` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-return-type] mkosi/util.py:49:16: Return type does not match returned value: expected `dict[T, V]`, found `dict[str | bytes, str | bytes]`
- Found 252 diagnostics
+ Found 250 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[unresolved-attribute] src/poetry/console/commands/add.py:427:16: Type `str` has no attribute `name`
- error[call-non-callable] src/poetry/installation/executor.py:197:21: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[invalid-argument-type] src/poetry/puzzle/solver.py:187:27: Argument to function `calculate_markers` is incorrect: Expected `dict[Unknown, TransitivePackageInfo]`, found `dict[str | bytes, str | bytes]`
- error[invalid-assignment] src/poetry/puzzle/solver.py:240:5: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[Unknown | tuple[@Todo(Generic tuple specializations), ...], list[PackageNode]]`
- error[invalid-assignment] src/poetry/puzzle/solver.py:384:5: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[int, list[Unknown]]`
- error[invalid-argument-type] tests/installation/test_installer.py:1256:31: Argument to bound method `mock_lock_data` is incorrect: Expected `dict[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] tests/installation/test_installer.py:1351:31: Argument to bound method `mock_lock_data` is incorrect: Expected `dict[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] tests/installation/test_installer.py:1482:31: Argument to bound method `mock_lock_data` is incorrect: Expected `dict[str, Any]`, found `dict[str | bytes, str | bytes]`
- Found 1026 diagnostics
+ Found 1018 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[call-non-callable] tornado/ioloop.py:280:20: Method `__getitem__` of type `Unknown | (bound method dict[str, Unknown].__getitem__(key: str, /) -> Unknown)` is not callable on object of type `Unknown | dict[str, Unknown]`
- warning[possibly-unbound-attribute] tornado/platform/asyncio.py:129:16: Attribute `is_closed` on type `Unknown | str` is possibly unbound
- error[no-matching-overload] tornado/platform/asyncio.py:136:25: No overload of bound method `setdefault` matches arguments
- error[unsupported-operator] tornado/test/log_test.py:203:16: Operator `in` is not supported for types `bytes` and `str`, in comparing `Literal[b"hello"]` with `bytes | str`
- error[invalid-argument-type] tornado/test/util.py:84:30: Argument to function `exec` is incorrect: Expected `dict[str, Any] | None`, found `dict[str | bytes, str | bytes]`
- Found 350 diagnostics
+ Found 345 diagnostics

black (https://github.com/psf/black)
- error[invalid-assignment] src/black/trans.py:346:5: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[tuple[@Todo(Generic tuple specializations), ...], tuple[CustomSplit, ...]]`
- Found 134 diagnostics
+ Found 133 diagnostics

asynq (https://github.com/quora/asynq)
- error[call-non-callable] asynq/tests/test_profiler.py:79:9: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] asynq/tests/test_profiler.py:84:35: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] asynq/tests/test_profiler.py:85:17: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] asynq/tests/test_profiler.py:89:22: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] asynq/tests/test_profiler.py:92:22: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] asynq/tests/test_profiler.py:93:17: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- Found 220 diagnostics
+ Found 214 diagnostics

paasta (https://github.com/yelp/paasta)
- error[call-non-callable] paasta_tools/async_utils.py:69:30: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown)` is not callable on object of type `dict[Unknown, Unknown] | defaultdict[str, Unknown]`
- error[call-non-callable] paasta_tools/contrib/check_orphans.py:227:13: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] paasta_tools/contrib/check_orphans.py:230:30: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] paasta_tools/contrib/rightsizer_soaconfigs_update.py:289:9: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] paasta_tools/firewall.py:221:16: Method `__getitem__` of type `(bound method ServiceNamespaceConfig.__getitem__(key: Unknown, /) -> Unknown) | (Overload[(key: SupportsIndex | slice[Any, Any, Any], /) -> LiteralString, (key: SupportsIndex | slice[Any, Any, Any], /) -> str]) | (Overload[(key: SupportsIndex, /) -> int, (key: slice[Any, Any, Any], /) -> bytes])` is not callable on object of type `ServiceNamespaceConfig | str | bytes`
- error[call-non-callable] paasta_tools/firewall.py:319:9: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] paasta_tools/instance/kubernetes.py:1052:9: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] paasta_tools/smartstack_tools.py:362:9: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] paasta_tools/smartstack_tools.py:364:22: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- warning[possibly-unbound-attribute] paasta_tools/utils.py:2849:17: Attribute `write` on type `Unknown | IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] paasta_tools/utils.py:2849:17: Attribute `write` on type `Unknown | IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] paasta_tools/utils.py:2850:17: Attribute `flush` on type `Unknown | IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] paasta_tools/utils.py:2850:17: Attribute `flush` on type `Unknown | IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] paasta_tools/utils.py:2862:31: Attribute `readline` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] paasta_tools/utils.py:2862:31: Attribute `readline` on type `IO[bytes] | None` is possibly unbound
- Found 944 diagnostics
+ Found 935 diagnostics

stone (https://github.com/dropbox/stone)
- warning[possibly-unbound-attribute] test/test_python_gen.py:838:34: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] test/test_python_gen.py:1760:34: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] test/test_tsd_types.py:485:34: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] test/test_tsd_types.py:517:34: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] test/test_tsd_types.py:547:34: Attribute `decode` on type `bytes | str` is possibly unbound
- Found 183 diagnostics
+ Found 178 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[invalid-argument-type] src/urllib3/contrib/emscripten/fetch.py:538:40: Argument is incorrect: Expected `dict[str, str]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] test/contrib/emscripten/test_emscripten.py:961:20: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] test/test_util.py:639:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] test/test_util.py:642:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[not-iterable] test/with_dummyserver/test_https.py:248:39: Object of type `list[WarningMessage] | list[Unknown] | None` may not be iterable
- error[not-iterable] test/with_dummyserver/test_https.py:299:39: Object of type `list[WarningMessage] | list[Unknown] | None` may not be iterable
- error[not-iterable] test/with_dummyserver/test_https.py:745:25: Object of type `list[WarningMessage] | list[Unknown] | None` may not be iterable
- error[not-iterable] test/with_dummyserver/test_https.py:864:35: Object of type `list[WarningMessage] | list[Unknown] | None` may not be iterable
- error[not-iterable] test/with_dummyserver/test_https.py:882:35: Object of type `list[WarningMessage] | list[Unknown] | None` may not be iterable
- Found 458 diagnostics
+ Found 449 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- warning[possibly-unbound-attribute] mkdocs/commands/build.py:341:21: Attribute `anchors` on type `Unknown | LegacyConfig` is possibly unbound
- warning[possibly-unbound-attribute] mkdocs/commands/gh_deploy.py:46:11: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] mkdocs/commands/gh_deploy.py:61:11: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] mkdocs/commands/gh_deploy.py:80:11: Attribute `decode` on type `bytes | str` is possibly unbound
- error[call-non-callable] mkdocs/config/base.py:140:17: Method `__getitem__` of type `bound method dict[str | bytes, str | bytes].__getitem__(key: str | bytes, /) -> str | bytes` is not callable on object of type `dict[str | bytes, str | bytes]`
- warning[possibly-unbound-attribute] mkdocs/structure/nav.py:159:13: Attribute `omitted_files` on type `Unknown | LegacyConfig` is possibly unbound
- warning[possibly-unbound-attribute] mkdocs/structure/nav.py:171:17: Attribute `absolute_links` on type `Unknown | LegacyConfig` is possibly unbound
- warning[possibly-unbound-attribute] mkdocs/structure/nav.py:175:17: Attribute `absolute_links` on type `Unknown | LegacyConfig` is possibly unbound
- warning[possibly-unbound-attribute] mkdocs/structure/nav.py:181:17: Attribute `not_found` on type `Unknown | LegacyConfig` is possibly unbound
- warning[possibly-unbound-attribute] mkdocs/structure/nav.py:207:13: Attribute `absolute_links` on type `Unknown | LegacyConfig` is possibly unbound
- warning[possibly-unbound-attribute] mkdocs/structure/nav.py:213:35: Attribute `not_found` on type `Unknown | LegacyConfig` is possibly unbound
- Found 221 diagnostics
+ Found 210 diagnostics

vision (https://github.com/pytorch/vision)
- warning[possibly-unbound-attribute] scripts/release_notes/retrieve_prs_data.py:44:14: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] scripts/release_notes/retrieve_prs_data.py:45:11: Attribute `decode` on type `bytes | str` is possibly unbound
- error[call-non-callable] test/builtin_dataset_mocks.py:1341:37: Method `__getitem__` of type `bound method Counter[str].__getitem__(key: str, /) -> int` is not callable on object of type `Counter[str]`
- error[call-non-callable] test/builtin_dataset_mocks.py:1341:56: Method `__getitem__` of type `bound method Counter[str].__getitem__(key: str, /) -> int` is not callable on object of type `Counter[str]`
- error[unresolved-attribute] test/common_extended_utils.py:178:17: Type `str | bytes` has no attribute `register_forward_pre_hook`
- error[unresolved-attribute] test/common_extended_utils.py:179:17: Type `str | bytes` has no attribute `register_forward_hook`
- error[invalid-return-type] torchvision/datasets/places365.py:128:16: Return type does not match returned value: expected `tuple[list[str], dict[str, int]]`, found `tuple[list[str | bytes], dict[str | bytes, str | bytes]]`
- Found 1854 diagnostics
+ Found 1847 diagnostics

rich (https://github.com/Textualize/rich)
- error[call-non-callable] rich/columns.py:132:21: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] rich/columns.py:132:45: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- warning[possibly-unbound-attribute] tests/test_console.py:1020:5: Attribute `close` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_console.py:1020:5: Attribute `close` on type `IO[bytes] | None` is possibly unbound
- Found 393 diagnostics
+ Found 391 diagnostics

meson (https://github.com/mesonbuild/meson)
- warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:144:24: Attribute `readline` on type `Unknown | IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:144:24: Attribute `readline` on type `Unknown | IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:148:13: Attribute `close` on type `Unknown | IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:148:13: Attribute `close` on type `Unknown | IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:159:28: Attribute `readline` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:159:28: Attribute `readline` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:172:13: Attribute `close` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:172:13: Attribute `close` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:182:20: Attribute `readline` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:182:20: Attribute `readline` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:186:9: Attribute `close` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/cmake/executor.py:186:9: Attribute `close` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/compilers/mixins/elbrus.py:69:18: Attribute `read` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] mesonbuild/compilers/mixins/elbrus.py:69:18: Attribute `read` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/compilers/mixins/elbrus.py:69:18: Attribute `decode` on type `bytes | str` is possibly unbound
- error[call-non-callable] mesonbuild/environment.py:824:29: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] mesonbuild/environment.py:828:29: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] mesonbuild/environment.py:834:25: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] mesonbuild/environment.py:852:37: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] mesonbuild/environment.py:853:21: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[invalid-argument-type] mesonbuild/scripts/clippy.py:76:61: Argument to bound method `__init__` is incorrect: Expected `str`, found `str | bytes`
- warning[possibly-unbound-attribute] mesonbuild/scripts/meson_exe.py:76:19: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/scripts/meson_exe.py:78:15: Attribute `decode` on type `bytes | str` is possibly unbound
- error[invalid-argument-type] mesonbuild/scripts/rustdoc.py:101:56: Argument to bound method `__init__` is incorrect: Expected `str`, found `str | bytes`
- warning[possibly-unbound-attribute] mesonbuild/utils/universal.py:1025:28: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/utils/universal.py:1709:17: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/utils/universal.py:1711:17: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/utils/universal.py:1714:17: Attribute `decode` on type `bytes | str` is possibly unbound
- warning[possibly-unbound-attribute] mesonbuild/utils/universal.py:1716:17: Attribute `decode` on type `bytes | str` is possibly unbound
- error[invalid-return-type] mesonbuild/utils/universal.py:1717:12: Return type does not match returned value: expected `tuple[Popen[str], str, str]`, found `tuple[Popen[bytes | str], str, str]`
+ error[invalid-return-type] mesonbuild/utils/universal.py:1717:12: Return type does not match returned value: expected `tuple[Popen[str], str, str]`, found `tuple[Popen[bytes], str, str]`
- error[invalid-argument-type] unittests/allplatformstests.py:2434:39: Argument to bound method `sanity_check` is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] unittests/cargotests.py:208:32: Argument to function `load_wraps` is incorrect: Expected `str`, found `str | bytes`
- error[unsupported-operator] unittests/linuxliketests.py:1094:45: Operator `+` is unsupported between objects of type `Literal["--prefix="]` and `str | bytes`
- error[unsupported-operator] unittests/linuxliketests.py:1198:47: Operator `+` is unsupported between objects of type `Literal["--prefix="]` and `str | bytes`
- Found 1422 diagnostics
+ Found 1403 diagnostics

altair (https://github.com/vega/altair)
- error[invalid-return-type] altair/utils/schemapi.py:322:12: Return type does not match returned value: expected `dict[str, list[ValidationError]]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] altair/utils/schemapi.py:430:12: Return type does not match returned value: expected `dict[str, list[ValidationError]]`, found `dict[str | bytes, str | bytes]`
+ warning[unused-ignore-comment] altair/utils/schemapi.py:429:57: Unused blanket `type: ignore` directive
- Found 1301 diagnostics
+ Found 1300 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[invalid-return-type] expression/collections/map.py:137:16: Return type does not match returned value: expected `ItemsView[_Key, _Value]`, found `ItemsView[str | bytes, str | bytes]`
- Found 287 diagnostics
+ Found 286 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-argument-type] src/jinja2/environment.py:441:29: Argument to function `setattr` is incorrect: Expected `str`, found `str | bytes`
- Found 236 diagnostics
+ Found 235 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-return-type] src/werkzeug/datastructures/structures.py:415:20: Return type does not match returned value: expected `dict[K, V] | dict[K, list[V]]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] src/werkzeug/datastructures/structures.py:416:16: Return type does not match returned value: expected `dict[K, V] | dict[K, list[V]]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] src/werkzeug/datastructures/structures.py:975:16: Return type does not match returned value: expected `dict[K, V]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] src/werkzeug/debug/__init__.py:409:44: Argument to bound method `__init__` is incorrect: Expected `dict[str, Any]`, found `dict[Unknown, Unknown] | dict[str | bytes, str | bytes]`
- Found 458 diagnostics
+ Found 454 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-return-type] src/_pytest/config/findpaths.py:53:20: Return type does not match returned value: expected `dict[str, str | list[str]] | None`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] src/_pytest/config/findpaths.py:64:20: Return type does not match returned value: expected `dict[str, str | list[str]] | None`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] src/_pytest/mark/structures.py:269:13: Argument to bound method `__init__` is incorrect: Expected `Mapping[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] src/_pytest/pytester.py:295:36: Argument to function `eval` is incorrect: Expected `dict[str, Any] | None`, found `dict[str | bytes, str | bytes]`
- warning[possibly-unbound-attribute] src/_pytest/pytester.py:1358:13: Attribute `close` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/_pytest/pytester.py:1358:13: Attribute `close` on type `IO[bytes] | None` is possibly unbound
- warning[possibly-unbound-attribute] src/_pytest/pytester.py:1361:13: Attribute `write` on type `IO[bytes | str] | None` is possibly unbound
+ warning[possibly-unbound-attribute] src/_pytest/pytester.py:1361:13: Attribute `write` on type `IO[bytes] | None` is possibly unbound
- error[invalid-argument-type] src/_pytest/python.py:1088:13: Argument is incorrect: Expected `Mapping[str, Scope]`, found `dict[str | bytes, str | bytes]`
- error[not-iterable] testing/test_runner.py:828:21: Object of type `None` is not iterable
- error[not-iterable] testing/test_tmpdir.py:535:44: Object of type `list[WarningMessage] | list[Unknown] | None` may not be iterable
- Found 893 diagnostics
+ Found 886 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- error[invalid-return-type] dulwich/client.py:1105:20: Return type does not match returned value: expected `dict[bytes, str | None] | None`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] dulwich/client.py:1720:16: Return type does not match returned value: expected `tuple[Protocol, () -> bool, IO[bytes] | None]`, found `tuple[Protocol, bound method SubprocessWrapper.can_read() -> Unknown, IO[bytes | str] | None]`
- error[invalid-assignment] dulwich/diff_tree.py:325:5: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[int, int]`
- error[invalid-argument-type] dulwich/pack.py:1957:24: Argument to function `find_reusable_deltas` is incorrect: Expected `set[bytes]`, found `set[Unknown | bytes | str]`
- error[call-non-callable] dulwich/pack.py:1976:40: Method `__getitem__` of type `bound method PackedObjectContainer.__getitem__(sha1: bytes) -> ShaFile` is not callable on object of type `PackedObjectContainer`
- error[call-non-callable] dulwich/pack.py:2193:9: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] dulwich/pack.py:2196:35: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] dulwich/pack.py:2197:9: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[call-non-callable] dulwich/pack.py:2197:33: Method `__getitem__` of type `bound method defaultdict[str, Unknown].__getitem__(key: str, /) -> Unknown` is not callable on object of type `defaultdict[str, Unknown]`
- error[invalid-assignment] dulwich/pack.py:2370:5: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[int, int]`
- error[invalid-assignment] dulwich/pack.py:2568:9: Object of type `defaultdict[str, Unknown]` is not assignable to `dict[int, list[UnpackedObject]]`
- Found 139 diagnostics
+ Found 128 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[invalid-return-type] psycopg/psycopg/rows.py:121:20: Return type does not match returned value: expected `dict[str, Any]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] psycopg_pool/psycopg_pool/base.py:165:16: Return type does not match returned value: expected `dict[str, int]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] psycopg_pool/psycopg_pool/base.py:176:16: Return type does not match returned value: expected `dict[str, int]`, found `dict[str | bytes, str | bytes]`
- Found 1151 diagnostics
+ Found 1148 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
- error[invalid-argument-type] boostedblob/cli.py:352:31: Argument is incorrect: Expected `str`, found `str | bytes`
- error[invalid-argument-type] boostedblob/request.py:249:72: Argument is incorrect: Expected `Mapping[str, str]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] boostedblob/request.py:278:72: Argument is incorrect: Expected `Mapping[str, str]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] boostedblob/request.py:288:13: Argument is incorrect: Expected `Mapping[str, str]`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] boostedblob/request.py:313:13: Argument is incorrect: Expected `Mapping[str, str]`, found `dict[str | bytes, str | bytes]`
- Found 45 diagnostics
+ Found 40 diagnostics

aiortc (https://github.com/aiortc/aiortc)
- error[call-non-callable] src/aiortc/sdp.py:422:33: Method `__getitem__` of type `Unknown | (bound method dict[str | bytes, str | bytes].__getitem__(key: str | bytes, /) -> str | bytes)` is not callable on object of type `Unknown | dict[str | bytes, str | bytes]`
- error[invalid-argument-type] src/aiortc/sdp.py:443:52: Argument is incorrect: Expected `str`, found `None | Unknown | str | bytes`
+ error[invalid-argument-type] src/aiortc/sdp.py:443:52: Argument is incorrect: Expected `str`, found `None | Unknown`
- error[invalid-assignment] src/aiortc/sdp.py:496:25: Object of type `Unknown | str | bytes` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
+ error[invalid-assignment] src/aiortc/sdp.py:496:25: Object of type `Unknown` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
- error[call-non-callable] src/aiortc/sdp.py:496:51: Method `__getitem__` of type `Unknown | (bound method dict[str | bytes, str | bytes].__getitem__(key: str | bytes, /) -> str | bytes)` is not callable on object of type `Unknown | dict[str | bytes, str | bytes]`
- Found 131 diagnostics
+ Found 129 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-return-type] scrapy/utils/conf.py:70:12: Return type does not match returned value: expected `dict[str, str]`, found `dict[str | bytes, str | bytes]`
- error[invalid-return-type] scrapy/utils/python.py:279:12: Return type does not match returned value: expected `tuple[list[str], dict[str, Any]]`, found `tuple[list[str], dict[str | bytes, str | bytes]]`
- error[invalid-assignment] scrapy/utils/trackref.py:28:1: Object of type `defaultdict[str, Unknown]` is not assignable to `defaultdict[type, WeakKeyDictionary[Unknown, Unknown]]`
- warning[possibly-unbound-attribute] tests/test_cmdline/__init__.py:26:16: Attribute `decode` on type `bytes | str` is possibly unbound
- error[invalid-argument-type] tests/test_core_downloader.py:112:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_core_downloader.py:115:24: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_crawler.py:711:16: Attribute `decode` on type `bytes | str` is possibly unbound
- error[invalid-argument-type] tests/test_downloader_handlers.py:1147:75: Argument to bound method `__init__` is incorrect: Expected `dict[str, Any] | None`, found `dict[str | bytes, str | bytes]`
- error[invalid-argument-type] tests/test_dupefilters.py:265:20: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_dupefilters.py:267:17: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_dupefilters.py:270:16: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_engine.py:334:28: Attribute `url` on type `Unknown | str` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_engine.py:336:30: Attribute `url` on type `Unknown | str` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_engine.py:338:30: Attribute `url` on type `Unknown | str` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_engine.py:340:30: Attribute `url` on type `Unknown | str` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_engine.py:342:30: Attribute `url` on type `Unknown | str` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_engine.py:353:30: Attribute `url` on type `Unknown | str` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_engine.py:363:30: Attribute `url` on type `Unknown | str` is possibly unbound
- error[unsupported-operator] tests/test_engine.py:477:16: Operator `not in` is not supported for types `bytes` and `str`, in comparing `Literal[b"Traceback"]` with `bytes | str`
- warning[possibly-unbound-attribute] tests/test_engine_stop_download_bytes.py:70:28: Attribute `url` on type `Unknown | str` is possibly unbound
- error[invalid-argument-type] tests/test_http_request.py:1525:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_http_request.py:1526:50: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_http_request.py:1537:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_http_request.py:1538:50: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_http_request.py:1548:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_http_request.py:1554:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_http_request.py:1644:17: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_pipeline_files.py:731:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_files.py:744:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_files.py:761:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_files.py:781:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_media.py:411:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_media.py:423:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_media.py:440:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_media.py:464:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_media.py:487:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_media.py:511:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_pipeline_media.py:532:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_scrapy__getattr__.py:12:16: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_scrapy__getattr__.py:12:16: Attribute `args` on type `Warning | str | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] tests/test_scrapy__getattr__.py:12:16: Attribute `args` on type `Warning | str` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_spiderloader/__init__.py:145:20: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_spiderloader/__init__.py:149:17: Attribute `pop` on type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_spiderloader/__init__.py:150:64: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_spiderloader/__init__.py:174:20: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/test_spiderloader/__init__.py:178:17: Attribute `pop` on type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_spiderloader/__init__.py:179:64: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_spiderloader/__init__.py:208:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_spiderloader/__init__.py:209:23: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_spiderloader/__init__.py:236:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_spiderloader/__init__.py:237:23: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_spidermiddleware_offsite.py:92:31: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_spidermiddleware_offsite.py:105:31: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_spidermiddleware_referer.py:849:32: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_spidermiddleware_referer.py:850:28: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_spidermiddleware_referer.py:850:61: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_utils_asyncio.py:26:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_utils_datatypes.py:227:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_utils_datatypes.py:228:31: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_utils_datatypes.py:230:21: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_utils_deprecate.py:145:19: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_utils_deprecate.py:271:55: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_utils_deprecate.py:286:20: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_utils_deprecate.py:287:52: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- warning[call-possibly-unbound-method] tests/test_utils_deprecate.py:288:55: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_utils_deprecate.py:300:20: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:98:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_utils_misc/test_return_with_argument_inside_generator.py:101:24: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:105:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_utils_misc/test_return_with_argument_inside_generator.py:106:71: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:109:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_utils_misc/test_return_with_argument_inside_generator.py:110:71: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:113:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_utils_misc/test_return_with_argument_inside_generator.py:114:71: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:117:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- warning[call-possibly-unbound-method] tests/test_utils_misc/test_return_with_argument_inside_generator.py:118:71: Method `__getitem__` of type `list[WarningMessage] | list[Unknown] | None` is possibly unbound
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:165:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:168:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:171:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:174:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argument-type] tests/test_utils_misc/test_return_with_argument_inside_generator.py:177:24: Argument to function `len` is incorrect: Expected `Sized`, found `list[WarningMessage] | list[Unknown] | None`
- error[invalid-argume...*[Comment body truncated]*

---

_Review comment by @dhruvmanila on `crates/ty_python_semantic/src/types.rs`:4659 on 2025-05-19 19:08_

I think this is my mistake as I didn't revert this part from https://github.com/astral-sh/ruff/pull/17618 and only reverted the intersection of return type, sorry! 🙈

---

_@dhruvmanila approved on 2025-05-19 19:08_

Thank you!

---

_Merged by @dcreager on 2025-05-19 19:12_

---

_Closed by @dcreager on 2025-05-19 19:12_

---

_Branch deleted on 2025-05-19 19:12_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:357 on 2025-05-19 19:20_

Maybe not worth a separate follow-up PR, but wouldn't we ideally have a case where the second argument is optional and so we can have multiple matching overloads, and show that we pick the first?

---

_@carljm reviewed on 2025-05-19 19:20_

---

_@dcreager reviewed on 2025-05-19 19:22_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/classes.md`:357 on 2025-05-19 19:22_

This gives us two matching overloads if you provide a `bytes` parameter, and we show that we pick the first (giving `C[bytes]` instead of `C[int]` or `C[bytes | int]`).  Maybe deserves a comment, since the argument / specialization pattern across the overloads is subtle?

---
