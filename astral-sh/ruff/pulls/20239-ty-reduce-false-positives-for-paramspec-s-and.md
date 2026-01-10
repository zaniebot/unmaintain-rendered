```yaml
number: 20239
title: "[ty] Reduce false positives for `ParamSpec`s and `TypeVarTuple`s"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/paramspec-declarations
created_at: 2025-09-04T15:26:25Z
updated_at: 2025-09-04T22:34:38Z
url: https://github.com/astral-sh/ruff/pull/20239
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Reduce false positives for `ParamSpec`s and `TypeVarTuple`s

---

_Pull request opened by @AlexWaygood on 2025-09-04 15:26_

## Summary

Helps with https://github.com/astral-sh/ty/issues/157#issuecomment-3253068088. We have existing workarounds to avoid emitting false positives if the inferred type of a variable is `ParamSpec` or `TypeVarTuple`, but these don't work if a `ParamSpec` or a `TypeVarTuple` from the global scope is used in an inner function, since the type of these variables seen by the nested scope will be `ParamSpec | Unknown` (or `TypeVarTuple | Unknown`). This can be fixed fairly easily by using `any_over_type()` to search for types that we don't support yet.

## Test Plan

Added an mdtest that uses the examples from https://github.com/astral-sh/ty/issues/157#issuecomment-3253068088

---

_Label `ty` added by @AlexWaygood on 2025-09-04 15:26_

---

_Comment by @github-actions[bot] on 2025-09-04 15:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-09-04 15:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/_backends/_asyncio.py:2224:24: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/anyio/_backends/_trio.py:922:24: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 222 diagnostics
+ Found 220 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/__init__.py:148:42: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/core/__init__.py:148:73: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/core/__init__.py:202:50: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/core/__init__.py:202:69: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/core/__init__.py:1316:66: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/core/_raw/__init__.py:89:27: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:363:25: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:409:25: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:519:26: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:609:26: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:687:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Unknown`
+ src/antidote/lib/interface_ext/__init__.py:687:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__wrapped__`
- src/antidote/lib/interface_ext/__init__.py:687:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:708:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[(...) -> Unknown]`
+ src/antidote/lib/interface_ext/__init__.py:708:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[(...) -> Out@single]`
- src/antidote/lib/interface_ext/__init__.py:708:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:738:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[Sequence[(...) -> Unknown]]`
+ src/antidote/lib/interface_ext/__init__.py:738:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Dependency[Sequence[(...) -> Out@all]]`
- src/antidote/lib/interface_ext/__init__.py:738:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:763:47: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Unknown`
+ src/antidote/lib/interface_ext/__init__.py:763:47: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__antidote_dependency_hint__`
- src/antidote/lib/interface_ext/__init__.py:763:56: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:776:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Unknown`
+ src/antidote/lib/interface_ext/__init__.py:776:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__wrapped__`
- src/antidote/lib/interface_ext/__init__.py:776:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:800:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Unknown`
+ src/antidote/lib/interface_ext/__init__.py:800:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Dependency[Out@single]`
- src/antidote/lib/interface_ext/__init__.py:800:19: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:830:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Unknown`
+ src/antidote/lib/interface_ext/__init__.py:830:10: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Dependency[Sequence[Out@all]]`
- src/antidote/lib/interface_ext/__init__.py:830:19: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:1178:38: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:1211:40: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:1226:40: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:1253:40: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:1303:54: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:1307:41: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:1335:54: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/__init__.py:1339:41: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
+ src/antidote/lib/interface_ext/_function.py:76:66: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/antidote/lib/interface_ext/_function.py:40:23: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:43:44: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:45:45: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:52:56: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:53:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:60:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:61:45: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:73:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:93:23: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:97:24: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:99:45: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:112:19: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_function.py:138:19: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_interface.py:84:25: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_interface.py:131:25: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_interface.py:230:26: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/interface_ext/_interface.py:290:26: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/lazy_ext/__init__.py:210:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/lazy_ext/__init__.py:222:26: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/lazy_ext/__init__.py:584:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Unknown`
+ src/antidote/lib/lazy_ext/__init__.py:584:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `(...) -> Out@__wrapped__`
- src/antidote/lib/lazy_ext/__init__.py:584:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/lazy_ext/__init__.py:635:54: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/lazy_ext/__init__.py:639:41: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/lazy_ext/_lazy.py:104:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/lazy_ext/_lazy.py:116:26: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/antidote/lib/lazy_ext/_lazy.py:259:37: error[invalid-argument-type] `Unknown | ParamSpec` is not a valid argument to `Generic`
- src/antidote/lib/lazy_ext/_lazy.py:314:35: error[invalid-argument-type] `Unknown | ParamSpec` is not a valid argument to `Generic`
- src/antidote/lib/lazy_ext/_lazy.py:369:37: error[invalid-argument-type] `Unknown | ParamSpec` is not a valid argument to `Generic`
- src/antidote/lib/lazy_ext/_lazy.py:388:28: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 382 diagnostics
+ Found 330 diagnostics

