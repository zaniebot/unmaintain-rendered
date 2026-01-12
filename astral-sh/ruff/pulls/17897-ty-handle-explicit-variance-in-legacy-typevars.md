```yaml
number: 17897
title: "[ty] Handle explicit variance in legacy typevars"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/legacy-variance
created_at: 2025-05-06T20:17:03Z
updated_at: 2025-05-07T12:44:54Z
url: https://github.com/astral-sh/ruff/pull/17897
synced_at: 2026-01-12T15:56:07Z
```

# [ty] Handle explicit variance in legacy typevars

---

_@dcreager_

We now track the variance of each typevar, and obey the `covariant` and `contravariant` parameters to the legacy `TypeVar` constructor. We still don't yet infer variance for PEP-695 typevars or for the `infer_variance` legacy constructor parameter.

---

_Review requested from @carljm by @dcreager on 2025-05-06 20:17_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-06 20:17_

---

_Review requested from @sharkdp by @dcreager on 2025-05-06 20:17_

---

_Label `ty` added by @dcreager on 2025-05-06 20:17_

---

_@dcreager reviewed on 2025-05-06 20:18_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5668 on 2025-05-06 20:18_

This lines up with what the Python interpreter displays for typevars, though the `~` sigil for invariant typevars is an unfortunate conflict with `~` for negation in an intersection!

I've made sure that this display update is in a distinct commit so that it's easy to revert if this part of the PR is controversial.

---

_Comment by @github-actions[bot] on 2025-05-06 20:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
- error[lint:no-matching-overload] parso/python/errors.py:650:22: No overload of bound method `__init__` matches arguments
- Found 86 diagnostics
+ Found 85 diagnostics

beartype (https://github.com/beartype/beartype)
- error[lint:no-matching-overload] beartype/_util/error/utilerrwarn.py:65:14: No overload of bound method `__init__` matches arguments
- Found 562 diagnostics
+ Found 561 diagnostics

trio (https://github.com/python-trio/trio)
- error[lint:invalid-argument-type] src/trio/_core/_run.py:662:52: Argument to this function is incorrect: Expected `BaseExceptionGroup[BaseException]`, found `BaseExceptionGroup[_BaseExceptionT_co] & ~AlwaysFalsy`
- error[lint:no-matching-overload] src/trio/_core/_tests/test_guest_mode.py:316:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_core/_tests/test_guest_mode.py:320:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_core/_tests/test_guest_mode.py:338:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_core/_tests/tutil.py:68:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:476:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:594:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:960:9: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:1022:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:1028:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:1080:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:1082:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:1084:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:62:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:65:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:91:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:94:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:97:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:101:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:105:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:121:5: No overload of bound method `__init__` matches arguments
- Found 1116 diagnostics
+ Found 1095 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[lint:no-matching-overload] src/check_jsonschema/parsers/yaml.py:61:14: No overload of bound method `__init__` matches arguments
- Found 64 diagnostics
+ Found 63 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[lint:no-matching-overload] porcupine/plugins/autocomplete.py:527:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] porcupine/plugins/autocomplete.py:530:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] porcupine/plugins/autocomplete.py:531:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] porcupine/plugins/autocomplete.py:532:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] porcupine/plugins/autocomplete.py:533:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] porcupine/plugins/autocomplete.py:534:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] porcupine/plugins/autocomplete.py:535:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] porcupine/plugins/matching_paren.py:76:5: No overload of bound method `bind` matches arguments
- error[lint:no-matching-overload] porcupine/plugins/rightclick_menu.py:54:5: No overload of bound method `bind` matches arguments
- Found 111 diagnostics
+ Found 102 diagnostics

isort (https://github.com/pycqa/isort)
- error[lint:invalid-return-type] isort/io.py:49:20: Return type does not match returned value: Expected `TextIOWrapper[_WrappedBuffer]`, found `TextIOWrapper[TextIOWrapper[_WrappedBuffer]]`
- Found 51 diagnostics
+ Found 50 diagnostics

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
+ warning[lint:unused-ignore-comment] aiohttp_devtools/runserver/serve.py:47:42: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] aiohttp_devtools/runserver/serve.py:57:42: Unused blanket `type: ignore` directive
- Found 62 diagnostics
+ Found 64 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[lint:invalid-argument-type] tests/conftest.py:493:17: Argument to this function is incorrect: Expected `Mapping[str, str | Mapping[str, Any]] | None`, found `Mapping[str, str] | None`
- error[lint:invalid-argument-type] tests/conftest.py:494:17: Argument to this function is incorrect: Expected `Mapping[str, str | Mapping[str, Any]] | None`, found `Mapping[str, str] | None`
- Found 1138 diagnostics
+ Found 1136 diagnostics

