```yaml
number: 22469
title: "[ty] Support calls to intersection types"
type: pull_request
state: open
author: carljm
labels:
  - ty
  - ecosystem-analyzer
assignees: []
draft: true
base: main
head: claude/fix-issue-1858-UjARA
created_at: 2026-01-09T00:06:59Z
updated_at: 2026-01-20T21:21:58Z
url: https://github.com/astral-sh/ruff/pull/22469
synced_at: 2026-01-20T21:54:18Z
```

# [ty] Support calls to intersection types

---

_@carljm_

Implement proper handling for calling intersection types. Previously, calling an intersection type would return a `@Todo` type that suppressed errors but provided no useful type information.

Now, when calling an intersection type:
- We check each positive element of the intersection for callability
- If at least one element is callable, the intersection is callable
- The return type is the intersection of the return types from all callable elements
- If no elements are callable, a proper error is reported

This is a partial fix for #1858. Further improvements could include better handling of signatures and parameter validation.

---

_Label `ty` added by @carljm on 2026-01-09 00:07_

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 00:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## [Typing conformance results](https://github.com/python/typing/blob/dece44f2922ca390fe314145d09939514a21e76e/conformance/)

No changes detected ✅





---

_Comment by @astral-sh-bot[bot] on 2026-01-09 00:10_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ mypy_primer/git_utils.py:65:23: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 3 diagnostics
+ Found 4 diagnostics