starlette (https://github.com/encode/starlette)
- starlette/authentication.py:39:24: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- starlette/authentication.py:40:19: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- starlette/background.py:19:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- starlette/background.py:36:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 149 diagnostics
+ Found 145 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/wrapper/_implementations.py:195:29: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/hydra_zen/wrapper/_implementations.py:433:24: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/hydra_zen/wrapper/_implementations.py:832:26: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 567 diagnostics
+ Found 564 diagnostics

trio (https://github.com/python-trio/trio)
- src/trio/_core/_tests/test_io.py:444:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/trio/_deprecate.py:87:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/trio/_deprecate.py:87:56: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/trio/_path.py:36:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/trio/_path.py:36:51: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/trio/testing/_check_streams.py:293:27: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 681 diagnostics
+ Found 675 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
- discord/utils.py:298:41: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- discord/utils.py:298:60: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 527 diagnostics
+ Found 525 diagnostics

optuna (https://github.com/optuna/optuna)
- optuna/_convert_positional_args.py:83:45: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- optuna/_convert_positional_args.py:83:68: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- optuna/_convert_positional_args.py:99:24: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ optuna/_convert_positional_args.py:99:24: error[unresolved-attribute] Type `(...) -> _T@convert_positional_args` has no attribute `__name__`
- optuna/_convert_positional_args.py:107:39: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ optuna/_convert_positional_args.py:107:39: error[unresolved-attribute] Type `(...) -> _T@convert_positional_args` has no attribute `__name__`
- optuna/_convert_positional_args.py:120:24: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ optuna/_convert_positional_args.py:120:24: error[unresolved-attribute] Type `(...) -> _T@convert_positional_args` has no attribute `__name__`
- optuna/_convert_positional_args.py:130:24: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ optuna/_convert_positional_args.py:130:24: error[unresolved-attribute] Type `(...) -> _T@convert_positional_args` has no attribute `__name__`
- optuna/_deprecated.py:89:35: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- optuna/_deprecated.py:89:58: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- optuna/_deprecated.py:107:53: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ optuna/_deprecated.py:107:53: error[unresolved-attribute] Type `(...) -> FT@deprecated_func` has no attribute `__name__`
- optuna/_experimental.py:66:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- optuna/_experimental.py:66:55: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- optuna/_experimental.py:74:46: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__qualname__`
+ optuna/_experimental.py:74:46: error[unresolved-attribute] Type `(...) -> FT@experimental_func` has no attribute `__qualname__`
- Found 560 diagnostics
+ Found 554 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/aglib/kernel/__init__.py:65:31: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/kernel/__init__.py:65:50: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/kernel/__init__.py:77:29: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ pwndbg/aglib/kernel/__init__.py:77:29: error[unresolved-attribute] Type `(...) -> T@requires_debug_symbols` has no attribute `__name__`
- pwndbg/aglib/kernel/__init__.py:86:31: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/kernel/__init__.py:86:50: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/kernel/__init__.py:97:41: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ pwndbg/aglib/kernel/__init__.py:97:41: error[unresolved-attribute] Type `(...) -> T@requires_debug_info` has no attribute `__name__`
- pwndbg/aglib/proc.py:142:46: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/proc.py:142:65: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/proc.py:151:49: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/proc.py:151:68: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/proc.py:162:29: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/proc.py:162:46: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/proc.py:170:42: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/aglib/proc.py:170:61: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
+ pwndbg/commands/got.py:144:28: warning[possibly-unbound-attribute] Attribute `items` on type `Unknown | None` is possibly unbound
- pwndbg/decorators.py:33:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/decorators.py:33:53: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/decorators.py:55:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/decorators.py:55:53: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/decorators.py:62:73: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ pwndbg/decorators.py:62:73: error[unresolved-attribute] Type `(...) -> T@suppress_errors` has no attribute `__name__`
- pwndbg/integration/binja.py:161:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/integration/binja.py:161:53: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/lib/cache.py:33:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/lib/cache.py:38:57: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ pwndbg/lib/cache.py:38:57: error[unresolved-attribute] Type `(...) -> T@__init__` has no attribute `__name__`
- pwndbg/lib/cache.py:137:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/lib/cache.py:137:49: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/lib/cache.py:140:37: error[unresolved-attribute] Type `((...) -> Unknown) & <Protocol with members 'cache'>` has no attribute `__name__`
+ pwndbg/lib/cache.py:140:37: error[unresolved-attribute] Type `((...) -> T@cache_until) & <Protocol with members 'cache'>` has no attribute `__name__`
- pwndbg/lib/cache.py:167:62: error[unresolved-attribute] Type `((...) -> Unknown) & ~<Protocol with members 'cache'>` has no attribute `__name__`
+ pwndbg/lib/cache.py:167:62: error[unresolved-attribute] Type `((...) -> T@cache_until) & ~<Protocol with members 'cache'>` has no attribute `__name__`
- pwndbg/wrappers/__init__.py:32:43: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/wrappers/__init__.py:32:62: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- pwndbg/wrappers/__init__.py:33:9: error[unresolved-attribute] Unresolved attribute `cmd` on type `(...) -> Unknown`.
+ pwndbg/wrappers/__init__.py:33:9: error[unresolved-attribute] Unresolved attribute `cmd` on type `(...) -> T@__call__`.
- Found 2503 diagnostics
+ Found 2481 diagnostics

xarray (https://github.com/pydata/xarray)
- xarray/core/common.py:740:66: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 1673 diagnostics
+ Found 1672 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- scrapy/utils/asyncio.py:118:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- scrapy/utils/asyncio.py:119:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- scrapy/utils/decorators.py:32:29: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- scrapy/utils/decorators.py:32:50: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- scrapy/utils/decorators.py:35:54: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ scrapy/utils/decorators.py:35:54: error[unresolved-attribute] Type `(...) -> _T@deprecated` has no attribute `__name__`
- scrapy/utils/reactor.py:55:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- scrapy/utils/reactor.py:56:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 1063 diagnostics
+ Found 1057 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- mitmproxy/utils/signals.py:69:42: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- mitmproxy/utils/signals.py:73:45: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- mitmproxy/utils/signals.py:82:42: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- mitmproxy/utils/signals.py:85:45: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 1824 diagnostics
+ Found 1820 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/_py/error.py:82:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/_pytest/_py/error.py:111:26: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ src/_pytest/_py/error.py:111:26: error[unresolved-attribute] Type `(...) -> R@checked_call` has no attribute `__name__`
- Found 469 diagnostics
+ Found 468 diagnostics

sphinx (https://github.com/sphinx-doc/sphinx)
- sphinx/util/display.py:93:36: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- sphinx/util/display.py:93:55: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 518 diagnostics
+ Found 516 diagnostics

meson (https://github.com/mesonbuild/meson)
- unittests/helpers.py:41:30: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:41:51: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:76:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:76:55: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:104:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:104:55: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:120:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:120:55: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:136:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:136:55: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:230:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- unittests/helpers.py:230:55: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 810 diagnostics
+ Found 798 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- src/prefect/_internal/compatibility/async_dispatch.py:79:36: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/compatibility/async_dispatch.py:82:27: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/compatibility/async_dispatch.py:83:19: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/compatibility/deprecated.py:108:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/compatibility/deprecated.py:108:51: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/compatibility/deprecated.py:179:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/compatibility/deprecated.py:179:51: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/compatibility/deprecated.py:181:48: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ src/prefect/_internal/compatibility/deprecated.py:181:48: error[unresolved-attribute] Type `(...) -> R@deprecated_parameter` has no attribute `__name__`
- src/prefect/_internal/retries.py:50:24: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/retries.py:51:19: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/_internal/retries.py:54:38: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ src/prefect/_internal/retries.py:54:38: error[unresolved-attribute] Type `(...) -> Coroutine[Any, Any, R@retry_async_fn]` has no attribute `__name__`
- src/prefect/assets/materialize.py:37:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:203:22: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:278:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:282:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:288:34: warning[possibly-unbound-attribute] Attribute `__name__` on type `(((...) -> Unknown) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> Unknown) & ((...) -> object) & ~staticmethod) | (((...) -> Unknown) & ((...) -> object))` is possibly unbound
+ src/prefect/flows.py:288:34: warning[possibly-unbound-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object) & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object))` is possibly unbound
- src/prefect/flows.py:405:68: warning[possibly-unbound-attribute] Attribute `__name__` on type `(((...) -> Unknown) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> Unknown) & ((...) -> object) & ~staticmethod) | (((...) -> Unknown) & ((...) -> object))` is possibly unbound
+ src/prefect/flows.py:405:68: warning[possibly-unbound-attribute] Attribute `__name__` on type `(((...) -> R@__init__) & ((...) -> object) & ~classmethod & ~staticmethod) | (@Todo(specialized non-generic class) & ((...) -> object) & ~classmethod & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object) & ~staticmethod) | (((...) -> R@__init__) & ((...) -> object))` is possibly unbound
- src/prefect/flows.py:1771:39: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:1797:29: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:1823:29: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:1827:33: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:1848:47: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/flows.py:1988:36: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/server/database/dependencies.py:276:40: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/server/database/dependencies.py:330:40: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/tasks.py:375:22: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/tasks.py:439:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/tasks.py:443:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/tasks.py:2053:32: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- src/prefect/tasks.py:2096:22: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- Found 3021 diagnostics
+ Found 2995 diagnostics

zulip (https://github.com/zulip/zulip)
- corporate/tests/test_stripe.py:358:51: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- corporate/tests/test_stripe.py:358:81: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- corporate/tests/test_stripe.py:366:21: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ corporate/tests/test_stripe.py:366:21: error[unresolved-attribute] Type `(...) -> ReturnT@mock_stripe` has no attribute `__name__`
- corporate/tests/test_stripe.py:369:51: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ corporate/tests/test_stripe.py:369:51: error[unresolved-attribute] Type `(...) -> ReturnT@mock_stripe` has no attribute `__name__`
- zerver/lib/cache.py:160:34: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- zerver/lib/cache.py:160:64: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- zerver/lib/cache.py:821:33: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- zerver/openapi/python_examples.py:50:37: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- zerver/openapi/python_examples.py:50:67: error[invalid-type-form] The first argument to `Callable` must be either a list of types, ParamSpec, Concatenate, or `...`
- zerver/openapi/python_examples.py:53:39: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ zerver/openapi/python_examples.py:53:39: error[unresolved-attribute] Type `(...) -> ReturnT@openapi_test_function` has no attribute `__name__`
- zerver/openapi/python_examples.py:56:39: error[unresolved-attribute] Type `(...) -> Unknown` has no attribute `__name__`
+ zerver/openapi/python_examples.py:56:39: error[unresolved-attribute] Type `(...) -> ReturnT@openapi_test_function` has no attribute `__name__`
- Found 2631 diagnostics
+ Found 2624 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-09-04 15:59_

---

_Review requested from @carljm by @AlexWaygood on 2025-09-04 15:59_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-09-04 15:59_

---

_Review requested from @dcreager by @AlexWaygood on 2025-09-04 15:59_

---

_Renamed from "[ty] Reduce false positives for `ParamSpec`s and `TypedDict`s" to "[ty] Reduce false positives for `ParamSpec`s and `TypedVarTuple`s" by @AlexWaygood on 2025-09-04 15:59_

---

_Renamed from "[ty] Reduce false positives for `ParamSpec`s and `TypedVarTuple`s" to "[ty] Reduce false positives for `ParamSpec`s and `TypeVarTuple`s" by @sharkdp on 2025-09-04 17:30_

---

_@carljm approved on 2025-09-04 22:31_

---

_Merged by @AlexWaygood on 2025-09-04 22:34_

---

_Closed by @AlexWaygood on 2025-09-04 22:34_

---

_Branch deleted on 2025-09-04 22:34_

---