ignite (https://github.com/pytorch/ignite)
- error[lint:no-matching-overload] ignite/handlers/neptune_logger.py:682:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/handlers/test_checkpoint.py:640:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/handlers/test_neptune_logger.py:541:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/nlp/test_bleu.py:53:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/nlp/test_bleu.py:61:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/nlp/test_bleu.py:152:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/nlp/test_bleu.py:188:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/nlp/test_bleu.py:243:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/nlp/test_bleu.py:289:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/test_precision.py:221:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/test_precision.py:287:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/test_precision.py:434:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/test_recall.py:223:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/test_recall.py:290:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/test_recall.py:437:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/test_running_average.py:38:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/ignite/metrics/test_running_average.py:224:10: No overload of bound method `__init__` matches arguments
- Found 2284 diagnostics
+ Found 2267 diagnostics

websockets (https://github.com/aaugustin/websockets)
- error[lint:no-matching-overload] src/websockets/auth.py:6:6: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/websockets/http.py:8:6: No overload of bound method `__init__` matches arguments
- Found 112 diagnostics
+ Found 110 diagnostics

black (https://github.com/psf/black)
- error[lint:invalid-argument-type] src/black/__init__.py:987:41: Argument to this function is incorrect: Expected `TextIOWrapper[_WrappedBuffer]`, found `TextIOWrapper[BinaryIO | @Todo(Support for `typing.TypeAlias`)]`
- error[lint:no-matching-overload] src/black/parsing.py:129:10: No overload of bound method `__init__` matches arguments
- Found 135 diagnostics
+ Found 133 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[lint:no-matching-overload] tornado/test/asyncio_test.py:129:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tornado/test/asyncio_test.py:234:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tornado/test/asyncio_test.py:254:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tornado/test/log_test.py:34:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tornado/test/testing_test.py:155:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tornado/test/util.py:91:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tornado/testing.py:151:46: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tornado/testing.py:164:46: No overload of bound method `__init__` matches arguments
- Found 542 diagnostics
+ Found 534 diagnostics

pip (https://github.com/pypa/pip)
- error[lint:no-matching-overload] src/pip/_internal/operations/install/wheel.py:606:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/pip/_vendor/pkg_resources/__init__.py:2448:14: No overload of bound method `__init__` matches arguments
- error[lint:invalid-legacy-type-variable] src/pip/_vendor/typing_extensions.py:1650:27: The name of a legacy `typing.TypeVar` must match the name of the variable it is assigned to (`typevar`)
+ error[lint:invalid-legacy-type-variable] src/pip/_vendor/typing_extensions.py:1650:27: The `contravariant` parameter of a legacy `typing.TypeVar` cannot have an ambiguous value
- error[lint:invalid-legacy-type-variable] src/pip/_vendor/typing_extensions.py:1654:27: The name of a legacy `typing.TypeVar` must match the name of the variable it is assigned to (`typevar`)
+ error[lint:invalid-legacy-type-variable] src/pip/_vendor/typing_extensions.py:1654:27: The `contravariant` parameter of a legacy `typing.TypeVar` cannot have an ambiguous value
- Found 1030 diagnostics
+ Found 1028 diagnostics

paasta (https://github.com/yelp/paasta)
- error[lint:invalid-argument-type] paasta_tools/cli/cmds/rollback.py:287:64: Argument to this function is incorrect: Expected `Mapping[DeploymentVersion, tuple]`, found `Mapping[DeploymentVersion, tuple[str, str]]`
- error[lint:invalid-argument-type] paasta_tools/cli/cmds/rollback.py:298:64: Argument to this function is incorrect: Expected `Mapping[DeploymentVersion, tuple]`, found `Mapping[DeploymentVersion, tuple[str, str]]`
- Found 1037 diagnostics
+ Found 1035 diagnostics

optuna (https://github.com/optuna/optuna)
- error[lint:no-matching-overload] tests/pruners_tests/test_percentile.py:153:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/pruners_tests/test_percentile.py:222:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_cmaes.py:431:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_gp.py:67:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_gp.py:87:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_nsgaii.py:179:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_nsgaii.py:202:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_nsgaii.py:373:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_nsgaii.py:660:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_nsgaiii.py:143:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_nsgaiii.py:167:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_partial_fixed.py:25:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_partial_fixed.py:49:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_partial_fixed.py:71:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_partial_fixed.py:92:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_partial_fixed.py:109:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_partial_fixed.py:125:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_qmc.py:31:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_samplers.py:580:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_samplers.py:817:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_samplers.py:838:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_samplers.py:857:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_samplers.py:879:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/test_samplers.py:907:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_probability_distributions.py:87:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:115:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:125:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:141:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:147:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:161:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:179:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:186:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:192:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:198:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:217:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:233:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:243:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:264:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:291:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:314:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:338:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:367:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:379:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:403:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:428:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:1024:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/samplers_tests/tpe_tests/test_sampler.py:1101:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/storages_tests/rdb_tests/test_storage.py:203:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/storages_tests/rdb_tests/test_storage.py:253:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/storages_tests/rdb_tests/test_storage.py:302:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/storages_tests/rdb_tests/test_storage.py:318:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/storages_tests/test_heartbeat.py:190:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/study_tests/test_study.py:951:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_distributions.py:298:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_distributions.py:322:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_distributions.py:406:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/trial_tests/test_trial.py:448:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/visualization_tests/test_pareto_front.py:97:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/visualization_tests/test_pareto_front.py:153:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/visualization_tests/test_pareto_front.py:208:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/visualization_tests/test_pareto_front.py:254:10: No overload of bound method `__init__` matches arguments
- Found 2271 diagnostics
+ Found 2210 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- error[lint:no-matching-overload] test/with_dummyserver/test_connectionpool.py:498:18: No overload of bound method `__init__` matches arguments
- Found 512 diagnostics
+ Found 511 diagnostics

vision (https://github.com/pytorch/vision)
- error[lint:no-matching-overload] test/common_utils.py:522:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] test/common_utils.py:533:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] torchvision/io/_video_opt.py:342:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] torchvision/io/_video_opt.py:387:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] torchvision/io/_video_opt.py:430:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] torchvision/io/video_reader.py:145:22: No overload of bound method `__init__` matches arguments
- Found 2211 diagnostics
+ Found 2205 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[lint:invalid-return-type] expression/extra/result/catch.py:45:24: Return type does not match returned value: Expected `Result[_TSource, _TError]`, found `Result[_TSource & ~Result[Unknown, Unknown], Any]`
- error[lint:invalid-assignment] tests/test_option.py:390:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, Any]`
- error[lint:invalid-assignment] tests/test_result.py:18:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, str]`
- error[lint:invalid-assignment] tests/test_result.py:53:5: Object of type `Result[Any, Literal["err"]]` is not assignable to `Result[int, str]`
- error[lint:invalid-assignment] tests/test_result.py:69:5: Object of type `Result[Any, CustomException]` is not assignable to `Result[str, Exception]`
- error[lint:invalid-assignment] tests/test_result.py:263:5: Object of type `Result[Any, Literal["failed"]]` is not assignable to `Result[int, str]`
- error[lint:invalid-assignment] tests/test_result.py:276:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, str]`
- error[lint:invalid-assignment] tests/test_result.py:283:5: Object of type `Result[Literal[5], Any]` is not assignable to `Result[int, str]`
- error[lint:invalid-assignment] tests/test_result.py:300:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, str]`
- error[lint:invalid-assignment] tests/test_result.py:307:5: Object of type `Result[Literal[5], Any]` is not assignable to `Result[int, str]`
- error[lint:invalid-assignment] tests/test_result.py:414:5: Object of type `Result[Any, Literal[0]]` is not assignable to `Result[int, int]`
- error[lint:invalid-assignment] tests/test_result.py:422:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, int]`
- error[lint:invalid-assignment] tests/test_result.py:429:5: Object of type `Result[Any, Literal[0]]` is not assignable to `Result[int, int]`
- error[lint:invalid-assignment] tests/test_result.py:437:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, int]`
- error[lint:invalid-assignment] tests/test_result.py:445:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, Any]`
- error[lint:invalid-assignment] tests/test_result.py:476:5: Object of type `Result[Literal[1], Any]` is not assignable to `Result[int, str]`
- error[lint:invalid-assignment] tests/test_result.py:482:5: Object of type `Result[Any, Literal[1]]` is not assignable to `Result[str, int]`
- error[lint:invalid-assignment] tests/test_result.py:490:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, str]`
- error[lint:invalid-argument-type] tests/test_result.py:491:21: Argument to this function is incorrect: Expected `Result[int, str]`, found `Result[Literal[0], Any]`
+ error[lint:invalid-argument-type] tests/test_result.py:491:21: Argument to this function is incorrect: Expected `Result[Literal[42], Any]`, found `Result[Literal[0], Any]`
- error[lint:invalid-assignment] tests/test_result.py:496:5: Object of type `Result[Literal[42], Any]` is not assignable to `Result[int, str]`
- error[lint:invalid-argument-type] tests/test_result.py:497:21: Argument to this function is incorrect: Expected `Result[int, str]`, found `Result[Any, Literal["new error"]]`
- error[lint:invalid-assignment] tests/test_result.py:502:5: Object of type `Result[Any, Literal["original error"]]` is not assignable to `Result[int, str]`
- error[lint:invalid-argument-type] tests/test_result.py:503:21: Argument to this function is incorrect: Expected `Result[int, str]`, found `Result[Literal[0], Any]`
- error[lint:invalid-assignment] tests/test_result.py:508:5: Object of type `Result[Any, Literal["original error"]]` is not assignable to `Result[int, str]`
- error[lint:invalid-argument-type] tests/test_result.py:509:21: Argument to this function is incorrect: Expected `Result[int, str]`, found `Result[Any, Literal["new error"]]`
+ error[lint:invalid-argument-type] tests/test_result.py:509:21: Argument to this function is incorrect: Expected `Result[Any, Literal["original error"]]`, found `Result[Any, Literal["new error"]]`
- error[lint:invalid-assignment] tests/test_result.py:516:5: Object of type `Result[Literal["good"], Any]` is not assignable to `Result[str, str]`
- error[lint:invalid-assignment] tests/test_result.py:522:5: Object of type `Result[Literal["good"], Any]` is not assignable to `Result[str, str]`
- error[lint:invalid-assignment] tests/test_result.py:528:5: Object of type `Result[Any, Literal["original error"]]` is not assignable to `Result[str, str]`
- error[lint:invalid-assignment] tests/test_result.py:534:5: Object of type `Result[Any, Literal["original error"]]` is not assignable to `Result[str, str]`
- error[lint:invalid-assignment] tests/test_result.py:549:5: Object of type `Result[Any, Literal["error"]]` is not assignable to `Result[str, str]`
- error[lint:invalid-assignment] tests/test_result.py:585:5: Object of type `Result[B, Any]` is not assignable to `Result[A, str]`
- error[lint:invalid-assignment] tests/test_result.py:592:5: Object of type `Result[Any, B]` is not assignable to `Result[str, A]`
- Found 537 diagnostics
+ Found 507 diagnostics