attrs (https://github.com/python-attrs/attrs)
- tests/test_make.py:2882:16: error[unsupported-operator] Operator `<` is not supported between two objects of type `C | @Todo`
+ tests/test_make.py:2882:16: error[unsupported-operator] Operator `<` is not supported between two objects of type `C`
- tests/test_make.py:2887:16: error[unsupported-operator] Operator `>` is not supported between two objects of type `C | @Todo`
+ tests/test_make.py:2887:16: error[unsupported-operator] Operator `>` is not supported between two objects of type `C`

aioredis (https://github.com/aio-libs/aioredis)
+ aioredis/connection.py:206:20: error[call-non-callable] Object of type `Mapping[str, type[Exception]]` is not callable
- aioredis/connection.py:206:20: error[invalid-return-type] Return type does not match returned value: expected `ResponseError`, found `Exception | @Todo`
+ aioredis/connection.py:206:20: error[invalid-return-type] Return type does not match returned value: expected `ResponseError`, found `Exception | Unknown`
- Found 29 diagnostics
+ Found 30 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_vendor/cachecontrol/controller.py:349:12: error[unresolved-attribute] Object of type `~None` has no attribute `status`
+ src/pip/_vendor/cachecontrol/controller.py:351:45: error[unresolved-attribute] Object of type `~None` has no attribute `status`
+ src/pip/_vendor/cachecontrol/controller.py:356:13: error[unresolved-attribute] Object of type `~None` has no attribute `headers`
+ src/pip/_vendor/cachecontrol/controller.py:420:49: error[invalid-argument-type] Argument to bound method `_cache_set` is incorrect: Expected `HTTPResponse`, found `~None`
+ src/pip/_vendor/cachecontrol/controller.py:424:18: error[unresolved-attribute] Object of type `~None` has no attribute `status`
+ src/pip/_vendor/cachecontrol/controller.py:426:49: error[invalid-argument-type] Argument to bound method `_cache_set` is incorrect: Expected `HTTPResponse`, found `~None`
+ src/pip/_vendor/cachecontrol/controller.py:443:21: error[invalid-argument-type] Argument to bound method `_cache_set` is incorrect: Expected `HTTPResponse`, found `~None`
+ src/pip/_vendor/cachecontrol/controller.py:466:25: error[invalid-argument-type] Argument to bound method `_cache_set` is incorrect: Expected `HTTPResponse`, found `~None`
+ src/pip/_vendor/rich/_log_render.py:60:36: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/pip/_vendor/rich/text.py:622:31: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/pip/_vendor/rich/text.py:622:31: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 595 diagnostics
+ Found 606 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/llnl/util/lock.py:735:24: error[call-non-callable] Object of type `ContextManager[Unknown]` is not callable
- lib/spack/spack/vendor/ruamel/yaml/main.py:1071:16: warning[possibly-missing-attribute] Attribute `_constructor` may be missing on object of type `@Todo | Loader`
+ lib/spack/spack/vendor/ruamel/yaml/main.py:1071:16: warning[possibly-missing-attribute] Attribute `_constructor` may be missing on object of type `Unknown | Loader`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1096:15: warning[possibly-missing-attribute] Attribute `_constructor` may be missing on object of type `@Todo | Loader`
+ lib/spack/spack/vendor/ruamel/yaml/main.py:1096:15: warning[possibly-missing-attribute] Attribute `_constructor` may be missing on object of type `Unknown | Loader`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1097:19: warning[possibly-missing-attribute] Attribute `_constructor` may be missing on object of type `@Todo | Loader`
+ lib/spack/spack/vendor/ruamel/yaml/main.py:1097:19: warning[possibly-missing-attribute] Attribute `_constructor` may be missing on object of type `Unknown | Loader`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1099:9: warning[possibly-missing-attribute] Attribute `_parser` may be missing on object of type `@Todo | Loader`
+ lib/spack/spack/vendor/ruamel/yaml/main.py:1099:9: warning[possibly-missing-attribute] Attribute `_parser` may be missing on object of type `Unknown | Loader`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1101:13: warning[possibly-missing-attribute] Attribute `_reader` may be missing on object of type `@Todo | Loader`
+ lib/spack/spack/vendor/ruamel/yaml/main.py:1101:13: warning[possibly-missing-attribute] Attribute `_reader` may be missing on object of type `Unknown | Loader`
- lib/spack/spack/vendor/ruamel/yaml/main.py:1105:13: warning[possibly-missing-attribute] Attribute `_scanner` may be missing on object of type `@Todo | Loader`
+ lib/spack/spack/vendor/ruamel/yaml/main.py:1105:13: warning[possibly-missing-attribute] Attribute `_scanner` may be missing on object of type `Unknown | Loader`
- Found 4336 diagnostics
+ Found 4337 diagnostics

beartype (https://github.com/beartype/beartype)
- beartype/_decor/_nontype/decornontype.py:156:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- beartype/_decor/_nontype/decornontype.py:215:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ beartype/vale/_core/_valecore.py:322:30: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 495 diagnostics
+ Found 494 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/middleware/profiler.py:135:28: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/werkzeug/utils.py:498:19: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 407 diagnostics
+ Found 409 diagnostics

websockets (https://github.com/aaugustin/websockets)
+ src/websockets/legacy/server.py:632:29: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/websockets/legacy/server.py:632:29: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/websockets/legacy/server.py:632:29: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 34 diagnostics
+ Found 37 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ src/_pytest/python.py:475:28: error[call-non-callable] Object of type `Class` is not callable
+ src/_pytest/python.py:475:28: error[call-non-callable] Object of type `<Protocol with members 'pytest_generate_tests'>` is not callable
- Found 407 diagnostics
+ Found 409 diagnostics

starlette (https://github.com/encode/starlette)
+ starlette/_exception_handler.py:59:42: error[invalid-argument-type] Argument is incorrect: Expected `Request[State]`, found `Request[State] | WebSocket`
+ starlette/middleware/errors.py:176:38: error[invalid-await] `Unknown | Response | Awaitable[Response]` is not awaitable
- starlette/middleware/errors.py:181:23: error[call-non-callable] Object of type `None` is not callable
- Found 216 diagnostics
+ Found 217 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/downloadermiddlewares/retry.py:110:22: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ scrapy/downloadermiddlewares/retry.py:110:22: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 1789 diagnostics
+ Found 1791 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ src/graphql/execution/execute.py:1437:34: error[invalid-await] `Awaitable[bool] | bool` is not awaitable
+ src/graphql/type/definition.py:302:12: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 641 diagnostics
+ Found 643 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/_log_render.py:60:36: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ rich/text.py:622:31: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ rich/text.py:622:31: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 345 diagnostics
+ Found 348 diagnostics

ignite (https://github.com/pytorch/ignite)
+ ignite/handlers/base_logger.py:48:20: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 2034 diagnostics
+ Found 2035 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- dulwich/object_store.py:2922:12: error[unsupported-operator] Operator `in` is not supported between objects of type `ObjectID` and `Unknown | ((() -> dict[ObjectID, ObjectID]) & ~AlwaysTruthy & ~AlwaysFalsy) | dict[Unknown, Unknown]`
+ dulwich/object_store.py:2922:12: error[unsupported-operator] Operator `in` is not supported between objects of type `ObjectID` and `Unknown | ((() -> dict[ObjectID, ObjectID]) & ~AlwaysTruthy & ~AlwaysFalsy) | (dict[ObjectID, ObjectID] & ~AlwaysFalsy) | dict[Unknown, Unknown]`
- dulwich/pack.py:2255:78: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ dulwich/pack.py:3518:13: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ dulwich/repo.py:2696:23: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ dulwich/worktree.py:570:23: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ dulwich/worktree.py:570:23: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 229 diagnostics
+ Found 232 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/Auth.py:199:27: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 299 diagnostics
+ Found 300 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_core/intents/registries.py:471:16: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 259 diagnostics
+ Found 260 diagnostics

poetry (https://github.com/python-poetry/poetry)
+ src/poetry/utils/cache.py:147:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 978 diagnostics
+ Found 979 diagnostics

nox (https://github.com/wntrblm/nox)
+ nox/_option_set.py:162:20: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ nox/_option_set.py:162:20: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 24 diagnostics
+ Found 26 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/engines/numpy_engine.py:64:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 1581 diagnostics
+ Found 1580 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
+ src/schemathesis/core/marks.py:34:20: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 282 diagnostics
+ Found 283 diagnostics

vision (https://github.com/pytorch/vision)
+ test/datasets_utils.py:835:57: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ test/datasets_utils.py:835:57: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ test/datasets_utils.py:956:57: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ test/datasets_utils.py:956:57: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ torchvision/models/_utils.py:201:43: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 1403 diagnostics
+ Found 1408 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/json_schema.py:550:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/json_schema.py:1676:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pydantic/json_schema.py:1678:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 3155 diagnostics
+ Found 3152 diagnostics

tornado (https://github.com/tornadoweb/tornado)
+ tornado/escape.py:352:28: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _T@next | _VT@next`
+ tornado/gen.py:255:62: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `None | Awaitable[Unknown] | list[Awaitable[Unknown]] | dict[Any, Awaitable[Unknown]] | Future[Unknown]`, found `_T@next | _VT@next | _T@next`
+ tornado/tcpclient.py:258:27: error[invalid-assignment] Object of type `_ComplexLike` is not assignable to `int | float | timedelta | None`
- Found 327 diagnostics
+ Found 329 diagnostics

psycopg (https://github.com/psycopg/psycopg)
+ psycopg_pool/psycopg_pool/pool.py:652:20: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ psycopg_pool/psycopg_pool/pool.py:662:20: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 652 diagnostics
+ Found 654 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
+ koda_validate/_internal.py:129:23: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[SuccessT@_ToTupleStandardValidator] | Nothing`
+ koda_validate/_internal.py:157:23: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[SuccessT@_ToTupleStandardValidator] | Nothing`
+ koda_validate/_internal.py:220:30: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Valid[Any] | Invalid`
+ koda_validate/_internal.py:254:30: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Valid[A@_wrap_sync_validator] | Invalid`
+ koda_validate/_internal.py:256:24: error[invalid-return-type] Return type does not match returned value: expected `tuple[Literal[True], A@_wrap_sync_validator] | tuple[Literal[False], Invalid]`, found `tuple[Literal[False], Valid[A@_wrap_sync_validator] | Invalid]`
+ koda_validate/dataclasses.py:173:47: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[dict[Any, Any]] | Nothing`
+ koda_validate/dataclasses.py:224:47: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[dict[Any, Any]] | Nothing`
+ koda_validate/dictionary.py:113:47: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[dict[Any, Any]] | Nothing`
+ koda_validate/dictionary.py:169:47: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[dict[Any, Any]] | Nothing`
+ koda_validate/list.py:48:42: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[list[Any]] | Nothing`
+ koda_validate/list.py:85:42: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[list[Any]] | Nothing`
+ koda_validate/namedtuple.py:166:47: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[dict[Any, Any]] | Nothing`
+ koda_validate/namedtuple.py:217:47: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[dict[Any, Any]] | Nothing`
+ koda_validate/set.py:45:41: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[set[Any]] | Nothing`
+ koda_validate/set.py:88:41: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[set[Any]] | Nothing`
+ koda_validate/signature.py:299:29: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `Valid[Any] | Invalid` on object of type `dict[str, Invalid]`
+ koda_validate/signature.py:301:50: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Valid[Any] | Invalid`
+ koda_validate/signature.py:304:46: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Valid[Any] | Invalid`
+ koda_validate/signature.py:306:25: error[invalid-assignment] Invalid subscript assignment with key of type `str` and value of type `Valid[Any] | Invalid` on object of type `dict[str, Invalid]`
+ koda_validate/signature.py:315:46: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `Invalid`, found `Valid[Any] | Invalid`
+ koda_validate/tuple.py:294:48: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[tuple[Any, ...]] | Nothing`
+ koda_validate/tuple.py:335:48: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[tuple[Any, ...]] | Nothing`
+ koda_validate/tuple.py:415:48: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[tuple[Any, ...]] | Nothing`
+ koda_validate/tuple.py:459:48: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[tuple[Any, ...]] | Nothing`
+ koda_validate/typeddict.py:167:47: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[dict[Any, Any]] | Nothing`
+ koda_validate/typeddict.py:209:47: warning[possibly-missing-attribute] Attribute `val` may be missing on object of type `Unknown | Just[dict[Any, Any]] | Nothing`
- Found 405 diagnostics
+ Found 431 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
+ freqtrade/strategy/informative_decorator.py:157:49: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ freqtrade/strategy/informative_decorator.py:157:49: error[missing-argument] No argument provided for required parameter 1
+ freqtrade/strategy/informative_decorator.py:157:59: error[unknown-argument] Argument `column` does not match any known parameter
+ freqtrade/strategy/informative_decorator.py:159:19: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ freqtrade/strategy/informative_decorator.py:159:19: error[missing-argument] No argument provided for required parameter 1
+ freqtrade/strategy/informative_decorator.py:159:29: error[unknown-argument] Argument `column` does not match any known parameter
- Found 645 diagnostics
+ Found 651 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/compilers/compilers.py:1329:26: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ mesonbuild/compilers/compilers.py:1329:26: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ mesonbuild/compilers/d.py:551:30: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ mesonbuild/compilers/mixins/clike.py:403:40: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ mesonbuild/compilers/vala.py:159:26: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ mesonbuild/compilers/vala.py:159:26: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- mesonbuild/dependencies/detect.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `list[DependencyCandidate[ExternalDependency]]`, found `list[Unknown | DependencyCandidate[Unknown]] | list[Unknown | (((Environment, DependencyObjectKWs, /) -> list[DependencyCandidate[ExternalDependency]]) & DependencyCandidate[object] & ~type) | (DependencyCandidate[Unknown] & ~type)] | @Todo`
+ mesonbuild/dependencies/detect.py:190:16: error[invalid-return-type] Return type does not match returned value: expected `list[DependencyCandidate[ExternalDependency]]`, found `list[Unknown | DependencyCandidate[Unknown]] | list[Unknown | (((Environment, DependencyObjectKWs, /) -> list[DependencyCandidate[ExternalDependency]]) & DependencyCandidate[object] & ~type) | (DependencyCandidate[Unknown] & ~type)] | list[DependencyCandidate[ExternalDependency]]`
+ mesonbuild/interpreter/interpreterobjects.py:928:16: error[invalid-return-type] Return type does not match returned value: expected `str | int | Sequence[Divergent] | ... omitted 5 union elements`, found `Unknown | str | int | ... omitted 7 union elements`
- Found 2154 diagnostics
+ Found 2161 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
+ openlibrary/core/cache.py:487:23: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ openlibrary/core/cache.py:504:23: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ openlibrary/plugins/worksearch/code.py:272:34: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ openlibrary/plugins/worksearch/schemes/__init__.py:75:33: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ openlibrary/plugins/worksearch/schemes/__init__.py:81:24: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 1146 diagnostics
+ Found 1151 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-dask/prefect_dask/task_runners.py:354:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/integrations/prefect-dask/prefect_dask/task_runners.py:354:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- src/prefect/server/models/workers.py:299:64: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/prefect/server/models/workers.py:298:19: error[missing-argument] No arguments provided for required parameters 1, 2, 3, 4
+ src/prefect/server/models/workers.py:300:17: error[unknown-argument] Argument `occurred` does not match any known parameter
+ src/prefect/server/models/workers.py:301:17: error[unknown-argument] Argument `pre_update_work_pool` does not match any known parameter
+ src/prefect/server/models/workers.py:302:17: error[unknown-argument] Argument `work_pool` does not match any known parameter
+ src/prefect/tasks.py:568:40: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/prefect/tasks.py:568:40: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/prefect/tasks.py:568:40: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- src/prefect/tasks.py:1634:20: error[invalid-return-type] Return type does not match returned value: expected `list[State[R@Task]] | PrefectFutureList[R@Task]`, found `list[Unknown | PrefectDistributedFuture[R@Task]] | @Todo`
+ src/prefect/tasks.py:1634:20: error[invalid-return-type] Return type does not match returned value: expected `list[State[R@Task]] | PrefectFutureList[R@Task]`, found `list[Unknown | PrefectDistributedFuture[R@Task]] | Any`
+ src/prefect/utilities/_engine.py:40:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/prefect/utilities/_engine.py:66:29: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/prefect/utilities/_engine.py:69:29: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 5411 diagnostics
+ Found 5422 diagnostics

archinstall (https://github.com/archlinux/archinstall)
+ archinstall/lib/output.py:33:12: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ archinstall/tui/ui/components.py:1011:39: error[invalid-argument-type] Argument is incorrect: Expected `<special-form 'typing.Self'>`, found `MenuItem`
- Found 127 diagnostics
+ Found 129 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
+ pwndbg/aglib/heap/ptmalloc.py:267:30: error[call-non-callable] Object of type `~None & ~Type` is not callable
- pwndbg/aglib/heap/ptmalloc.py:268:28: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Value | None | @Todo`
+ pwndbg/aglib/heap/ptmalloc.py:268:28: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Value | None | Unknown`
+ pwndbg/aglib/heap/ptmalloc.py:600:30: error[call-non-callable] Object of type `~None & ~Type` is not callable
- pwndbg/aglib/heap/ptmalloc.py:602:28: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Value | None | @Todo`
+ pwndbg/aglib/heap/ptmalloc.py:602:28: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `Value | None | Unknown`
+ pwndbg/commands/ptmalloc2.py:51:15: error[call-non-callable] Object of type `~None` is not callable
- Found 1999 diagnostics
+ Found 2002 diagnostics

setuptools (https://github.com/pypa/setuptools)
+ setuptools/config/_apply_pyprojecttoml.py:82:13: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ setuptools/config/expand.py:329:14: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 1131 diagnostics
+ Found 1133 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/backends/zarr.py:1243:21: error[invalid-argument-type] Argument to function `grid_rechunk` is incorrect: Expected `tuple[slice[Any, Any, Any], ...]`, found `tuple[Unknown | str | slice[Any, Any, Any], ...]`
+ xarray/backends/zarr.py:1243:21: error[invalid-argument-type] Argument to function `grid_rechunk` is incorrect: Expected `tuple[slice[Any, Any, Any], ...]`, found `tuple[Unknown | Divergent | str | slice[Any, Any, Any], ...]`
- xarray/backends/zarr.py:1260:21: error[invalid-argument-type] Argument to function `validate_grid_chunks_alignment` is incorrect: Expected `tuple[slice[Any, Any, Any], ...]`, found `tuple[Unknown | str | slice[Any, Any, Any], ...]`
+ xarray/backends/zarr.py:1260:21: error[invalid-argument-type] Argument to function `validate_grid_chunks_alignment` is incorrect: Expected `tuple[slice[Any, Any, Any], ...]`, found `tuple[Unknown | Divergent | str | slice[Any, Any, Any], ...]`
+ xarray/core/common.py:518:20: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ xarray/core/dataarray.py:3250:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ xarray/core/dataset.py:5880:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ xarray/core/datatree_render.py:287:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 1753 diagnostics
+ Found 1757 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/wrapper/_implementations.py:435:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/hydra_zen/wrapper/_implementations.py:437:60: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 521 diagnostics
+ Found 519 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
+ strawberry/types/fields/resolver.py:250:16: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 345 diagnostics
+ Found 346 diagnostics

pycryptodome (https://github.com/Legrandin/pycryptodome)
- lib/Crypto/IO/_PBES.py:353:38: error[invalid-argument-type] Argument to function `scrypt` is incorrect: Expected `str`, found `@Todo | bytes`
+ lib/Crypto/IO/_PBES.py:353:38: error[invalid-argument-type] Argument to function `scrypt` is incorrect: Expected `str`, found `Unknown | bytes`
- lib/Crypto/Math/_IntegerBase.py:338:20: error[invalid-argument-type] Argument to function `bord` is incorrect: Expected `bytes`, found `@Todo | int`
+ lib/Crypto/Math/_IntegerBase.py:338:20: error[invalid-argument-type] Argument to function `bord` is incorrect: Expected `bytes`, found `Unknown | int`

pywin32 (https://github.com/mhammond/pywin32)
+ com/win32com/server/policy.py:145:18: error[call-non-callable] Object of type `str` is not callable
- Found 2694 diagnostics
+ Found 2695 diagnostics

bokeh (https://github.com/bokeh/bokeh)
+ src/bokeh/core/property/bases.py:188:20: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/bokeh/core/property/instance.py:106:29: error[invalid-assignment] Object of type `type[T@Object]` is not assignable to `type[Serializable]`
+ src/bokeh/core/property/instance.py:108:13: error[invalid-assignment] Object of type `type[Serializable]` is not assignable to attribute `_instance_type` of type `type[T@Object] | (() -> type[T@Object]) | str`
+ src/bokeh/io/notebook.py:566:18: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/bokeh/io/notebook.py:578:15: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/bokeh/plotting/graph.py:123:28: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ src/bokeh/resources.py:473:41: error[invalid-argument-type] Argument to function `__call__` is incorrect: Expected `list[str]`, found `list[Literal["bokeh", "bokeh-gl", "bokeh-widgets", "bokeh-tables", "bokeh-mathjax", "bokeh-api"]]`
+ src/bokeh/server/views/metadata_handler.py:60:24: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- src/bokeh/settings.py:414:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.IMMEDIATE]]`
+ src/bokeh/settings.py:414:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.IMMEDIATE]]`
- src/bokeh/settings.py:418:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.USER_SET]]`
+ src/bokeh/settings.py:418:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.USER_SET]]`
+ src/bokeh/settings.py:418:34: error[invalid-argument-type] Argument is incorrect: Expected `T@PrioritizedSetting | str`, found `str | (T@PrioritizedSetting & ~<class '_Unset'>) | (type[_Unset] & ~<class '_Unset'>)`
- src/bokeh/settings.py:422:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.CONFIG_OVERRIDE]]`
+ src/bokeh/settings.py:422:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.CONFIG_OVERRIDE]]`
- src/bokeh/settings.py:426:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.ENV_VAR]]`
+ src/bokeh/settings.py:426:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.ENV_VAR]]`
- src/bokeh/settings.py:430:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.CONFIG_USER]]`
+ src/bokeh/settings.py:430:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.CONFIG_USER]]`
- src/bokeh/settings.py:434:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.CONFIG_SYSTEM]]`
+ src/bokeh/settings.py:434:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.CONFIG_SYSTEM]]`
- src/bokeh/settings.py:438:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.DEV_DEFAULT]]`
+ src/bokeh/settings.py:438:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.DEV_DEFAULT]]`
+ src/bokeh/settings.py:438:34: error[invalid-argument-type] Argument is incorrect: Expected `T@PrioritizedSetting | str`, found `(Unknown & ~<class '_Unset'>) | (T@PrioritizedSetting & ~<class '_Unset'>) | (type[_Unset] & ~<class '_Unset'>)`
- src/bokeh/settings.py:442:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.DEFAULT]]`
+ src/bokeh/settings.py:442:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.DEFAULT]]`
+ src/bokeh/settings.py:442:34: error[invalid-argument-type] Argument is incorrect: Expected `T@PrioritizedSetting | str`, found `(T@PrioritizedSetting & ~<class '_Unset'>) | (type[_Unset] & ~<class '_Unset'>)`
- src/bokeh/settings.py:446:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | str, Literal[SettingProvenance.GLOBAL_DEFAULT]]`
+ src/bokeh/settings.py:446:20: error[invalid-return-type] Return type does not match returned value: expected `tuple[T@PrioritizedSetting, SettingProvenance]`, found `tuple[Unknown | T@PrioritizedSetting | str, Literal[SettingProvenance.GLOBAL_DEFAULT]]`
+ src/bokeh/settings.py:446:34: error[invalid-argument-type] Argument is incorrect: Expected `T@PrioritizedSetting | str`, found `(Unknown & ~<class '_Unset'>) | (T@PrioritizedSetting & ~<class '_Unset'>) | (type[_Unset] & ~<class '_Unset'>)`
- Found 871 diagnostics
+ Found 883 diagnostics

