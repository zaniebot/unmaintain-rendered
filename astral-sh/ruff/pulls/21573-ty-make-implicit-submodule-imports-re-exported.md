```yaml
number: 21573
title: "[ty] make implicit submodule imports re-exported"
type: pull_request
state: merged
author: Gankra
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: gankra/pub-imp
created_at: 2025-11-21T21:32:43Z
updated_at: 2025-11-22T01:42:12Z
url: https://github.com/astral-sh/ruff/pull/21573
synced_at: 2026-01-12T15:57:28Z
```

# [ty] make implicit submodule imports re-exported

---

_@Gankra_

Thus they work in `.pyi` files

Closes https://github.com/astral-sh/ty/issues/1609

---

_Review requested from @carljm by @Gankra on 2025-11-21 21:32_

---

_Label `ty` added by @Gankra on 2025-11-21 21:32_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-21 21:32_

---

_Review requested from @sharkdp by @Gankra on 2025-11-21 21:32_

---

_Review requested from @dcreager by @Gankra on 2025-11-21 21:32_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-21 21:32_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 21:34_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 21:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:287:34: error[unresolved-attribute] Module `asyncio` has no member `futures`
- src/anyio/_backends/_asyncio.py:1095:15: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- Found 91 diagnostics
+ Found 89 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ pyinstrument/frame.py:346:89: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pyinstrument/renderers/jsonrenderer.py:17:81: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 40 diagnostics
+ Found 42 diagnostics

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/verify.py:154:12: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 105 diagnostics
+ Found 104 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/cray_manifest.py:223:29: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 7925 diagnostics
+ Found 7924 diagnostics