mypy (https://github.com/python/mypy)
- error[lint:no-matching-overload] mypy/fastparse.py:231:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] mypy/stubtest.py:205:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] mypy/stubtest.py:245:10: No overload of bound method `__init__` matches arguments
- Found 3246 diagnostics
+ Found 3243 diagnostics

setuptools (https://github.com/pypa/setuptools)
- error[lint:no-matching-overload] pkg_resources/__init__.py:2479:14: No overload of bound method `__init__` matches arguments
- error[lint:invalid-legacy-type-variable] setuptools/_vendor/typing_extensions.py:1513:27: The name of a legacy `typing.TypeVar` must match the name of the variable it is assigned to (`typevar`)
+ error[lint:invalid-legacy-type-variable] setuptools/_vendor/typing_extensions.py:1513:27: The `contravariant` parameter of a legacy `typing.TypeVar` cannot have an ambiguous value
- error[lint:invalid-legacy-type-variable] setuptools/_vendor/typing_extensions.py:1517:27: The name of a legacy `typing.TypeVar` must match the name of the variable it is assigned to (`typevar`)
+ error[lint:invalid-legacy-type-variable] setuptools/_vendor/typing_extensions.py:1517:27: The `contravariant` parameter of a legacy `typing.TypeVar` cannot have an ambiguous value
- error[lint:no-matching-overload] setuptools/build_meta.py:142:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] setuptools/tests/test_build_meta.py:394:18: No overload of bound method `__init__` matches arguments
- Found 1212 diagnostics
+ Found 1209 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[lint:no-matching-overload] scrapy/utils/reactor.py:121:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_core_downloader.py:79:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_crawler.py:80:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_downloadermiddleware_offsite.py:123:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_downloadermiddleware_offsite.py:213:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_feedexport.py:2832:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_feedexport.py:2866:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_feedexport.py:2882:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_feedexport.py:2899:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_http_request.py:399:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_utils_curl.py:216:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_utils_deprecate.py:120:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_utils_deprecate.py:150:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_utils_deprecate.py:187:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_utils_deprecate.py:221:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_utils_deprecate.py:291:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_utils_project.py:47:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/test_utils_request.py:384:14: No overload of bound method `__init__` matches arguments
- Found 1499 diagnostics
+ Found 1481 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- error[lint:no-matching-overload] tests/Framework.py:417:14: No overload of bound method `__init__` matches arguments
- Found 326 diagnostics
+ Found 325 diagnostics

