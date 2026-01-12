```yaml
number: 17591
title: "[red-knot] Special case `@abstractmethod` for function type"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/abstractmethod
created_at: 2025-04-23T18:57:00Z
updated_at: 2025-04-23T22:24:54Z
url: https://github.com/astral-sh/ruff/pull/17591
synced_at: 2026-01-12T15:56:02Z
```

# [red-knot] Special case `@abstractmethod` for function type

---

_@dhruvmanila_

## Summary

This is required because otherwise the inferred type is not going to be `Type::FunctionLiteral` but a todo type because we don't recognize `TypeVar` yet:

```py
_FuncT = TypeVar("_FuncT", bound=Callable[..., Any])

def abstractmethod(funcobj: _FuncT) -> _FuncT: ...
```

This is mainly required to raise diagnostic when only some (and not all) `@overload`-ed functions are decorated with `@abstractmethod`.




---

_Label `red-knot` added by @dhruvmanila on 2025-04-23 18:57_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-23 18:57_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-23 18:57_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-23 18:57_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-23 18:57_

---

_@dcreager approved on 2025-04-23 18:59_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:5884 on 2025-04-23 19:01_

```suggestion
        const ABSTRACTMETHOD = 1 << 3;
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1495 on 2025-04-23 19:01_

```suggestion
                    function_decorators |= FunctionDecorators::ABSTRACTMETHOD;
```

---

_@dhruvmanila reviewed on 2025-04-23 19:01_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:1496 on 2025-04-23 19:05_

nit: I think it would probably be slightly more efficient to do a single `match` over `function.known()` rather than repeated `function.is_known()` calls -- it would involve fewer Salsa lookups:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 234791bd6..815559eaf 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -1482,18 +1482,23 @@ impl<'db> TypeInferenceBuilder<'db> {
             let decorator_ty = self.infer_decorator(decorator);
 
             if let Type::FunctionLiteral(function) = decorator_ty {
-                if function.is_known(self.db(), KnownFunction::NoTypeCheck) {
-                    // If the function is decorated with the `no_type_check` decorator,
-                    // we need to suppress any errors that come after the decorators.
-                    self.context.set_in_no_type_check(InNoTypeCheck::Yes);
-                    function_decorators |= FunctionDecorators::NO_TYPE_CHECK;
-                    continue;
-                } else if function.is_known(self.db(), KnownFunction::Overload) {
-                    function_decorators |= FunctionDecorators::OVERLOAD;
-                    continue;
-                } else if function.is_known(self.db(), KnownFunction::AbstractMethod) {
-                    function_decorators |= FunctionDecorators::ABSTRACT_METHOD;
-                    continue;
+                match function.known(self.db()) {
+                    Some(KnownFunction::NoTypeCheck) => {
+                        // If the function is decorated with the `no_type_check` decorator,
+                        // we need to suppress any errors that come after the decorators.
+                        self.context.set_in_no_type_check(InNoTypeCheck::Yes);
+                        function_decorators |= FunctionDecorators::NO_TYPE_CHECK;
+                        continue;
+                    }
+                    Some(KnownFunction::Overload) => {
+                        function_decorators |= FunctionDecorators::OVERLOAD;
+                        continue;
+                    }
+                    Some(KnownFunction::AbstractMethod) => {
+                        function_decorators |= FunctionDecorators::ABSTRACT_METHOD;
+                        continue;
+                    }
+                    _ => {}
                 }
```

---

_@AlexWaygood approved on 2025-04-23 19:05_

---

