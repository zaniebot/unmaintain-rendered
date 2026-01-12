```yaml
number: 19085
title: "[ty] Calls returning `Never` (alternative)"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: main
head: david/calls-returning-never-alternative
created_at: 2025-07-02T07:50:35Z
updated_at: 2025-07-07T11:47:53Z
url: https://github.com/astral-sh/ruff/pull/19085
synced_at: 2026-01-12T15:56:31Z
```

# [ty] Calls returning `Never` (alternative)

---

_@sharkdp_

## Summary

A functionality-reduced alternative to https://github.com/astral-sh/ruff/pull/18333, with the idea to reduce the amount of reachability constraints for performance reasons.

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-07-02 07:50_

---

_Comment by @github-actions[bot] on 2025-07-02 07:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~49MB

pegen (https://github.com/we-like-parsers/pegen)
- error[invalid-return-type] src/pegen/__main__.py:27:6: Function can implicitly return `None`, which is not assignable to return type `tuple[Grammar, Parser, Tokenizer, ParserGenerator]`
- warning[possibly-unresolved-reference] src/pegen/first_sets.py:144:37: Name `grammar` used when possibly not defined
- warning[possibly-unresolved-reference] src/pegen/grammar_visualizer.py:59:31: Name `grammar` used when possibly not defined
- Found 50 diagnostics
+ Found 47 diagnostics

python-chess (https://github.com/niklasf/python-chess)
-     memo fields = ~72MB
+     memo fields = ~80MB

attrs (https://github.com/python-attrs/attrs)
-     memo fields = ~60MB
+     memo fields = ~66MB

pyinstrument (https://github.com/joerick/pyinstrument)
- error[invalid-return-type] pyinstrument/__main__.py:594:55: Function can implicitly return `None`, which is not assignable to return type `Session`
- Found 50 diagnostics
+ Found 49 diagnostics
-     memo fields = ~54MB
+     memo fields = ~60MB

Expression (https://github.com/cognitedata/Expression)
- error[invalid-return-type] expression/collections/maptree.py:253:53: Function can implicitly return `None`, which is not assignable to return type `tuple[Key, Value, Option[MapTreeLeaf[Key, Value]]]`
- error[invalid-return-type] expression/collections/maptree.py:493:27: Function can implicitly return `None`, which is not assignable to return type `tuple[Key, Value]`
- Found 234 diagnostics
+ Found 232 diagnostics

websockets (https://github.com/aaugustin/websockets)
- warning[possibly-unresolved-reference] src/websockets/cli.py:116:33: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:119:32: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:139:11: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:140:12: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:140:49: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:141:26: Name `websocket` used when possibly not defined
- warning[possibly-unresolved-reference] src/websockets/cli.py:141:48: Name `websocket` used when possibly not defined
- Found 72 diagnostics
+ Found 65 diagnostics

koda-validate (https://github.com/keithasaurus/koda-validate)
- error[invalid-return-type] koda_validate/serialization/json_schema.py:302:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, Unknown]`
- error[invalid-return-type] koda_validate/serialization/json_schema.py:409:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, Unknown]`
- error[invalid-return-type] koda_validate/serialization/json_schema.py:492:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, Unknown]`
- error[invalid-return-type] koda_validate/serialization/json_schema.py:503:6: Function can implicitly return `None`, which is not assignable to return type `dict[str, Unknown]`
- Found 44 diagnostics
+ Found 40 diagnostics

comtypes (https://github.com/enthought/comtypes)
- warning[possibly-unresolved-reference] comtypes/clear_cache.py:47:20: Name `comtypes` used when possibly not defined
- Found 484 diagnostics
+ Found 483 diagnostics
-     memo metadata = ~10MB
+     memo metadata = ~11MB

dulwich (https://github.com/dulwich/dulwich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB
-     memo metadata = ~14MB
+     memo metadata = ~15MB
-     memo fields = ~117MB
+     memo fields = ~129MB

rich (https://github.com/Textualize/rich)
- warning[possibly-unresolved-reference] rich/json.py:139:24: Name `json_data` used when possibly not defined
- Found 325 diagnostics
+ Found 324 diagnostics
-     memo fields = ~106MB
+     memo fields = ~117MB

trio (https://github.com/python-trio/trio)
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:19:18: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:23:20: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/_core/_tests/test_exceptiongroup_gc.py:31:18: Function always implicitly returns `None`, which is not assignable to return type `Never`
- warning[possibly-unresolved-reference] src/trio/_tests/test_dtls.py:30:6: Name `trustme` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:50:18: Name `run` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:160:18: Name `PyLinter` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:174:18: Name `jedi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_exports.py:391:22: Name `jedi` used when possibly not defined
- warning[possibly-unresolved-reference] src/trio/_tests/test_ssl.py:82:16: Name `trustme` used when possibly not defined
- error[unresolved-attribute] src/trio/_tests/test_util.py:351:12: Type `BaseException` has no attribute `code`
- error[invalid-return-type] src/trio/_util.py:381:6: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/trio/testing/_fake_net.py:409:30: Function can implicitly return `None`, which is not assignable to return type `tuple[str, int] | tuple[str, int, int, int]`
- Found 841 diagnostics
+ Found 829 diagnostics
-     struct fields = ~8MB
+     struct fields = ~9MB
-     memo metadata = ~19MB
+     memo metadata = ~21MB

kopf (https://github.com/nolar/kopf)
-     struct fields = ~4MB
+     struct fields = ~5MB

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
-     memo fields = ~34MB
+     memo fields = ~37MB

svcs (https://github.com/hynek/svcs)
-     struct metadata = ~1MB
+     struct metadata = ~2MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB
-     struct metadata = ~3MB
+     struct metadata = ~4MB
-     memo metadata = ~9MB
+     memo metadata = ~11MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
-     memo fields = ~72MB
+     memo fields = ~80MB

strawberry (https://github.com/strawberry-graphql/strawberry)
- warning[possibly-unresolved-reference] strawberry/relay/utils.py:71:32: Name `type_name` used when possibly not defined
- Found 363 diagnostics
+ Found 362 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo metadata = ~8MB
+     memo metadata = ~9MB
-     memo fields = ~97MB
+     memo fields = ~106MB

PyGithub (https://github.com/PyGithub/PyGithub)
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB
-     struct metadata = ~5MB
+     struct metadata = ~6MB
-     memo metadata = ~17MB
+     memo metadata = ~19MB
-     memo fields = ~129MB
+     memo fields = ~142MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- warning[possibly-unresolved-reference] bson/__init__.py:365:51: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:367:40: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:369:16: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:574:71: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:576:40: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] bson/__init__.py:578:30: Name `value` used when possibly not defined
- error[invalid-return-type] pymongo/asynchronous/encryption.py:114:65: Function can implicitly return `None`, which is not assignable to return type `socket | _sslConn`
- error[invalid-return-type] pymongo/asynchronous/pool.py:622:72: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] pymongo/common.py:832:6: Function can implicitly return `None`, which is not assignable to return type `(...) -> Unknown`
+ warning[unused-ignore-comment] pymongo/pool_shared.py:319:72: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:321:13: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:324:5: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:325:12: Name `ssl_sock` used when possibly not defined
+ warning[unused-ignore-comment] pymongo/pool_shared.py:374:86: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:376:13: Name `transport` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:379:38: Name `transport` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:379:49: Name `protocol` used when possibly not defined
+ warning[unused-ignore-comment] pymongo/pool_shared.py:493:72: Unused blanket `type: ignore` directive
+ warning[unused-ignore-comment] pymongo/pool_shared.py:542:72: Unused blanket `type: ignore` directive
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:495:13: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:498:5: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:499:12: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:544:13: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:547:5: Name `ssl_sock` used when possibly not defined
- warning[possibly-unresolved-reference] pymongo/pool_shared.py:548:32: Name `ssl_sock` used when possibly not defined
- error[invalid-return-type] pymongo/synchronous/encryption.py:113:59: Function can implicitly return `None`, which is not assignable to return type `socket | _sslConn`
- error[invalid-return-type] pymongo/synchronous/pool.py:620:66: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 455 diagnostics
+ Found 436 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-return-type] pydantic/json_schema.py:1608:59: Function can implicitly return `None`, which is not assignable to return type `bool`
- Found 705 diagnostics
+ Found 704 diagnostics

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB
-     memo fields = ~72MB
+     memo fields = ~80MB

PyWinCtl (https://github.com/Kalmat/PyWinCtl)
- TOTAL MEMORY USAGE: ~41MB
+ TOTAL MEMORY USAGE: ~45MB

sockeye (https://github.com/awslabs/sockeye)
- warning[possibly-unresolved-reference] sockeye/train.py:1141:37: Name `apex` used when possibly not defined
- Found 322 diagnostics
+ Found 321 diagnostics
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo metadata = ~9MB
+     memo metadata = ~10MB

pylox (https://github.com/sco1/pylox)
-     memo metadata = ~4MB
+     memo metadata = ~5MB

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
-     struct metadata = ~2MB
+     struct metadata = ~3MB
-     struct fields = ~4MB
+     struct fields = ~5MB
-     memo fields = ~88MB
+     memo fields = ~97MB

ignite (https://github.com/pytorch/ignite)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB
-     memo metadata = ~25MB
+     memo metadata = ~28MB

ppb-vector (https://github.com/ppb/ppb-vector)
- TOTAL MEMORY USAGE: ~37MB
+ TOTAL MEMORY USAGE: ~41MB

pybind11 (https://github.com/pybind/pybind11)
-     memo metadata = ~8MB
+     memo metadata = ~9MB

optuna (https://github.com/optuna/optuna)
-     memo metadata = ~28MB
+     memo metadata = ~30MB

asynq (https://github.com/quora/asynq)
-     memo fields = ~54MB
+     memo fields = ~60MB

flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~72MB
+ TOTAL MEMORY USAGE: ~80MB
-     struct fields = ~3MB
+     struct fields = ~4MB

mkosi (https://github.com/systemd/mkosi)
- error[invalid-return-type] mkosi/__init__.py:1569:53: Function can implicitly return `None`, which is not assignable to return type `Path`
- error[invalid-return-type] mkosi/__init__.py:2351:71: Function can implicitly return `None`, which is not assignable to return type `list[Unknown]`
- warning[possibly-unresolved-reference] mkosi/completion.py:249:11: Name `func` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:910:8: Name `timestamp` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:913:12: Name `timestamp` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:925:8: Name `level` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:928:12: Name `level` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:940:8: Name `mode` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:943:8: Name `mode` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:946:12: Name `mode` used when possibly not defined
- error[invalid-return-type] mkosi/config.py:1068:35: Function can implicitly return `None`, which is not assignable to return type `SE`
- warning[possibly-unresolved-reference] mkosi/config.py:1447:8: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1447:22: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1448:54: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1450:26: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1451:44: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1453:12: Name `size` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1471:8: Name `cid` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1472:16: Name `cid` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:1474:12: Name `cid` used when possibly not defined
- error[invalid-argument-type] mkosi/config.py:1564:22: Argument is incorrect: Expected `KeySourceType`, found `KeySourceType | <class 'type'>`
- error[invalid-argument-type] mkosi/config.py:1594:30: Argument is incorrect: Expected `CertificateSourceType`, found `CertificateSourceType | <class 'type'>`
- warning[possibly-unresolved-reference] mkosi/config.py:4787:30: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:4788:36: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/config.py:4796:68: Name `result` used when possibly not defined
- error[invalid-return-type] mkosi/log.py:20:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] mkosi/mounts.py:33:35: Function can implicitly return `None`, which is not assignable to return type `Path`
- error[invalid-return-type] mkosi/qemu.py:141:56: Function can implicitly return `None`, which is not assignable to return type `int`
- error[invalid-return-type] mkosi/qemu.py:189:41: Function can implicitly return `None`, which is not assignable to return type `Path`
- warning[possibly-unresolved-reference] mkosi/run.py:232:19: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:233:13: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:235:13: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:238:13: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:242:13: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/run.py:243:26: Name `proc` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/user.py:82:12: Name `count` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/user.py:85:20: Name `count` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/user.py:85:57: Name `line` used when possibly not defined
- warning[possibly-unresolved-reference] mkosi/user.py:88:16: Name `start` used when possibly not defined
- error[invalid-return-type] mkosi/util.py:267:38: Function can implicitly return `None`, which is not assignable to return type `str`
- Found 130 diagnostics
+ Found 90 diagnostics
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~142MB
-     struct metadata = ~3MB
+     struct metadata = ~4MB
-     memo metadata = ~9MB
+     memo metadata = ~10MB
-     memo fields = ~97MB
+     memo fields = ~129MB

stone (https://github.com/dropbox/stone)
- warning[possibly-unresolved-reference] stone/cli.py:159:31: Name `logging_level` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:243:12: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:250:42: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:254:30: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:262:42: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:267:33: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:273:30: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:288:50: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:292:26: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:299:22: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:301:17: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:302:21: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:343:9: Name `api` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:344:9: Name `backend_module` used when possibly not defined
- warning[possibly-unresolved-reference] stone/cli.py:360:16: Name `api` used when possibly not defined
- Found 128 diagnostics
+ Found 113 diagnostics
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB
-     struct fields = ~4MB
+     struct fields = ~5MB
-     memo metadata = ~8MB
+     memo metadata = ~10MB
-     memo fields = ~72MB
+     memo fields = ~80MB

cki-lib (https://gitlab.com/cki-project/cki-lib)
-     memo fields = ~72MB
+     memo fields = ~80MB

schemathesis (https://github.com/schemathesis/schemathesis)
- error[invalid-return-type] src/schemathesis/core/loaders.py:61:86: Function can implicitly return `None`, which is not assignable to return type `Response`
- warning[possibly-unresolved-reference] src/schemathesis/graphql/loaders.py:198:22: Name `schema` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:107:14: Name `expected_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:107:39: Name `expected_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:108:17: Name `expected_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:108:34: Name `received_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:108:52: Name `expected_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:109:17: Name `expected_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:109:42: Name `expected_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:109:58: Name `received_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:110:17: Name `expected_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:110:34: Name `received_main` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:110:52: Name `expected_sub` used when possibly not defined
- warning[possibly-unresolved-reference] src/schemathesis/specs/openapi/checks.py:110:68: Name `received_sub` used when possibly not defined
- Found 304 diagnostics
+ Found 290 diagnostics
- TOTAL MEMORY USAGE: ~156MB
+ TOTAL MEMORY USAGE: ~171MB
-     memo metadata = ~14MB
+     memo metadata = ~15MB
-     memo fields = ~129MB
+     memo fields = ~142MB

mkdocs (https://github.com/mkdocs/mkdocs)
-     memo metadata = ~9MB
+     memo metadata = ~10MB

poetry (https://github.com/python-poetry/poetry)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB
-     struct metadata = ~7MB
+     struct metadata = ~8MB
-     struct fields = ~9MB
+     struct fields = ~10MB

boostedblob (https://github.com/hauntsaninja/boostedblob)
-     struct metadata = ~2MB
+     struct metadata = ~3MB

apprise (https://github.com/caronc/apprise)
- TOTAL MEMORY USAGE: ~304MB
+ TOTAL MEMORY USAGE: ~334MB
-     struct fields = ~11MB
+     struct fields = ~13MB
-     memo metadata = ~41MB
+     memo metadata = ~45MB
-     memo fields = ~251MB
+     memo fields = ~276MB

tornado (https://github.com/tornadoweb/tornado)
- error[invalid-return-type] tornado/process.py:85:6: Function can implicitly return `None`, which is not assignable to return type `int`
- Found 245 diagnostics
+ Found 244 diagnostics
-     memo metadata = ~15MB
+     memo metadata = ~17MB

pwndbg (https://github.com/pwndbg/pwndbg)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB
-     struct fields = ~8MB
+     struct fields = ~9MB
-     memo metadata = ~21MB
+     memo metadata = ~23MB
-     memo fields = ~171MB
+     memo fields = ~189MB

vision (https://github.com/pytorch/vision)
-     memo metadata = ~37MB
+     memo metadata = ~41MB

colour (https://github.com/colour-science/colour)
- TOTAL MEMORY USAGE: ~405MB
+ TOTAL MEMORY USAGE: ~445MB
-     memo metadata = ~37MB
+     memo metadata = ~41MB
-     memo fields = ~334MB
+     memo fields = ~368MB

dragonchain (https://github.com/dragonchain/dragonchain)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo metadata = ~8MB
+     memo metadata = ~9MB

jinja (https://github.com/pallets/jinja)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     memo fields = ~80MB
+     memo fields = ~88MB

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
-     memo metadata = ~4MB
+     memo metadata = ~5MB
-     memo fields = ~97MB
+     memo fields = ~106MB

schema_salad (https://github.com/common-workflow-language/schema_salad)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB
-     struct fields = ~7MB
+     struct fields = ~8MB
-     memo fields = ~117MB
+     memo fields = ~129MB

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- error[type-assertion-failure] tests/test_frame.py:4198:9: Argument does not have asserted type `Never`
- error[no-matching-overload] tests/test_frame.py:4198:22: No overload of bound method `select_dtypes` matches arguments
- Found 2779 diagnostics
+ Found 2777 diagnostics
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB
-     memo metadata = ~21MB
+     memo metadata = ~25MB
-     memo fields = ~228MB
+     memo fields = ~251MB

altair (https://github.com/vega/altair)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~304MB

pytest (https://github.com/pytest-dev/pytest)
- warning[possibly-unresolved-reference] src/_pytest/fixtures.py:224:49: Name `scoped_item_path` used when possibly not defined
- warning[possibly-unresolved-reference] src/_pytest/reports.py:589:16: Name `reprentry` used when possibly not defined
- Found 537 diagnostics
+ Found 535 diagnostics
-     struct metadata = ~9MB
+     struct metadata = ~10MB
-     memo metadata = ~30MB
+     memo metadata = ~34MB
-     memo fields = ~189MB
+     memo fields = ~207MB

paasta (https://github.com/yelp/paasta)
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:66:8: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:72:12: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:83:44: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:136:9: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cleanup_expired_autoscaling_overrides.py:142:18: Name `configmap` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cli/cmds/local_run.py:919:28: Name `secret_environment` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cli/cmds/local_run.py:942:48: Name `chosen_port` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cli/cmds/local_run.py:951:45: Name `chosen_port` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/cli/cmds/local_run.py:992:21: Name `chosen_port` used when possibly not defined
- error[invalid-return-type] paasta_tools/cli/cmds/logs.py:485:45: Function can implicitly return `None`, which is not assignable to return type `LogReader`
- error[invalid-return-type] paasta_tools/cli/cmds/logs.py:1150:54: Function can implicitly return `None`, which is not assignable to return type `str`
- warning[possibly-unresolved-reference] paasta_tools/setup_tron_namespace.py:140:12: Name `services` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/setup_tron_namespace.py:174:27: Name `services` used when possibly not defined
- error[invalid-return-type] paasta_tools/utils.py:4056:66: Function can implicitly return `None`, which is not assignable to return type `str`
- warning[possibly-unresolved-reference] paasta_tools/utils.py:4089:19: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/utils.py:4089:38: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] paasta_tools/utils.py:4092:16: Name `result` used when possibly not defined
- Found 905 diagnostics
+ Found 888 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:42:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:45:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:48:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:51:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:54:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:57:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:60:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:63:51: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:66:41: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:69:33: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:72:66: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:117:64: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:120:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:123:40: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:126:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:129:26: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:132:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:135:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:138:24: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:156:48: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:159:30: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:162:38: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:165:55: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:168:73: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:185:59: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:188:56: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:191:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:194:53: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:197:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:200:75: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:203:37: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:206:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:209:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:212:40: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:215:51: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:218:68: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:221:26: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:224:57: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[invalid-return-type] src/werkzeug/datastructures/mixins.py:227:61: Function always implicitly returns `None`, which is not assignable to return type `Never`
- error[unresolved-attribute] tests/test_exceptions.py:19:12: Type `BaseException` has no attribute `get_response`
- Found 426 diagnostics
+ Found 386 diagnostics
-     memo metadata = ~14MB
+     memo metadata = ~15MB

AutoSplit (https://github.com/Toufool/AutoSplit)
- error[invalid-return-type] src/AutoSplit.py:989:31: Function always implicitly returns `None`, which is not assignable to return type `Never`
- warning[possibly-unresolved-reference] src/AutoSplit.py:1106:14: Name `exit_code` used when possibly not defined
- error[invalid-return-type] src/error_messages.py:248:58: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 42 diagnostics
+ Found 39 diagnostics

psycopg (https://github.com/psycopg/psycopg)
-     memo metadata = ~23MB
+     memo metadata = ~25MB
-     memo fields = ~171MB
+     memo fields = ~189MB

pywin32 (https://github.com/mhammond/pywin32)
- warning[possibly-unresolved-reference] .github/workflows/download-arm64-libs.py:19:8: Name `dest` used when possibly not defined
- error[invalid-return-type] com/win32comext/axscript/client/framework.py:1198:71: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 1976 diagnostics
+ Found 1974 diagnostics
- TOTAL MEMORY USAGE: ~445MB
+ TOTAL MEMORY USAGE: ~490MB
-     struct metadata = ~19MB
+     struct metadata = ~21MB
-     struct fields = ~28MB
+     struct fields = ~30MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

scrapy (https://github.com/scrapy/scrapy)
- warning[possibly-unresolved-reference] docs/utils/linkfix.py:38:17: Name `output_lines` used when possibly not defined
- Found 1158 diagnostics
+ Found 1157 diagnostics
-     memo fields = ~189MB
+     memo fields = ~207MB

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- warning[possibly-unresolved-reference] mitmproxy/proxy/layers/http/_http2.py:187:68: Name `error_code` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/proxy/layers/http/_http3.py:128:68: Name `error_code` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:85:23: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:91:43: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:93:16: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:96:16: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:100:20: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:102:73: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] mitmproxy/tools/main.py:104:37: Name `args` used when possibly not defined
- warning[possibly-unresolved-reference] release/deploy-microsoft-store.py:142:18: Name `msi_file` used when possibly not defined
- warning[possibly-unresolved-reference] release/deploy-microsoft-store.py:145:17: Name `msi_file` used when possibly not defined
- Found 1818 diagnostics
+ Found 1807 diagnostics
-     struct fields = ~14MB
+     struct fields = ~15MB
-     memo metadata = ~34MB
+     memo metadata = ~37MB
-     memo fields = ~228MB
+     memo fields = ~251MB

cwltool (https://github.com/common-workflow-language/cwltool)
- TOTAL MEMORY USAGE: ~251MB
+ TOTAL MEMORY USAGE: ~304MB
-     struct fields = ~11MB
+     struct fields = ~13MB
-     memo metadata = ~17MB
+     memo metadata = ~19MB
-     memo fields = ~228MB
+     memo fields = ~251MB

rclip (https://github.com/yurijmikhalevich/rclip)
- warning[possibly-unresolved-reference] rclip/model.py:193:66: Name `images` used when possibly not defined
- Found 13 diagnostics
+ Found 12 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:118:8: Name `ret` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:120:10: Name `ret` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:120:41: Name `ret` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:120:55: Name `ret` used when possibly not defined
- warning[possibly-unresolved-reference] src/bokeh/command/bootstrap.py:121:18: Name `ret` used when possibly not defined
- error[invalid-return-type] src/bokeh/command/util.py:58:43: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 863 diagnostics
+ Found 857 diagnostics

django-stubs (https://github.com/typeddjango/django-stubs)
-     memo fields = ~142MB
+     memo fields = ~156MB

static-frame (https://github.com/static-frame/static-frame)
-     struct metadata = ~14MB
+     struct metadata = ~15MB
-     memo metadata = ~49MB
+     memo metadata = ~54MB
-     memo fields = ~304MB
+     memo fields = ~334MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     struct metadata = ~10MB
+     struct metadata = ~11MB
-     memo metadata = ~25MB
+     memo metadata = ~28MB
-     memo fields = ~276MB
+     memo fields = ~304MB

cloud-init (https://github.com/canonical/cloud-init)
- warning[possibly-unresolved-reference] cloudinit/analyze/__init__.py:302:12: Name `infh` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/analyze/__init__.py:302:18: Name `outfh` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/devel/hotplug_hook.py:325:36: Name `datasource` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1386:8: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1399:18: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1399:40: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1404:8: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1412:10: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1417:10: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1424:17: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1425:45: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1434:22: Name `functor` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1434:30: Name `name` used when possibly not defined
- warning[possibly-unresolved-reference] cloudinit/cmd/main.py:1439:21: Name `name` used when possibly not defined
- Found 713 diagnostics
+ Found 699 diagnostics
- TOTAL MEMORY USAGE: ~368MB
+ TOTAL MEMORY USAGE: ~405MB
-     struct fields = ~19MB
+     struct fields = ~21MB
-     memo metadata = ~45MB
+     memo metadata = ~49MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo metadata = ~34MB
+     memo metadata = ~37MB
-     memo fields = ~228MB
+     memo fields = ~251MB

meson (https://github.com/mesonbuild/meson)
- error[invalid-return-type] mesonbuild/linkers/detect.py:40:69: Function can implicitly return `None`, which is not assignable to return type `DynamicLinker`
- warning[possibly-unresolved-reference] mesonbuild/linkers/detect.py:257:12: Name `linker` used when possibly not defined
- error[invalid-return-type] mesonbuild/mdevenv.py:160:41: Function can implicitly return `None`, which is not assignable to return type `int`
- warning[possibly-unresolved-reference] mesonbuild/minstall.py:733:16: Name `rc` used when possibly not defined
- warning[possibly-unresolved-reference] mesonbuild/minstall.py:734:82: Name `rc` used when possibly not defined
- warning[possibly-unresolved-reference] mesonbuild/minstall.py:735:26: Name `rc` used when possibly not defined
- warning[possibly-unresolved-reference] mesonbuild/scripts/depfixer.py:181:16: Name `ptrsize` used when possibly not defined
- warning[possibly-unresolved-reference] mesonbuild/scripts/depfixer.py:181:25: Name `is_le` used when possibly not defined
- warning[possibly-unresolved-reference] test cases/common/208 link custom/custom_stlib.py:41:17: Name `static_linker` used when possibly not defined
- warning[possibly-unresolved-reference] test cases/common/209 link custom_i single from multiple/generate_conflicting_stlibs.py:36:17: Name `static_linker` used when possibly not defined
- warning[possibly-unresolved-reference] test cases/common/210 link custom_i multiple from multiple/generate_stlibs.py:38:17: Name `static_linker` used when possibly not defined
- warning[possibly-unresolved-reference] test cases/common/51 run target/fakeburner.py:15:11: Name `content` used when possibly not defined
- warning[possibly-unresolved-reference] test cases/windows/16 gui app/gui_app_tester.py:16:10: Name `pefile` used when possibly not defined
- Found 930 diagnostics
+ Found 917 diagnostics
- TOTAL MEMORY USAGE: ~405MB
+ TOTAL MEMORY USAGE: ~445MB

prefect (https://github.com/PrefectHQ/prefect)
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:394:41: Name `block_document` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:443:46: Name `block_type` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:447:17: Name `block_type` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:452:12: Name `latest_schema` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:455:59: Name `latest_schema` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/block.py:457:43: Name `latest_schema` used when possibly not defined
- error[invalid-return-type] src/prefect/cli/cloud/__init__.py:269:35: Function can implicitly return `None`, which is not assignable to return type `str`
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:534:26: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:539:16: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:542:53: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:550:39: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:554:46: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:557:54: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:562:8: Name `prompt_switch_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:566:17: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:572:12: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:573:34: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:574:18: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:575:34: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:639:14: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:643:33: Name `current_workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:660:47: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:667:70: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:699:30: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:705:20: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:709:35: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:722:78: Name `workspaces` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:727:60: Name `workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/cloud/__init__.py:729:46: Name `workspace` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:85:23: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:92:66: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:98:28: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:102:17: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:103:17: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:104:40: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:104:59: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:105:40: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/concurrency_limit.py:105:59: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:52:12: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:53:53: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:56:12: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:58:28: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:62:25: Name `setting` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:62:36: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/config.py:81:42: Name `new_profile` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deploy.py:226:23: Name `files` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:111:12: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:281:27: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:283:12: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:284:40: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:295:39: Name `deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:830:21: Name `start_time_raw` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:842:53: Name `start_time_raw` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:844:12: Name `start_time_parsed` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:845:69: Name `start_time_raw` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:847:57: Name `start_time_parsed` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:882:48: Name `scheduled_start_time` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:895:28: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:896:37: Name `scheduled_start_time` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:898:26: Name `human_dt_diff` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:900:43: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:904:20: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:905:26: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:906:29: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:914:48: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:916:13: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:1048:20: Name `key` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:1048:38: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:1050:46: Name `value` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/deployment.py:1052:60: Name `key` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow.py:170:49: Name `runner_deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow.py:173:29: Name `runner_deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow.py:176:14: Name `runner_deployment` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:92:29: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:98:38: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:254:8: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:257:18: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/flow_run.py:355:45: Name `flow_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/global_concurrency_limit.py:124:26: Name `gcl_limit` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/global_concurrency_limit.py:135:34: Name `gcl_limit` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/global_concurrency_limit.py:384:85: Name `gcl` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/global_concurrency_limit.py:392:72: Name `gcl_id` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task.py:98:33: Name `module_tasks` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task.py:99:39: Name `module_tasks` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task.py:100:26: Name `module_tasks` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task_run.py:86:29: Name `task_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task_run.py:92:38: Name `task_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/task_run.py:247:29: Name `task_run` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:496:17: Name `work_pool` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:500:56: Name `work_pool` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:515:25: Name `work_pool` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:676:47: Name `responses` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:868:31: Name `block_type` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:869:33: Name `block_schema` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:928:47: Name `credentials_block_document` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:1013:47: Name `credentials_block_document` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_pool.py:1098:47: Name `credentials_block_document` used when possibly not defined
- error[invalid-return-type] src/prefect/cli/work_queue.py:35:6: Function can implicitly return `None`, which is not assignable to return type `UUID`
- warning[possibly-unresolved-reference] src/prefect/cli/work_queue.py:115:19: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_queue.py:118:39: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_queue.py:121:42: Name `result` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_queue.py:448:33: Name `queues` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_queue.py:529:23: Name `runs` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_queue.py:541:8: Name `runs` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_queue.py:623:21: Name `runs` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/cli/work_queue.py:623:79: Name `runs` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/events/cli/automations.py:239:44: Name `automation` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/events/cli/automations.py:240:65: Name `automation` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/events/cli/automations.py:294:43: Name `automation` used when possibly not defined
- warning[possibly-unresolved-reference] src/prefect/events/cli/automations.py:295:64: Name `automation` used when possibly not defined
- Found 4128 diagnostics
+ Found 4018 diagnostics
-     memo metadata = ~66MB
+     memo metadata = ~72MB

materialize (https://github.com/MaterializeInc/materialize)
-     struct metadata = ~17MB
+     struct metadata = ~19MB
-     memo metadata = ~37MB
+     memo metadata = ~41MB
-     memo fields = ~445MB
+     memo fields = ~490MB

rotki (https://github.com/rotki/rotki)
- warning[possibly-unresolved-reference] rotkehlchen/__main__.py:37:5: Name `rotkehlchen_server` used when possibly not defined
- Found 1766 diagnostics
+ Found 1765 diagnostics
-     struct fields = ~25MB
+     struct fields = ~28MB
-     memo fields = ~445MB
+     memo fields = ~490MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~652MB
+ TOTAL MEMORY USAGE: ~717MB
-     struct metadata = ~23MB
+     struct metadata = ~25MB
-     struct fields = ~30MB
+     struct fields = ~34MB
-     memo metadata = ~80MB
+     memo metadata = ~88MB

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-return-type] ddtrace/internal/datadog/profiling/ddup/test/interface.py:53:33: Function can implicitly return `None`, which is not assignable to return type `bool`
- warning[possibly-unresolved-reference] lib-injection/dl_wheels.py:111:13: Name `ddtrace_specifier` used when possibly not defined
- warning[possibly-unresolved-reference] tests/tracer/test_memory_leak.py:181:5: Name `span` used when possibly not defined
- warning[possibly-unresolved-reference] tests/tracer/test_memory_leak.py:182:9: Name `span` used when possibly not defined
- Found 6823 diagnostics
+ Found 6819 diagnostics
-     struct fields = ~37MB
+     struct fields = ~41MB
-     memo metadata = ~106MB
+     memo metadata = ~117MB
-     memo fields = ~593MB
+     memo fields = ~652MB

zulip (https://github.com/zulip/zulip)
- error[invalid-return-type] tools/droplets/create.py:45:56: Function can implicitly return `None`, which is not assignable to return type `bool`
- error[invalid-return-type] tools/droplets/create.py:59:62: Function can implicitly return `None`, which is not assignable to return type `list[dict[str, Any]]`
- error[invalid-return-type] tools/droplets/create.py:78:60: Function can implicitly return `None`, which is not assignable to return type `bool`
- warning[possibly-unresolved-reference] tools/lib/provision.py:159:23: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:161:34: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:177:23: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:178:23: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:184:22: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:185:22: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:186:22: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:187:22: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:193:22: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:194:22: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:195:22: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:204:44: Name `POSTGRESQL_VERSION` used when possibly not defined
- warning[possibly-unresolved-reference] tools/lib/provision.py:206:55: Name `POSTGRESQL_VERSION` used when possibly not defined
- error[invalid-return-type] tools/lib/provision.py:343:42: Function always implicitly returns `None`, which is not assignable to return type `Never`
- Found 7248 diagnostics
+ Found 7231 diagnostics
- TOTAL MEMORY USAGE: ~789MB
+ TOTAL MEMORY USAGE: ~868MB
-     struct metadata = ~28MB
+     struct metadata = ~30MB
-     memo metadata = ~97MB
+     memo metadata = ~117MB
-     memo fields = ~652MB
+     memo fields = ~717MB

manticore (https://github.com/trailofbits/manticore)
- TOTAL MEMORY USAGE: ~652MB
+ TOTAL MEMORY USAGE: ~789MB
-     struct metadata = ~21MB
+     struct metadata = ~25MB
-     struct fields = ~30MB
+     struct fields = ~34MB
-     memo metadata = ~80MB
+     memo metadata = ~117MB
-     memo fields = ~539MB
+     memo fields = ~593MB

scipy (https://github.com/scipy/scipy)
-     struct metadata = ~41MB
+     struct metadata = ~45MB
-     struct fields = ~54MB
+     struct fields = ~60MB
-     memo metadata = ~156MB
+     memo metadata = ~171MB
-     memo fields = ~955MB
+     memo fields = ~1051MB

sympy (https://github.com/sympy/sympy)
-     memo metadata = ~276MB
+     memo metadata = ~304MB

```
</details>


---

_Comment by @codspeed-hq[bot] on 2025-07-02 08:00_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fcalls-returning-never-alternative?runnerMode=Instrumentation)

### Merging #19085 will **degrade performances by 8.15%**

<sub>Comparing <code>david/calls-returning-never-alternative</code> (2eae76b) with <code>main</code> (cdf91b8)</sub>



### Summary

`❌ 1` regressions  
`✅ 38` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/david%2Fcalls-returning-never-alternative?runnerMode=Instrumentation)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` hydra-zen `` | 780.3 ms | 849.5 ms | -8.15% |


---

_Comment by @codspeed-hq[bot] on 2025-07-02 08:02_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/david%2Fcalls-returning-never-alternative?runnerMode=WallTime)

### Merging #19085 will **improve performances by 4.46%**

<sub>Comparing <code>david/calls-returning-never-alternative</code> (2eae76b) with <code>main</code> (cdf91b8)</sub>



### Summary

`⚡ 1` improvements  
`✅ 7` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` multithreaded[pydantic] `` | 3 s | 2.8 s | +4.46% |


---

_Closed by @sharkdp on 2025-07-07 11:47_

---