arviz (https://github.com/arviz-devs/arviz)
- error[lint:no-matching-overload] arviz/stats/stats_utils.py:42:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] arviz/tests/base_tests/test_data.py:1185:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] arviz/tests/helpers.py:662:10: No overload of bound method `__init__` matches arguments
- Found 2605 diagnostics
+ Found 2602 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- error[lint:invalid-return-type] cwltool/cwlprov/writablebagfile.py:120:16: Return type does not match returned value: Expected `TextIOWrapper[_WrappedBuffer] | WritableBagFile`, found `TextIOWrapper[BinaryIO]`
- Found 412 diagnostics
+ Found 411 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[lint:no-matching-overload] src/_pytest/_code/source.py:189:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/_pytest/outcomes.py:273:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/_pytest/raises.py:284:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/_pytest/raises.py:295:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/_py/test_local.py:20:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/acceptance_test.py:714:14: No overload of bound method `__init__` matches arguments
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:227:39: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:758:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:781:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:792:68: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | None`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:814:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:841:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:878:64: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | None`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:931:67: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | None`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:947:35: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException]`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:950:35: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException]`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:975:78: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | None`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:996:39: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException]`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:1000:35: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException]`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:1105:39: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException]`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:1150:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:1184:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:1216:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:1272:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:invalid-argument-type] testing/code/test_excinfo.py:1328:54: Argument to this function is incorrect: Expected `ExceptionInfo[BaseException] | ((TracebackEntry, /) -> bool)`, found `ExceptionInfo[E]`
- error[lint:no-matching-overload] testing/python/raises.py:230:18: No overload of bound method `__init__` matches arguments
+ warning[lint:unused-ignore-comment] testing/python/raises_group.py:41:23: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] testing/python/raises_group.py:46:25: Unused blanket `type: ignore` directive
+ warning[lint:unused-ignore-comment] testing/python/raises_group.py:1085:28: Unused blanket `type: ignore` directive
- error[lint:no-matching-overload] testing/python/raises_group.py:348:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:458:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:477:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:512:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:516:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:521:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:525:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:536:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:540:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:606:13: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:628:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:652:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:878:25: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:953:49: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1028:9: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1087:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1093:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1097:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1103:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1107:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1117:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1125:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1130:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1138:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1143:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1151:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1156:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1162:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1178:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1186:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1196:33: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1202:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1203:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1205:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1207:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1209:13: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1235:12: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1239:16: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1251:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1313:9: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1318:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1325:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/python/raises_group.py:1334:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/test_nodes.py:45:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/test_recwarn.py:458:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/test_recwarn.py:579:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:72:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:73:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:74:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:75:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:77:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:78:5: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:143:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:183:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:183:44: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:184:33: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:195:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:195:44: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:196:21: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:219:17: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] testing/typing_raises_group.py:228:17: No overload of bound method `__init__` matches arguments
- Found 895 diagnostics
+ Found 811 diagnostics

aiohttp (https://github.com/aio-libs/aiohttp)
- error[lint:no-matching-overload] aiohttp/multipart.py:410:18: No overload of bound method `__init__` matches arguments
- Found 204 diagnostics
+ Found 203 diagnostics

pyodide (https://github.com/pyodide/pyodide)
- error[lint:no-matching-overload] pyodide-build/pyodide_build/common.py:625:14: No overload of bound method `__init__` matches arguments
- Found 1066 diagnostics
+ Found 1065 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[lint:no-matching-overload] src/bokeh/sphinxext/bokeh_model.py:143:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/bokeh/sphinxext/bokeh_plot.py:348:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] src/bokeh/sphinxext/bokeh_prop.py:123:14: No overload of bound method `__init__` matches arguments
- Found 1071 diagnostics
+ Found 1068 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- error[lint:no-matching-overload] sphinx/builders/html/__init__.py:448:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sphinx/builders/latex/__init__.py:304:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sphinx/builders/manpage.py:57:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sphinx/builders/texinfo.py:122:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sphinx/util/requests.py:108:14: No overload of bound method `__init__` matches arguments
- Found 751 diagnostics
+ Found 746 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[lint:no-matching-overload] tests/contrib/futures/test_propagation.py:451:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/profiling/test_profiler.py:380:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] tests/profiling_v2/test_profiler.py:190:10: No overload of bound method `__init__` matches arguments
- Found 8677 diagnostics
+ Found 8674 diagnostics

rotki (https://github.com/rotki/rotki)
- error[lint:invalid-argument-type] rotkehlchen/tests/unit/test_evm_names.py:46:33: Argument to this function is incorrect: Expected `Mapping[Unknown, str | None]`, found `Mapping[Unknown, str]`
- Found 2172 diagnostics
+ Found 2171 diagnostics

jax (https://github.com/google/jax)
- error[lint:no-matching-overload] jax/_src/test_warning_util.py:55:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] jax/_src/test_warning_util.py:96:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] jax/experimental/jax2tf/examples/keras_reuse_main.py:28:6: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] jax/experimental/jax2tf/examples/mnist_lib.py:73:14: No overload of bound method `__init__` matches arguments
- Found 5375 diagnostics
+ Found 5371 diagnostics

scipy (https://github.com/scipy/scipy)
- error[lint:no-matching-overload] benchmarks/benchmarks/sparse.py:327:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] benchmarks/benchmarks/sparse_linalg_lobpcg.py:86:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] benchmarks/benchmarks/sparse_linalg_lobpcg.py:127:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] benchmarks/benchmarks/sparse_linalg_lobpcg.py:157:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] benchmarks/benchmarks/sparse_linalg_svds.py:64:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] benchmarks/benchmarks/stats.py:22:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/_lib/_util.py:28:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/_lib/array_api_extra/tests/test_funcs.py:612:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/conftest.py:551:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/interpolate/tests/test_polyint.py:634:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/io/matlab/tests/test_mio.py:446:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/io/tests/test_netcdf.py:341:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/optimize/_constraints.py:181:18: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/optimize/_constraints.py:396:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/optimize/_linprog_ip.py:102:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/optimize/_optimize.py:1184:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/optimize/_trustregion_constr/projections.py:64:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/optimize/tests/test_minpack.py:973:22: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/sparse/linalg/_dsolve/linsolve.py:688:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/special/_basic.py:2968:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/special/_basic.py:2984:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/special/_basic.py:2994:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/special/tests/test_sf_error.py:45:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/_binned_statistic.py:649:45: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/_stats_py.py:741:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/_stats_py.py:982:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/_stats_py.py:2931:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/_stats_py.py:7406:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/_stats_py.py:10928:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_axis_nan_policy.py:555:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_axis_nan_policy.py:589:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_distributions.py:428:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_distributions.py:2542:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_distributions.py:3771:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_distributions.py:3859:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_distributions.py:4759:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_distributions.py:7288:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_distributions.py:8817:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_distributions.py:8825:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_fast_gen_inversion.py:229:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_morestats.py:131:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_mstats_basic.py:184:14: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_stats.py:3878:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] scipy/stats/tests/test_stats.py:4331:14: No overload of bound method `__init__` matches arguments
- error[lint:invalid-return-type] subprojects/highs/src/highspy/highs.py:263:20: Return type does not match returned value: Expected `int | float | Mapping[Any, Any] | ndarray[Any, dtype[float64]]`, found `@Todo(specialized non-generic class) | ndarray[@Todo(Support for `typing.TypeAlias`), _DType_co] | int | float`
- Found 36741 diagnostics
+ Found 36696 diagnostics

sympy (https://github.com/sympy/sympy)
- error[lint:no-matching-overload] sympy/diffgeom/rn.py:43:6: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sympy/diffgeom/rn.py:104:6: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sympy/parsing/tests/test_ast_parser.py:23:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sympy/physics/vector/tests/test_point.py:219:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sympy/physics/vector/tests/test_point.py:250:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sympy/physics/vector/tests/test_point.py:265:10: No overload of bound method `__init__` matches arguments
- error[lint:no-matching-overload] sympy/testing/runtests.py:189:10: No overload of bound method `__init__` matches arguments
- Found 17069 diagnostics
+ Found 17062 diagnostics

```
</details>


