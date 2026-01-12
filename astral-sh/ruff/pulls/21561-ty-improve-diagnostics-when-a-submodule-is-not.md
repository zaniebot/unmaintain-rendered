```yaml
number: 21561
title: "[ty] Improve diagnostics when a submodule is not available as an attribute on a module-literal type"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - diagnostics
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/submodule-attribute-hint
created_at: 2025-11-21T15:03:21Z
updated_at: 2025-11-22T14:07:49Z
url: https://github.com/astral-sh/ruff/pull/21561
synced_at: 2026-01-12T15:57:28Z
```

# [ty] Improve diagnostics when a submodule is not available as an attribute on a module-literal type

---

_@AlexWaygood_

## Summary

It's often not obvious to users why we sometimes report `unresolved-attribute` errors for submodules-accessed-on-parent-modules, even though these attributes often are available at runtime (sometimes by sheer luck!). This PR adds a dedicated diagnostic for this case that helps guide the user towards how they can fix the error.

## Test Plan

Added snapshots


---

_Review requested from @carljm by @AlexWaygood on 2025-11-21 15:03_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-21 15:03_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-21 15:03_

---

_Label `ty` added by @AlexWaygood on 2025-11-21 15:03_

---

_Label `diagnostics` added by @AlexWaygood on 2025-11-21 15:03_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 15:05_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-21 15:13_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:287:34: error[unresolved-attribute] Module `asyncio` has no member `futures`
+ src/anyio/_backends/_asyncio.py:287:34: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `asyncio`
- src/anyio/_backends/_asyncio.py:1095:15: error[unresolved-attribute] Module `asyncio` has no member `subprocess`
+ src/anyio/_backends/_asyncio.py:1095:15: error[unresolved-attribute] Submodule `subprocess` may not be available as an attribute on module `asyncio`