websockets (https://github.com/aaugustin/websockets)
- src/websockets/legacy/server.py:679:28: error[unresolved-attribute] Module `asyncio` has no member `base_events`
- Found 41 diagnostics
+ Found 40 diagnostics

twine (https://github.com/pypa/twine)
- twine/auth.py:74:25: error[unresolved-attribute] Module `requests` has no member `models`
- twine/auth.py:75:11: error[unresolved-attribute] Module `requests` has no member `models`
- twine/auth.py:88:21: error[unresolved-attribute] Module `requests` has no member `models`
- Found 11 diagnostics
+ Found 8 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/core/engine.py:295:17: error[unresolved-attribute] Module `asyncio` has no member `exceptions`
- Found 1727 diagnostics
+ Found 1726 diagnostics

paasta (https://github.com/yelp/paasta)
+ paasta_tools/api/tweens/profiling.py:26:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- paasta_tools/check_oom_events.py:120:20: error[unresolved-attribute] Module `json` has no member `decoder`
- paasta_tools/check_spark_jobs.py:61:13: error[unresolved-attribute] Module `requests` has no member `exceptions`
- paasta_tools/check_spark_jobs.py:61:42: error[unresolved-attribute] Module `requests` has no member `exceptions`
- paasta_tools/cli/cmds/remote_run.py:81:12: error[unresolved-attribute] Module `json` has no member `decoder`
- paasta_tools/kubernetes_tools.py:2786:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- paasta_tools/mesos/parallel.py:34:9: error[unresolved-attribute] Module `concurrent.futures` has no member `thread`
+ paasta_tools/mesos/parallel.py:34:9: error[unresolved-attribute] Object of type `Mapping[Any, Any]` has no attribute `clear`
- paasta_tools/secret_tools.py:75:12: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 1106 diagnostics
+ Found 1101 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/benchmarks/test_async_iterable.py:26:12: error[unresolved-attribute] Module `asyncio` has no member `events`
- tests/benchmarks/test_async_iterable.py:27:5: error[unresolved-attribute] Module `asyncio` has no member `events`
- tests/benchmarks/test_async_iterable.py:29:5: error[unresolved-attribute] Module `asyncio` has no member `events`
- tests/benchmarks/test_execution_async.py:43:12: error[unresolved-attribute] Module `asyncio` has no member `events`
- tests/benchmarks/test_execution_async.py:44:5: error[unresolved-attribute] Module `asyncio` has no member `events`
- tests/benchmarks/test_execution_async.py:48:5: error[unresolved-attribute] Module `asyncio` has no member `events`
- Found 537 diagnostics
+ Found 531 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/endpoints.py:108:20: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 225 diagnostics
+ Found 224 diagnostics

alerta (https://github.com/alerta/alerta)
- alerta/utils/client.py:126:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- alerta/utils/client.py:135:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- alerta/utils/client.py:144:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- alerta/utils/client.py:152:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- Found 552 diagnostics
+ Found 548 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/run/no_terminal.py:87:16: error[unresolved-attribute] Module `tkinter` has no member `font`
- porcupine/plugins/run/no_terminal.py:481:17: error[unresolved-attribute] Module `tkinter` has no member `font`
- porcupine/settings.py:453:17: error[unresolved-attribute] Module `tkinter` has no member `font`
- porcupine/settings.py:920:31: error[unresolved-attribute] Module `tkinter` has no member `font`
- Found 21 diagnostics
+ Found 17 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- src/check_jsonschema/checker.py:54:10: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/cli/main_command.py:254:27: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/cli/param_types.py:75:15: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/cli/param_types.py:123:30: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/cli/parse_result.py:36:36: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/cli/parse_result.py:86:37: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/reporter.py:83:26: error[unresolved-attribute] Module `jsonschema` has no member `exceptions`
- src/check_jsonschema/reporter.py:170:34: error[unresolved-attribute] Module `jsonschema` has no member `exceptions`
- src/check_jsonschema/schema_loader/main.py:23:27: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:44:12: error[unresolved-attribute] Module `jsonschema` has no member `validators`
- src/check_jsonschema/schema_loader/main.py:51:27: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:54:12: error[unresolved-attribute] Module `jsonschema` has no member `validators`
- src/check_jsonschema/schema_loader/main.py:71:10: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:76:27: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:84:31: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:145:10: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:154:10: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:170:29: error[unresolved-attribute] Module `jsonschema` has no member `validators`
- src/check_jsonschema/schema_loader/main.py:200:23: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:204:25: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:212:28: error[unresolved-attribute] Module `jsonschema` has no member `validators`
- src/check_jsonschema/schema_loader/main.py:230:15: error[unresolved-attribute] Module `jsonschema` has no member `exceptions`
- src/check_jsonschema/schema_loader/main.py:274:10: error[unresolved-attribute] Module `jsonschema` has no member `protocols`
- src/check_jsonschema/schema_loader/main.py:275:28: error[unresolved-attribute] Module `jsonschema` has no member `validators`
- src/check_jsonschema/schema_loader/main.py:276:32: error[unresolved-attribute] Module `jsonschema` has no member `validators`
- Found 52 diagnostics
+ Found 27 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/Requester.py:294:27: error[unresolved-attribute] Module `requests` has no member `models`
- github/Requester.py:294:63: error[unresolved-attribute] Module `requests` has no member `models`
- tests/Retry.py:77:32: error[unresolved-attribute] Module `requests` has no member `exceptions`
- Found 292 diagnostics
+ Found 289 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
- tests/test_runserver_cleanup.py:30:16: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- tests/test_runserver_cleanup.py:31:16: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- Found 39 diagnostics
+ Found 37 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/repositories/http_repository.py:418:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- src/poetry/repositories/pypi_repository.py:221:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- tests/utils/test_authenticator.py:259:19: error[unresolved-attribute] Module `requests` has no member `exceptions`
- tests/utils/test_authenticator.py:278:15: error[unresolved-attribute] Module `requests` has no member `exceptions`
- tests/utils/test_authenticator.py:348:24: error[unresolved-attribute] Module `requests` has no member `exceptions`
- Found 972 diagnostics
+ Found 967 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/cli/commands/run/loaders.py:47:42: error[unresolved-attribute] Module `requests` has no member `exceptions`
- src/schemathesis/core/loaders.py:26:24: error[unresolved-attribute] Module `requests` has no member `exceptions`
- src/schemathesis/core/loaders.py:28:26: error[unresolved-attribute] Module `requests` has no member `exceptions`
- src/schemathesis/core/loaders.py:98:43: error[unresolved-attribute] Module `requests` has no member `exceptions`
- src/schemathesis/engine/errors.py:301:30: error[unresolved-attribute] Module `requests` has no member `exceptions`
- src/schemathesis/engine/errors.py:303:30: error[unresolved-attribute] Module `requests` has no member `exceptions`
- src/schemathesis/engine/errors.py:425:43: error[unresolved-attribute] Module `requests` has no member `exceptions`
- src/schemathesis/engine/phases/stateful/_executor.py:257:16: error[unresolved-attribute] Module `unittest` has no member `case`
- src/schemathesis/engine/phases/unit/_executor.py:140:23: error[unresolved-attribute] Module `unittest` has no member `case`
- Found 299 diagnostics
+ Found 290 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- dragonchain/contract_invoker/contract_invoker_service.py:67:28: error[unresolved-attribute] Module `json` has no member `decoder`
- dragonchain/lib/dto/bnb_utest.py:227:37: error[unresolved-attribute] Module `requests` has no member `exceptions`
- dragonchain/lib/dto/bnb_utest.py:232:37: error[unresolved-attribute] Module `requests` has no member `exceptions`
- dragonchain/lib/interfaces/docker_registry.py:55:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- dragonchain/scheduler/scheduler.py:47:12: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 438 diagnostics
+ Found 433 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/test/concurrent_test.py:172:33: error[unresolved-attribute] Module `concurrent.futures` has no member `thread`
- tornado/test/concurrent_test.py:186:33: error[unresolved-attribute] Module `concurrent.futures` has no member `thread`
- tornado/test/concurrent_test.py:200:35: error[unresolved-attribute] Module `concurrent.futures` has no member `thread`
- tornado/test/concurrent_test.py:214:33: error[unresolved-attribute] Module `concurrent.futures` has no member `thread`
- Found 329 diagnostics
+ Found 325 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/pandas/types.py:18:5: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandera/api/pandas/types.py:18:5: error[unresolved-attribute] Module `pandas.core` has no member `dtypes`
- pandera/api/pandas/types.py:23:5: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandera/api/pandas/types.py:23:5: error[unresolved-attribute] Module `pandas.core` has no member `groupby`
- pandera/api/pandas/types.py:24:5: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandera/api/pandas/types.py:24:5: error[unresolved-attribute] Module `pandas.core` has no member `groupby`
- pandera/engines/type_aliases.py:10:23: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandera/engines/type_aliases.py:10:23: error[unresolved-attribute] Module `pandas.core` has no member `dtypes`
- pandera/engines/type_aliases.py:11:24: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandera/engines/type_aliases.py:11:24: error[unresolved-attribute] Module `pandas.core` has no member `dtypes`
- pandera/io/pandas_io.py:429:16: error[unresolved-attribute] Module `json` has no member `decoder`
- pandera/typing/pandas.py:106:9: error[unresolved-attribute] Module `pandas` has no member `core`
+ pandera/typing/pandas.py:106:9: error[unresolved-attribute] Module `pandas.core` has no member `dtypes`
- tests/pandas/test_pandas_engine.py:200:16: error[unresolved-attribute] Module `pytz` has no member `exceptions`
- tests/pandas/test_pandas_engine.py:210:28: error[unresolved-attribute] Module `pytz` has no member `exceptions`
- tests/pandas/test_typing.py:320:19: error[unresolved-attribute] Module `pandas` has no member `core`
+ tests/pandas/test_typing.py:320:19: error[unresolved-attribute] Module `pandas.core` has no member `dtypes`
- tests/pandas/test_typing.py:482:12: error[unresolved-attribute] Module `pandas` has no member `core`
+ tests/pandas/test_typing.py:482:12: error[unresolved-attribute] Module `pandas.core` has no member `dtypes`
- Found 1640 diagnostics
+ Found 1637 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
- cki_lib/footer.py:75:25: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/footer.py:75:50: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `tuple[str, str]`?
- cki_lib/footer.py:160:20: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/gitlab.py:290:6: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/cluster.py:38:36: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/cluster.py:38:61: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[Any]`?
- cki_lib/inttests/cluster.py:106:26: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/cluster.py:192:47: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/cluster.py:352:56: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/remote_responses.py:31:13: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/remote_responses.py:37:17: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/remote_responses.py:109:10: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/remote_responses.py:109:35: error[invalid-type-form] List literals are not allowed in this context in a type expression: Did you mean `list[PreparedRequest]`?
- cki_lib/inttests/remote_responses.py:132:35: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/remote_responses.py:166:15: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/remote_responses.py:179:28: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/inttests/sqs.py:33:22: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/messagequeue.py:396:10: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/messagequeue.py:550:10: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/messagequeue.py:562:10: error[unresolved-attribute] Module `collections` has no member `abc`
- cki_lib/yaml.py:76:41: error[unresolved-attribute] Module `yaml` has no member `nodes`
- cki_lib/yaml.py:78:62: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- inttests/test_vault.py:25:32: error[unresolved-attribute] Module `requests` has no member `exceptions`
- tests/kcidb/test_kcidb_file.py:93:36: error[unresolved-attribute] Module `json` has no member `decoder`
- tests/test_messagequeue.py:839:42: error[unresolved-attribute] Module `collections` has no member `abc`
- Found 239 diagnostics
+ Found 214 diagnostics

vision (https://github.com/pytorch/vision)
- torchvision/utils.py:805:22: error[unresolved-attribute] Module `collections` has no member `abc`
- Found 1461 diagnostics
+ Found 1460 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/addons/view.py:146:12: error[unresolved-attribute] Module `collections` has no member `abc`
- mitmproxy/addons/view.py:724:16: error[unresolved-attribute] Module `collections` has no member `abc`
- test/bench/benchmark.py:34:20: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
+ test/mitmproxy/addons/test_view.py:543:16: warning[possibly-missing-attribute] Attribute `timestamp_created` may be missing on object of type `Unknown | Flow | None`
+ test/mitmproxy/addons/test_view.py:547:16: warning[possibly-missing-attribute] Attribute `timestamp_created` may be missing on object of type `Unknown | Flow | None`
+ test/mitmproxy/addons/test_view.py:552:16: warning[possibly-missing-attribute] Attribute `timestamp_created` may be missing on object of type `Unknown | Flow | None`
+ test/mitmproxy/addons/test_view.py:557:16: warning[possibly-missing-attribute] Attribute `timestamp_created` may be missing on object of type `Unknown | Flow | None`
- test/mitmproxy/proxy/test_mode_servers.py:214:20: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- test/mitmproxy/proxy/test_mode_servers.py:215:20: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- test/mitmproxy/test_http.py:1220:28: error[unresolved-attribute] Module `json` has no member `decoder`
- test/mitmproxy/test_http.py:1231:28: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 1846 diagnostics
+ Found 1843 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- schema_salad/fetcher.py:61:18: error[unresolved-attribute] Module `requests` has no member `sessions`
- schema_salad/ref_resolver.py:151:18: error[unresolved-attribute] Module `requests` has no member `sessions`
- schema_salad/tests/test_codegen_errors.py:174:36: error[unresolved-attribute] Module `importlib` has no member `abc`
- schema_salad/tests/test_fetch.py:16:18: error[unresolved-attribute] Module `requests` has no member `sessions`
- schema_salad/tests/test_fetch.py:49:18: error[unresolved-attribute] Module `requests` has no member `sessions`
- schema_salad/utils.py:34:55: error[unresolved-attribute] Module `requests` has no member `sessions`
- Found 210 diagnostics
+ Found 204 diagnostics

speedrun.com_global_scoreboard_webapp (https://github.com/Avasam/speedrun.com_global_scoreboard_webapp)
- backend/services/user_updater.py:114:9: error[unresolved-attribute] Module `requests` has no member `exceptions`
- Found 65 diagnostics
+ Found 64 diagnostics

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- sklearn/decomposition/tests/test_nmf.py:38:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:84:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:96:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:138:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:161:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:203:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:220:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:289:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:303:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:336:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:355:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:457:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:478:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:513:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:588:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:640:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:716:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:840:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/decomposition/tests/test_nmf.py:866:11: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/preprocessing/_polynomial.py:326:37: error[unresolved-attribute] Module `collections` has no member `abc`
- sklearn/utils/_testing.py:67:12: error[unresolved-attribute] Module `unittest` has no member `case`
- sklearn/utils/tests/test_validation.py:100:40: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/utils/tests/test_validation.py:101:45: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- sklearn/utils/validation.py:1462:16: error[unresolved-attribute] Module `numpy.random` has no member `mtrand`
- Found 2277 diagnostics
+ Found 2253 diagnostics

freqtrade (https://github.com/freqtrade/freqtrade)
- freqtrade/plugins/pairlist/RemotePairList.py:200:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- Found 686 diagnostics
+ Found 685 diagnostics

mkdocs (https://github.com/mkdocs/mkdocs)
- mkdocs/tests/build_tests.py:950:22: error[unresolved-attribute] Module `markdown` has no member `extensions`
- Found 228 diagnostics
+ Found 227 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- src/py/pyodide/webloop.py:959:16: error[unresolved-attribute] Module `asyncio` has no member `events`
- Found 990 diagnostics
+ Found 989 diagnostics

pyppeteer (https://github.com/pyppeteer/pyppeteer)
- pyppeteer/page.py:192:25: error[unresolved-attribute] Module `asyncio` has no member `futures`
- Found 88 diagnostics
+ Found 87 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_io_windows.py:711:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_core/_io_windows.py:713:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_core/_windows_cffi.py:9:24: error[unresolved-attribute] Module `cffi` has no member `api`
- src/trio/_core/_windows_cffi.py:10:24: error[unresolved-attribute] Module `cffi` has no member `api`
- src/trio/_tests/test_file_io.py:101:31: error[unresolved-attribute] Module `importlib` has no member `abc`
- src/trio/_tests/test_wait_for_object.py:149:43: error[invalid-argument-type] Argument to bound method `cast` is incorrect: Expected `_CDataBase | int`, found `Handle`
- src/trio/_tests/test_wait_for_object.py:164:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/trio/_tests/test_wait_for_object.py:200:43: error[invalid-argument-type] Argument to bound method `cast` is incorrect: Expected `_CDataBase | int`, found `Handle`
- src/trio/_wait_for_object.py:63:9: error[invalid-assignment] Cannot assign to a subscript on an object of type `HandleArray`
- Found 528 diagnostics
+ Found 519 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- cloudinit/safeyaml.py:238:25: error[unresolved-attribute] Module `yaml` has no member `dumper`
- cloudinit/safeyaml.py:277:51: error[unresolved-attribute] Module `yaml` has no member `dumper`
- cloudinit/sources/DataSourceCloudCIX.py:152:16: error[unresolved-attribute] Module `json` has no member `decoder`
- cloudinit/sources/DataSourceScaleway.py:89:43: error[unresolved-attribute] Module `requests` has no member `exceptions`
- cloudinit/sources/DataSourceVMware.py:938:32: error[unresolved-attribute] Module `collections` has no member `abc`
- cloudinit/url_helper.py:809:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- tests/unittests/sources/azure/test_imds.py:87:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- tests/unittests/sources/test_azure.py:401:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- tests/unittests/sources/test_ec2.py:705:22: error[unresolved-attribute] Module `requests` has no member `exceptions`
- tests/unittests/test_url_helper.py:251:23: error[unresolved-attribute] Module `requests` has no member `exceptions`
- Found 1201 diagnostics
+ Found 1191 diagnostics

apprise (https://github.com/caronc/apprise)
- apprise/persistent_store.py:959:13: error[unresolved-attribute] Module `json` has no member `decoder`
- apprise/plugins/vapid/subscription.py:89:21: error[unresolved-attribute] Module `json` has no member `decoder`
- apprise/plugins/vapid/subscription.py:345:17: error[unresolved-attribute] Module `json` has no member `decoder`
- apprise/utils/base64.py:74:13: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 2586 diagnostics
+ Found 2582 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- openlibrary/core/admin.py:82:13: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/core/admin.py:127:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/core/fulltext.py:43:12: error[unresolved-attribute] Module `json` has no member `decoder`
- openlibrary/core/models.py:77:17: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/core/models.py:451:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/core/models.py:453:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/core/vendors.py:476:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/core/vendors.py:478:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/core/wikidata.py:215:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/plugins/openlibrary/code.py:1100:25: error[unresolved-attribute] Module `json` has no member `decoder`
- openlibrary/plugins/openlibrary/pd.py:63:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- openlibrary/plugins/recaptcha/recaptcha.py:46:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- Found 957 diagnostics
+ Found 945 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/arglist.py:296:33: error[unresolved-attribute] Module `collections` has no member `abc`
- mesonbuild/compilers/mixins/clike.py:407:43: error[unresolved-attribute] Module `collections` has no member `abc`
- mesonbuild/mtest.py:1342:27: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- mesonbuild/mtest.py:1378:55: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
+ mesonbuild/mtest.py:1375:46: error[invalid-argument-type] Argument to function `collect_stdo` is incorrect: Expected `StreamReader`, found `Unknown | StreamReader | None`
+ mesonbuild/mtest.py:1379:46: error[invalid-argument-type] Argument to function `collect_stde` is incorrect: Expected `StreamReader`, found `Unknown | StreamReader | None`
- mesonbuild/mtest.py:1617:21: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- mesonbuild/mtest.py:1618:22: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- mesonbuild/mtest.py:1619:22: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- mesonbuild/mtest.py:1621:22: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- Found 1871 diagnostics
+ Found 1865 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/http/async_base_view.py:171:16: error[unresolved-attribute] Module `json` has no member `decoder`
- strawberry/http/sync_base_view.py:80:16: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 400 diagnostics
+ Found 398 diagnostics

setuptools (https://github.com/pypa/setuptools)
- setuptools/tests/test_build_meta.py:65:21: error[unresolved-attribute] Module `concurrent.futures` has no member `process`
- Found 1251 diagnostics
+ Found 1250 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- tests/test_fetch.py:24:18: error[unresolved-attribute] Module `requests` has no member `sessions`
- Found 193 diagnostics
+ Found 192 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- src/scikit_build_core/program_search.py:144:17: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 46 diagnostics
+ Found 44 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/cli/deployment.py:66:27: error[unresolved-attribute] Module `yaml` has no member `representer`
- src/prefect/cli/deployment.py:78:1: error[unresolved-attribute] Module `yaml` has no member `representer`
- src/prefect/cli/events.py:83:46: error[unresolved-attribute] Module `asyncio` has no member `exceptions`
- src/prefect/utilities/schema_tools/hydration.py:213:17: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 3273 diagnostics
+ Found 3269 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/embed/bundle.py:301:24: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 692 diagnostics
+ Found 691 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
- django-stubs/views/generic/dates.pyi:61:52: error[unresolved-attribute] Module `django.db.models` has no member `query`
- django-stubs/views/generic/dates.pyi:64:25: error[unresolved-attribute] Module `django.db.models` has no member `query`
- django-stubs/views/generic/dates.pyi:65:10: error[unresolved-attribute] Module `django.db.models` has no member `query`
- django-stubs/views/generic/detail.pyi:11:15: error[unresolved-attribute] Module `django.db.models` has no member `query`
- django-stubs/views/generic/detail.pyi:17:36: error[unresolved-attribute] Module `django.db.models` has no member `query`
- django-stubs/views/generic/detail.pyi:18:31: error[unresolved-attribute] Module `django.db.models` has no member `query`
- tests/assert_type/views/test_generic.py:15:1: error[type-assertion-failure] Type `QuerySet[MyModel, MyModel] | None` does not match asserted type `Unknown | None`
- Found 496 diagnostics
+ Found 489 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- ddtrace/contrib/internal/unittest/patch.py:303:46: error[unresolved-attribute] Module `unittest` has no member `runner`
- tests/contrib/kafka/test_kafka.py:1017:16: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 8012 diagnostics
+ Found 8010 diagnostics

zulip (https://github.com/zulip/zulip)
- scripts/lib/check_rabbitmq_queue.py:197:20: error[unresolved-attribute] Module `json` has no member `decoder`
- zerver/lib/outgoing_webhook.py:397:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/outgoing_webhook.py:409:13: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/outgoing_webhook.py:409:50: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/outgoing_webhook.py:419:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/remote_server.py:175:9: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/remote_server.py:176:9: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/remote_server.py:177:9: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/tex.py:51:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/tex.py:54:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/url_preview/oembed.py:12:32: error[unresolved-attribute] Module `json` has no member `decoder`
- zerver/lib/url_preview/oembed.py:12:62: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/lib/url_preview/preview.py:79:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/migrations/0590_alter_realm_can_create_groups.py:16:27: error[unresolved-attribute] Module `django.db.models` has no member `deletion`
- zerver/migrations/0593_alter_realm_manage_all_groups.py:16:27: error[unresolved-attribute] Module `django.db.models` has no member `deletion`
- zerver/tests/test_auth_backends.py:1945:29: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_markdown.py:457:55: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_markdown.py:464:55: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_outgoing_webhook_system.py:35:11: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_outgoing_webhook_system.py:39:11: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_outgoing_webhook_system.py:43:11: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_outgoing_webhook_system.py:576:18: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_outgoing_webhook_system.py:633:18: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_push_notifications.py:3071:18: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_push_notifications.py:3087:18: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_push_notifications.py:3105:18: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tests/test_push_notifications.py:3121:18: error[unresolved-attribute] Module `requests` has no member `exceptions`
- zerver/tornado/handlers.py:162:37: error[unresolved-attribute] Module `django.http` has no member `cookie`
- Found 3521 diagnostics
+ Found 3493 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/accounting/export/csv.py:112:20: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/accounting/structures/processed_event.py:246:16: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/accounting/types.py:87:16: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/chain/aggregator.py:1021:29: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/base/modules/basenames/decoder.py:136:17: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/bitcoin/manager.py:129:21: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/bitcoin/manager.py:131:21: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/ethereum/abi.py:43:12: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/chain/ethereum/airdrops.py:254:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/ethereum/airdrops.py:315:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/ethereum/airdrops.py:470:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/ethereum/airdrops.py:794:40: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/ethereum/modules/eth2/beacon.py:113:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/ethereum/modules/l2/loopring.py:341:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/ethereum/modules/yearn/utils.py:100:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/evm/decoding/balancer/utils.py:57:13: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/evm/node_inquirer.py:373:21: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/substrate/manager.py:304:17: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/substrate/manager.py:344:13: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/substrate/manager.py:367:13: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/substrate/manager.py:408:17: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/substrate/manager.py:446:17: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/substrate/manager.py:474:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/chain/zksync_lite/manager.py:127:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/db/updates.py:142:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/db/updates.py:147:16: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/db/upgrades/v35_v36.py:59:20: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/db/upgrades/v40_v41.py:175:20: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/exchanges/binance.py:361:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/bitcoinde.py:134:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/bitfinex.py:214:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/bitmex.py:243:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/bitpanda.py:364:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/bitpanda.py:391:16: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/exchanges/bitstamp.py:546:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/bybit.py:230:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/coinbase.py:363:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/cryptocom.py:192:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/gemini.py:218:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/iconomi.py:124:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/independentreserve.py:204:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/kraken.py:134:12: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/exchanges/kraken.py:371:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/kucoin.py:247:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/okx.py:205:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/okx.py:209:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/poloniex.py:267:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/utils.py:206:34: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/exchanges/woo.py:277:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/beaconchain/service.py:122:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/blockscout.py:90:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/coingecko.py:582:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/coingecko.py:601:16: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/externalapis/cowswap.py:60:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/cowswap.py:68:16: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/externalapis/cowswap.py:122:23: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/externalapis/cryptocompare.py:294:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/defillama.py:84:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/defillama.py:103:16: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/externalapis/etherscan.py:228:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/github.py:22:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/gnosispay.py:122:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/graph.py:84:17: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/helius.py:81:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/hyperliquid.py:64:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/opensea.py:184:20: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/externalapis/xratescom.py:31:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/globaldb/asset_updates/manager.py:204:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/globaldb/asset_updates/manager.py:209:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/icons.py:157:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:384:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:417:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:485:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:517:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:565:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:615:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:714:24: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:758:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:786:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:826:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:858:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/premium/premium.py:890:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/tests/api/test_premium.py:153:21: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/tests/api/test_premium.py:197:21: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/tests/exchanges/test_bitstamp.py:47:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/tests/exchanges/test_kucoin.py:53:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/tests/exchanges/test_woo.py:36:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/tests/unit/test_inquirer.py:709:84: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/tests/utils/substrate.py:66:17: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/usage_analytics.py:59:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/usage_analytics.py:83:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/usage_analytics.py:150:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/utils/network.py:112:12: error[unresolved-attribute] Module `json` has no member `decoder`
- rotkehlchen/utils/network.py:174:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/utils/network.py:204:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- rotkehlchen/utils/network.py:219:16: error[unresolved-attribute] Module `json` has no member `decoder`
- Found 2135 diagnostics
+ Found 2039 diagnostics

core (https://github.com/home-assistant/core)
- homeassistant/auth/providers/command_line.py:76:24: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- homeassistant/components/amcrest/camera.py:177:30: error[unresolved-attribute] Module `asyncio` has no member `tasks`
- homeassistant/components/arest/binary_sensor.py:51:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/binary_sensor.py:56:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/binary_sensor.py:111:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/sensor.py:74:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/sensor.py:79:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/sensor.py:207:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/switch.py:62:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/switch.py:67:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/switch.py:160:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/arest/switch.py:207:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/bbox/sensor.py:105:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/bbox/sensor.py:201:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/bosch_alarm/config_flow.py:120:17: error[unresolved-attribute] Module `asyncio` has no member `exceptions`
- homeassistant/components/bosch_alarm/config_flow.py:191:13: error[unresolved-attribute] Module `asyncio` has no member `exceptions`
- homeassistant/components/command_line/utils.py:54:20: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
- homeassistant/components/concord232/alarm_control_panel.py:62:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
- homeassistant/components/concord232/alarm_control_panel.py:90:16: error[unresolved-attribute] Module `requests` has no member `exceptio

... (truncated 137 lines) ...
```

</details>


No memory usage changes detected ✅



---

_@carljm approved on 2025-11-21 21:37_

---

_Label `ecosystem-analyzer` removed by @Gankra on 2025-11-21 21:46_

---

_Label `ecosystem-analyzer` added by @Gankra on 2025-11-21 21:46_

---

_Comment by @carljm on 2025-11-21 21:53_

Ecosystem looks great to me here -- the few added diagnostics just look like cases where we now have type information instead of `Unknown`.

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 21:54_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 470 | 11 |
| `unused-ignore-comment` | 3 | 4 | 0 |
| `invalid-argument-type` | 2 | 2 | 0 |
| `possibly-missing-attribute` | 4 | 0 | 0 |
| `invalid-type-form` | 0 | 3 | 0 |
| `invalid-assignment` | 0 | 1 | 0 |
| `type-assertion-failure` | 0 | 1 | 0 |
| **Total** | **9** | **481** | **11** |

**[Full report with detailed diff](https://gankra-pub-imp.ecosystem-663.pages.dev/diff)** ([timing results](https://gankra-pub-imp.ecosystem-663.pages.dev/timing))




---

_Comment by @Gankra on 2025-11-21 21:56_

oh THERE's the missing 400 fixes I thought one of my followups would hit!

The one followup I forgot to try 😅 

---

_Comment by @codspeed-hq[bot] on 2025-11-21 22:05_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fpub-imp?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21573 will **degrade performances by 4.98%**

<sub>Comparing <code>gankra/pub-imp</code> (2900cf4) with <code>main</code> (09d457a)</sub>



### Summary

`❌ 1 (👁 1)` regression  
`✅ 21` untouched  
`⏩ 30` skipped[^skipped]  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| 👁 | WallTime | [`` large[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fpub-imp?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 199.7 s | 210.2 s | -4.98% |
[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/gankra%2Fpub-imp?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @carljm on 2025-11-21 22:10_

WTF, is it a new rule that everything must regress performance on pydantic?

This doesn't even seem to cause any new diagnostics on pydantic...

---

_Comment by @carljm on 2025-11-22 01:41_

I tried to explore this regression and I can't even reproduce it locally. Locally when I run `main` vs this branch, single-threaded, on pydantic, with a release build, via hyperfine, I consistently see this branch as a couple percent _faster_ than main. So I'm going to acknowledge the regression and go ahead and land this.

---

_Merged by @carljm on 2025-11-22 01:42_

---

_Closed by @carljm on 2025-11-22 01:42_

---

_Branch deleted on 2025-11-22 01:42_

---