---

_Review requested from @MichaReiser by @dcreager on 2025-05-06 20:32_

---

_@MichaReiser reviewed on 2025-05-06 20:34_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types.rs`:5654 on 2025-05-06 20:34_

Nit: Consider making this type `Copy` to make it clear that we don't need to use `return_ref` (and maybe also `TypeVarBoundOrConstraints`)

---

_@MichaReiser reviewed on 2025-05-06 20:34_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/display.rs`:329 on 2025-05-06 20:34_

@AlexWaygood will change this to
```suggestion
            var.variance(self.db).fmt(f)?;
```

---

_@AlexWaygood reviewed on 2025-05-06 20:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:5668 on 2025-05-06 20:39_

Hmm, yeah... Not sure I like this 😄 it feels very confusing considering that we already use ~ for negated intersections.

How do other type checkers display covariant/contravariant/invariant TypeVars? I'm not sure I've seen ~T in the output of any other type checkers.

Even if we want to do this, an argument for deferring it for now is that it would make the mypy_primer diff on this PR much easier to review!

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variance.md`:1 on 2025-05-06 20:48_

This is really beautifully explained! The only thing I feel is missing is discussion of assignability where a dynamic type is provided as a type argument. `list[Any]` is not a subtype of `list[int]`, but it is _assignable_ to `list[int]`. But maybe that's coming in a followup PR? 😃

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:5030 on 2025-05-06 20:52_

```suggestion
                                                        "The `covariant` parameter of \
```