_Comment by @github-actions[bot] on 2025-04-23 19:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
nionutils (https://github.com/nion-software/nionutils)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/nionutils/nion/utils/Model.py:106:56: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def eval() -> None`
- Found 33 diagnostics
+ Found 34 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/corofunc_cache.py:95:33: Argument to this function is incorrect: Expected `int | float`, found `int | float | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/corofunc_cache.py:174:33: Argument to this function is incorrect: Expected `int | float`, found `int | float | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/corofunc_cache.py:174:38: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method LRU[Hashable, @Todo(specialized non-generic class)].remove(key: Hashable, /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/bg_loop.py:113:31: Argument to this function is incorrect: Expected `_TaskFactory | None`, found `def eager_task_factory(loop: AbstractEventLoop | None, coro: @Todo(specialized non-generic class), *, name: str | None = None, context: Context | None = None) -> @Todo(specialized non-generic class)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/bg_loop.py:172:35: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method AbstractEventLoop.stop() -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/task_cache.py:154:33: Argument to this function is incorrect: Expected `int | float`, found `int | float | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/task_cache.py:240:33: Argument to this function is incorrect: Expected `int | float`, found `int | float | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/async-utils/src/async_utils/task_cache.py:240:38: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method LRU[Hashable, @Todo(specialized non-generic class)].remove(key: Hashable, /) -> None`
- Found 21 diagnostics
+ Found 29 diagnostics

anyio (https://github.com/agronholm/anyio)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/abc/_sockets.py:149:39: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `(SocketStream, /) -> Any`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:1339:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:1349:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2590:19: No overload of bound method `create_connection` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2590:19: No overload of bound method `create_connection` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2590:19: No overload of bound method `create_connection` matches arguments
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2608:45: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[DatagramProtocol]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[DatagramProtocol]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[DatagramProtocol]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2662:49: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2731:33: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def cb() -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/anyio/src/anyio/_backends/_asyncio.py:2784:33: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def cb() -> None`
- Found 148 diagnostics
+ Found 161 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bandersnatch/src/bandersnatch/verify.py:85:42: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def find_all_files(files: @Todo(specialized non-generic class), base_dir: Path) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bandersnatch/src/bandersnatch/verify.py:105:48: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def unlink_parent_dir(path: Path) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/bandersnatch/src/bandersnatch/verify.py:187:64: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def hash(path: Path, function: str = Literal["sha256"]) -> str`
- Found 163 diagnostics
+ Found 166 diagnostics

kopf (https://github.com/nolar/kopf)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_cogs/clients/api.py:40:45: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def get_server_certificate(addr: tuple[str, int], ssl_version: int = ellipsis, ca_certs: str | None = None) -> str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/processing.py:443:81: Argument to this function is incorrect: Expected `BodyEssence`, found `Unknown | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/running.py:353:52: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/running.py:354:53: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method Future.set_result(result: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/running.py:476:33: Argument to this function is incorrect: Expected `int | float`, found `int | float | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/kopf/kopf/_core/reactor/running.py:477:33: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def pthread_kill(thread_id: int, signalnum: int, /) -> None`
- Found 165 diagnostics
+ Found 171 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/aiohttp-devtools/tests/test_runserver_main.py:212:26: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method AbstractEventLoop.stop() -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/aiohttp-devtools/aiohttp_devtools/runserver/serve.py:120:50: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def shutdown() -> Never`
- Found 65 diagnostics
+ Found 67 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/tornado/tornado/test/asyncio_test.py:50:60: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/tornado/tornado/test/asyncio_test.py:59:61: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/tornado/tornado/test/asyncio_test.py:142:28: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `bound method AbstractEventLoop.stop() -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/tornado/tornado/test/gen_test.py:1079:19: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `() -> Unknown`
- Found 376 diagnostics
+ Found 380 diagnostics

python-chess (https://github.com/niklasf/python-chess)
+ error[lint:missing-argument] /tmp/mypy_primer/projects/python-chess/chess/engine.py:1171:22: No argument provided for required parameter `program` of bound method `subprocess_exec`
- Found 80 diagnostics
+ Found 81 diagnostics

black (https://github.com/psf/black)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/concurrency.py:157:27: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def format_file_in_place(src: Path, fast: bool, mode: Mode, write_back: WriteBack = @Todo(Attribute access on enum classes), lock: Any = None, *, lines: @Todo(specialized non-generic class) = tuple[()]) -> bool`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/concurrency.py:164:48: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def cancel(tasks: @Todo(specialized non-generic class)) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/concurrency.py:165:49: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def cancel(tasks: @Todo(specialized non-generic class)) -> None`
- Found 151 diagnostics
+ Found 154 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/cki-lib/tests/test_yaml.py:52:36: Argument to this function is incorrect: Expected `Path | str | None`, found `Traversable`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/cki-lib/tests/test_yaml.py:64:31: Argument to this function is incorrect: Expected `Path | str | None`, found `Traversable`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/cki-lib/tests/test_yaml.py:65:31: Argument to this function is incorrect: Expected `Path | str | None`, found `Traversable`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/cki-lib/tests/test_yaml.py:76:31: Argument to this function is incorrect: Expected `Path | str | None`, found `Traversable`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/cki-lib/tests/test_yaml.py:77:31: Argument to this function is incorrect: Expected `Path | str | None`, found `Traversable`
- Found 172 diagnostics
+ Found 177 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/schema_salad/schema_salad/python_codegen_support.py:221:50: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/schema_salad/schema_salad/python_codegen_support.py:232:50: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/schema_salad/schema_salad/metaschema.py:224:50: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/schema_salad/schema_salad/metaschema.py:235:50: Argument to this function is incorrect: Expected `str`, found `str | None`
- Found 584 diagnostics
+ Found 588 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/cwltool/cwltool/checker.py:98:13: Value is already of type `Unknown`
- warning[lint:redundant-cast] /tmp/mypy_primer/projects/cwltool/cwltool/command_line_tool.py:1392:38: Value is already of type `Unknown`
- Found 411 diagnostics
+ Found 409 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/utils/asyncio_utils.py:96:60: Unused blanket `type: ignore` directive
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/script/concurrent.py:30:64: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def run() -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/tools/main.py:113:17: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `Any | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/tools/main.py:122:52: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def _sigint(*_) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/tools/main.py:123:53: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def _sigterm(*_) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/certs.py:551:21: Argument to this function is incorrect: Expected `Encoding`, found `Unknown | Literal["PEM"]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mitmproxy/mitmproxy/certs.py:552:21: Argument to this function is incorrect: Expected `PrivateFormat`, found `Unknown | Literal["TraditionalOpenSSL"]`
- Found 1319 diagnostics
+ Found 1324 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/_conninfo_attempts_async.py:101:19: Argument to this function is incorrect: Expected `bytes | str | int | None`, found `(str & ~AlwaysFalsy) | (ParamDef & ~AlwaysTruthy & ~AlwaysFalsy) | (@Todo(map_with_boundness: intersections with negative contributions) & ~AlwaysFalsy)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/waiting.py:140:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def wakeup(state: Ready) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/waiting.py:142:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def wakeup(state: Ready) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/waiting.py:201:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def wakeup(state: Ready) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/waiting.py:203:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def wakeup(state: Ready) -> None`
- Found 1293 diagnostics
+ Found 1298 diagnostics

websockets (https://github.com/aaugustin/websockets)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/websockets/src/websockets/cli.py:114:56: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[ReadLines]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/websockets/src/websockets/cli.py:114:56: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[ReadLines]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/websockets/src/websockets/cli.py:114:56: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[ReadLines]`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/websockets/src/websockets/asyncio/server.py:808:85: Unused blanket `type: ignore` directive
- Found 112 diagnostics
+ Found 114 diagnostics

optuna (https://github.com/optuna/optuna)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/optuna/tests/trial_tests/test_frozen.py:85:16: Operator `+` is unsupported between objects of type `int | float` and `None`
- Found 2107 diagnostics
+ Found 2108 diagnostics

boostedblob (https://github.com/hauntsaninja/boostedblob)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/boostedblob/boostedblob/cli.py:229:46: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `(Overload[(s: @Todo(Support for `typing.TypeAlias`), /) -> int, (s: @Todo(Support for `typing.TypeVar` instances in type expressions), /) -> int]) | @Todo(Support for `typing.TypeAlias`)`
- Found 99 diagnostics
+ Found 100 diagnostics

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/pool_shared.py:290:89: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:87:84: Unused blanket `type: ignore` directive
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:118:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def _is_ready(fut: Future) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:125:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def _is_ready(fut: Future) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:132:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def _is_ready(fut: Future) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:134:45: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def _is_ready(fut: Future) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:167:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def _is_ready(fut: Future) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:174:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def _is_ready(fut: Future) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:181:41: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def _is_ready(fut: Future) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/mongo-python-driver/pymongo/network_layer.py:183:45: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def _is_ready(fut: Future) -> None`
- Found 557 diagnostics
+ Found 563 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/keys/__init__.py:34:13: Argument to this function is incorrect: Expected `Encoding`, found `Unknown | Literal["PEM"]`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/keys/__init__.py:35:13: Argument to this function is incorrect: Expected `PrivateFormat`, found `Unknown | Literal["TraditionalOpenSSL"]`
- Found 1489 diagnostics
+ Found 1491 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/strawberry/strawberry/dataloader.py:223:27: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def create_task(coro: @Todo(Support for `typing.TypeAlias`), *, name: str | None = None) -> @Todo(specialized non-generic class)`
- Found 623 diagnostics
+ Found 624 diagnostics

vision (https://github.com/pytorch/vision)
+ error[lint:missing-argument] /tmp/mypy_primer/projects/vision/torchvision/prototype/utils/_internal.py:87:13: No argument provided for required parameter `offset` of bound method `seek`
- Found 2248 diagnostics
+ Found 2249 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dd-trace-py/tests/llmobs/test_llmobs_integrations.py:97:5: Type `bound method BaseLLMIntegration._llmobs_set_tags(span: Span, args: list, kwargs: dict, response: Any | None = None, operation: str = Literal[""]) -> None` has no attribute `assert_called_once_with`
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/dd-trace-py/tests/llmobs/test_llmobs_integrations.py:103:5: Type `bound method BaseLLMIntegration._llmobs_set_tags(span: Span, args: list, kwargs: dict, response: Any | None = None, operation: str = Literal[""]) -> None` has no attribute `assert_called_once_with`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/appsec/appsec/test_processor.py:508:26: Argument to this function is incorrect: Expected `ddwaf_context_capsule`, found `ddwaf_context_capsule | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/appsec/appsec/test_processor.py:526:26: Argument to this function is incorrect: Expected `ddwaf_context_capsule`, found `ddwaf_context_capsule | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/dd-trace-py/tests/appsec/appsec/api_security/test_schema_fuzz.py:19:9: Argument to this function is incorrect: Expected `ddwaf_context_capsule`, found `ddwaf_context_capsule | None`
- Found 6899 diagnostics
+ Found 6904 diagnostics

zulip (https://github.com/zulip/zulip)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/zulip/zerver/tests/test_tornado.py:107:50: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def process_events() -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/zulip/zerver/management/commands/runtornado.py:89:56: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def stop() -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/zulip/zerver/management/commands/runtornado.py:90:57: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> object`, found `def stop() -> None`
- Found 4339 diagnostics
+ Found 4342 diagnostics

```
</details>


---

_@dhruvmanila reviewed on 2025-04-23 20:04_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/infer.rs`:1496 on 2025-04-23 20:04_

Agreed. I avoided that keep the diff simpler but will make this change before merging. Thanks!

---

_Comment by @dhruvmanila on 2025-04-23 20:28_

(Updated to use match arms instead of if blocks for `decorator_ty` and known functions using `function.known()` instead of `function.is_known()`.)

---

_Comment by @dhruvmanila on 2025-04-23 21:59_

The ecosystem changes are mainly because we now type check the method that exists in the subclass of an abstract base class and in their stubs the methods are marked as `@abstractmethod`.

---

_@carljm approved on 2025-04-23 22:19_

---

_Comment by @dhruvmanila on 2025-04-23 22:24_

Reported https://github.com/astral-sh/ruff/issues/17595, https://github.com/astral-sh/ruff/issues/17596 based on ecosystem report.

(I have to pause now, might look at a few more cases later.)

---

_Merged by @dhruvmanila on 2025-04-23 22:24_

---

_Closed by @dhruvmanila on 2025-04-23 22:24_

---

_Branch deleted on 2025-04-23 22:24_

---