kornia (https://github.com/kornia/kornia)
- kornia/utils/download.py:99:20: error[unresolved-attribute] Module `urllib` has no member `error`
+ kornia/utils/download.py:99:20: error[unresolved-attribute] Submodule `error` may not be available as an attribute on module `urllib`

spack (https://github.com/spack/spack)
- lib/spack/spack/ci/gitlab.py:148:22: error[unresolved-attribute] Module `urllib` has no member `parse`
+ lib/spack/spack/ci/gitlab.py:148:22: error[unresolved-attribute] Submodule `parse` may not be available as an attribute on module `urllib`
- lib/spack/spack/cray_manifest.py:223:29: error[unresolved-attribute] Module `json` has no member `decoder`
+ lib/spack/spack/cray_manifest.py:223:29: error[unresolved-attribute] Submodule `decoder` may not be available as an attribute on module `json`

bandersnatch (https://github.com/pypa/bandersnatch)
- src/bandersnatch/tests/test_main.py:87:31: error[unresolved-attribute] Module `bandersnatch` has no member `master`
+ src/bandersnatch/tests/test_main.py:87:31: error[unresolved-attribute] Submodule `master` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/tests/test_verify.py:169:25: error[unresolved-attribute] Module `bandersnatch` has no member `verify`
+ src/bandersnatch/tests/test_verify.py:169:25: error[unresolved-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/tests/test_verify.py:237:25: error[unresolved-attribute] Module `bandersnatch` has no member `verify`
+ src/bandersnatch/tests/test_verify.py:237:25: error[unresolved-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/tests/test_verify.py:238:25: error[unresolved-attribute] Module `bandersnatch` has no member `verify`
+ src/bandersnatch/tests/test_verify.py:238:25: error[unresolved-attribute] Submodule `verify` may not be available as an attribute on module `bandersnatch`
- src/bandersnatch/verify.py:154:12: error[unresolved-attribute] Module `json` has no member `decoder`
+ src/bandersnatch/verify.py:154:12: error[unresolved-attribute] Submodule `decoder` may not be available as an attribute on module `json`

pip (https://github.com/pypa/pip)
- src/pip/_internal/models/link.py:134:11: error[unresolved-attribute] Module `urllib` has no member `request`
- src/pip/_internal/models/link.py:134:39: error[unresolved-attribute] Module `urllib` has no member `request`
+ src/pip/_internal/models/link.py:134:11: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
+ src/pip/_internal/models/link.py:134:39: error[unresolved-attribute] Submodule `request` may not be available as an attribute on module `urllib`
- src/pip/_internal/operations/install/wheel.py:613:16: error[unresolved-attribute] Module `importlib` has no member `util`
+ src/pip/_internal/operations/install/wheel.py:613:16: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
- src/pip/_internal/utils/logging.py:294:5: error[unresolved-attribute] Module `logging` has no member `config`
+ src/pip/_internal/utils/logging.py:294:5: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `logging`
- src/pip/_vendor/urllib3/util/retry.py:378:32: error[unresolved-attribute] Module `email` has no member `utils`
- src/pip/_vendor/urllib3/util/retry.py:388:26: error[unresolved-attribute] Module `email` has no member `utils`
+ src/pip/_vendor/urllib3/util/retry.py:378:32: error[unresolved-attribute] Submodule `utils` may not be available as an attribute on module `email`
+ src/pip/_vendor/urllib3/util/retry.py:388:26: error[unresolved-attribute] Submodule `utils` may not be available as an attribute on module `email`

isort (https://github.com/pycqa/isort)
- isort/place.py:104:31: error[unresolved-attribute] Module `importlib` has no member `machinery`
+ isort/place.py:104:31: error[unresolved-attribute] Submodule `machinery` may not be available as an attribute on module `importlib`

asynq (https://github.com/quora/asynq)
- asynq/tests/test_mock.py:242:28: error[unresolved-attribute] Module `asynq` has no member `tests`
+ asynq/tests/test_mock.py:242:28: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `asynq`
- asynq/tests/test_mock.py:244:23: error[unresolved-attribute] Module `asynq` has no member `tests`
+ asynq/tests/test_mock.py:244:23: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `asynq`
- asynq/tests/test_mock.py:246:28: error[unresolved-attribute] Module `asynq` has no member `tests`
+ asynq/tests/test_mock.py:246:28: error[unresolved-attribute] Submodule `tests` may not be available as an attribute on module `asynq`

websockets (https://github.com/aaugustin/websockets)
- src/websockets/legacy/server.py:679:28: error[unresolved-attribute] Module `asyncio` has no member `base_events`
+ src/websockets/legacy/server.py:679:28: error[unresolved-attribute] Submodule `base_events` may not be available as an attribute on module `asyncio`

twine (https://github.com/pypa/twine)
- twine/auth.py:74:25: error[unresolved-attribute] Module `requests` has no member `models`
- twine/auth.py:75:11: error[unresolved-attribute] Module `requests` has no member `models`
- twine/auth.py:88:21: error[unresolved-attribute] Module `requests` has no member `models`
+ twine/auth.py:74:25: error[unresolved-attribute] Submodule `models` may not be available as an attribute on module `requests`
+ twine/auth.py:75:11: error[unresolved-attribute] Submodule `models` may not be available as an attribute on module `requests`
+ twine/auth.py:88:21: error[unresolved-attribute] Submodule `models` may not be available as an attribute on module `requests`

starlette (https://github.com/encode/starlette)
- starlette/endpoints.py:108:20: error[unresolved-attribute] Module `json` has no member `decoder`
+ starlette/endpoints.py:108:20: error[unresolved-attribute] Submodule `decoder` may not be available as an attribute on module `json`

graphql-core (https://github.com/graphql-python/graphql-core)
- tests/benchmarks/test_async_iterable.py:26:12: error[unresolved-attribute] Module `asyncio` has no member `events`
+ tests/benchmarks/test_async_iterable.py:26:12: error[unresolved-attribute] Submodule `events` may not be available as an attribute on module `asyncio`
- tests/benchmarks/test_async_iterable.py:27:5: error[unresolved-attribute] Module `asyncio` has no member `events`
+ tests/benchmarks/test_async_iterable.py:27:5: error[unresolved-attribute] Submodule `events` may not be available as an attribute on module `asyncio`
- tests/benchmarks/test_async_iterable.py:29:5: error[unresolved-attribute] Module `asyncio` has no member `events`
+ tests/benchmarks/test_async_iterable.py:29:5: error[unresolved-attribute] Submodule `events` may not be available as an attribute on module `asyncio`
- tests/benchmarks/test_execution_async.py:43:12: error[unresolved-attribute] Module `asyncio` has no member `events`
+ tests/benchmarks/test_execution_async.py:43:12: error[unresolved-attribute] Submodule `events` may not be available as an attribute on module `asyncio`
- tests/benchmarks/test_execution_async.py:44:5: error[unresolved-attribute] Module `asyncio` has no member `events`
+ tests/benchmarks/test_execution_async.py:44:5: error[unresolved-attribute] Submodule `events` may not be available as an attribute on module `asyncio`
- tests/benchmarks/test_execution_async.py:48:5: error[unresolved-attribute] Module `asyncio` has no member `events`
+ tests/benchmarks/test_execution_async.py:48:5: error[unresolved-attribute] Submodule `events` may not be available as an attribute on module `asyncio`

paasta (https://github.com/yelp/paasta)
- paasta_tools/check_oom_events.py:120:20: error[unresolved-attribute] Module `json` has no member `decoder`
+ paasta_tools/check_oom_events.py:120:20: error[unresolved-attribute] Submodule `decoder` may not be available as an attribute on module `json`
- paasta_tools/check_spark_jobs.py:61:13: error[unresolved-attribute] Module `requests` has no member `exceptions`
+ paasta_tools/check_spark_jobs.py:61:13: error[unresolved-attribute] Submodule `exceptions` may not be available as an attribute on module `requests`
- paasta_tools/check_spark_jobs.py:61:42: error[unresolved-attribute] Module `requests` has no member `exceptions`
+ paasta_tools/check_spark_jobs.py:61:42: error[unresolved-attribute] Submodule `exceptions` may not be available as an attribute on module `requests`
- paasta_tools/cli/cmds/mark_for_deployment.py:1413:15: error[unresolved-attribute] Module `concurrent` has no member `futures`
- paasta_tools/cli/cmds/mark_for_deployment.py:1462:15: error[unresolved-attribute] Module `concurrent` has no member `futures`
+ paasta_tools/cli/cmds/mark_for_deployment.py:1413:15: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
+ paasta_tools/cli/cmds/mark_for_deployment.py:1462:15: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
- paasta_tools/cli/cmds/mark_for_deployment.py:1856:14: error[unresolved-attribute] Module `concurrent` has no member `futures`
+ paasta_tools/cli/cmds/mark_for_deployment.py:1856:14: error[unresolved-attribute] Submodule `futures` may not be available as an attribute on module `concurrent`
- paasta_tools/cli/cmds/remote_run.py:81:12: error[unresolved-attribute] Module `json` has no member `decoder`
+ paasta_tools/cli/cmds/remote_run.py:81:12: error[unresolved-attribute] Submodule `decoder` may not be available as an attribute on module `json`
- paasta_tools/envoy_tools.py:118:42: error[unresolved-attribute] Module `requests` has no member `adapters`
+ paasta_tools/envoy_tools.py:118:42: error[unresolved-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
- paasta_tools/envoy_tools.py:119:43: error[unresolved-attribute] Module `requests` has no member `adapters`
+ paasta_tools/envoy_tools.py:119:43: error[unresolved-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
- paasta_tools/kubernetes_tools.py:2786:12: error[unresolved-attribute] Module `requests` has no member `exceptions`
+ paasta_tools/kubernetes_tools.py:2786:12: error[unresolved-attribute] Submodule `exceptions` may not be available as an attribute on module `requests`
- paasta_tools/mesos/parallel.py:34:9: error[unresolved-attribute] Module `concurrent.futures` has no member `thread`
+ paasta_tools/mesos/parallel.py:34:9: error[unresolved-attribute] Submodule `thread` may not be available as an attribute on module `concurrent.futures`
- paasta_tools/secret_tools.py:75:12: error[unresolved-attribute] Module `json` has no member `decoder`
+ paasta_tools/secret_tools.py:75:12: error[unresolved-attribute] Submodule `decoder` may not be available as an attribute on module `json`
- paasta_tools/smartstack_tools.py:84:38: error[unresolved-attribute] Module `requests` has no member `adapters`
+ paasta_tools/smartstack_tools.py:84:38: error[unresolved-attribute] Submodule `adapters` may not be available as an attribute on module `requests`
- paasta_tools/smartstack_tools.py:85:39: error[unresolved-attribute] Module `requests` has no member `adapters`
+ paasta_tools/smartstack_tools.py:85:39: error[unresolved-attribute] Submodule `adapters` may not be available as an attribute on module `requests`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:145:31: error[unresolved-attribute] Module `_pytest` has no member `main`
+ src/_pytest/fixtures.py:145:31: error[unresolved-attribute] Submodule `main` may not be available as an attribute on module `_pytest`
- src/_pytest/python.py:1692:20: error[unresolved-attribute] Module `_pytest` has no member `_code`
+ src/_pytest/python.py:1692:20: error[unresolved-attribute] Submodule `_code` may not be available as an attribute on module `_pytest`
- testing/test_assertrewrite.py:1246:18: error[unresolved-attribute] Module `importlib` has no member `util`
- testing/test_assertrewrite.py:1396:17: error[unresolved-attribute] Module `importlib` has no member `util`
- testing/test_assertrewrite.py:1904:14: error[unresolved-attribute] Module `importlib` has no member `util`
- testing/test_assertrewrite.py:2084:13: error[unresolved-attribute] Module `_pytest` has no member `assertion`
+ testing/test_assertrewrite.py:1246:18: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
- testing/test_assertrewrite.py:2111:13: error[unresolved-attribute] Module `_pytest` has no member `assertion`
+ testing/test_assertrewrite.py:1396:17: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ testing/test_assertrewrite.py:1904:14: error[unresolved-attribute] Submodule `util` may not be available as an attribute on module `importlib`
+ testing/test_assertrewrite.py:2084:13: error[unresolved-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
+ testing/test_assertrewrite.py:2111:13: error[unresolved-attribute] Submodule `assertion` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2313:30: error[unresolved-attribute] Module `_pytest` has no member `config`
+ testing/test_config.py:2313:30: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2487:18: error[unresolved-attribute] Module `_pytest` has no member `config`
+ testing/test_config.py:2487:18: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`
- testing/test_config.py:2488:21: error[unresolved-attribute] Module `_pytest` has no member `config`
+ testing/test_config.py:2488:21: error[unresolved-attribute] Submodule `config` may not be available as an attribute on module `_pytest`

scrapy (https://github.com/scrapy/scrapy)
- scrapy/core/engine.py:295:17: error[unresolved-attribute] Module `asyncio` has no member `exceptions`
+ scrapy/core/engine.py:295:17: error[unresolved-attribute] Submodule `exceptions` may not be available as an attribute on module `asyncio`
- tests/test_commands.py:46:59: error[unresolved-attribute] Module `scrapy` has no member `settings`
+ tests/test_commands.py:46:59: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `scrapy`
- tests/test_feedexport.py:1348:29: error[unresolved-attribute] Module `scrapy` has no member `extensions`
- tests/test_feedexport.py:1352:29: error[unresolved-attribute] Module `scrapy` has no member `extensions`
+ tests/test_feedexport.py:1348:29: error[unresolved-attribute] Submodule `extensions` may not be available as an attribute on module `scrapy`
+ tests/test_feedexport.py:1352:29: error[unresolved-attribute] Submodule `extensions` may not be available as an attribute on module `scrapy`

alerta (https://github.com/alerta/alerta)
- alerta/utils/client.py:126:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- alerta/utils/client.py:135:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- alerta/utils/client.py:144:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
- alerta/utils/client.py:152:16: error[unresolved-attribute] Module `requests` has no member `exceptions`
+ alerta/utils/client.py:126:16: error[unresolved-attribute] Submodule `exceptions` may not be available as an attribute on module `requests`
+ alerta/utils/client.py:135:16: error[unresolved-attribute] Submodule `exceptions` may not be available as an attribute on module `requests`
+ alerta/utils/client.py:144:16: error[unresolved-attribute] Submodule `exceptions` may not be available as an attribute on module `requests`
+ alerta/utils/client.py:152:16: error[unresolved-attribute] Submodule `exceptions` may not be available as an attribute on module `requests`

sockeye (https://github.com/awslabs/sockeye)
- sockeye/utils.py:627:16: error[unresolved-attribute] Module `multiprocessing` has no member `pool`
+ sockeye/utils.py:627:16: error[unresolved-attribute] Submodule `pool` may not be available as an attribute on module `multiprocessing`
- test/unit/test_knn.py:67:20: error[unresolved-attribute] Module `sockeye` has no member `encoder`
- test/unit/test_knn.py:68:22: error[unresolved-attribute] Module `sockeye` has no member `encoder`
- test/unit/test_knn.py:75:19: error[unresolved-attribute] Module `sockeye` has no member `data_io`
+ test/unit/test_knn.py:67:20: error[unresolved-attribute] Submodule `encoder` may not be available as an attribute on module `sockeye`
+ test/unit/test_knn.py:68:22: error[unresolved-attribute] Submodule `encoder` may not be available as an attribute on module `sockeye`
+ test/unit/test_knn.py:75:19: error[unresolved-attribute] Submodule `data_io` may not be available as an attribute on module `sockeye`
- test/unit/test_knn.py:79:14: error[unresolved-attribute] Module `sockeye` has no member `model`
+ test/unit/test_knn.py:79:14: error[unresolved-attribute] Submodule `model` may not be available as an attribute on module `sockeye`
- test/unit/test_knn.py:88:17: error[unresolved-attribute] Module `sockeye` has no member `model`
+ test/unit/test_knn.py:88:17: error[unresolved-attribute] Submodule `model` may not be available as an attribute on module `sockeye`
- test/unit/test_utils.py:434:2: error[unresolved-attribute] Module `unittest` has no member `mock`
+ test/unit/test_utils.py:434:2: error[unresolved-attribute] Submodule `mock` may not be available as an attribute on module `unittest`

porcupine (https://github.com/Akuli/porcupine)
- porcupine/plugins/run/no_terminal.py:87:16: error[unresolved-attribute] Module `tkinter` has no member `font`
+ porcupine/plugins/run/no_terminal.py:87:16: error[unresolved-attribute] Submodule `font` may not be available as an attribute on module `tkinter`
- porcupine/plugins/run/no_terminal.py:481:17: error[unresolved-attribute] Module `tkinter` has no member `font`
+ porcupine/plugins/run/no_terminal.py:481:17: error[unresolved-attribute] Submodule `font` may not be available as an attribute on module `tkinter`
- porcupine/settings.py:453:17: error[unresolved-attribute] Module `tkinter` has no member `font`
+ porcupine/settings.py:453:17: error[unresolved-attribute] Submodule `font` may not be available as an attribute on module `tkinter`
- porcupine/settings.py:732:72: error[unresolved-attribute] Module `tkinter` has no member `ttk`
+ porcupine/settings.py:732:72: error[unresolved-attribute] Submodule `ttk` may not be available as an attribute on module `tkinter`
- porcupine/settings.py:920:31: error[unresolved-attribute] Module `tkinter` has no member `font`
+ porcupine/settings.py:920:31: error[unresolved-attribute] Submodule `font` may not be available as an attribute on module `tkinter`
- porcupine/utils.py:570:22: error[unresolved-attribute] Module `porcupine` has no member `settings`
- porcupine/utils.py:571:6: error[unresolved-attribute] Module `porcupine` has no member `settings`
- porcupine/utils.py:638:12: error[unresolved-attribute] Module `porcupine` has no member `settings`
+ porcupine/utils.py:570:22: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`
+ porcupine/utils.py:571:6: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`
+ porcupine/utils.py:638:12: error[unresolved-attribute] Submodule `settings` may not be available as an attribute on module `porcupine`

PyGithub (https://github.com/PyGithub/PyGithub)
- github/AuthenticatedUser.py:609:16: error[unresolved-attribute] Module `github` has no member `Project`
+ github/AuthenticatedUser.py:609:16: error[unresolved-attribute] Submodule `Project` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:774:41: error[unresolved-attribute] Module `github` has no member `Label`
+ github/AuthenticatedUser.py:774:41: error[unresolved-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:807:41: error[unresolved-attribute] Module `github` has no member `Label`
+ github/AuthenticatedUser.py:807:41: error[unresolved-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:955:30: error[unresolved-attribute] Module `github` has no member `Team`
+ github/AuthenticatedUser.py:955:30: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/AuthenticatedUser.py:968:13: error[unresolved-attribute] Module `github` has no member `Installation`
+ github/AuthenticatedUser.py:968:13: error[unresolved-attribute] Submodule `Installation` may not be available as an attribute on module `github`
- github/Branch.py:100:43: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/Branch.py:100:43: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:101:38: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/Branch.py:101:38: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:103:44: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/Branch.py:103:44: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:105:48: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/Branch.py:105:48: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/Branch.py:500:16: error[unresolved-attribute] Module `github` has no member `PaginatedList`
+ github/Branch.py:500:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/Branch.py:501:13: error[unresolved-attribute] Module `github` has no member `NamedUser`
+ github/Branch.py:501:13: error[unresolved-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- github/Branch.py:511:16: error[unresolved-attribute] Module `github` has no member `PaginatedList`
+ github/Branch.py:511:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/Branch.py:512:13: error[unresolved-attribute] Module `github` has no member `Team`
+ github/Branch.py:512:13: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/BranchProtection.py:176:16: error[unresolved-attribute] Module `github` has no member `PaginatedList`
- github/Commit.py:289:13: error[unresolved-attribute] Module `github` has no member `PullRequest`
- github/CommitComment.py:191:13: error[unresolved-attribute] Module `github` has no member `Reaction`
- github/CommitComment.py:213:16: error[unresolved-attribute] Module `github` has no member `Reaction`
+ github/BranchProtection.py:176:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
+ github/Commit.py:289:13: error[unresolved-attribute] Submodule `PullRequest` may not be available as an attribute on module `github`
+ github/CommitComment.py:191:13: error[unresolved-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
+ github/CommitComment.py:213:16: error[unresolved-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/Enterprise.py:140:16: error[unresolved-attribute] Module `github` has no member `Enterprise`
+ github/Enterprise.py:140:16: error[unresolved-attribute] Submodule `Enterprise` may not be available as an attribute on module `github`
- github/Gist.py:148:26: error[unresolved-attribute] Module `github` has no member `Gist`
+ github/Gist.py:148:26: error[unresolved-attribute] Submodule `Gist` may not be available as an attribute on module `github`
- github/GistHistoryState.py:207:69: error[unresolved-attribute] Module `github` has no member `GistFile`
+ github/GistHistoryState.py:207:69: error[unresolved-attribute] Submodule `GistFile` may not be available as an attribute on module `github`
- github/GitRelease.py:316:16: error[unresolved-attribute] Module `github` has no member `GitRelease`
+ github/GitRelease.py:316:16: error[unresolved-attribute] Submodule `GitRelease` may not be available as an attribute on module `github`
- github/GitRelease.py:384:16: error[unresolved-attribute] Module `github` has no member `PaginatedList`
+ github/GitRelease.py:384:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/IssueComment.py:194:13: error[unresolved-attribute] Module `github` has no member `Reaction`
- github/IssueComment.py:216:16: error[unresolved-attribute] Module `github` has no member `Reaction`
+ github/IssueComment.py:194:13: error[unresolved-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
+ github/IssueComment.py:216:16: error[unresolved-attribute] Submodule `Reaction` may not be available as an attribute on module `github`
- github/MainClass.py:447:16: error[unresolved-attribute] Module `github` has no member `Organization`
+ github/MainClass.py:447:16: error[unresolved-attribute] Submodule `Organization` may not be available as an attribute on module `github`
- github/MainClass.py:458:13: error[unresolved-attribute] Module `github` has no member `Organization`
+ github/MainClass.py:458:13: error[unresolved-attribute] Submodule `Organization` may not be available as an attribute on module `github`
- github/MainClass.py:489:16: error[unresolved-attribute] Module `github` has no member `Repository`
- github/MainClass.py:509:13: error[unresolved-attribute] Module `github` has no member `Repository`
+ github/MainClass.py:489:16: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/MainClass.py:509:13: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/MainClass.py:517:49: error[unresolved-attribute] Module `github` has no member `RepositoryDiscussion`
+ github/MainClass.py:517:49: error[unresolved-attribute] Submodule `RepositoryDiscussion` may not be available as an attribute on module `github`
- github/MainClass.py:525:16: error[unresolved-attribute] Module `github` has no member `Project`
+ github/MainClass.py:525:16: error[unresolved-attribute] Submodule `Project` may not be available as an attribute on module `github`
- github/MainClass.py:536:16: error[unresolved-attribute] Module `github` has no member `ProjectColumn`
+ github/MainClass.py:536:16: error[unresolved-attribute] Submodule `ProjectColumn` may not be available as an attribute on module `github`
- github/MainClass.py:606:24: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:606:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:607:27: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:607:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:608:26: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:608:26: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:609:29: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:609:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:610:28: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:610:28: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:611:24: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:611:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:612:32: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:612:32: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:613:27: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:613:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:614:29: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:614:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:615:27: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:615:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:616:28: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:616:28: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:617:26: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:617:26: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:618:25: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:618:25: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:619:24: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:619:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:620:29: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:620:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:623:24: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:623:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:626:27: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:626:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:628:26: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:628:26: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:632:29: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:632:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:634:28: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:634:28: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:637:24: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:637:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:641:32: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:641:32: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:643:27: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:643:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:647:29: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:647:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:649:27: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:649:27: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:651:28: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:651:28: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:653:26: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:653:26: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:655:25: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:655:25: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:657:24: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:657:24: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:660:29: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ github/MainClass.py:660:29: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- github/MainClass.py:663:16: error[unresolved-attribute] Module `github` has no member `PaginatedList`
+ github/MainClass.py:663:16: error[unresolved-attribute] Submodule `PaginatedList` may not be available as an attribute on module `github`
- github/MainClass.py:704:13: error[unresolved-attribute] Module `github` has no member `Repository`
- github/MainClass.py:826:13: error[unresolved-attribute] Module `github` has no member `ContentFile`
- github/MainClass.py:911:57: error[unresolved-attribute] Module `github` has no member `Repository`
+ github/MainClass.py:704:13: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/MainClass.py:826:13: error[unresolved-attribute] Submodule `ContentFile` may not be available as an attribute on module `github`
+ github/MainClass.py:911:57: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/NamedUser.py:469:51: error[unresolved-attribute] Module `github` has no member `UserKey`
- github/NamedUser.py:486:13: error[unresolved-attribute] Module `github` has no member `Project`
- github/NamedUser.py:585:38: error[unresolved-attribute] Module `github` has no member `NamedUser`
+ github/NamedUser.py:469:51: error[unresolved-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
+ github/NamedUser.py:486:13: error[unresolved-attribute] Submodule `Project` may not be available as an attribute on module `github`
+ github/NamedUser.py:585:38: error[unresolved-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- github/NamedUser.py:598:16: error[unresolved-attribute] Module `github` has no member `Membership`
- github/NamedUser.py:652:54: error[unresolved-attribute] Module `github` has no member `NamedUser`
+ github/NamedUser.py:598:16: error[unresolved-attribute] Submodule `Membership` may not be available as an attribute on module `github`
+ github/NamedUser.py:652:54: error[unresolved-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- github/Organization.py:756:16: error[unresolved-attribute] Module `github` has no member `Hook`
+ github/Organization.py:756:16: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1156:16: error[unresolved-attribute] Module `github` has no member `Hook`
+ github/Organization.py:1156:16: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1165:36: error[unresolved-attribute] Module `github` has no member `Hook`
+ github/Organization.py:1165:36: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1171:16: error[unresolved-attribute] Module `github` has no member `Hook`
+ github/Organization.py:1171:16: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1177:30: error[unresolved-attribute] Module `github` has no member `Hook`
+ github/Organization.py:1177:30: error[unresolved-attribute] Submodule `Hook` may not be available as an attribute on module `github`
- github/Organization.py:1229:41: error[unresolved-attribute] Module `github` has no member `Label`
+ github/Organization.py:1229:41: error[unresolved-attribute] Submodule `Label` may not be available as an attribute on module `github`
- github/Organization.py:1240:30: error[unresolved-attribute] Module `github` has no member `Issue`
+ github/Organization.py:1240:30: error[unresolved-attribute] Submodule `Issue` may not be available as an attribute on module `github`
- github/Organization.py:1333:16: error[unresolved-attribute] Module `github` has no member `PublicKey`
- github/Organization.py:1543:16: error[unresolved-attribute] Module `github` has no member `Migration`
- github/Organization.py:1551:13: error[unresolved-attribute] Module `github` has no member `Migration`
+ github/Organization.py:1333:16: error[unresolved-attribute] Submodule `PublicKey` may not be available as an attribute on module `github`
+ github/Organization.py:1543:16: error[unresolved-attribute] Submodule `Migration` may not be available as an attribute on module `github`
+ github/Organization.py:1551:13: error[unresolved-attribute] Submodule `Migration` may not be available as an attribute on module `github`
- github/Organization.py:1565:13: error[unresolved-attribute] Module `github` has no member `Installation`
+ github/Organization.py:1565:13: error[unresolved-attribute] Submodule `Installation` may not be available as an attribute on module `github`
- github/PullRequest.py:454:16: error[unresolved-attribute] Module `github` has no member `Issue`
+ github/PullRequest.py:454:16: error[unresolved-attribute] Submodule `Issue` may not be available as an attribute on module `github`
- github/Repository.py:1210:38: error[unresolved-attribute] Module `github` has no member `Repository`
- github/Repository.py:1273:39: error[unresolved-attribute] Module `github` has no member `Repository`
- github/Repository.py:1274:29: error[unresolved-attribute] Module `github` has no member `Repository`
+ github/Repository.py:1210:38: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/Repository.py:1273:39: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
+ github/Repository.py:1274:29: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/Repository.py:4757:17: error[unresolved-attribute] Module `github` has no member `Repository`
+ github/Repository.py:4757:17: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/RepositoryDiscussionCategory.py:137:17: error[unresolved-attribute] Module `github` has no member `Repository`
+ github/RepositoryDiscussionCategory.py:137:17: error[unresolved-attribute] Submodule `Repository` may not be available as an attribute on module `github`
- github/RepositoryDiscussionComment.py:111:13: error[unresolved-attribute] Module `github` has no member `RepositoryDiscussionComment`
- github/RepositoryDiscussionComment.py:122:13: error[unresolved-attribute] Module `github` has no member `RepositoryDiscussionComment`
- github/RepositoryDiscussionComment.py:135:17: error[unresolved-attribute] Module `github` has no member `RepositoryDiscussion`
+ github/RepositoryDiscussionComment.py:111:13: error[unresolved-attribute] Submodule `RepositoryDiscussionComment` may not be available as an attribute on module `github`
+ github/RepositoryDiscussionComment.py:122:13: error[unresolved-attribute] Submodule `RepositoryDiscussionComment` may not be available as an attribute on module `github`
+ github/RepositoryDiscussionComment.py:135:17: error[unresolved-attribute] Submodule `RepositoryDiscussion` may not be available as an attribute on module `github`
- github/Requester.py:294:27: error[unresolved-attribute] Module `requests` has no member `models`
+ github/Requester.py:294:27: error[unresolved-attribute] Submodule `models` may not be available as an attribute on module `requests`
- github/Requester.py:294:63: error[unresolved-attribute] Module `requests` has no member `models`
+ github/Requester.py:294:63: error[unresolved-attribute] Submodule `models` may not be available as an attribute on module `requests`
- github/Team.py:128:33: error[unresolved-attribute] Module `github` has no member `Team`
+ github/Team.py:128:33: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/Team.py:322:16: error[unresolved-attribute] Module `github` has no member `Membership`
+ github/Team.py:322:16: error[unresolved-attribute] Submodule `Membership` may not be available as an attribute on module `github`
- github/Team.py:429:13: error[unresolved-attribute] Module `github` has no member `Team`
+ github/Team.py:429:13: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/Team.py:566:53: error[unresolved-attribute] Module `github` has no member `Team`
+ github/Team.py:566:53: error[unresolved-attribute] Submodule `Team` may not be available as an attribute on module `github`
- github/WorkflowRun.py:320:13: error[unresolved-attribute] Module `github` has no member `Artifact`
+ github/WorkflowRun.py:320:13: error[unresolved-attribute] Submodule `Artifact` may not be available as an attribute on module `github`
- tests/AuthenticatedUser.py:796:81: error[unresolved-attribute] Module `github` has no member `Migration`
+ tests/AuthenticatedUser.py:796:81: error[unresolved-attribute] Submodule `Migration` may not be available as an attribute on module `github`
- tests/Exceptions.py:167:9: error[unresolved-attribute] Module `github` has no member `UserKey`
- tests/Exceptions.py:168:15: error[unresolved-attribute] Module `github` has no member `UserKey`
+ tests/Exceptions.py:167:9: error[unresolved-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
+ tests/Exceptions.py:168:15: error[unresolved-attribute] Submodule `UserKey` may not be available as an attribute on module `github`
- tests/Framework.py:202:23: error[unresolved-attribute] Module `github` has no member `Requester`
- tests/Framework.py:209:23: error[unresolved-attribute] Module `github` has no member `Requester`
+ tests/Framework.py:202:23: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:209:23: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:328:23: error[unresolved-attribute] Module `github` has no member `Requester`
- tests/Framework.py:335:23: error[unresolved-attribute] Module `github` has no member `Requester`
- tests/Framework.py:395:13: error[unresolved-attribute] Module `github` has no member `Requester`
- tests/Framework.py:412:13: error[unresolved-attribute] Module `github` has no member `Requester`
- tests/Framework.py:454:9: error[unresolved-attribute] Module `github` has no member `Requester`
+ tests/Framework.py:328:23: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:335:23: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:395:13: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:412:13: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:454:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Framework.py:528:9: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ tests/Framework.py:528:9: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- tests/Framework.py:529:9: error[unresolved-attribute] Module `github` has no member `Requester`
- tests/Framework.py:530:9: error[unresolved-attribute] Module `github` has no member `Requester`
+ tests/Framework.py:529:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Framework.py:530:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/GithubIntegration.py:148:45: error[unresolved-attribute] Module `github` has no member `Requester`
+ tests/GithubIntegration.py:148:45: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/GithubObject.py:39:7: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ tests/GithubObject.py:39:7: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- tests/GithubObject.py:40:9: error[unresolved-attribute] Module `github` has no member `NamedUser`
+ tests/GithubObject.py:40:9: error[unresolved-attribute] Submodule `NamedUser` may not be available as an attribute on module `github`
- tests/GithubObject.py:41:9: error[unresolved-attribute] Module `github` has no member `Organization`
+ tests/GithubObject.py:41:9: error[unresolved-attribute] Submodule `Organization` may not be available as an attribute on module `github`
- tests/Github_.py:292:49: error[unresolved-attribute] Module `github` has no member `HookDelivery`
+ tests/Github_.py:292:49: error[unresolved-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
- tests/Github_.py:295:50: error[unresolved-attribute] Module `github` has no member `HookDelivery`
+ tests/Github_.py:295:50: error[unresolved-attribute] Submodule `HookDelivery` may not be available as an attribute on module `github`
- tests/Github_.py:431:30: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ tests/Github_.py:431:30: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- tests/Installation.py:117:45: error[unresolved-attribute] Module `github` has no member `Requester`
+ tests/Installation.py:117:45: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
- tests/Issue.py:52:7: error[unresolved-attribute] Module `github` has no member `GithubObject`
+ tests/Issue.py:52:7: error[unresolved-attribute] Submodule `GithubObject` may not be available as an attribute on module `github`
- tests/Issue572.py:49:42: error[unresolved-attribute] Module `github` has no member `PullRequest`
+ tests/Issue572.py:49:42: error[unresolved-attribute] Submodule `PullRequest` may not be available as an attribute on module `github`
- tests/Issue572.py:55:43: error[unresolved-attribute] Module `github` has no member `Issue`
+ tests/Issue572.py:55:43: error[unresolved-attribute] Submodule `Issue` may not be available as an attribute on module `github`
- tests/Logging_.py:85:9: error[unresolved-attribute] Module `github` has no member `Requester`
- tests/Logging_.py:88:9: error[unresolved-attribute] Module `github` has no member `Requester`
+ tests/Logging_.py:85:9: error[unresolved-attribute] Submodule `Requester` may not be available as an attribute on module `github`
+ tests/Logging_.py:88:9: error[unresolved-attribute] Submodule `Requester` may not be a

... (truncated 4465 lines) ...
```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-21 15:17_

---

_Comment by @astral-sh-bot[bot] on 2025-11-21 15:26_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 0 | 0 | 1,858 |
| **Total** | **0** | **0** | **1,858** |

**[Full report with detailed diff](https://alex-submodule-attribute-hin.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-submodule-attribute-hin.ecosystem-663.pages.dev/timing))




---

_Comment by @AlexWaygood on 2025-11-21 16:31_

I skimmed the ecosystem results, and couldn't see any false positives where the new diagnostic message was inappropriate.

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/import/nonstandard_conventions.md`:63 on 2025-11-21 21:51_

I think the old form of the assertion here was intentional -- it ensures we are erroring on the attribute we think we are, not e.g. the attribute `Y`. So I consider all these changes in this file to be somewhat of a regression in assertion precision. We could do this to make it even less sensitive to changes in the message, and add the code?
```suggestion
# error: [unresolved-attribute] "`fails`"
```

---

_@carljm approved on 2025-11-21 21:52_

---

_@AlexWaygood reviewed on 2025-11-22 13:53_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer/builder.rs`:9096 on 2025-11-22 13:53_

if we hit this branch, we have pretty high confidence that we know what the user _meant_ here -- we could even consider inferring the module-literal type of the submodule rather than inferring `Unknown`. (Or `<submodule type> | Unknown`...?)

And since this often will (by pure luck!) work at runtime, we could also consider downgrading the diagnostic to warning-level rather than error-level

But either of those would be separate changes from simply improving the diagnostic error message, which is all that this humble PR seeks to achieve.

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-22 13:53_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-22 13:53_

---

_Label `ecosystem-analyzer` removed by @AlexWaygood on 2025-11-22 13:58_

---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-22 13:58_

---

_Merged by @AlexWaygood on 2025-11-22 14:07_

---

_Closed by @AlexWaygood on 2025-11-22 14:07_

---

_Branch deleted on 2025-11-22 14:07_

---
