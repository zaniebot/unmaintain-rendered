```yaml
number: 19064
title: "[ty] Object type narrowing from attribute constraints"
type: pull_request
state: open
author: mtshiba
labels:
  - ty
assignees: []
draft: true
base: main
head: 643-adv-attr-narrowing
created_at: 2025-07-01T05:54:19Z
updated_at: 2025-10-29T15:05:28Z
url: https://github.com/astral-sh/ruff/pull/19064
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Object type narrowing from attribute constraints

---

_Pull request opened by @mtshiba on 2025-07-01 05:54_

## Summary

This PR closes astral-sh/ty#643 (and depends on #19029).
It synthesizes constraints on objects (e.g. `x: Protocol[{"tag": (read) Literal["C"]}]`) from constraints on attributes (e.g. `x.tag: (read) Literal["C"]`).

## Test Plan

New tests in `complex_target.md`.

---

_Renamed from "[ty] " to "[ty] Object type narrowing from attribute constraints" by @mtshiba on 2025-07-01 05:55_

---

_Comment by @github-actions[bot] on 2025-07-01 05:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~45MB
+ TOTAL MEMORY USAGE: ~49MB

parso (https://github.com/davidhalter/parso)
-     struct metadata = ~1MB
+     struct metadata = ~2MB

python-chess (https://github.com/niklasf/python-chess)
-     struct metadata = ~2MB
+     struct metadata = ~3MB

beartype (https://github.com/beartype/beartype)
- warning[possibly-unbound-attribute] beartype/_util/api/external/utilclick.py:108:5: Attribute `callback` on type `BeartypeableT` is possibly unbound
+ warning[possibly-unbound-attribute] beartype/_util/api/external/utilclick.py:108:5: Attribute `callback` on type `BeartypeableT & Unknown` is possibly unbound

attrs (https://github.com/python-attrs/attrs)
- error[too-many-positional-arguments] tests/test_slots.py:327:15: Too many positional arguments: expected 0, got 1
- Found 602 diagnostics
+ Found 601 diagnostics
-     struct metadata = ~2MB
+     struct metadata = ~3MB

yarl (https://github.com/aio-libs/yarl)
-     struct metadata = ~1MB
+     struct metadata = ~2MB

jinja (https://github.com/pallets/jinja)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~106MB
-     struct metadata = ~3MB
+     struct metadata = ~4MB

anyio (https://github.com/agronholm/anyio)
-     memo metadata = ~6MB
+     memo metadata = ~7MB

aioredis (https://github.com/aio-libs/aioredis)
-     struct metadata = ~1MB
+     struct metadata = ~2MB
-     memo metadata = ~3MB
+     memo metadata = ~4MB

aiortc (https://github.com/aiortc/aiortc)
- error[invalid-assignment] src/aiortc/rtcpeerconnection.py:582:17: Object of type `Literal["client"]` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
+ error[invalid-assignment] src/aiortc/rtcpeerconnection.py:582:17: Object of type `Literal["client"]` is not assignable to attribute `role` of type `RTCDtlsParameters | None`
- error[invalid-assignment] src/aiortc/rtcpeerconnection.py:584:17: Object of type `Unknown & ~Literal["auto"]` is not assignable to attribute `role` on type `RTCDtlsParameters | None`
+ error[invalid-assignment] src/aiortc/rtcpeerconnection.py:584:17: Object of type `Unknown & ~Literal["auto"]` is not assignable to attribute `role` of type `RTCDtlsParameters | None`
- warning[possibly-unbound-attribute] src/aiortc/rtcpeerconnection.py:1362:54: Attribute `password` on type `RTCIceParameters | None` is possibly unbound
- error[invalid-assignment] src/aiortc/sdp.py:486:25: Object of type `str | None` is not assignable to attribute `muxId` on type `Unknown | RTCRtpParameters`
+ error[invalid-assignment] src/aiortc/sdp.py:486:25: Object of type `str | None` is not assignable to attribute `muxId` of type `Unknown | RTCRtpParameters`
- Found 98 diagnostics
+ Found 97 diagnostics
-     struct metadata = ~2MB
+     struct metadata = ~3MB

starlette (https://github.com/encode/starlette)
-     struct fields = ~4MB
+     struct fields = ~5MB

black (https://github.com/psf/black)
- error[unresolved-attribute] src/black/handle_ipynb_magics.py:455:18: Type `expr` has no attribute `attr`
- error[unresolved-attribute] src/black/handle_ipynb_magics.py:461:49: Type `expr` has no attribute `attr`
- error[unresolved-attribute] src/black/handle_ipynb_magics.py:499:18: Type `expr` has no attribute `attr`
- error[unresolved-attribute] src/black/handle_ipynb_magics.py:501:18: Type `expr` has no attribute `attr`
- Found 73 diagnostics
+ Found 69 diagnostics
-     memo metadata = ~8MB
+     memo metadata = ~9MB

rich (https://github.com/Textualize/rich)
- error[unresolved-attribute] tests/test_progress.py:616:24: Type `BinaryIO` has no attribute `handle`
+ error[unresolved-attribute] tests/test_progress.py:616:24: Type `BinaryIO & <Protocol with members 'closed'>` has no attribute `handle`

alerta (https://github.com/alerta/alerta)
-     struct metadata = ~3MB
+     struct metadata = ~4MB

trio (https://github.com/python-trio/trio)
+ warning[unused-ignore-comment] src/trio/_core/_run.py:178:30: Unused blanket `type: ignore` directive
- error[invalid-argument-type] src/trio/_core/_run.py:2035:33: Argument is incorrect: Expected `bool`, found `RuntimeError`
- error[unresolved-attribute] src/trio/_path.py:47:20: Type `(...) -> object` has no attribute `__qualname__`
+ error[unresolved-attribute] src/trio/_path.py:47:20: Type `((...) -> object) & <Protocol with members '__doc__'>` has no attribute `__qualname__`
- error[invalid-assignment] src/trio/_util.py:223:21: Object of type `str` is not assignable to attribute `__qualname__` on type `<Protocol with members '__name__'> & <Protocol with members '__qualname__'>`
+ error[invalid-assignment] src/trio/_util.py:223:21: Object of type `str` is not assignable to attribute `__qualname__` of type `<Protocol with members '__name__'> & <Protocol with members '__qualname__'>`
-     struct fields = ~8MB
+     struct fields = ~9MB

discord.py (https://github.com/Rapptz/discord.py)
- error[invalid-assignment] discord/ext/commands/cog.py:525:13: Object of type `Literal[True]` is not assignable to attribute `__cog_listener__` on type `(FuncT & ~staticmethod) | ((...) -> Unknown)`
+ error[invalid-assignment] discord/ext/commands/cog.py:525:13: Object of type `Literal[True]` is not assignable to attribute `__cog_listener__` of type `(FuncT & ~staticmethod) | ((...) -> Unknown)`
- error[invalid-assignment] discord/ext/commands/cog.py:530:17: Object of type `list[Unknown]` is not assignable to attribute `__cog_listener_names__` on type `(FuncT & ~staticmethod) | ((...) -> Unknown)`
+ error[invalid-assignment] discord/ext/commands/cog.py:530:17: Object of type `list[Unknown]` is not assignable to attribute `__cog_listener_names__` of type `(FuncT & ~staticmethod) | ((...) -> Unknown)`
- error[invalid-assignment] discord/file.py:106:9: Object of type `() -> Unknown` is not assignable to attribute `close` on type `BufferedIOBase | Unknown`
+ error[invalid-assignment] discord/file.py:106:9: Object of type `() -> Unknown` is not assignable to attribute `close` of type `BufferedIOBase | Unknown`
+ warning[unused-ignore-comment] discord/state.py:969:64: Unused blanket `type: ignore` directive
- Found 553 diagnostics
+ Found 554 diagnostics
-     struct metadata = ~8MB
+     struct metadata = ~9MB

kornia (https://github.com/kornia/kornia)
+ error[invalid-super-argument] kornia/augmentation/container/augment.py:515:15: `Unknown & <Protocol with members '_disable_features'>` is not an instance or subclass of `<class 'ImageSequential'>` in `super(<class 'ImageSequential'>, Unknown & <Protocol with members '_disable_features'>)` call
+ error[invalid-super-argument] kornia/augmentation/container/augment.py:540:29: `Unknown & <Protocol with members '_disable_features'>` is not an instance or subclass of `<class 'ImageSequential'>` in `super(<class 'ImageSequential'>, Unknown & <Protocol with members '_disable_features'>)` call
- warning[possibly-unbound-attribute] kornia/enhance/normalize.py:249:27: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] kornia/enhance/normalize.py:250:16: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] kornia/enhance/normalize.py:250:52: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] kornia/enhance/normalize.py:251:90: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] kornia/enhance/normalize.py:254:26: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] kornia/enhance/normalize.py:255:16: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] kornia/enhance/normalize.py:255:51: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
- warning[possibly-unbound-attribute] kornia/enhance/normalize.py:256:89: Attribute `shape` on type `(Unknown & ~float) | int | Unknown` is possibly unbound
- Found 800 diagnostics
+ Found 794 diagnostics
-     struct metadata = ~6MB
+     struct metadata = ~7MB

kopf (https://github.com/nolar/kopf)
-     struct fields = ~4MB
+     struct fields = ~5MB

pybind11 (https://github.com/pybind/pybind11)
- warning[possibly-unresolved-reference] tests/test_class_sh_trampoline_shared_from_this.py:213:20: Name `obj0_wr` used when possibly not defined
- warning[possibly-unresolved-reference] tests/test_class_sh_trampoline_shared_from_this.py:216:20: Name `obj0_wr` used when possibly not defined
- Found 212 diagnostics
+ Found 210 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
-     struct fields = ~6MB
+     struct fields = ~7MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
- error[invalid-assignment] pymongo/ssl_support.py:100:13: Object of type `bool` is not assignable to attribute `check_ocsp_endpoint` on type `SSLContext | (SSLContext & <Protocol with members 'check_ocsp_endpoint'>)`
+ error[invalid-assignment] pymongo/ssl_support.py:100:13: Object of type `bool` is not assignable to attribute `check_ocsp_endpoint` of type `SSLContext | (SSLContext & <Protocol with members 'check_ocsp_endpoint'>)`
- error[invalid-assignment] pymongo/ssl_support.py:119:13: Object of type `Any | Literal[0]` is not assignable to attribute `verify_flags` on type `SSLContext | SSLContext`
+ error[invalid-assignment] pymongo/ssl_support.py:119:13: Object of type `Any | Literal[0]` is not assignable to attribute `verify_flags` of type `SSLContext | SSLContext`
-     struct metadata = ~7MB
+     struct metadata = ~8MB
-     memo metadata = ~21MB
+     memo metadata = ~23MB

porcupine (https://github.com/Akuli/porcupine)
-     memo metadata = ~8MB
+     memo metadata = ~9MB

pydantic (https://github.com/pydantic/pydantic)
-     struct fields = ~7MB
+     struct fields = ~8MB

ignite (https://github.com/pytorch/ignite)
- error[invalid-assignment] examples/mnist/mnist_with_tqdm_logger.py:94:9: Object of type `Literal[0]` is not assignable to attribute `n` on type `Unknown | ProgressBar`
+ error[invalid-assignment] examples/mnist/mnist_with_tqdm_logger.py:94:9: Object of type `Literal[0]` is not assignable to attribute `n` of type `Unknown | ProgressBar`
- error[invalid-assignment] examples/mnist/mnist_with_tqdm_logger.py:94:18: Object of type `Literal[0]` is not assignable to attribute `last_print_n` on type `Unknown | ProgressBar`
+ error[invalid-assignment] examples/mnist/mnist_with_tqdm_logger.py:94:18: Object of type `Literal[0]` is not assignable to attribute `last_print_n` of type `Unknown | ProgressBar`
- error[invalid-assignment] examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:74:5: Object of type `Literal[0]` is not assignable to attribute `n` on type `Unknown | ProgressBar`
+ error[invalid-assignment] examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:74:5: Object of type `Literal[0]` is not assignable to attribute `n` of type `Unknown | ProgressBar`
- error[invalid-assignment] examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:74:14: Object of type `Literal[0]` is not assignable to attribute `last_print_n` on type `Unknown | ProgressBar`
+ error[invalid-assignment] examples/notebooks/HandlersTimeProfiler_MNIST.ipynb:74:14: Object of type `Literal[0]` is not assignable to attribute `last_print_n` of type `Unknown | ProgressBar`
- error[invalid-assignment] examples/notebooks/TextCNN.ipynb:221:5: Object of type `Literal[0]` is not assignable to attribute `n` on type `Unknown | ProgressBar`
+ error[invalid-assignment] examples/notebooks/TextCNN.ipynb:221:5: Object of type `Literal[0]` is not assignable to attribute `n` of type `Unknown | ProgressBar`
- error[invalid-assignment] examples/notebooks/TextCNN.ipynb:221:14: Object of type `Literal[0]` is not assignable to attribute `last_print_n` on type `Unknown | ProgressBar`
+ error[invalid-assignment] examples/notebooks/TextCNN.ipynb:221:14: Object of type `Literal[0]` is not assignable to attribute `last_print_n` of type `Unknown | ProgressBar`
- error[invalid-assignment] examples/reinforcement_learning/actor_critic.py:147:5: Object of type `Literal[10]` is not assignable to attribute `running_reward` on type `Unknown | State`
+ error[invalid-assignment] examples/reinforcement_learning/actor_critic.py:147:5: Object of type `Literal[10]` is not assignable to attribute `running_reward` of type `Unknown | State`
- error[invalid-assignment] examples/reinforcement_learning/actor_critic.py:154:9: Object of type `Literal[0]` is not assignable to attribute `ep_reward` on type `Unknown | State`
+ error[invalid-assignment] examples/reinforcement_learning/actor_critic.py:154:9: Object of type `Literal[0]` is not assignable to attribute `ep_reward` of type `Unknown | State`
- error[invalid-assignment] examples/reinforcement_learning/actor_critic.py:160:9: Object of type `Unknown` is not assignable to attribute `running_reward` on type `Unknown | State`
+ error[invalid-assignment] examples/reinforcement_learning/actor_critic.py:160:9: Object of type `Unknown` is not assignable to attribute `running_reward` of type `Unknown | State`
- error[invalid-assignment] examples/reinforcement_learning/reinforce.py:90:5: Object of type `Literal[10]` is not assignable to attribute `running_reward` on type `Unknown | State`
+ error[invalid-assignment] examples/reinforcement_learning/reinforce.py:90:5: Object of type `Literal[10]` is not assignable to attribute `running_reward` of type `Unknown | State`
- error[invalid-assignment] examples/reinforcement_learning/reinforce.py:96:9: Object of type `Literal[0]` is not assignable to attribute `ep_reward` on type `Unknown | State`
+ error[invalid-assignment] examples/reinforcement_learning/reinforce.py:96:9: Object of type `Literal[0]` is not assignable to attribute `ep_reward` of type `Unknown | State`
- error[invalid-assignment] examples/reinforcement_learning/reinforce.py:100:9: Object of type `Unknown` is not assignable to attribute `running_reward` on type `Unknown | State`
+ error[invalid-assignment] examples/reinforcement_learning/reinforce.py:100:9: Object of type `Unknown` is not assignable to attribute `running_reward` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/engine/test_engine.py:1210:9: Object of type `Unknown` is not assignable to attribute `dataiter` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/engine/test_engine.py:1210:9: Object of type `Unknown` is not assignable to attribute `dataiter` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/engine/test_engine_state_dict.py:188:9: Object of type `float` is not assignable to attribute `alpha` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/engine/test_engine_state_dict.py:188:9: Object of type `float` is not assignable to attribute `alpha` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/engine/test_engine_state_dict.py:247:9: Object of type `float` is not assignable to attribute `alpha` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/engine/test_engine_state_dict.py:247:9: Object of type `float` is not assignable to attribute `alpha` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/engine/test_engine_state_dict.py:248:9: Object of type `float` is not assignable to attribute `beta` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/engine/test_engine_state_dict.py:248:9: Object of type `float` is not assignable to attribute `beta` of type `Unknown | State`
- warning[possibly-unbound-attribute] tests/ignite/handlers/test_ema_handler.py:68:20: Attribute `ema_momentum` on type `Unknown | State` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/handlers/test_ema_handler.py:70:20: Attribute `ema_momentum` on type `Unknown | State` is possibly unbound
- warning[possibly-unbound-attribute] tests/ignite/handlers/test_ema_handler.py:74:36: Attribute `ema_momentum` on type `Unknown | State` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/handlers/test_ema_handler.py:68:20: Attribute `ema_momentum` on type `(Unknown & <Protocol with members 'iteration'>) | (State & <Protocol with members 'iteration'>)` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/handlers/test_ema_handler.py:70:20: Attribute `ema_momentum` on type `(Unknown & <Protocol with members 'iteration'>) | (State & <Protocol with members 'iteration'>)` is possibly unbound
+ warning[possibly-unbound-attribute] tests/ignite/handlers/test_ema_handler.py:74:36: Attribute `ema_momentum` on type `(Unknown & <Protocol with members 'iteration'>) | (State & <Protocol with members 'iteration'>)` is possibly unbound
- error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:79:5: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `output` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:79:5: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `output` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:81:5: Object of type `Literal["4.2"]` is not assignable to attribute `output` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:81:5: Object of type `Literal["4.2"]` is not assignable to attribute `output` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:83:5: Object of type `list[Unknown]` is not assignable to attribute `output` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:83:5: Object of type `list[Unknown]` is not assignable to attribute `output` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:85:5: Object of type `tuple[float, float]` is not assignable to attribute `output` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:85:5: Object of type `tuple[float, float]` is not assignable to attribute `output` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:101:5: Object of type `float` is not assignable to attribute `alpha` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:101:5: Object of type `float` is not assignable to attribute `alpha` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:102:5: Object of type `Unknown` is not assignable to attribute `beta` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:102:5: Object of type `Unknown` is not assignable to attribute `beta` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:103:5: Object of type `Unknown` is not assignable to attribute `gamma` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_fbresearch_logger.py:103:5: Object of type `Unknown` is not assignable to attribute `gamma` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_lr_finder.py:596:5: Object of type `int | float` is not assignable to attribute `output` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_lr_finder.py:596:5: Object of type `int | float` is not assignable to attribute `output` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_param_scheduler.py:491:9: Object of type `None` is not assignable to attribute `param_history` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_param_scheduler.py:491:9: Object of type `None` is not assignable to attribute `param_history` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_param_scheduler.py:562:9: Object of type `None` is not assignable to attribute `param_history` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_param_scheduler.py:562:9: Object of type `None` is not assignable to attribute `param_history` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_param_scheduler.py:635:9: Object of type `None` is not assignable to attribute `param_history` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_param_scheduler.py:635:9: Object of type `None` is not assignable to attribute `param_history` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_param_scheduler.py:1124:13: Object of type `None` is not assignable to attribute `param_history` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_param_scheduler.py:1124:13: Object of type `None` is not assignable to attribute `param_history` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_time_limit.py:28:9: Object of type `Literal[True]` is not assignable to attribute `is_terminated` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_time_limit.py:28:9: Object of type `Literal[True]` is not assignable to attribute `is_terminated` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_time_limit.py:31:5: Object of type `Literal[False]` is not assignable to attribute `is_terminated` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_time_limit.py:31:5: Object of type `Literal[False]` is not assignable to attribute `is_terminated` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_tqdm_logger.py:217:5: Object of type `float` is not assignable to attribute `alpha` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_tqdm_logger.py:217:5: Object of type `float` is not assignable to attribute `alpha` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_tqdm_logger.py:218:5: Object of type `Unknown` is not assignable to attribute `beta` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_tqdm_logger.py:218:5: Object of type `Unknown` is not assignable to attribute `beta` of type `Unknown | State`
- error[invalid-assignment] tests/ignite/handlers/test_tqdm_logger.py:219:5: Object of type `Unknown` is not assignable to attribute `gamma` on type `Unknown | State`
+ error[invalid-assignment] tests/ignite/handlers/test_tqdm_logger.py:219:5: Object of type `Unknown` is not assignable to attribute `gamma` of type `Unknown | State`
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB

pylox (https://github.com/sco1/pylox)
-     memo fields = ~49MB
+     memo fields = ~54MB

schemathesis (https://github.com/schemathesis/schemathesis)
-     memo metadata = ~14MB
+     memo metadata = ~15MB

aiohttp-devtools (https://github.com/aio-libs/aiohttp-devtools)
-     memo fields = ~88MB
+     memo fields = ~97MB

optuna (https://github.com/optuna/optuna)
- error[invalid-argument-type] optuna/_gp/search_space.py:148:17: Argument to function `normalize_one_param` is incorrect: Expected `ScaleType`, found `ndarray[Unknown, Unknown]`
+ error[invalid-argument-type] optuna/_gp/search_space.py:148:17: Argument to function `normalize_one_param` is incorrect: Expected `ScaleType`, found `ndarray[Unknown, <class 'signedinteger[_64Bit]'>]`
- error[invalid-argument-type] optuna/_gp/search_space.py:150:17: Argument to function `normalize_one_param` is incorrect: Expected `int | float`, found `ndarray[Unknown, Unknown]`
+ error[invalid-argument-type] optuna/_gp/search_space.py:150:17: Argument to function `normalize_one_param` is incorrect: Expected `int | float`, found `ndarray[Unknown, <class 'float64'>]`
- error[invalid-assignment] optuna/storages/_rdb/storage.py:460:17: Object of type `Unknown | Column[Unknown]` is not assignable to attribute `number` on type `FrozenTrial & ~AlwaysFalsy`
+ error[invalid-assignment] optuna/storages/_rdb/storage.py:460:17: Object of type `Unknown | Column[Unknown]` is not assignable to attribute `number` of type `FrozenTrial & ~AlwaysFalsy`
- error[invalid-assignment] optuna/storages/_rdb/storage.py:461:17: Object of type `Unknown | Column[Unknown]` is not assignable to attribute `datetime_start` on type `FrozenTrial & ~AlwaysFalsy`
+ error[invalid-assignment] optuna/storages/_rdb/storage.py:461:17: Object of type `Unknown | Column[Unknown]` is not assignable to attribute `datetime_start` of type `FrozenTrial & ~AlwaysFalsy`
-     memo metadata = ~28MB
+     memo metadata = ~30MB

cloud-init (https://github.com/canonical/cloud-init)
- error[invalid-assignment] tests/unittests/runs/test_merge_run.py:54:9: Object of type `Unknown` is not assignable to attribute `userdata_raw` on type `DataSource | None`
+ error[invalid-assignment] tests/unittests/runs/test_merge_run.py:54:9: Object of type `Unknown` is not assignable to attribute `userdata_raw` of type `DataSource | None`
- error[invalid-assignment] tests/unittests/sources/test_azure.py:1248:9: Object of type `Mock` is not assignable to attribute `get_tmp_exec_path` on type `(Unknown & ~str) | Distro`
+ error[invalid-assignment] tests/unittests/sources/test_azure.py:1248:9: Object of type `Mock` is not assignable to attribute `get_tmp_exec_path` of type `(Unknown & ~str) | Distro`
-     struct fields = ~19MB
+     struct fields = ~21MB

pwndbg (https://github.com/pwndbg/pwndbg)
- warning[possibly-unbound-attribute] pwndbg/aglib/regs.py:252:17: Attribute `ptid` on type `Thread | None` is possibly unbound
- error[unresolved-attribute] pwndbg/aglib/regs.py:263:50: Type `<module 'pwndbg.aglib.arch'>` has no attribute `ptrmask`
- error[unresolved-attribute] pwndbg/aglib/tls.py:81:10: Type `<module 'pwndbg.aglib.arch'>` has no attribute `name`
- error[unresolved-attribute] pwndbg/aglib/tls.py:83:10: Type `<module 'pwndbg.aglib.arch'>` has no attribute `name`
- error[unresolved-attribute] pwndbg/aglib/tls.py:85:20: Type `<module 'pwndbg.aglib.regs'>` has no attribute `tpidr`
- error[unresolved-attribute] pwndbg/aglib/tls.py:85:47: Type `<module 'pwndbg.aglib.regs'>` has no attribute `TPIDR_EL0`
- error[unresolved-attribute] pwndbg/aglib/tls.py:86:10: Type `<module 'pwndbg.aglib.arch'>` has no attribute `name`
- error[unresolved-attribute] pwndbg/aglib/tls.py:92:20: Type `<module 'pwndbg.aglib.regs'>` has no attribute `tpidruro`
- error[unresolved-attribute] pwndbg/aglib/tls.py:93:10: Type `<module 'pwndbg.aglib.arch'>` has no attribute `name`
- error[unresolved-attribute] pwndbg/aglib/tls.py:94:20: Type `<module 'pwndbg.aglib.regs'>` has no attribute `tp`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:59:5: Object of type `Type` is not assignable to attribute `char` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:59:5: Object of type `Type` is not assignable to attribute `char` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:60:5: Object of type `Type` is not assignable to attribute `ulong` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:60:5: Object of type `Type` is not assignable to attribute `ulong` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:61:5: Object of type `Type` is not assignable to attribute `long` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:61:5: Object of type `Type` is not assignable to attribute `long` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:62:5: Object of type `Type` is not assignable to attribute `uchar` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:62:5: Object of type `Type` is not assignable to attribute `uchar` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:63:5: Object of type `Type` is not assignable to attribute `ushort` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:63:5: Object of type `Type` is not assignable to attribute `ushort` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:64:5: Object of type `Type` is not assignable to attribute `uint` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:64:5: Object of type `Type` is not assignable to attribute `uint` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:67:5: Object of type `Type` is not assignable to attribute `sint` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:67:5: Object of type `Type` is not assignable to attribute `sint` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:68:5: Object of type `Type` is not assignable to attribute `void` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:68:5: Object of type `Type` is not assignable to attribute `void` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:70:5: Object of type `Unknown` is not assignable to attribute `uint8` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:70:5: Object of type `Unknown` is not assignable to attribute `uint8` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:71:5: Object of type `Unknown` is not assignable to attribute `uint16` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:71:5: Object of type `Unknown` is not assignable to attribute `uint16` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:72:5: Object of type `Unknown` is not assignable to attribute `uint32` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:72:5: Object of type `Unknown` is not assignable to attribute `uint32` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:73:5: Object of type `Type` is not assignable to attribute `uint64` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:73:5: Object of type `Type` is not assignable to attribute `uint64` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:74:5: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `unsigned` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:74:5: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `unsigned` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:81:5: Object of type `Type` is not assignable to attribute `int8` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:81:5: Object of type `Type` is not assignable to attribute `int8` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:82:5: Object of type `Type` is not assignable to attribute `int16` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:82:5: Object of type `Type` is not assignable to attribute `int16` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:83:5: Object of type `Type` is not assignable to attribute `int32` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:83:5: Object of type `Type` is not assignable to attribute `int32` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:84:5: Object of type `Type` is not assignable to attribute `int64` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:84:5: Object of type `Type` is not assignable to attribute `int64` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:85:5: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `signed` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:85:5: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `signed` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:87:5: Object of type `Type` is not assignable to attribute `pvoid` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:87:5: Object of type `Type` is not assignable to attribute `pvoid` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:88:5: Object of type `Type` is not assignable to attribute `ppvoid` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:88:5: Object of type `Type` is not assignable to attribute `ppvoid` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:89:5: Object of type `Type` is not assignable to attribute `pchar` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:89:5: Object of type `Type` is not assignable to attribute `pchar` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:91:5: Object of type `int` is not assignable to attribute `ptrsize` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:91:5: Object of type `int` is not assignable to attribute `ptrsize` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:94:9: Object of type `Unknown` is not assignable to attribute `ptrdiff` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:94:9: Object of type `Unknown` is not assignable to attribute `ptrdiff` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:95:9: Object of type `Unknown` is not assignable to attribute `size_t` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:95:9: Object of type `Unknown` is not assignable to attribute `size_t` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:96:9: Object of type `Unknown` is not assignable to attribute `ssize_t` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:96:9: Object of type `Unknown` is not assignable to attribute `ssize_t` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:98:9: Object of type `Unknown` is not assignable to attribute `ptrdiff` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:98:9: Object of type `Unknown` is not assignable to attribute `ptrdiff` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:99:9: Object of type `Unknown` is not assignable to attribute `size_t` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:99:9: Object of type `Unknown` is not assignable to attribute `size_t` of type `Unknown | ModuleType`
- error[invalid-assignment] pwndbg/aglib/typeinfo.py:100:9: Object of type `Unknown` is not assignable to attribute `ssize_t` on type `Unknown | ModuleType`
+ error[invalid-assignment] pwndbg/aglib/typeinfo.py:100:9: Object of type `Unknown` is not assignable to attribute `ssize_t` of type `Unknown | ModuleType`
- error[unresolved-attribute] pwndbg/auxv.py:78:10: Type `<module 'pwndbg.aglib.arch'>` has no attribute `ptrsize`
- error[unresolved-attribute] pwndbg/commands/elf.py:84:24: Type `<module 'pwndbg.aglib'>` has no attribute `vmmap`
- error[no-matching-overload] pwndbg/commands/got_tracking.py:149:12: No overload of bound method `match` matches arguments
- error[unresolved-attribute] pwndbg/commands/hexdump.py:75:19: Type `CommandObj` has no attribute `last_address`
+ error[unresolved-attribute] pwndbg/commands/hexdump.py:75:19: Type `CommandObj & <Protocol with members 'repeat'>` has no attribute `last_address`
- error[unresolved-attribute] pwndbg/commands/hexdump.py:77:9: Unresolved attribute `offset` on type `CommandObj`.
+ error[invalid-assignment] pwndbg/commands/hexdump.py:77:9: Object of type `Literal[0]` is not assignable to attribute `offset` of type `CommandObj & <Protocol with members 'repeat'>`
- error[unresolved-attribute] pwndbg/commands/hexdump.py:115:62: Type `<module 'pwndbg.aglib.arch'>` has no attribute `endian`
+ warning[unused-ignore-comment] pwndbg/commands/ida.py:149:99: Unused blanket `type: ignore` directive
- error[unresolved-attribute] pwndbg/commands/ida.py:148:14: Type `<module 'pwndbg.dbg'>` has no attribute `selected_frame`
- error[invalid-argument-type] pwndbg/commands/ida.py:161:20: Argument to function `__new__` is incorrect: Expected `SupportsIndex`, found `Unknown | None`
- error[non-subscriptable] pwndbg/commands/ida.py:172:20: Cannot subscript object of type `<module 'pwndbg.aglib.regs'>` with no `__getitem__` method
- error[non-subscriptable] pwndbg/commands/ida.py:173:16: Cannot subscript object of type `<module 'pwndbg.aglib.regs'>` with no `__getitem__` method
- error[unresolved-attribute] pwndbg/commands/procinfo.py:276:33: Type `Process` has no attribute `ppid`
+ error[unresolved-attribute] pwndbg/commands/procinfo.py:276:33: Type `Process & <Protocol with members 'status'>` has no attribute `ppid`
- error[unresolved-attribute] pwndbg/commands/procinfo.py:278:32: Type `Process` has no attribute `uid`
+ error[unresolved-attribute] pwndbg/commands/procinfo.py:278:32: Type `Process & <Protocol with members 'status'>` has no attribute `uid`
- error[unresolved-attribute] pwndbg/commands/procinfo.py:279:32: Type `Process` has no attribute `gid`
+ error[unresolved-attribute] pwndbg/commands/procinfo.py:279:32: Type `Process & <Protocol with members 'status'>` has no attribute `gid`
- error[unresolved-attribute] pwndbg/commands/procinfo.py:280:35: Type `Process` has no attribute `groups`
+ error[unresolved-attribute] pwndbg/commands/procinfo.py:280:35: Type `Process & <Protocol with members 'status'>` has no attribute `groups`
- warning[possibly-unbound-attribute] pwndbg/commands/radare2.py:52:25: Attribute `address` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] pwndbg/commands/radare2.py:54:41: Attribute `address` on type `Unknown | None` is possibly unbound
- error[invalid-argument-type] pwndbg/commands/radare2.py:56:35: Argument to function `hex` is incorrect: Expected `SupportsIndex`, found `int | None | Unknown`
- warning[possibly-unbound-attribute] pwndbg/commands/rizin.py:50:25: Attribute `address` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] pwndbg/commands/rizin.py:52:41: Attribute `address` on type `Unknown | None` is possibly unbound
- error[invalid-argument-type] pwndbg/commands/rizin.py:54:35: Argument to function `hex` is incorrect: Expected `SupportsIndex`, found `int | None | Unknown`
- warning[possibly-unbound-attribute] pwndbg/commands/rop.py:170:17: Attribute `vmmap` on type `Process | None` is possibly unbound
- warning[possibly-unbound-attribute] pwndbg/commands/rop.py:193:32: Attribute `read_memory` on type `Process | None` is possibly unbound
- error[unresolved-attribute] pwndbg/commands/telescope.py:115:19: Type `CommandObj` has no attribute `last_address`
+ error[unresolved-attribute] pwndbg/commands/telescope.py:115:19: Type `CommandObj & <Protocol with members 'repeat'>` has no attribute `last_address`
- error[unresolved-attribute] pwndbg/commands/telescope.py:116:9: Type `CommandObj` has no attribute `offset`
+ error[unresolved-attribute] pwndbg/commands/telescope.py:116:9: Type `CommandObj & <Protocol with members 'repeat'>` has no attribute `offset`
- error[unresolved-attribute] pwndbg/commands/telescope.py:118:9: Unresolved attribute `offset` on type `CommandObj`.
+ error[invalid-assignment] pwndbg/commands/telescope.py:118:9: Object of type `Literal[0]` is not assignable to attribute `offset` of type `CommandObj & <Protocol with members 'repeat'>`
- error[non-subscriptable] pwndbg/ghidra.py:70:17: Cannot subscript object of type `<module 'pwndbg.aglib.regs'>` with no `__getitem__` method
- error[non-subscriptable] pwndbg/ghidra.py:84:14: Cannot subscript object of type `<module 'pwndbg.aglib.regs'>` with no `__getitem__` method
- error[unresolved-attribute] pwndbg/ghidra.py:108:12: Type `<module 'pwndbg.dbg'>` has no attribute `is_gdblib_available`
- error[unresolved-attribute] pwndbg/ghidra.py:111:24: Type `<module 'pwndbg.dbg'>` has no attribute `selected_inferior`
- error[invalid-argument-type] pwndbg/integration/binja.py:217:27: Argument to function `l2r` is incorrect: Expected `int`, found `int | None`
- warning[possibly-unbound-attribute] pwndbg/integration/binja.py:238:14: Attribute `break_at` on type `Process | None` is possibly unbound
- Found 2254 diagnostics
+ Found 2223 diagnostics
-     struct metadata = ~7MB
+     struct metadata = ~8MB
-     struct fields = ~8MB
+     struct fields = ~9MB
-     memo metadata = ~21MB
+     memo metadata = ~23MB

mkosi (https://github.com/systemd/mkosi)
-     struct metadata = ~3MB
+     struct metadata = ~4MB
-     struct fields = ~5MB
+     struct fields = ~6MB
-     memo metadata = ~9MB
+     memo metadata = ~10MB

poetry (https://github.com/python-poetry/poetry)
-     struct metadata = ~7MB
+     struct metadata = ~8MB
-     struct fields = ~9MB
+     struct fields = ~10MB

vision (https://github.com/pytorch/vision)
- warning[possibly-unbound-attribute] test/smoke_test.py:73:60: Attribute `shape` on type `Unknown | list[Unknown]` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:31:23: Attribute `FFmpegError` on type `Unknown | ImportError` is possibly unbound
+ warning[possibly-unbound-attribute] torchvision/io/video.py:31:23: Attribute `FFmpegError` on type `(Unknown & <Protocol with members 'video'>) | ImportError` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:33:23: Attribute `AVError` on type `Unknown | ImportError` is possibly unbound
+ warning[possibly-unbound-attribute] torchvision/io/video.py:33:23: Attribute `AVError` on type `(Unknown & <Protocol with members 'video'>) | ImportError` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:180:17: Attribute `container` on type `Unknown | ImportError` is possibly unbound
+ warning[possibly-unbound-attribute] torchvision/io/video.py:180:17: Attribute `container` on type `(Unknown & <Protocol with members 'video'>) | ImportError` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:184:14: Attribute `stream` on type `Unknown | ImportError` is possibly unbound
+ warning[possibly-unbound-attribute] torchvision/io/video.py:184:14: Attribute `stream` on type `(Unknown & <Protocol with members 'video'>) | ImportError` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:186:12: Attribute `frame` on type `Unknown | ImportError` is possibly unbound
+ warning[possibly-unbound-attribute] torchvision/io/video.py:186:12: Attribute `frame` on type `(Unknown & <Protocol with members 'video'>) | ImportError` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:260:48: Attribute `frame` on type `Unknown | ImportError` is possibly unbound
+ warning[possibly-unbound-attribute] torchvision/io/video.py:260:48: Attribute `frame` on type `(Unknown & <Protocol with members 'video'>) | ImportError` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:395:51: Attribute `container` on type `Unknown | ImportError` is possibly unbound
+ warning[possibly-unbound-attribute] torchvision/io/video.py:395:51: Attribute `container` on type `(Unknown & <Protocol with members 'video'>) | ImportError` is possibly unbound
- warning[possibly-unbound-attribute] torchvision/io/video.py:404:42: Attribute `container` on type `Unknown | ImportError` is possibly unbound
+ warning[possibly-unbound-attribute] torchvision/io/video.py:404:42: Attribute `container` on type `(Unknown & <Protocol with members 'video'>) | ImportError` is possibly unbound
- Found 1474 diagnostics
+ Found 1473 diagnostics
-     memo metadata = ~37MB
+     memo metadata = ~41MB

flake8 (https://github.com/pycqa/flake8)
-     struct fields = ~3MB
+     struct fields = ~4MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     struct metadata = ~2MB
+     struct metadata = ~3MB

mkdocs (https://github.com/mkdocs/mkdocs)
- error[invalid-assignment] mkdocs/tests/build_tests.py:398:9: Object of type `Literal["Title"]` is not assignable to attribute `title` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:398:9: Object of type `Literal["Title"]` is not assignable to attribute `title` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:399:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:399:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:400:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:400:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:432:9: Object of type `Literal["Title"]` is not assignable to attribute `title` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:432:9: Object of type `Literal["Title"]` is not assignable to attribute `title` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:433:9: Object of type `Literal["new page content"]` is not assignable to attribute `markdown` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:433:9: Object of type `Literal["new page content"]` is not assignable to attribute `markdown` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:434:9: Object of type `Literal["<p>new page content</p>"]` is not assignable to attribute `content` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:434:9: Object of type `Literal["<p>new page content</p>"]` is not assignable to attribute `content` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:449:9: Object of type `Literal["Title"]` is not assignable to attribute `title` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:449:9: Object of type `Literal["Title"]` is not assignable to attribute `title` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:450:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:450:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:451:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:451:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:465:9: Object of type `Literal["Title"]` is not assignable to attribute `title` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:465:9: Object of type `Literal["Title"]` is not assignable to attribute `title` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:466:9: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `meta` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:466:9: Object of type `dict[Unknown, Unknown]` is not assignable to attribute `meta` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:467:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:467:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:468:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:468:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:481:9: Object of type `Literal["Title"]` is not assignable to attribute `title` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:481:9: Object of type `Literal["Title"]` is not assignable to attribute `title` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:482:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:482:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:483:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:483:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:505:9: Object of type `Literal["Title"]` is not assignable to attribute `title` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:505:9: Object of type `Literal["Title"]` is not assignable to attribute `title` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:506:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:506:9: Object of type `Literal["page content"]` is not assignable to attribute `markdown` of type `Page | None`
- error[invalid-assignment] mkdocs/tests/build_tests.py:507:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` on type `Page | None`
+ error[invalid-assignment] mkdocs/tests/build_tests.py:507:9: Object of type `Literal["<p>page content</p>"]` is not assignable to attribute `content` of type `Page | None`

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[invalid-assignment] tests/test_psql.py:40:9: Object of type `Literal[0]` is not assignable to attribute `closed` on type `Unknown | None`
+ error[invalid-assignment] tests/test_psql.py:40:9: Object of type `Literal[0]` is not assignable to attribute `closed` of type `Unknown | None`
- error[invalid-assignment] tests/test_psql.py:50:9: Object of type `Literal[2]` is not assignable to attribute `closed` on type `Unknown | None`
+ error[invalid-assignment] tests/test_psql.py:50:9: Object of type `Literal[2]` is not assignable to attribute `closed` of type `Unknown | None`
- error[invalid-assignment] tests/test_psql.py:83:9: Object of type `Literal[0]` is not assignable to attribute `closed` on type `Unknown | None`
+ error[invalid-assignment] tests/test_psql.py:83:9: Object of type `Literal[0]` is not assignable to attribute `closed` of type `Unknown | None`