streamlit (https://github.com/streamlit/streamlit)
+ lib/streamlit/navigation/page.py:304:17: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ lib/streamlit/web/server/server_util.py:101:13: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 103 diagnostics
+ Found 105 diagnostics

jax (https://github.com/google/jax)
+ jax/_src/linear_util.py:323:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ jax/_src/linear_util.py:323:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ jax/_src/numpy/ufunc_api.py:182:12: error[call-non-callable] Object of type `int` is not callable
+ jax/_src/numpy/ufunc_api.py:257:12: error[call-non-callable] Object of type `int` is not callable
+ jax/_src/numpy/ufunc_api.py:377:12: error[call-non-callable] Object of type `int` is not callable
+ jax/_src/numpy/ufunc_api.py:444:12: error[call-non-callable] Object of type `int` is not callable
+ jax/_src/numpy/ufunc_api.py:444:45: error[call-non-callable] Object of type `int` is not callable
+ jax/_src/numpy/ufunc_api.py:521:12: error[call-non-callable] Object of type `int` is not callable
+ jax/_src/pallas/fuser/block_spec.py:2145:35: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `BlockSpec | tuple[BlockSpec, ...]`, found `BlockSpec | NoBlockSpec`
+ jax/_src/pallas/fuser/block_spec.py:2147:36: error[invalid-argument-type] Argument to bound method `__call__` is incorrect: Expected `BlockSpec | tuple[BlockSpec, ...]`, found `BlockSpec | NoBlockSpec`
+ jax/_src/pallas/fuser/block_spec.py:2149:51: error[invalid-argument-type] Argument to function `safe_map` is incorrect: Expected `Iterable[BlockSpec | NoBlockSpec]`, found `BlockSpec | tuple[BlockSpec, ...] | list[Unknown | BlockSpec | tuple[BlockSpec, ...]]`
+ jax/_src/xla_bridge.py:547:28: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ jax/experimental/pallas/ops/tpu/megablox/gmm.py:373:14: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ jax/experimental/pallas/ops/tpu/megablox/gmm.py:621:14: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 2873 diagnostics
+ Found 2887 diagnostics

ibis (https://github.com/ibis-project/ibis)
- ibis/expr/operations/core.py:65:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `(@Todo & ~Value[object, object]) | None`
+ ibis/expr/operations/core.py:65:34: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsInt | SupportsIndex | SupportsTrunc`, found `(Any & ~Expr & ~Value[object, object]) | None`
- ibis/expr/operations/core.py:67:36: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `(@Todo & ~Value[object, object]) | None`
+ ibis/expr/operations/core.py:67:36: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `(Any & ~Expr & ~Value[object, object]) | None`
+ ibis/expr/visualize.py:121:46: error[invalid-argument-type] Argument is incorrect: Expected `ibis.expr.operations.core.Node`, found `ibis.common.graph.Node`
+ ibis/expr/visualize.py:131:50: error[invalid-argument-type] Argument is incorrect: Expected `ibis.expr.operations.core.Node`, found `ibis.common.graph.Node`
+ ibis/expr/visualize.py:163:50: error[invalid-argument-type] Argument is incorrect: Expected `ibis.expr.operations.core.Node`, found `ibis.common.graph.Node`
+ ibis/expr/visualize.py:163:53: error[invalid-argument-type] Argument is incorrect: Expected `ibis.expr.operations.core.Node`, found `ibis.common.graph.Node`
+ ibis/selectors.py:456:28: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- Found 4609 diagnostics
+ Found 4614 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
+ hydpy/exe/xmltools.py:2373:20: error[invalid-assignment] Object of type `SetItem` is not assignable to `_TypeSetOrAddOrMultiplyItem@_get_changeitem`
- Found 669 diagnostics
+ Found 670 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/cluster/tests/test_dbscan.py:120:28: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `@Todo | ndarray[tuple[Any, ...], dtype[float64]]`
+ sklearn/cluster/tests/test_dbscan.py:120:28: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `Unknown | ndarray[tuple[Any, ...], dtype[float64]]`
- sklearn/cluster/tests/test_dbscan.py:120:41: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `@Todo | ndarray[tuple[Any, ...], dtype[float64]]`
+ sklearn/cluster/tests/test_dbscan.py:120:41: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `Unknown | ndarray[tuple[Any, ...], dtype[float64]]`
- sklearn/linear_model/tests/test_base.py:314:25: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `@Todo | ndarray[tuple[Any, ...], dtype[float64]]`
+ sklearn/linear_model/tests/test_base.py:314:25: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `Unknown | ndarray[tuple[Any, ...], dtype[float64]]`
- sklearn/linear_model/tests/test_base.py:328:25: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `@Todo | ndarray[tuple[Any, ...], dtype[float64]]`
+ sklearn/linear_model/tests/test_base.py:328:25: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `Unknown | ndarray[tuple[Any, ...], dtype[float64]]`
- sklearn/linear_model/tests/test_base.py:476:13: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `ndarray[tuple[Any, ...], dtype[float64]] | @Todo`
+ sklearn/linear_model/tests/test_base.py:476:13: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `ndarray[tuple[Any, ...], dtype[float64]] | Unknown`
- sklearn/linear_model/tests/test_coordinate_descent.py:1378:26: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `ndarray[tuple[Any, ...], dtype[float64]] | @Todo`
+ sklearn/linear_model/tests/test_coordinate_descent.py:1378:26: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `ndarray[tuple[Any, ...], dtype[float64]] | Unknown`
- sklearn/linear_model/tests/test_ridge.py:2282:13: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `ndarray[tuple[Any, ...], dtype[float64]] | @Todo`
+ sklearn/linear_model/tests/test_ridge.py:2282:13: warning[possibly-missing-attribute] Attribute `toarray` may be missing on object of type `ndarray[tuple[Any, ...], dtype[float64]] | Unknown`
- sklearn/metrics/_classification.py:2458:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `float | @Todo | int | None`
+ sklearn/metrics/_classification.py:2458:18: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `float | Unknown | int | None`
- sklearn/metrics/_classification.py:2458:52: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `float | @Todo | int | None`
+ sklearn/metrics/_classification.py:2458:52: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | Buffer | SupportsFloat | SupportsIndex`, found `float | Unknown | int | None`

pandas (https://github.com/pandas-dev/pandas)
+ pandas/core/apply.py:1062:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1062:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1062:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1066:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1066:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1066:25: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1078:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1078:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1078:21: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1146:19: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1146:19: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1146:19: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1183:26: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1183:26: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1183:26: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1529:22: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1529:22: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
+ pandas/core/apply.py:1529:22: error[call-top-callable] Object of type `Top[(...) -> object]` is not safe to call; its signature is not known
- pandas/core/arrays/boolean.py:260:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[tuple[Any, ...], dtype[Any]], ndarray[tuple[Any, ...], dtype[Any]]]`, found `tuple[@Todo | ndarray[tuple[int], dtype[Any]], Unknown | None | ndarray[tuple[Any, ...], dtype[numpy.bool[builtins.bool]]] | ndarray[tuple[Any, ...], dtype[Any]]]`
+ pandas/core/arrays/boolean.py:260:12: error[invalid-return-type] Return type does not match returned value: expected `tuple[ndarray[tuple[Any, ...], dtype[Any]], ndarray[tuple[Any, ...], dtype[Any]]]`, found `tuple[(Unknown & ndarray[tuple[object, ...], dtype[object]]) | ndarray[tuple[int], dtype[Any]], Unknown | None | ndarray[tuple[Any, ...], dtype[numpy.bool[builtins.bool]]] | ndarray[tuple[Any, ...], dtype[Any]]]`
+ pandas/core/computation/expr.py:715:37: error[call-non-callable] Object of type `~FuncNode` is not callable
- pandas/core/frame.py:882:56: error[invalid-argument-type] Argument to function `construct_1d_arraylike_from_scalar` is incorrect: Expected `str | bytes | date | ... omitted 10 union elements`, found `(@Todo & ~BlockManager & ~None & ~Top[dict[Unknown, Unknown]] & ~ndarray[tuple[object, ...], dtype[object]] & ~Series & ~Index & ~ExtensionArray) | (list[Unknown] & ~BlockManager & ~ndarray[tuple[object, ...], dtype[object]] & ~Series & ~Index & ~ExtensionArray)`
+ pandas/core/frame.py:882:56: error[invalid-argument-type] Argument to function `construct_1d_arraylike_from_scalar` is incorrect: Expected `str | bytes | date | ... omitted 10 union elements`, found `(Unknown & ~DataFrame & ~BlockManager & ~None & ~Top[dict[Unknown, Unknown]] & ~ndarray[tuple[object, ...], dtype[object]] & ~Series & ~Index & ~ExtensionArray) | (list[Unknown] & ~BlockManager & ~ndarray[tuple[object, ...], dtype[object]] & ~Series & ~Index & ~ExtensionArray)`
- pandas/core/frame.py:888:21: error[invalid-argument-type] Argument to function `construct_2d_arraylike_from_scalar` is incorrect: Expected `str | bytes | dat

... (truncated 103 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @carljm on 2026-01-09 03:43_

---

_Comment by @astral-sh-bot[bot] on 2026-01-09 03:48_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `call-top-callable` | 112 | 0 | 0 |
| `invalid-argument-type` | 23 | 62 | 17 |
| `invalid-await` | 42 | 1 | 0 |
| `possibly-missing-attribute` | 22 | 2 | 18 |
| `unused-ignore-comment` | 2 | 24 | 0 |
| `invalid-return-type` | 6 | 0 | 18 |
| `call-non-callable` | 16 | 1 | 0 |
| `unknown-argument` | 15 | 0 | 0 |
| `missing-argument` | 12 | 0 | 0 |
| `invalid-assignment` | 6 | 0 | 5 |
| `unresolved-attribute` | 7 | 0 | 0 |
| `index-out-of-bounds` | 5 | 0 | 0 |
| `too-many-positional-arguments` | 5 | 0 | 0 |
| `unsupported-operator` | 1 | 0 | 3 |
| `no-matching-overload` | 3 | 0 | 0 |
| `type-assertion-failure` | 2 | 0 | 0 |
| **Total** | **279** | **90** | **61** |


**[Full report with detailed diff](https://cf1b373d.ty-ecosystem-ext.pages.dev/diff)** ([timing results](https://cf1b373d.ty-ecosystem-ext.pages.dev/timing))



---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:96 on 2026-01-12 19:59_

Is it correct to just overwrite the previous `forms` here? Can you come up with a test case in which this loss of information causes a problem?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:118 on 2026-01-12 19:59_

It seems like we should be able to collapse these into a single case -- if we can't, it suggests the intersection case isn't being handled quite right.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:111 on 2026-01-12 20:01_

How do we know that `BindingError` is correct here? Don't we need to look at the individual results of the individual elements and use our error priority levels?

If this does need fixing, let's also add a test that fails with the current code and passes with the fixed code.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:139 on 2026-01-12 20:03_

In what scenario do we need to unwrap here? Is that scenario reachable, or should this just be an `.unwrap()`?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:139 on 2026-01-12 20:04_

This comment is misleading. `Bindings` does not represent a "union or intersection" -- it always represents a union (possibly size one) of callables, each element of which is an intersection (possibly size one).

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:222 on 2026-01-12 20:11_

Each input binding should have a length-one `.elements`, because in our DNF representation of types, intersections cannot contain unions. So we can just assert that invariant (with a comment) instead of flat-mapping.

If we did need to handle input unions here, we would need to distribute them, not just flat-map (the flat-map effectively turns an input union into an intersection, which is wrong.) But that's a bit complex, so no reason to do it when it should never happen.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:283 on 2026-01-12 20:12_

How is this method used? Do callers implicitly assume it's a union? Can flattening the union and intersection cause callers to treat the returned bindings incorrectly? Do we need to instead update callers to understand the two-level structure?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:291 on 2026-01-12 20:13_

Same question as for `iter` -- is this flattening correct, given how callers use this?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:375 on 2026-01-12 20:17_

Let's just call `element.retain_successful()` here, and handle the logic about which cases are no-ops etc internally within the `retain_successful()` method.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:586 on 2026-01-12 20:21_

I think the handling of `UnionDiagnostic` and `IntersectionDiagnostic` needs to be layered instead. If we have an intersection inside a union, and multiple bindings in that intersection fail with the same priority, we should report those errors with _both_ union and intersection context. (Let's add a test demonstrating this. It should be a test with snapshotted diagnostics.)

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/call/bind.rs`:470 on 2026-01-12 20:30_

We could use `is_intersection()` here.

---

_@carljm reviewed on 2026-01-12 20:32_

---
