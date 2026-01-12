```yaml
number: 18751
title: "Reapply \"[ty] Offer \"Did you mean...?\" suggestions for unresolved `from` imports and unresolved attributes (#18705)\""
type: pull_request
state: open
author: AlexWaygood
labels:
  - ty
  - diagnostics
assignees: []
base: main
head: alex/reapply-levenshtein-2
created_at: 2025-06-18T13:42:40Z
updated_at: 2025-10-29T15:05:28Z
url: https://github.com/astral-sh/ruff/pull/18751
synced_at: 2026-01-12T15:56:25Z
```

# Reapply "[ty] Offer "Did you mean...?" suggestions for unresolved `from` imports and unresolved attributes (#18705)"

---

_@AlexWaygood_

## Summary

This PR reapplies https://github.com/astral-sh/ruff/pull/18705, which was previously reverted in https://github.com/astral-sh/ruff/pull/18721.

#18705 was previously reverted because the original implementation of the feature caused catastrophic execution time on dd-trace-py, which meant that mypy_primer runs timed out on all PRs (they wouldn't finish within 20 minutes). The reason for the catastrophic execution time was cycles such as the following scenario:

1. We start analyzing the global scope of a file `foo/__init__.py`
2. As part of that analysis, we attempt to infer the type of the binding for a `from . import bar` import (`infer_import_from_definition`)
3. We resolve the `.` to `foo/__init__.py`, realize `bar` is an unknown symbol/submodule, and call `all_members(<module 'foo'>)` to calculate suggestions for the diagnostic
4. `all_members` tries to infer the types of all symbols in the global scope of `foo/__init__.py` due to [this call](https://github.com/astral-sh/ruff/blob/37fdece72f7a5bdb24bbd56aa20a43ee4d142001/crates/ty_python_semantic/src/types/ide_support.rs#L141-L147)... and we're back at step (1)

The cycles described above eventually resolved themselves due to our fixpoint iteration, but it led to a catastrophic execution time if it was a module with several `from . import <SUBMODULE>` imports in it. Similar cycles could also manifest _between_ `foo/__init__.py` files and `foo`-package submodules:

1. We infer global types for a file `foo/whatever.py`
2. That involves analyzing an unresolved import in the submodule that looks like `from . import bar`
3. To compute suggestions for that unresolved import, we call `all_members(module <'foo'>)`, which triggers us trying to infer global types for `foo/__init__.py`
4. `foo/__init__.py` has an import `from .whatever import something_else`, which is also unresolved. To compute suggestions for that unresolved import, we infer global types for `foo/whatever.py`... and we're back at step (1)

To avoid the catastrophic execution time when creating diagnostics for these cyclic imports, this PR refrains from attempting to compute suggestions for an `unresolved-import` diagnostic if the import is in the global scope and the import refers to module that is an ancestor or child of the module represented by the file currently being analyzed. This is implemented in https://github.com/astral-sh/ruff/pull/18751/commits/79f3c8cf6738dc297e13d56d81d496e604a43b8c.

## Test Plan

- All the original tests from #18705 have been added again
- Mypy_primer now definitely completed on this PR ([primer report](https://github.com/astral-sh/ruff/pull/18751#issuecomment-2984288829)) and the suggestions look good to me ([primer analysis](https://github.com/astral-sh/ruff/pull/18751#issuecomment-2984357296))
- I confirmed locally that running this branch on dd-trace-py now completes within a reasonable execution time

There is a performance regression on the hydra-zen benchmark. My analysis of this regression is in https://github.com/astral-sh/ruff/pull/18751#issuecomment-2985230500. I think it's unavoidable if we want this feature, unfortunately.

---

_Label `ty` added by @AlexWaygood on 2025-06-18 13:42_

---

_Comment by @github-actions[bot] on 2025-06-18 13:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
- error[unresolved-attribute] dacite/types.py:17:16: Type `type` has no attribute `__extra__`
+ error[unresolved-attribute] dacite/types.py:17:16: Type `type` has no attribute `__extra__`: Did you mean `__str__`?
- error[unresolved-attribute] dacite/types.py:108:12: Type `type` has no attribute `__args__`
+ error[unresolved-attribute] dacite/types.py:108:12: Type `type` has no attribute `__args__`: Did you mean `__bases__`?
- error[unresolved-attribute] dacite/types.py:109:21: Type `type` has no attribute `__args__`
+ error[unresolved-attribute] dacite/types.py:109:21: Type `type` has no attribute `__args__`: Did you mean `__bases__`?

zipp (https://github.com/jaraco/zipp)
- error[unresolved-attribute] zipp/__init__.py:203:31: Type `<module 'sys'>` has no attribute `pypy_version_info`
+ error[unresolved-attribute] zipp/__init__.py:203:31: Type `<module 'sys'>` has no attribute `pypy_version_info`: Did you mean `version_info`?

beartype (https://github.com/beartype/beartype)
- error[unresolved-import] beartype/typing/__init__.py:226:9: Module `typing` has no member `TypeGuard`
+ error[unresolved-import] beartype/typing/__init__.py:226:9: Module `typing` has no member `TypeGuard`: Did you mean `TypeVar`?
- error[unresolved-import] beartype/typing/__init__.py:227:9: Module `typing` has no member `is_typeddict`
+ error[unresolved-import] beartype/typing/__init__.py:227:9: Module `typing` has no member `is_typeddict`: Did you mean `TypedDict`?
- error[unresolved-import] beartype/typing/__init__.py:260:21: Module `typing` has no member `TypeIs`
+ error[unresolved-import] beartype/typing/__init__.py:260:21: Module `typing` has no member `TypeIs`: Did you mean `Type`?
- error[unresolved-import] beartype/typing/__init__.py:262:21: Module `typing` has no member `is_protocol`
+ error[unresolved-import] beartype/typing/__init__.py:262:21: Module `typing` has no member `is_protocol`: Did you mean `Protocol`?

pyinstrument (https://github.com/joerick/pyinstrument)
- error[unresolved-import] pyinstrument/vendor/decorator.py:279:28: Module `contextlib` has no member `GeneratorContextManager`
+ error[unresolved-import] pyinstrument/vendor/decorator.py:279:28: Module `contextlib` has no member `GeneratorContextManager`: Did you mean `AbstractContextManager`?

attrs (https://github.com/python-attrs/attrs)
- error[unresolved-import] src/attr/validators.pyi:1:19: Module `types` has no member `UnionType`
+ error[unresolved-import] src/attr/validators.pyi:1:19: Module `types` has no member `UnionType`: Did you mean `FunctionType`?
- error[unresolved-attribute] tests/test_converters.py:32:16: Type `Converter[Unknown, Unknown]` has no attribute `__call__`
+ error[unresolved-attribute] tests/test_converters.py:32:16: Type `Converter[Unknown, Unknown]` has no attribute `__call__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_converters.py:106:25: Type `Converter[Unknown, Unknown]` has no attribute `__call__`
+ error[unresolved-attribute] tests/test_converters.py:106:25: Type `Converter[Unknown, Unknown]` has no attribute `__call__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_converters.py:112:25: Type `Converter[Unknown, Unknown]` has no attribute `__call__`
+ error[unresolved-attribute] tests/test_converters.py:112:25: Type `Converter[Unknown, Unknown]` has no attribute `__call__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_converters.py:113:24: Type `Converter[Unknown, Unknown]` has no attribute `__call__`
+ error[unresolved-attribute] tests/test_converters.py:113:24: Type `Converter[Unknown, Unknown]` has no attribute `__call__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_make.py:2309:32: Type `object` has no attribute `__le__`
+ error[unresolved-attribute] tests/test_make.py:2309:32: Type `object` has no attribute `__le__`: Did you mean `__ne__`?
- error[unresolved-attribute] tests/test_make.py:2310:32: Type `object` has no attribute `__lt__`
+ error[unresolved-attribute] tests/test_make.py:2310:32: Type `object` has no attribute `__lt__`: Did you mean `__eq__`?
- error[unresolved-attribute] tests/test_make.py:2311:32: Type `object` has no attribute `__ge__`
+ error[unresolved-attribute] tests/test_make.py:2311:32: Type `object` has no attribute `__ge__`: Did you mean `__ne__`?
- error[unresolved-attribute] tests/test_make.py:2312:32: Type `object` has no attribute `__gt__`
+ error[unresolved-attribute] tests/test_make.py:2312:32: Type `object` has no attribute `__gt__`: Did you mean `__eq__`?
- error[unresolved-attribute] tests/test_slots.py:91:45: Type `C1Slots` has no attribute `__slots__`
+ error[unresolved-attribute] tests/test_slots.py:91:45: Type `C1Slots` has no attribute `__slots__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_slots.py:155:25: Type `<class 'C2Slots'>` has no attribute `__slots__`
+ error[unresolved-attribute] tests/test_slots.py:155:25: Type `<class 'C2Slots'>` has no attribute `__slots__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_slots.py:246:25: Type `<class 'C2Slots'>` has no attribute `__slots__`
+ error[unresolved-attribute] tests/test_slots.py:246:25: Type `<class 'C2Slots'>` has no attribute `__slots__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_slots.py:297:25: Type `<class 'C2Slots'>` has no attribute `__slots__`
+ error[unresolved-attribute] tests/test_slots.py:297:25: Type `<class 'C2Slots'>` has no attribute `__slots__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_slots.py:754:16: Type `<class 'A'>` has no attribute `__slots__`
+ error[unresolved-attribute] tests/test_slots.py:754:16: Type `<class 'A'>` has no attribute `__slots__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_slots.py:1021:12: Type `<class 'B'>` has no attribute `__slots__`
+ error[unresolved-attribute] tests/test_slots.py:1021:12: Type `<class 'B'>` has no attribute `__slots__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_slots.py:1042:12: Type `<class 'B'>` has no attribute `__slots__`
+ error[unresolved-attribute] tests/test_slots.py:1042:12: Type `<class 'B'>` has no attribute `__slots__`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_validators.py:110:40: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:110:40: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:159:39: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:159:39: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:271:33: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:271:33: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:320:37: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:320:37: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:385:32: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:385:32: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:477:42: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:477:42: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:634:41: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:634:41: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:734:40: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:734:40: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:804:27: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:804:27: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:879:36: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:879:36: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:952:36: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:952:36: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:1066:33: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:1066:33: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test_validators.py:1286:32: Type `<module 'src.attr.validators'>` has no attribute `__all__`
+ error[unresolved-attribute] tests/test_validators.py:1286:32: Type `<module 'src.attr.validators'>` has no attribute `__all__`: Did you mean `__file__`?

anyio (https://github.com/agronholm/anyio)
- error[unresolved-attribute] src/anyio/_backends/_asyncio.py:284:34: Type `<module 'asyncio'>` has no attribute `futures`
+ error[unresolved-attribute] src/anyio/_backends/_asyncio.py:284:34: Type `<module 'asyncio'>` has no attribute `futures`: Did you mean `Future`?
- error[unresolved-attribute] src/anyio/_backends/_asyncio.py:292:12: Type `AbstractEventLoop` has no attribute `_default_executor`
+ error[unresolved-attribute] src/anyio/_backends/_asyncio.py:292:12: Type `AbstractEventLoop` has no attribute `_default_executor`: Did you mean `set_default_executor`?

kornia (https://github.com/kornia/kornia)
- error[unresolved-attribute] kornia/nerf/samplers.py:233:57: Type `Points2D_FlatTensors` has no attribute `_x`
+ error[unresolved-attribute] kornia/nerf/samplers.py:233:57: Type `Points2D_FlatTensors` has no attribute `_x`: Did you mean `_y`?
- error[unresolved-attribute] kornia/nerf/samplers.py:234:57: Type `Points2D_FlatTensors` has no attribute `_y`
+ error[unresolved-attribute] kornia/nerf/samplers.py:234:57: Type `Points2D_FlatTensors` has no attribute `_y`: Did you mean `_x`?
- error[unresolved-attribute] kornia/nerf/samplers.py:259:30: Type `Points2D_FlatTensors` has no attribute `_x`
+ error[unresolved-attribute] kornia/nerf/samplers.py:259:30: Type `Points2D_FlatTensors` has no attribute `_x`: Did you mean `_y`?
- error[unresolved-attribute] kornia/nerf/samplers.py:259:58: Type `Points2D_FlatTensors` has no attribute `_y`
+ error[unresolved-attribute] kornia/nerf/samplers.py:259:58: Type `Points2D_FlatTensors` has no attribute `_y`: Did you mean `_x`?

trio (https://github.com/python-trio/trio)
- error[unresolved-attribute] src/trio/_core/_tests/test_asyncgen.py:201:18: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[unresolved-attribute] src/trio/_core/_tests/test_asyncgen.py:201:18: Type `<module 'src.trio._core'>` has no attribute `_run`: Did you mean `run`?
- error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2207:9: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2207:9: Type `<module 'src.trio._core'>` has no attribute `_run`: Did you mean `run`?
- error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2218:27: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[unresolved-attribute] src/trio/_core/_tests/test_run.py:2218:27: Type `<module 'src.trio._core'>` has no attribute `_run`: Did you mean `run`?
- error[unresolved-attribute] src/trio/_tests/test_path.py:267:9: Type `<module 'trio'>` has no attribute `_path`
+ error[unresolved-attribute] src/trio/_tests/test_path.py:267:9: Type `<module 'trio'>` has no attribute `_path`: Did you mean `Path`?
- error[unresolved-attribute] src/trio/_tests/test_path.py:268:9: Type `<module 'trio'>` has no attribute `_path`
+ error[unresolved-attribute] src/trio/_tests/test_path.py:268:9: Type `<module 'trio'>` has no attribute `_path`: Did you mean `Path`?
- error[unresolved-attribute] src/trio/_tests/test_path.py:269:9: Type `<module 'trio'>` has no attribute `_path`
+ error[unresolved-attribute] src/trio/_tests/test_path.py:269:9: Type `<module 'trio'>` has no attribute `_path`: Did you mean `Path`?
- error[unresolved-attribute] src/trio/_tests/test_path.py:270:9: Type `<module 'trio'>` has no attribute `_path`
+ error[unresolved-attribute] src/trio/_tests/test_path.py:270:9: Type `<module 'trio'>` has no attribute `_path`: Did you mean `Path`?
- error[unresolved-attribute] src/trio/_tests/test_subprocess.py:736:25: Type `<module 'trio'>` has no attribute `_subprocess`
+ error[unresolved-attribute] src/trio/_tests/test_subprocess.py:736:25: Type `<module 'trio'>` has no attribute `_subprocess`: Did you mean `run_process`?
- error[unresolved-attribute] src/trio/_tests/test_testing_raisesgroup.py:1110:9: Type `<module 'src.trio.testing'>` has no attribute `_raises_group`
+ error[unresolved-attribute] src/trio/_tests/test_testing_raisesgroup.py:1110:9: Type `<module 'src.trio.testing'>` has no attribute `_raises_group`: Did you mean `RaisesGroup`?
- error[unresolved-attribute] src/trio/_tests/test_testing_raisesgroup.py:1112:9: Type `<module 'src.trio.testing'>` has no attribute `_raises_group`
+ error[unresolved-attribute] src/trio/_tests/test_testing_raisesgroup.py:1112:9: Type `<module 'src.trio.testing'>` has no attribute `_raises_group`: Did you mean `RaisesGroup`?
- error[unresolved-import] src/trio/socket.py:130:9: Module `socket` has no member `AF_LINK`
+ error[unresolved-import] src/trio/socket.py:130:9: Module `socket` has no member `AF_LINK`: Did you mean `AF_NETLINK`?
- error[unresolved-import] src/trio/socket.py:143:9: Module `socket` has no member `AF_SYSTEM`
+ error[unresolved-import] src/trio/socket.py:143:9: Module `socket` has no member `AF_SYSTEM`: Did you mean `EAI_SYSTEM`?
- error[unresolved-import] src/trio/socket.py:159:9: Module `socket` has no member `AI_V4MAPPED_CFG`
+ error[unresolved-import] src/trio/socket.py:159:9: Module `socket` has no member `AI_V4MAPPED_CFG`: Did you mean `AI_V4MAPPED`?
- error[unresolved-import] src/trio/socket.py:170:9: Module `socket` has no member `BDADDR_ANY`
+ error[unresolved-import] src/trio/socket.py:170:9: Module `socket` has no member `BDADDR_ANY`: Did you mean `INADDR_ANY`?
- error[unresolved-import] src/trio/socket.py:172:9: Module `socket` has no member `BTPROTO_HCI`
+ error[unresolved-import] src/trio/socket.py:172:9: Module `socket` has no member `BTPROTO_HCI`: Did you mean `IPPROTO_TCP`?
- error[unresolved-import] src/trio/socket.py:175:9: Module `socket` has no member `BTPROTO_SCO`
+ error[unresolved-import] src/trio/socket.py:175:9: Module `socket` has no member `BTPROTO_SCO`: Did you mean `IPPROTO_SCTP`?
- error[unresolved-import] src/trio/socket.py:223:9: Module `socket` has no member `EAI_BADHINTS`
+ error[unresolved-import] src/trio/socket.py:223:9: Module `socket` has no member `EAI_BADHINTS`: Did you mean `EAI_BADFLAGS`?
- error[unresolved-import] src/trio/socket.py:226:9: Module `socket` has no member `EAI_MAX`
+ error[unresolved-import] src/trio/socket.py:226:9: Module `socket` has no member `EAI_MAX`: Did you mean `EAI_FAIL`?
- error[unresolved-import] src/trio/socket.py:231:9: Module `socket` has no member `EAI_PROTOCOL`
+ error[unresolved-import] src/trio/socket.py:231:9: Module `socket` has no member `EAI_PROTOCOL`: Did you mean `SO_PROTOCOL`?
- error[unresolved-import] src/trio/socket.py:274:9: Module `socket` has no member `IP_ADD_SOURCE_MEMBERSHIP`
+ error[unresolved-import] src/trio/socket.py:274:9: Module `socket` has no member `IP_ADD_SOURCE_MEMBERSHIP`: Did you mean `IP_ADD_MEMBERSHIP`?
- error[unresolved-import] src/trio/socket.py:279:9: Module `socket` has no member `IP_DROP_SOURCE_MEMBERSHIP`
+ error[unresolved-import] src/trio/socket.py:279:9: Module `socket` has no member `IP_DROP_SOURCE_MEMBERSHIP`: Did you mean `IP_DROP_MEMBERSHIP`?
- error[unresolved-import] src/trio/socket.py:286:9: Module `socket` has no member `IP_PKTINFO`
+ error[unresolved-import] src/trio/socket.py:286:9: Module `socket` has no member `IP_PKTINFO`: Did you mean `IPV6_PKTINFO`?
- error[unresolved-import] src/trio/socket.py:290:9: Module `socket` has no member `IP_RECVTOS`
+ error[unresolved-import] src/trio/socket.py:290:9: Module `socket` has no member `IP_RECVTOS`: Did you mean `IP_RECVOPTS`?
- error[unresolved-import] src/trio/socket.py:299:9: Module `socket` has no member `IPPROTO_CBT`
+ error[unresolved-import] src/trio/socket.py:299:9: Module `socket` has no member `IPPROTO_CBT`: Did you mean `IPPROTO_AH`?
- error[unresolved-import] src/trio/socket.py:302:9: Module `socket` has no member `IPPROTO_EON`
+ error[unresolved-import] src/trio/socket.py:302:9: Module `socket` has no member `IPPROTO_EON`: Did you mean `IPPROTO_EGP`?
- error[unresolved-import] src/trio/socket.py:305:9: Module `socket` has no member `IPPROTO_GGP`
+ error[unresolved-import] src/trio/socket.py:305:9: Module `socket` has no member `IPPROTO_GGP`: Did you mean `IPPROTO_EGP`?
- error[unresolved-import] src/trio/socket.py:307:9: Module `socket` has no member `IPPROTO_HELLO`
+ error[unresolved-import] src/trio/socket.py:307:9: Module `socket` has no member `IPPROTO_HELLO`: Did you mean `IPPROTO_EGP`?
- error[unresolved-import] src/trio/socket.py:309:9: Module `socket` has no member `IPPROTO_ICLFXBM`
+ error[unresolved-import] src/trio/socket.py:309:9: Module `socket` has no member `IPPROTO_ICLFXBM`: Did you mean `IPPROTO_ICMP`?
- error[unresolved-import] src/trio/socket.py:314:9: Module `socket` has no member `IPPROTO_IGP`
+ error[unresolved-import] src/trio/socket.py:314:9: Module `socket` has no member `IPPROTO_IGP`: Did you mean `IPPROTO_EGP`?
- error[unresolved-import] src/trio/socket.py:316:9: Module `socket` has no member `IPPROTO_IPCOMP`
+ error[unresolved-import] src/trio/socket.py:316:9: Module `socket` has no member `IPPROTO_IPCOMP`: Did you mean `IPPROTO_ICMP`?
- error[unresolved-import] src/trio/socket.py:318:9: Module `socket` has no member `IPPROTO_IPV4`
+ error[unresolved-import] src/trio/socket.py:318:9: Module `socket` has no member `IPPROTO_IPV4`: Did you mean `IPPROTO_IPV6`?
- error[unresolved-import] src/trio/socket.py:320:9: Module `socket` has no member `IPPROTO_L2TP`
+ error[unresolved-import] src/trio/socket.py:320:9: Module `socket` has no member `IPPROTO_L2TP`: Did you mean `IPPROTO_SCTP`?
- error[unresolved-import] src/trio/socket.py:321:9: Module `socket` has no member `IPPROTO_MAX`
+ error[unresolved-import] src/trio/socket.py:321:9: Module `socket` has no member `IPPROTO_MAX`: Did you mean `IPPROTO_AH`?
- error[unresolved-import] src/trio/socket.py:322:9: Module `socket` has no member `IPPROTO_MOBILE`
+ error[unresolved-import] src/trio/socket.py:322:9: Module `socket` has no member `IPPROTO_MOBILE`: Did you mean `IPPROTO_NONE`?
- error[unresolved-import] src/trio/socket.py:323:9: Module `socket` has no member `IPPROTO_MPTCP`
+ error[unresolved-import] src/trio/socket.py:323:9: Module `socket` has no member `IPPROTO_MPTCP`: Did you mean `IPPROTO_TCP`?
- error[unresolved-import] src/trio/socket.py:324:9: Module `socket` has no member `IPPROTO_ND`
+ error[unresolved-import] src/trio/socket.py:324:9: Module `socket` has no member `IPPROTO_ND`: Did you mean `IPPROTO_AH`?
- error[unresolved-import] src/trio/socket.py:326:9: Module `socket` has no member `IPPROTO_PGM`
+ error[unresolved-import] src/trio/socket.py:326:9: Module `socket` has no member `IPPROTO_PGM`: Did you mean `IPPROTO_PIM`?
- error[unresolved-import] src/trio/socket.py:330:9: Module `socket` has no member `IPPROTO_RDP`
+ error[unresolved-import] src/trio/socket.py:330:9: Module `socket` has no member `IPPROTO_RDP`: Did you mean `IPPROTO_IDP`?
- error[unresolved-import] src/trio/socket.py:334:9: Module `socket` has no member `IPPROTO_ST`
+ error[unresolved-import] src/trio/socket.py:334:9: Module `socket` has no member `IPPROTO_ST`: Did you mean `IPPROTO_AH`?
- error[unresolved-import] src/trio/socket.py:339:9: Module `socket` has no member `IPPROTO_XTP`
+ error[unresolved-import] src/trio/socket.py:339:9: Module `socket` has no member `IPPROTO_XTP`: Did you mean `IPPROTO_TP`?
- error[unresolved-import] src/trio/socket.py:382:9: Module `socket` has no member `LOCAL_PEERCRED`
+ error[unresolved-import] src/trio/socket.py:382:9: Module `socket` has no member `LOCAL_PEERCRED`: Did you mean `SO_PEERCRED`?
- error[unresolved-import] src/trio/socket.py:389:9: Module `socket` has no member `MSG_EOF`
+ error[unresolved-import] src/trio/socket.py:389:9: Module `socket` has no member `MSG_EOF`: Did you mean `MSG_EOR`?
- error[unresolved-import] src/trio/socket.py:427:9: Module `socket` has no member `PF_SYSTEM`
+ error[unresolved-import] src/trio/socket.py:427:9: Module `socket` has no member `PF_SYSTEM`: Did you mean `EAI_SYSTEM`?
- error[unresolved-import] src/trio/socket.py:514:9: Module `socket` has no member `TCP_CC_INFO`
+ error[unresolved-import] src/trio/socket.py:514:9: Module `socket` has no member `TCP_CC_INFO`: Did you mean `TCP_INFO`?
- error[unresolved-import] src/trio/socket.py:521:9: Module `socket` has no member `TCP_FASTOPEN_KEY`
+ error[unresolved-import] src/trio/socket.py:521:9: Module `socket` has no member `TCP_FASTOPEN_KEY`: Did you mean `TCP_FASTOPEN`?
- error[unresolved-import] src/trio/socket.py:524:9: Module `socket` has no member `TCP_INQ`
+ error[unresolved-import] src/trio/socket.py:524:9: Module `socket` has no member `TCP_INQ`: Did you mean `TCP_INFO`?
- error[unresolved-import] src/trio/socket.py:525:9: Module `socket` has no member `TCP_KEEPALIVE`
+ error[unresolved-import] src/trio/socket.py:525:9: Module `socket` has no member `TCP_KEEPALIVE`: Did you mean `SO_KEEPALIVE`?
- error[unresolved-import] src/trio/socket.py:531:9: Module `socket` has no member `TCP_MD5SIG`
+ error[unresolved-import] src/trio/socket.py:531:9: Module `socket` has no member `TCP_MD5SIG`: Did you mean `TCP_MAXSEG`?
- error[unresolved-import] src/trio/socket.py:547:9: Module `socket` has no member `TCP_TX_DELAY`
+ error[unresolved-import] src/trio/socket.py:547:9: Module `socket` has no member `TCP_TX_DELAY`: Did you mean `TCP_NODELAY`?

comtypes (https://github.com/enthought/comtypes)
- error[unresolved-attribute] comtypes/_memberspec.py:484:17: Type `<module 'ctypes'>` has no attribute `WINFUNCTYPE`
+ error[unresolved-attribute] comtypes/_memberspec.py:484:17: Type `<module 'ctypes'>` has no attribute `WINFUNCTYPE`: Did you mean `CFUNCTYPE`?
- error[unresolved-import] comtypes/_vtbl.py:3:20: Module `ctypes` has no member `WINFUNCTYPE`
+ error[unresolved-import] comtypes/_vtbl.py:3:20: Module `ctypes` has no member `WINFUNCTYPE`: Did you mean `CFUNCTYPE`?
- error[unresolved-import] comtypes/client/_events.py:5:38: Module `ctypes` has no member `WINFUNCTYPE`
+ error[unresolved-import] comtypes/client/_events.py:5:38: Module `ctypes` has no member `WINFUNCTYPE`: Did you mean `CFUNCTYPE`?
- error[unresolved-attribute] comtypes/client/_events.py:385:16: Type `OSError` has no attribute `winerror`
+ error[unresolved-attribute] comtypes/client/_events.py:385:16: Type `OSError` has no attribute `winerror`: Did you mean `strerror`?
- error[unresolved-attribute] comtypes/server/register.py:69:16: Type `OSError` has no attribute `winerror`
+ error[unresolved-attribute] comtypes/server/register.py:69:16: Type `OSError` has no attribute `winerror`: Did you mean `strerror`?
- error[unresolved-attribute] comtypes/test/test_comserver.py:26:12: Type `OSError` has no attribute `winerror`
+ error[unresolved-attribute] comtypes/test/test_comserver.py:26:12: Type `OSError` has no attribute `winerror`: Did you mean `strerror`?
- error[unresolved-attribute] comtypes/test/test_dispinterface.py:20:12: Type `OSError` has no attribute `winerror`
+ error[unresolved-attribute] comtypes/test/test_dispinterface.py:20:12: Type `OSError` has no attribute `winerror`: Did you mean `strerror`?

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
- error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:43:12: Type `<module 'jsonschema'>` has no attribute `validators`
+ error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:43:12: Type `<module 'jsonschema'>` has no attribute `validators`: Did you mean `Validator`?
- error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:53:12: Type `<module 'jsonschema'>` has no attribute `validators`
+ error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:53:12: Type `<module 'jsonschema'>` has no attribute `validators`: Did you mean `Validator`?
- error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:169:29: Type `<module 'jsonschema'>` has no attribute `validators`
+ error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:169:29: Type `<module 'jsonschema'>` has no attribute `validators`: Did you mean `Validator`?
- error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:207:28: Type `<module 'jsonschema'>` has no attribute `validators`
+ error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:207:28: Type `<module 'jsonschema'>` has no attribute `validators`: Did you mean `Validator`?
- error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:268:28: Type `<module 'jsonschema'>` has no attribute `validators`
+ error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:268:28: Type `<module 'jsonschema'>` has no attribute `validators`: Did you mean `Validator`?
- error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:269:32: Type `<module 'jsonschema'>` has no attribute `validators`
+ error[unresolved-attribute] src/check_jsonschema/schema_loader/main.py:269:32: Type `<module 'jsonschema'>` has no attribute `validators`: Did you mean `Validator`?

pydantic (https://github.com/pydantic/pydantic)
- error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2044:45: Type `TypeVar` has no attribute `__default__`
+ error[unresolved-attribute] pydantic/_internal/_generate_schema.py:2044:45: Type `TypeVar` has no attribute `__default__`: Did you mean `__delattr__`?
- error[unresolved-attribute] pydantic/_internal/_generics.py:385:53: Type `TypeVar` has no attribute `__default__`
+ error[unresolved-attribute] pydantic/_internal/_generics.py:385:53: Type `TypeVar` has no attribute `__default__`: Did you mean `__delattr__`?
- error[unresolved-attribute] pydantic/v1/_hypothesis_plugin.py:94:13: Type `<module 'pydantic.color'>` has no attribute `r_rgba`
+ error[unresolved-attribute] pydantic/v1/_hypothesis_plugin.py:94:13: Type `<module 'pydantic.color'>` has no attribute `r_rgba`: Did you mean `r_rgb`?
- error[unresolved-attribute] pydantic/v1/_hypothesis_plugin.py:96:13: Type `<module 'pydantic.color'>` has no attribute `r_hsla`
+ error[unresolved-attribute] pydantic/v1/_hypothesis_plugin.py:96:13: Type `<module 'pydantic.color'>` has no attribute `r_hsla`: Did you mean `r_hsl`?

graphql-core (https://github.com/graphql-python/graphql-core)
- error[unresolved-attribute] tests/benchmarks/test_async_iterable.py:27:12: Type `<module 'asyncio'>` has no attribute `events`
+ error[unresolved-attribute] tests/benchmarks/test_async_iterable.py:27:12: Type `<module 'asyncio'>` has no attribute `events`: Did you mean `Event`?
- error[unresolved-attribute] tests/benchmarks/test_async_iterable.py:28:5: Type `<module 'asyncio'>` has no attribute `events`
+ error[unresolved-attribute] tests/benchmarks/test_async_iterable.py:28:5: Type `<module 'asyncio'>` has no attribute `events`: Did you mean `Event`?
- error[unresolved-attribute] tests/benchmarks/test_async_iterable.py:30:5: Type `<module 'asyncio'>` has no attribute `events`
+ error[unresolved-attribute] tests/benchmarks/test_async_iterable.py:30:5: Type `<module 'asyncio'>` has no attribute `events`: Did you mean `Event`?
- error[unresolved-attribute] tests/benchmarks/test_execution_async.py:43:12: Type `<module 'asyncio'>` has no attribute `events`
+ error[unresolved-attribute] tests/benchmarks/test_execution_async.py:43:12: Type `<module 'asyncio'>` has no attribute `events`: Did you mean `Event`?
- error[unresolved-attribute] tests/benchmarks/test_execution_async.py:44:5: Type `<module 'asyncio'>` has no attribute `events`
+ error[unresolved-attribute] tests/benchmarks/test_execution_async.py:44:5: Type `<module 'asyncio'>` has no attribute `events`: Did you mean `Event`?
- error[unresolved-attribute] tests/benchmarks/test_execution_async.py:48:5: Type `<module 'asyncio'>` has no attribute `events`
+ error[unresolved-attribute] tests/benchmarks/test_execution_async.py:48:5: Type `<module 'asyncio'>` has no attribute `events`: Did you mean `Event`?

sockeye (https://github.com/awslabs/sockeye)
- error[unresolved-attribute] sockeye/utils.py:627:16: Type `<module 'multiprocessing'>` has no attribute `pool`
+ error[unresolved-attribute] sockeye/utils.py:627:16: Type `<module 'multiprocessing'>` has no attribute `pool`: Did you mean `Pool`?

strawberry (https://github.com/strawberry-graphql/strawberry)
- error[unresolved-import] strawberry/experimental/pydantic/fields.py:18:23: Module `types` has no member `UnionType`
+ error[unresolved-import] strawberry/experimental/pydantic/fields.py:18:23: Module `types` has no member `UnionType`: Did you mean `FunctionType`?

PyGithub (https://github.com/PyGithub/PyGithub)
- error[unresolved-attribute] github/Requester.py:299:27: Type `<module 'requests'>` has no attribute `models`
+ error[unresolved-attribute] github/Requester.py:299:27: Type `<module 'requests'>` has no attribute `models`: Did you mean `codes`?
- error[unresolved-attribute] github/Requester.py:299:63: Type `<module 'requests'>` has no attribute `models`
+ error[unresolved-attribute] github/Requester.py:299:63: Type `<module 'requests'>` has no attribute `models`: Did you mean `codes`?

twine (https://github.com/pypa/twine)
- error[unresolved-attribute] twine/auth.py:74:25: Type `<module 'requests'>` has no attribute `models`
+ error[unresolved-attribute] twine/auth.py:74:25: Type `<module 'requests'>` has no attribute `models`: Did you mean `codes`?
- error[unresolved-attribute] twine/auth.py:75:11: Type `<module 'requests'>` has no attribute `models`
+ error[unresolved-attribute] twine/auth.py:75:11: Type `<module 'requests'>` has no attribute `models`: Did you mean `codes`?
- error[unresolved-attribute] twine/auth.py:88:21: Type `<module 'requests'>` has no attribute `models`
+ error[unresolved-attribute] twine/auth.py:88:21: Type `<module 'requests'>` has no attribute `models`: Did you mean `codes`?

mkdocs (https://github.com/mkdocs/mkdocs)
- error[unresolved-attribute] mkdocs/tests/build_tests.py:950:22: Type `<module 'markdown'>` has no attribute `extensions`
+ error[unresolved-attribute] mkdocs/tests/build_tests.py:950:22: Type `<module 'markdown'>` has no attribute `extensions`: Did you mean `Extension`?

cki-lib (https://gitlab.com/cki-project/cki-lib)
- error[unresolved-attribute] cki_lib/yaml.py:76:41: Type `<module 'yaml'>` has no attribute `nodes`
+ error[unresolved-attribute] cki_lib/yaml.py:76:41: Type `<module 'yaml'>` has no attribute `nodes`: Did you mean `Node`?

asynq (https://github.com/quora/asynq)
- error[unresolved-attribute] asynq/async_task.py:78:24: Type `<module 'asynq'>` has no attribute `scheduler`
+ error[unresolved-attribute] asynq/async_task.py:78:24: Type `<module 'asynq'>` has no attribute `scheduler`: Did you mean `TaskScheduler`?
- error[unresolved-attribute] asynq/async_task.py:115:9: Type `<module 'asynq'>` has no attribute `scheduler`
+ error[unresolved-attribute] asynq/async_task.py:115:9: Type `<module 'asynq'>` has no attribute `scheduler`: Did you mean `TaskScheduler`?
- error[unresolved-attribute] asynq/contexts.py:59:19: Type `<module 'asynq'>` has no attribute `scheduler`
+ error[unresolved-attribute] asynq/contexts.py:59:19: Type `<module 'asynq'>` has no attribute `scheduler`: Did you mean `TaskScheduler`?
- error[unresolved-attribute] asynq/tests/test_futures.py:22:5: Type `Future[Unknown]` has no attribute `on_computed`
+ error[unresolved-attribute] asynq/tests/test_futures.py:22:5: Type `Future[Unknown]` has no attribute `on_computed`: Did you mean `is_computed`?
- error[unresolved-attribute] asynq/tests/test_futures.py:25:5: Type `Future[Unknown]` has no attribute `on_computed`
+ error[unresolved-attribute] asynq/tests/test_futures.py:25:5: Type `Future[Unknown]` has no attribute `on_computed`: Did you mean `is_computed`?

poetry (https://github.com/python-poetry/poetry)
- error[unresolved-attribute] tests/integration/test_utils_vcs_git.py:432:43: Type `Literal[b""]` has no attribute `encode`
+ error[unresolved-attribute] tests/integration/test_utils_vcs_git.py:432:43: Type `Literal[b""]` has no attribute `encode`: Did you mean `decode`?

vision (https://github.com/pytorch/vision)
- error[unresolved-attribute] test/preprocess-bench.py:34:13: Type `<module 'torchvision.transforms'>` has no attribute `RandomSizedCrop`
+ error[unresolved-attribute] test/preprocess-bench.py:34:13: Type `<module 'torchvision.transforms'>` has no attribute `RandomSizedCrop`: Did you mean `RandomResizedCrop`?
- error[unresolved-attribute] test/test_models.py:646:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`
+ error[unresolved-attribute] test/test_models.py:646:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`: Did you mean `VisionTransformer`?
- error[unresolved-attribute] test/test_models.py:647:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`
+ error[unresolved-attribute] test/test_models.py:647:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`: Did you mean `VisionTransformer`?
- error[unresolved-attribute] test/test_models.py:648:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`
+ error[unresolved-attribute] test/test_models.py:648:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`: Did you mean `VisionTransformer`?
- error[unresolved-attribute] test/test_models.py:649:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`
+ error[unresolved-attribute] test/test_models.py:649:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`: Did you mean `VisionTransformer`?
- error[unresolved-attribute] test/test_models.py:650:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`
+ error[unresolved-attribute] test/test_models.py:650:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`: Did you mean `VisionTransformer`?
- error[unresolved-attribute] test/test_models.py:651:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`
+ error[unresolved-attribute] test/test_models.py:651:5: Type `<module 'torchvision.models'>` has no attribute `vision_transformer`: Did you mean `VisionTransformer`?
- error[unresolved-attribute] test/test_onnx.py:428:17: Type `<module 'torchvision.models.detection'>` has no attribute `faster_rcnn`
+ error[unresolved-attribute] test/test_onnx.py:428:17: Type `<module 'torchvision.models.detection'>` has no attribute `faster_rcnn`: Did you mean `FasterRCNN`?
- error[unresolved-attribute] test/test_onnx.py:429:21: Type `<module 'torchvision.models.detection'>` has no attribute `faster_rcnn`
+ error[unresolved-attribute] test/test_onnx.py:429:21: Type `<module 'torchvision.models.detection'>` has no attribute `faster_rcnn`: Did you mean `FasterRCNN`?
- error[unresolved-attribute] test/test_onnx.py:546:17: Type `<module 'torchvision.models.detection'>` has no attribute `keypoint_rcnn`
+ error[unresolved-attribute] test/test_onnx.py:546:17: Type `<module 'torchvision.models.detection'>` has no attribute `keypoint_rcnn`: Did you mean `KeypointRCNN`?
- error[unresolved-attribute] test/test_onnx.py:547:21: Type `<module 'torchvision.models.detection'>` has no attribute `keypoint_rcnn`
+ error[unresolved-attribute] test/test_onnx.py:547:21: Type `<module 'torchvision.models.detection'>` has no attribute `keypoint_rcnn`: Did you mean `KeypointRCNN`?
- error[unresolved-attribute] test/test_prototype_models.py:17:20: Type `<module 'torchvision.prototype.models.depth.stereo'>` has no attribute `raft_stereo`
+ error[unresolved-attribute] test/test_prototype_models.py:17:20: Type `<module 'torchvision.prototype.models.depth.stereo'>` has no attribute `raft_stereo`: Did you mean `RaftStereo`?
- error[unresolved-attribute] test/test_prototype_models.py:18:18: Type `<module 'torchvision.prototype.models.depth.stereo'>` has no attribute `raft_stereo`
+ error[unresolved-attribute] test/test_prototype_models.py:18:18: Type `<module 'torchvision.prototype.models.depth.stereo'>` has no attribute `raft_stereo`: Did you mean `RaftStereo`?

colour (https://github.com/colour-science/colour)
- error[unresolved-attribute] colour/examples/models/examples_models.py:193:7: Type `<module 'colour'>` has no attribute `Luv_to_LCHuv`
+ error[unresolved-attribute] colour/examples/models/examples_models.py:193:7: Type `<module 'colour'>` has no attribute `Luv_to_LCHuv`: Did you mean `Luv_to_uv`?
- error[unresolved-attribute] colour/examples/models/examples_models.py:202:7: Type `<module 'colour'>` has no attribute `LCHuv_to_Luv`
+ error[unresolved-attribute] colour/examples/models/examples_models.py:202:7: Type `<module 'colour'>` has no attribute `LCHuv_to_Luv`: Did you mean `Luv_to_uv`?
- error[unresolved-attribute] colour/plotting/common.py:817:24: Type `Patch` has no attribute `get_width`
+ error[unresolved-attribute] colour/plotting/common.py:817:24: Type `Patch` has no attribute `get_width`: Did you mean `get_gid`?
- error[unresolved-attribute] colour/plotting/volume.py:640:5: Type `Poly3DCollection` has no attribute `set_facecolors`
+ error[unresolved-attribute] colour/plotting/volume.py:640:5: Type `Poly3DCollection` has no attribute `set_facecolors`: Did you mean `set_facecolor`?
- error[unresolved-attribute] colour/plotting/volume.py:641:5: Type `Poly3DCollection` has no attribute `set_edgecolors`
+ error[unresolved-attribute] colour/plotting/volume.py:641:5: Type `Poly3DCollection` has no attribute `set_edgecolors`: Did you mean `set_edgecolor`?
- error[unresolved-attribute] colour/utilities/tests/test_deprecation.py:326:13: Type `<module 'colour.utilities.tests.test_deprecated'>` has no attribute `OLD_NAME`
+ error[unresolved-attribute] colour/utilities/tests/test_deprecation.py:326:13: Type `<module 'colour.utilities.tests.test_deprecated'>` has no attribute `OLD_NAME`: Did you mean `NEW_NAME`?

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[unresolved-attribute] tests/test___all__.py:34:23: Type `ModuleType` has no attribute `__all__`
+ error[unresolved-attribute] tests/test___all__.py:34:23: Type `ModuleType` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test___all__.py:35:50: Type `ModuleType` has no attribute `__all__`
+ error[unresolved-attribute] tests/test___all__.py:35:50: Type `ModuleType` has no attribute `__all__`: Did you mean `__file__`?
- error[unresolved-attribute] tests/test___all__.py:57:25: Type `ModuleType` has no attribute `__all__`
+ error[unresolved-attribute] tests/test___all__.py:57:25: Type `ModuleType` has no attribute `__all__`: Did you mean `__file__`?

ignite (https://github.com/pytorch/ignite)
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:479:28: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:479:28: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`: Did you mean `TensorboardLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:480:32: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:480:32: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`: Did you mean `TensorboardLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:488:28: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:488:28: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`: Did you mean `TensorboardLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:489:32: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:489:32: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`: Did you mean `TensorboardLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:497:28: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:497:28: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`: Did you mean `TensorboardLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:498:32: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:498:32: Type `<module 'ignite.handlers'>` has no attribute `tensorboard_logger`: Did you mean `TensorboardLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:512:28: Type `<module 'ignite.handlers'>` has no attribute `visdom_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:512:28: Type `<module 'ignite.handlers'>` has no attribute `visdom_logger`: Did you mean `VisdomLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:513:32: Type `<module 'ignite.handlers'>` has no attribute `visdom_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:513:32: Type `<module 'ignite.handlers'>` has no attribute `visdom_logger`: Did you mean `VisdomLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:522:28: Type `<module 'ignite.handlers'>` has no attribute `visdom_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:522:28: Type `<module 'ignite.handlers'>` has no attribute `visdom_logger`: Did you mean `VisdomLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:523:32: Type `<module 'ignite.handlers'>` has no attribute `visdom_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:523:32: Type `<module 'ignite.handlers'>` has no attribute `visdom_logger`: Did you mean `VisdomLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:536:28: Type `<module 'ignite.handlers'>` has no attribute `polyaxon_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:536:28: Type `<module 'ignite.handlers'>` has no attribute `polyaxon_logger`: Did you mean `PolyaxonLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:537:32: Type `<module 'ignite.handlers'>` has no attribute `polyaxon_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:537:32: Type `<module 'ignite.handlers'>` has no attribute `polyaxon_logger`: Did you mean `PolyaxonLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:544:28: Type `<module 'ignite.handlers'>` has no attribute `polyaxon_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:544:28: Type `<module 'ignite.handlers'>` has no attribute `polyaxon_logger`: Did you mean `PolyaxonLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:545:32: Type `<module 'ignite.handlers'>` has no attribute `polyaxon_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:545:32: Type `<module 'ignite.handlers'>` has no attribute `polyaxon_logger`: Did you mean `PolyaxonLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:556:28: Type `<module 'ignite.handlers'>` has no attribute `mlflow_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:556:28: Type `<module 'ignite.handlers'>` has no attribute `mlflow_logger`: Did you mean `MLflowLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:557:32: Type `<module 'ignite.handlers'>` has no attribute `mlflow_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:557:32: Type `<module 'ignite.handlers'>` has no attribute `mlflow_logger`: Did you mean `MLflowLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:565:28: Type `<module 'ignite.handlers'>` has no attribute `mlflow_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:565:28: Type `<module 'ignite.handlers'>` has no attribute `mlflow_logger`: Did you mean `MLflowLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:566:32: Type `<module 'ignite.handlers'>` has no attribute `mlflow_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:566:32: Type `<module 'ignite.handlers'>` has no attribute `mlflow_logger`: Did you mean `MLflowLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:581:5: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:581:5: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:587:32: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:587:32: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:588:36: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:588:36: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:596:32: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:596:32: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:597:36: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:597:36: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:605:32: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:605:32: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:606:36: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:606:36: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:616:32: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:616:32: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:617:36: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:617:36: Type `<module 'ignite.handlers'>` has no attribute `clearml_logger`: Did you mean `ClearMLLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:628:28: Type `<module 'ignite.handlers'>` has no attribute `neptune_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:628:28: Type `<module 'ignite.handlers'>` has no attribute `neptune_logger`: Did you mean `NeptuneLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:629:32: Type `<module 'ignite.handlers'>` has no attribute `neptune_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:629:32: Type `<module 'ignite.handlers'>` has no attribute `neptune_logger`: Did you mean `NeptuneLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:637:28: Type `<module 'ignite.handlers'>` has no attribute `neptune_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:637:28: Type `<module 'ignite.handlers'>` has no attribute `neptune_logger`: Did you mean `NeptuneLogger`?
- error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:638:32: Type `<module 'ignite.handlers'>` has no attribute `neptune_logger`
+ error[unresolved-attribute] tests/ignite/contrib/engines/test_common.py:638:32: Type `<module 'ignite.handlers'>` has no attribute `neptune_logger`: Did you mean `NeptuneLogger`?
- error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1295:33: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`
+ error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1295:33: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`: Did you mean `__state_dict_key_per_rank`?
- error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1335:60: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`
+ error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1335:60: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`: Did you mean `__state_dict_key_per_rank`?
- error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1340:30: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`
+ error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1340:30: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`: Did you mean `__state_dict_key_per_rank`?
- error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1389:22: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`
+ error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1389:22: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`: Did you mean `__state_dict_key_per_rank`?
- error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1390:28: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`
+ error[unresolved-attribute] tests/ignite/metrics/test_metric.py:1390:28: Type `<class 'Metric'>` has no attribute `_Metric__state_dict_key_per_rank`: Did you mean `__state_dict_key_per_rank`?

apprise (https://github.com/caronc/apprise)
- error[unresolved-import] apprise/url.pyi:1:21: Module `logging` has no member `logger`
+ error[unresolved-import] apprise/url.pyi:1:21: Module `logging` has no member `logger`: Did you mean `Logger`?
- error[unresolved-attribute] setup.py:41:28: Type `<module 'babel.messages.frontend'>` has no attribute `compile_catalog`
+ error[unresolved-attribute] setup.py:41:28: Type `<module 'babel.messages.frontend'>` has no attribute `compile_catalog`: Did you mean `CompileCatalog`?
- error[unresolved-attribute] setup.py:42:29: Type `<module 'babel.messages.frontend'>` has no attribute `extract_messages`
+ error[unresolved-attribute] setup.py:42:29: Type `<module 'babel.messages.frontend'>` has no attribute `extract_messages`: Did you mean `ExtractMessages`?
- error[unresolved-attribute] setup.py:43:25: Type `<module 'babel.messages.frontend'>` has no attribute `init_catalog`
+ error[unresolved-attribute] setup.py:43:25: Type `<module 'babel.messages.frontend'>` has no attribute `init_catalog`: Did you mean `InitCatalog`?
- error[unresolved-attribute] setup.py:44:27: Type `<module 'babel.messages.frontend'>` has no attribute `update_catalog`
+ error[unresolved-attribute] setup.py:44:27: Type `<module 'babel.messages.frontend'>` has no attribute `update_catalog`: Did you mean `UpdateCatalog`?
- error[unresolved-attribute] test/test_config_http.py:310:23: Type `ConfigHTTP` has no attribute `max_buffer_size`
+ error[unresolved-attribute] test/test_config_http.py:310:23: Type `ConfigHTTP` has no attribute `max_buffer_size`: Did you mean `max_error_buffer_size`?
- error[unresolved-attribute] test/test_config_http.py:329:33: Type `ConfigHTTP` has no attribute `max_buffer_size`
+ error[unresolved-attribute] test/test_config_http.py:329:33: Type `ConfigHTTP` has no attribute `max_buffer_size`: Did you mean `max_error_buffer_size`?
- error[unresolved-attribute] test/test_config_http.py:334:34: Type `ConfigHTTP` has no attribute `max_buffer_size`
+ error[unresolved-attribute] test/test_config_http.py:334:34: Type `ConfigHTTP` has no attribute `max_buffer_size`: Did you mean `max_error_buffer_size`?

boostedblob (https://github.com/hauntsaninja/boostedblob)
- error[unresolved-attribute] boostedblob/cli.py:248:35: Type `<module 'boostedblob'>` has no attribute `copying`
+ error[unresolved-attribute] boostedblob/cli.py:248:35: Type `<module 'boostedblob'>` has no attribute `copying`: Did you mean `copyfile`?
- error[unresolved-attribute] boostedblob/cli.py:267:24: Type `<module 'boostedblob'>` has no attribute `copying`
+ error[unresolved-attribute] boostedblob/cli.py:267:24: Type `<module 'boostedblob'>` has no attribute `copying`: Did you mean `copyfile`?

scrapy (https://github.com/scrapy/scrapy)
- error[unresolved-attribute] scrapy/item.py:38:27: Type `type` has no attribute `_class`
+ error[unresolved-attribute] scrapy/item.py:38:27: Type `type` has no attribute `_class`: Did you mean `__class__`?
- error[unresolved-attribute] tests/test_spiderstate.py:22:13: Type `Spider` has no attribute `state`
+ error[unresolved-attribute] tests/test_spiderstate.py:22:13: Type `Spider` has no attribute `state`: Did you mean `start`?
- error[unresolved-attribute] tests/test_spiderstate.py:23:13: Type `Spider` has no attribute `state`
+ error[unresolved-attribute] tests/test_spiderstate.py:23:13: Type `Spider` has no attribute `state`: Did you mean `start`?
- error[unresolved-attribute] tests/test_spiderstate.py:29:20: Type `Spider` has no attribute `state`
+ error[unresolved-attribute] tests/test_spiderstate.py:29:20: Type `Spider` has no attribute `state`: Did you mean `start`?
- error[unresolved-attribute] tests/test_spiderstate.py:40:16: Type `Spider` has no attribute `state`
+ error[unresolved-attribute] tests/test_spiderstate.py:40:16: Type `Spider` has no attribute `state`: Did you mean `start`?

discord.py (https://github.com/Rapptz/discord.py)
- error[unresolved-attribute] discord/ext/commands/hybrid.py:201:9: Type `<module 'discord.app_commands'>` has no attribute `transformers`
+ error[unresolved-attribute] discord/ext/commands/hybrid.py:201:9: Type `<module 'discord.app_commands'>` has no attribute `transformers`: Did you mean `Transformer`?

pwndbg (https://github.com/pwndbg/pwndbg)
- error[unresolved-attribute] pwndbg/aglib/disasm/arch.py:605:12: Type `<module 'pwndbg.aglib.arch'>` has no attribute `syscall_abi`
+ error[unresolved-attribute] pwndbg/aglib/disasm/arch.py:605:12: Type `<module 'pwndbg.aglib.arch'>` has no attribute `syscall_abi`: Did you mean `SyscallABI`?
- error[unresolved-attribute] pwndbg/aglib/disasm/arch.py:607:41: Type `<module 'pwndbg.aglib.arch'>` has no attribute `syscall_abi`
+ error[unresolved-attribute] pwndbg/aglib/disasm/arch.py:607:41: Type `<module 'pwndbg.aglib.arch'>` has no attribute `syscall_abi`: Did you mean `SyscallABI`?
- error[unresolved-attribute] pwndbg/arguments.py:66:15: Type `<module 'pwndbg.aglib.arch'>` has no attribute `syscall_abi`
+ error[unresolved-attribute] pwndbg/arguments.py:66:15: Type `<module 'pwndbg.aglib.arch'>` has no attribute `syscall_abi`: Did you mean `SyscallABI`?
- error[unresolved-attribute] pwndbg/commands/__init__.py:420:48: Type `<module 'pwndbg.dbg'>` has no attribute `name`
+ error[unresolved-attribute] pwndbg/commands/__init__.py:420:48: Type `<module 'pwndbg.dbg'>` has no attribute `name`: Did you mean `Frame`?
- error[unresolved-attribute] pwndbg/commands/__init__.py:424:55: Type `<module 'pwndbg.dbg'>` has no attribute `name`
+ error[unresolved-attribute] pwndbg/commands/__init__.py:424:55: Type `<module 'pwndbg.dbg'>` has no attribute `name`: Did you mean `Frame`?
- error[unresolved-attribute] pwndbg/commands/__init__.py:429:51: Type `<module 'pwndbg.dbg'>` has no attribute `name`
+ error[unresolved-attribute] pwndbg/commands/__init__.py:429:51: Type `<module 'pwndbg.dbg'>` has no attribute `name`: Did you mean `Frame`?
- error[unresolved-attribute] pwndbg/commands/__init__.py:433:55: Type `<module 'pwndbg.dbg'>` has no attribute `name`
+ error[unresolved-attribute] pwndbg/commands/__init__.py:433:55: Type `<module 'pwndbg.dbg'>` has no attribute `name`: Did you mean `Frame`?
- error[unresolved-attribute] pwndbg/commands/procinfo.py:276:33: Type `Process` has no attribute `ppid`
+ error[unresolved-attribute] pwndbg/commands/procinfo.py:276:33: Type `Process` has no attribute `ppid`: Did you mean `pid`?
- error[unresolved-attribute] pwndbg/commands/procinfo.py:278:32: Type `Process` has no attribute `uid`
+ error[unresolved-attribute] pwndbg/commands/procinfo.py:278:32: Type `Process` has no attribute `uid`: Did you mean `pid`?
- error[unresolved-attribute] pwndbg/commands/procinfo.py:279:32: Type `Process` has no attribute `gid`
+ error[unresolved-attribute] pwndbg/commands/procinfo.py:279:32: Type `Process` has no attribute `gid`: Did you mean `pid`?
- error[unresolved-attribute] pwndbg/dbg/lldb/__init__.py:1769:9: Type `<module 'pwndbg.commands'>` has no attribute `comments`
+ error[unresolved-attribute] pwndbg/dbg/lldb/__init__.py:1769:9: Type `<module 'pwndbg.commands'>` has no attribute `comments`: Did you mean `commands`?
- error[unresolved-attribute] pwndbg/lib/tips.py:96:8: Type `<module 'pwndbg.dbg'>` has no attribute `name`
+ error[unresolved-attribute] pwndbg/lib/tips.py:96:8: Type `<module 'pwndbg.dbg'>` has no attribute `name`: Did you mean `Frame`?
- error[unresolved-attribute] pwndbg/lib/tips.py:98:10: Type `<module 'pwndbg.dbg'>` has no attribute `name`
+ error[unresolved-attribute] pwndbg/lib/tips.py:98:10: Type `<module 'pwndbg.dbg'>` has no attribute `name`: Did you mean `Frame`?

operator (https://github.com/canonical/operator)
- error[unresolved-attribute] ops/_private/harness.py:405:24: Type `property` has no attribute `Model`
+ error[unresolved-attribute] ops/_private/harness.py:405:24: Type `property` has no attribute `Model`: Did you mean `fdel`?

paasta (https://github.com/yelp/paasta)
- error[unresolved-attribute] paasta_tools/api/api.py:331:16: Type `<module 'sys'>` has no attribute `_argv`
+ error[unresolved-attribute] paasta_tools/api/api.py:331:16: Type `<module 'sys'>` has no attribute `_argv`: Did you mean `argv`?
- error[unresolved-attribute] paasta_tools/contrib/emit_allocated_cpu_metrics.py:36:16: Type `InstanceConfig & <Protocol with members 'get_instances'>` has no attribute `get_max_instances`
+ error[unresolved-attribute] paasta_tools/contrib/emit_allocated_cpu_metrics.py:36:16: Type `InstanceConfig & <Protocol with members 'get_instances'>` has no attribute `get_max_instances`: Did you mean `get_instance`?
- error[unresolved-attribute] paasta_tools/contrib/render_template.py:53:31: Type `<module 'os'>` has no attribute `errno`
+ error[unresolved-attribute] paasta_tools/contrib/render_template.py:53:31: Type `<module 'os'>` has no attribute `errno`: Did you mean `error`?

pywin32 (https://github.com/mhammond/pywin32)
- error[unresolved-attribute] Pythonwin/pywin/Demos/dibdemo.py:39:15: Type `<module 'win32ui'>` has no attribute `CreateDIBitmap`
+ error[unresolved-attribute] Pythonwin/pywin/Demos/dibdemo.py:39:15: Type `<module 'win32ui'>` has no attribute `CreateDIBitmap`: Did you mean `CreateBitmap`?
- error[unresolved-attribute] Pythonwin/pywin/Demos/dibdemo.py:55:20: Type `<module 'win32ui'>` has no attribute `CreateDoc`
+ error[unresolved-attribute] Pythonwin/pywin/Demos/dibdemo.py:55:20: Type `<module 'win32ui'>` has no attribute `CreateDoc`: Did you mean `CreateDC`?
- error[unresolved-attribute] Pythonwin/pywin/Demos/dlgtest.py:136:32: Type `<module 'win32ui'>` has no attribute `IDD_DEBUGGER`
+ error[unresolved-attribute] Pythonwin/pywin/Demos/dlgtest.py:136:32: Type `<module 'win32ui'>` has no attribute `IDD_DEBUGGER`: Did you mean `IDR_DEBUGGER`?
- error[unresolved-attribute] Pythonwin/pywin/Demos/dlgtest.py:137:26: Type `<module 'win32ui'>` has no attribute `IDC_DBG_RADIOSTACK`
+ error[unresolved-attribute] Pythonwin/pywin/Demos/dlgtest.py:137:26: Type `<module 'win32ui'>` has no attribute `IDC_DBG_RADIOSTACK`: Did you mean `IDC_DBG_STACK`?
- error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:616:21: Type `PyCWnd` has no attribute `ShowControlBar`
+ error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:616:21: Type `PyCWnd` has no attribute `ShowControlBar`: Did you mean `ShowScrollBar`?
- error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:918:17: Type `PyCWnd` has no attribute `ShowControlBar`
+ error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:918:17: Type `PyCWnd` has no attribute `ShowControlBar`: Did you mean `ShowScrollBar`?
- error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:922:9: Type `PyCWnd` has no attribute `ShowControlBar`
+ error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:922:9: Type `PyCWnd` has no attribute `ShowControlBar`: Did you mean `ShowScrollBar`?
- error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:970:30: Type `PyCWnd` has no attribute `IsWindowEnabled`
+ error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:970:30: Type `PyCWnd` has no attribute `IsWindowEnabled`: Did you mean `IsWindowVisible`?
- error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:976:40: Type `PyCWnd` has no attribute `IsWindowEnabled`
+ error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:976:40: Type `PyCWnd` has no attribute `IsWindowEnabled`: Did you mean `IsWindowVisible`?
- error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:1003:25: Type `PyCWinApp` has no attribute `GetDocTemplateList`
+ error[unresolved-attribute] Pythonwin/pywin/debugger/debugger.py:1003:25: Type `PyCWinApp` has no attribute `GetDocTemplateList`: Did you mean `GetDocTemplatelist`?
- error[unresolved-attribute] Pythonwin/pywin/framework/dbgcommands.py:188:13: Type `PyCWnd` has no attribute `ShowControlBar`
+ error[unresolved-attribute] Pythonwin/pywin/framework/dbgcommands.py:188:13: Type `PyCWnd` has no attribute `ShowControlBar`: Did you mean `ShowScrollBar`?
- error[unresolved-attribute] Pythonwin/pywin/framework/editor/ModuleBrowser.py:168:25: Type `<module 'pyclbr'>` has no attribute `_modules`
+ error[unresolved-attribute] Pythonwin/pywin/framework/editor/ModuleBrowser.py:168:25: Type `<module 'pyclbr'>` has no attribute `_modules`: Did you mean `__module__`?
- error[unresolved-attribute] Pythonwin/pywin/framework/interact.py:806:16: Type `PyCWnd` has no attribute `GetActiveView`
+ error[unresolved-attribute] Pythonwin/pywin/framework/interact.py:806:16: Type `PyCWnd` has no attribute `GetActiveView`: Did you mean `SetActiveWindow`?
- error[unresolved-attribute] Pythonwin/pywin/framework/interact.py:807:17: Type `PyCWnd` has no attribute `SetActiveView`
+...*[Comment body truncated]*

---

_Comment by @codspeed-hq[bot] on 2025-06-18 13:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Freapply-levenshtein-2?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #18751 will **degrade performances by 10.22%**

<sub>Comparing <code>alex/reapply-levenshtein-2</code> (592f30f) with <code>main</code> (2a425e4)</sub>



### Summary

`❌ 1` regression  
`✅ 36` untouched  


> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Freapply-levenshtein-2?utm_source=github&utm_medium=comment&utm_content=acknowledge)._

### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ❌ | Simulation | [`` hydra-zen ``](https://codspeed.io/astral-sh/ruff/branches/alex%2Freapply-levenshtein-2?uri=crates%2Fruff_benchmark%2Fbenches%2Fty.rs%3A%3Aproject%3A%3Ahydra%3A%3Aproject%3A%3Ahydra-zen&runnerMode=Instrumentation&utm_source=github&utm_medium=comment&utm_content=benchmark) | 719.7 ms | 801.7 ms | -10.22% |


---

_Comment by @MichaReiser on 2025-06-18 13:54_

> ## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Freapply-levenshtein-2)
> ### Merging #18751 will **degrade performances by 10.65%**
> 
> Comparing `alex/reapply-levenshtein-2` ([b845698](https://github.com/astral-sh/ruff/commit/b845698ce3b1dbdf2b0793f38216eedfdb39db4e)) with `main` ([1188ffc](https://github.com/astral-sh/ruff/commit/1188ffccc4b8fc25ad5b459ac77b9189b225dc6f))
> ### Summary
> 
> `❌ 1` regressions `✅ 36` untouched benchmarks
> 
> > ⚠️ _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/alex%2Freapply-levenshtein-2)._
> 
> ### Benchmarks breakdown
> 	Benchmark 	`BASE` 	`HEAD` 	Change
> ❌ 	`hydra-zen` 	719.7 ms 	805.5 ms 	-10.65%

Nice! (Sorry Alex)

---

_Comment by @AlexWaygood on 2025-06-18 13:55_

> Nice! (Sorry Alex)

No, this is great!! It's fantastic that we're aware of this now!

---

_Comment by @MichaReiser on 2025-06-18 13:55_

It might also just be that we issue this diagnostic now and do more work because of it?

---

_Comment by @AlexWaygood on 2025-06-18 13:58_

yeah this might be an "acceptable" regression if it only happens because there's a lot of unresolved imports and unresolved attributes in hydra-zen (and we now compute suggestions for every one of them). The easy way to speed ty up in that situation is to either fix the type errors or suppress them (either in configuration or suppression comments).

But I'll nonetheless (1) see if I can repro the regression locally and (2) see if there are easy ways of speeding it up.

---

_Comment by @AlexWaygood on 2025-06-18 14:02_

> ## `mypy_primer` results
> Changes were detected when running on open source projects

I spot-checked the suggestions being given in the diagnostics here. Most of them seem self-evidently good! Some of them are a bit surprising to me, e.g. this one:

> ```diff
> + error[unresolved-attribute] dacite/types.py:17:16: Type `type` has no attribute `__extra__`: Did you mean `__str__`?
> ```

But all the surprising ones seem to match CPython's behaviour, so I think this is fine for an initial version of the feature. CPython's implementation of this feature has been very popular with users and doesn't get many bug reports these days.

---

_@MichaReiser reviewed on 2025-06-18 14:04_

One problem here could be that `extend_with_instance_members` calls `semantic_index` of another file.... which adds a cross-file dependency. `extend_with_instance_members` or some parent should probably be a salsa query

---

_Comment by @AlexWaygood on 2025-06-18 16:59_

> One problem here could be that `extend_with_instance_members` calls `semantic_index` of another file.... which adds a cross-file dependency. `extend_with_instance_members` or some parent should probably be a salsa query

I made that change in https://github.com/astral-sh/ruff/pull/18751/commits/2a0e9d42daf4036e38057fd225b78992ad8abc07, but it doesn't seem to help with the performance regression locally (and the regression is easily reproducible locally)

---

_Comment by @AlexWaygood on 2025-06-18 18:00_

The slowdown on the hydra-zen benchmark is due to us attempting to calculate suggestions for `unresolved-attribute` diagnostics. If I run ty on hydra-zen with the same setup that we have in the benchmarks, there are 6 `unresolved-attribute` diagnostics but no `unresolved-import` diagnostics.

We call `all_members` on only two types during the run: a nominal-instance type representing `types.ModuleType` (which we calculate as having 29 members) and a module-literal type representing `<module 'pydantic'>` (which we calculate as having 187 members).

Of the six `unresolved-attribute` diagnostics issued when checking hydra-zen, one concerns pydantic:

```
error[unresolved-attribute]: Type `<module 'pydantic'>` has no attribute `validate_arguments`
  --> src/hydra_zen/third_party/pydantic.py:20:23
   |
18 |     )
19 | else:  # pragma: no cover
20 |     _default_parser = _pyd.validate_arguments(
   |                       ^^^^^^^^^^^^^^^^^^^^^^^
21 |         config={"arbitrary_types_allowed": True, "validate_return": False}  # type: ignore
22 |     )
   |
info: rule `unresolved-attribute` was selected on the command line
```

And the entire slowdown on the benchmark is due to us attempting to calculate "Did you mean...?" suggestions for this diagnostic. This can be seen by the fact that applying this diff to my PR branch fixes the regression completely for me locally:

```diff
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index eed93853a5..cee63226c1 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -6574,12 +6574,14 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                                 HideUnderscoredSuggestions::Yes
                             };
 
-                            if let Some(suggestion) =
-                                find_best_suggestion_for_unresolved_member(db, value_type, &attr.id, underscore_policy)
-                            {
-                                diagnostic.set_primary_message(format_args!(
-                                    "Did you mean `{suggestion}`?",
-                                ));
+                            if value_type.into_module_literal().is_none_or(|module|module.module(self.db()).name() != "pydantic") {
+                                if let Some(suggestion) =
+                                    find_best_suggestion_for_unresolved_member(db, value_type, &attr.id, underscore_policy)
+                                {
+                                    diagnostic.set_primary_message(format_args!(
+                                        "Did you mean `{suggestion}`?",
+                                    ));
+                                }
                             }
                         }
                     }
```

The specific thing that appears to be expensive about calculating the "Did you mean...?" suggestion for this diagnostic is calculating the list of members the `<module 'pydantic'>` type has. This can be seen by the fact that this diff has no impact on the regression -- if it was calculating the Levenshtein difference of each member that caused the regression, we would expect this diff to fix the regression, since `<module 'pydantic'>` has >128 members:

```diff
diff --git a/crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs b/crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs
index 4bd4a2c8c8..74031ff047 100644
--- a/crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs
+++ b/crates/ty_python_semantic/src/types/diagnostic/levenshtein.rs
@@ -63,7 +63,7 @@ where
 
     // Don't spend a *huge* amount of time computing suggestions if there are many candidates.
     // This limit is fairly arbitrary and can be adjusted as needed.
-    if options.len() > 4096 {
+    if options.len() > 128 {
         return None;
     }
```

I can't see any obvious way of optimizing `all_members()`, except for adding Salsa caching to ensure that it's never computed twice for any given type: but this PR already does that. As such, I think if we want this feature then we have to eat the regression.

I still think the feature is worth the regression, since `unresolved-import` diagnostics and `unresolved-attribute` diagnostics should both be rare in a real-world context when a user is actually running ty: you'd either fix the error or suppress it somehow, and both of those actions would avoid us hitting this slow path. If we do end up emitting an `unresolved-import` or `unresolved-attribute` diagnostic, however, the suggestions created by this feature will be very helpful for users IMO.

---

_Renamed from "Revert " Revert "[ty] Offer "Did you mean...?" suggestions for unreso…" to "Reapply "[ty] Offer "Did you mean...?" suggestions for unresolved `from` imports and unresolved attributes (#18705)"" by @AlexWaygood on 2025-06-18 18:13_

---

_Marked ready for review by @AlexWaygood on 2025-06-18 18:14_

---

_Review requested from @carljm by @AlexWaygood on 2025-06-18 18:14_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-18 18:14_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-18 18:14_

---

_Label `diagnostics` added by @AlexWaygood on 2025-06-18 18:16_

---

_Comment by @carljm on 2025-06-18 18:49_

I do think (as I mentioned on the first iteration of the PR) that a likely consequence here is that our performance will look dramatically worse on real-world benchmarks on some codebases that haven't been adapted to pass diagnostic-free on ty. I agree with your analysis that in the long run perhaps this shouldn't concern us, but I think "looking slow" is a valid short-term concern. I think it might be sufficiently concerning that it would be worth adding a configuration option to disable this feature, so that at least we can tell people who say "ty is slow on my codebase", "hey, maybe try with this option and see if it helps."

---

_Comment by @AlexWaygood on 2025-06-18 18:57_

> I think it might be sufficiently concerning that it would be worth adding a configuration option to disable this feature, so that at least we can tell people who say "ty is slow on my codebase", "hey, maybe try with this option and see if it helps."

That makes sense to me — I'm open to that if others are!

Would we want to just suppress "Did you mean...?" subdiagnostics specifically with this option (on the grounds that it's the only case where we have direct evidence that computing the subdiagnostic information can negatively impact performance) or would we want to suppress other (all?) subdiagnostics, on the grounds that lots of them do additional computation that could slow us down?

Would you want this CLI option implemented as part of this PR or as a followup?

---

_Comment by @carljm on 2025-06-18 19:02_

Maybe that option already exists, and it's spelled `--concise`? Do we already avoid doing work to generate extra sub-diagnostics that won't be shown by `--concise`? If not, maybe that's the follow-up? It seems like ideally it would apply generally, but seems like maybe this diagnostic is the most expensive sub-diagnostic we have so far.

I'm ok with that being done as follow-up, not in this PR.

---

_Comment by @AlexWaygood on 2025-06-18 19:08_

> Do we already avoid doing work to generate extra sub-diagnostics that won't be shown by `--concise`? If not, maybe that's the follow-up? It seems like ideally it would apply generally, but seems like maybe this diagnostic is the most expensive sub-diagnostic we have so far.

I think we currently do the same work whether `--output=format=concise` was specified or not, but I agree that it would make sense to do less work if `--output-format=concise` has been specified, yeah

---

_Comment by @MichaReiser on 2025-06-18 20:01_

The challenge with the output format is that a project can use consise but we still want to show all/more information in the lsp. So I don't think this will work. (Too late here to think about alternatives)

---

_Comment by @MichaReiser on 2025-06-18 20:33_

The general problem with the suggestion here is that computing it can take an arbitrary long time, depending on how large the object is (it could be seconds!) 

I wonder if we should time box the computation or if there's some other way to limit the runaway risk.

Either way; While very nice, I'm not sure if we should prioritize this right now 

---

_Comment by @AlexWaygood on 2025-06-18 22:42_

> The general problem with the suggestion here is that computing it can take an arbitrary long time, depending on how large the object is (it could be seconds!)

Right — though since the expensive API (`all_members()`) is also used by the autocompletions logic, this is to some extent a pre-existing issue. Finding the list of candidates for autocompletions could also take several seconds if the cursor is at exactly the wrong place in exactly the wrong file.

---

_Comment by @MichaReiser on 2025-06-19 09:42_

> Right — though since the expensive API (all_members()) is also used by the autocompletions logic, this is to some extent a pre-existing issue. Finding the list of candidates for autocompletions could also take several seconds if the cursor is at exactly the wrong place in exactly the wrong file.

That's somewhat different because it's okay for LSP actions to take longer. The client will send a cancellation request if the response is now outdated. So it's okay if LSP actions take longer (they're also in response to a user action). But obviously, they would ideally also not cause a run-away like this. 

---