apprise (https://github.com/caronc/apprise)
- error[unresolved-attribute] test/test_api.py:2032:12: Type `ConfigBase` has no attribute `asset`
- error[unresolved-attribute] test/test_api.py:2033:12: Type `ConfigBase` has no attribute `asset`
- error[unresolved-attribute] test/test_api.py:2036:5: Type `ConfigBase` has no attribute `asset`
- error[unresolved-attribute] test/test_api.py:2037:12: Type `ConfigBase` has no attribute `asset`
- error[unresolved-attribute] test/test_api.py:2040:12: Type `ConfigBase` has no attribute `asset`
- warning[possibly-unbound-attribute] test/test_attach_http.py:422:5: Attribute `detected_name` on type `AttachHTTP` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_attach_http.py:422:5: Attribute `detected_name` on type `AttachHTTP & ~AlwaysFalsy` is possibly unbound
- warning[possibly-unbound-attribute] test/test_plugin_email.py:496:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:496:12: Attribute `port` on type `NotifyEmail & <Protocol with members 'secure'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1036:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1036:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'reply_to'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1062:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1062:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'reply_to'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1088:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1088:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'reply_to'>` has no attribute `notify`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1110:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1110:12: Attribute `user` on type `NotifyEmail & <Protocol with members 'smtp_host'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1111:12: Type `NotifyEmail` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:1111:12: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `password`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1113:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1113:12: Attribute `port` on type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1118:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1118:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` has no attribute `notify`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1138:25: Attribute `user` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1138:25: Attribute `user` on type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1139:29: Type `NotifyEmail` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:1139:29: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` has no attribute `password`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1141:25: Attribute `port` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1141:25: Attribute `port` on type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` is possibly unbound
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1166:12: Attribute `user` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1166:12: Attribute `user` on type `NotifyEmail & <Protocol with members 'smtp_host'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1167:12: Type `NotifyEmail` has no attribute `password`
+ error[unresolved-attribute] test/test_plugin_email.py:1167:12: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `password`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1169:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1169:12: Attribute `port` on type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1174:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1174:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1199:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1199:12: Type `NotifyEmail & <Protocol with members 'reply_to'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1224:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1224:12: Type `NotifyEmail & <Protocol with members 'reply_to'>` has no attribute `notify`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1250:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1250:12: Attribute `port` on type `NotifyEmail & <Protocol with members 'smtp_host'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1251:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1251:32: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `app_id`
- error[unresolved-attribute] test/test_plugin_email.py:1271:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1271:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'> & <Protocol with members 'reply_to'>` has no attribute `notify`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1294:12: Attribute `port` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1294:12: Attribute `port` on type `NotifyEmail & <Protocol with members 'smtp_host'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1295:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1295:32: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `app_id`
- error[unresolved-attribute] test/test_plugin_email.py:1304:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1304:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'> & <Protocol with members 'reply_to'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1328:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1328:32: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `app_id`
- error[unresolved-attribute] test/test_plugin_email.py:1339:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1339:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1363:32: Type `NotifyEmail` has no attribute `app_id`
+ error[unresolved-attribute] test/test_plugin_email.py:1363:32: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `app_id`
- error[unresolved-attribute] test/test_plugin_email.py:1374:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1374:12: Type `NotifyEmail & <Protocol with members 'smtp_host'> & <Protocol with members 'secure_mode'>` has no attribute `notify`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1462:12: Attribute `host` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1462:12: Attribute `host` on type `NotifyEmail & <Protocol with members 'smtp_host'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1468:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1468:12: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `notify`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1513:12: Attribute `host` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1513:12: Attribute `host` on type `NotifyEmail & <Protocol with members 'smtp_host'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1519:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1519:12: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `notify`
- warning[possibly-unbound-attribute] test/test_plugin_email.py:1629:12: Attribute `host` on type `NotifyEmail` is possibly unbound
+ warning[possibly-unbound-attribute] test/test_plugin_email.py:1629:12: Attribute `host` on type `NotifyEmail & <Protocol with members 'smtp_host'>` is possibly unbound
- error[unresolved-attribute] test/test_plugin_email.py:1633:12: Type `NotifyEmail` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1633:12: Type `NotifyEmail & <Protocol with members 'smtp_host'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1865:12: Type `NotifyBase` has no attribute `port`
+ error[unresolved-attribute] test/test_plugin_email.py:1865:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `port`
- error[unresolved-attribute] test/test_plugin_email.py:1866:12: Type `NotifyBase` has no attribute `smtp_host`
+ error[unresolved-attribute] test/test_plugin_email.py:1866:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `smtp_host`
- error[unresolved-attribute] test/test_plugin_email.py:1870:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1870:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1918:12: Type `NotifyBase` has no attribute `port`
+ error[unresolved-attribute] test/test_plugin_email.py:1918:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `port`
- error[unresolved-attribute] test/test_plugin_email.py:1919:12: Type `NotifyBase` has no attribute `smtp_host`
+ error[unresolved-attribute] test/test_plugin_email.py:1919:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `smtp_host`
- error[unresolved-attribute] test/test_plugin_email.py:1923:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1923:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:1993:12: Type `NotifyBase` has no attribute `port`
+ error[unresolved-attribute] test/test_plugin_email.py:1993:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `port`
- error[unresolved-attribute] test/test_plugin_email.py:1994:12: Type `NotifyBase` has no attribute `smtp_host`
+ error[unresolved-attribute] test/test_plugin_email.py:1994:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `smtp_host`
- error[unresolved-attribute] test/test_plugin_email.py:1998:12: Type `NotifyBase` has no attribute `notify`
+ error[unresolved-attribute] test/test_plugin_email.py:1998:12: Type `NotifyBase & <Protocol with members 'secure'>` has no attribute `notify`
- error[unresolved-attribute] test/test_plugin_email.py:2079:5: Type `<module 'apprise.utils'>` has no attribute `pgp`
- error[unresolved-attribute] test/test_plugin_email.py:2087:5: Type `<module 'apprise.utils'>` has no attribute `pgp`
- error[unresolved-attribute] test/test_plugin_email.py:2186:12: Type `NotifyBase` has no attribute `pgp`
+ error[unresolved-attribute] test/test_plugin_email.py:2186:12: Type `<Protocol with members 'pub_keyfile'>` has no attribute `public_keyfile`
- error[unresolved-attribute] test/test_plugin_email.py:2449:5: Type `<module 'apprise.utils'>` has no attribu...*[Comment body truncated]*

---

_Comment by @codspeed-hq[bot] on 2025-07-01 06:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A643-adv-attr-narrowing?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #19064 will **degrade performances by 11.83%**

<sub>Comparing <code>mtshiba:643-adv-attr-narrowing</code> (c893c5d) with <code>main</code> (b8653a9)</sub>



### Summary

`❌ 3` regressions  
`✅ 5` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A643-adv-attr-narrowing?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | WallTime | [`` large[sympy] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A643-adv-attr-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Alarge%5Bsympy%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 53.4 s | 60.5 s | -11.83% |
| ❌ | WallTime | [`` small[pydantic] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A643-adv-attr-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Bpydantic%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2.9 s | 3 s | -4.71% |
| ❌ | WallTime | [`` small[tanjun] ``](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A643-adv-attr-narrowing?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Asmall%5Btanjun%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 2 s | 2.1 s | -4.14% |


---

_Comment by @codspeed-hq[bot] on 2025-07-01 06:06_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A643-adv-attr-narrowing?runnerMode=WallTime)

### Merging #19064 will **degrade performances by 11.83%**

<sub>Comparing <code>mtshiba:643-adv-attr-narrowing</code> (c893c5d) with <code>main</code> (b8653a9)</sub>



### Summary

`❌ 3` regressions  
`✅ 4` untouched benchmarks  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/mtshiba%3A643-adv-attr-narrowing?runnerMode=WallTime)._

### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `` large[sympy] `` | 53.4 s | 60.5 s | -11.83% |
| ❌ | `` small[pydantic] `` | 2.9 s | 3 s | -4.71% |
| ❌ | `` small[tanjun] `` | 2 s | 2.1 s | -4.14% |


---

_Label `ty` added by @AlexWaygood on 2025-07-01 12:13_

---