---

_@AlexWaygood reviewed on 2025-05-06 20:52_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5668 on 2025-05-07 01:08_

Reverted this; if we decide to do this it will be better in a separate PR as you say

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:5654 on 2025-05-07 01:09_

Done

---

_@dcreager reviewed on 2025-05-07 01:12_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/display.rs`:329 on 2025-05-07 01:13_

This is moot since I reverted the rendering changes

---

_@dcreager reviewed on 2025-05-07 01:13_

---

_@dcreager reviewed on 2025-05-07 01:16_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variance.md`:1 on 2025-05-07 01:16_

We do _check_ assignability in all of these tests, though you're right that I don't mention that much in the prose.  I'll try to add something for that...

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variance.md`:1 on 2025-05-07 01:38_

Took a stab at this, lmkwyt

---

_@dcreager reviewed on 2025-05-07 01:38_

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variance.md`:12 on 2025-05-07 03:28_

```suggestion
(Note that dynamic types like `Any` never participate in subtyping, so `C[Any]` is neither a subtype
nor supertype of any other specialization of `C`, regardless of `T`'s variance. It is, however,
assignable to any specialization of `C`, regardless of variance, via materialization.)
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/generics/legacy/variance.md`:129 on 2025-05-07 03:49_

```suggestion
With an invariant typevar, only equivalent specializations of the generic class are subtypes of or assignable
```

---

_@carljm approved on 2025-05-07 04:00_

Looks good!

---

_Comment by @AlexWaygood on 2025-05-07 10:07_

(I merged in `main` so I could more easily stack https://github.com/astral-sh/ruff/pull/17832 on top of this PR -- I hope that's okay!

---

_Closed by @AlexWaygood on 2025-05-07 12:38_

---

_Reopened by @AlexWaygood on 2025-05-07 12:38_

---

_Merged by @dcreager on 2025-05-07 12:44_

---

_Closed by @dcreager on 2025-05-07 12:44_

---

_Branch deleted on 2025-05-07 12:44_

---
