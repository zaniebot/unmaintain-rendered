```yaml
number: 18021
title: "[ty] Infer parameter specializations of generic aliases"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/specialize-deepish
created_at: 2025-05-11T21:07:10Z
updated_at: 2025-05-13T02:12:46Z
url: https://github.com/astral-sh/ruff/pull/18021
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Infer parameter specializations of generic aliases

---

_Pull request opened by @dcreager on 2025-05-11 21:07_

This updates our function specialization inference to infer type mappings from parameters that are generic aliases, e.g.:

```py
def f[T](x: list[T]) -> T: ...

reveal_type(f(["a", "b"]))  # revealed: str
```

Though note that we're still inferring the type of list literals as `list[Unknown]`, so for now we actually need something like the following in our tests:

```py
def _(x: list[str]):
    reveal_type(f(x))  # revealed: str
```

---

_Review requested from @carljm by @dcreager on 2025-05-11 21:07_

---

_Label `ty` added by @dcreager on 2025-05-11 21:07_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-11 21:07_

---

_Review requested from @sharkdp by @dcreager on 2025-05-11 21:07_

---

_Comment by @github-actions[bot] on 2025-05-11 21:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
trio (https://github.com/python-trio/trio)
- error[invalid-assignment] src/trio/_core/_tests/test_run.py:2745:9: Object of type `ExceptionGroup[Exception]` is not assignable to `ValueError | ExceptionGroup[ValueError]`
+ error[no-matching-overload] src/trio/_tests/test_testing_raisesgroup.py:394:10: No overload of bound method `__init__` matches arguments
+ error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:98:5: No overload of bound method `__init__` matches arguments
+ error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:103:5: No overload of bound method `__init__` matches arguments
+ error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:119:5: No overload of bound method `__init__` matches arguments
+ error[no-matching-overload] src/trio/_tests/type_tests/raisesgroup.py:121:5: No overload of bound method `__init__` matches arguments
- warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:92:51: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:95:49: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:99:57: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:102:48: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:106:67: Unused blanket `type: ignore` directive
- warning[unused-ignore-comment] src/trio/_tests/type_tests/raisesgroup.py:107:69: Unused blanket `type: ignore` directive
- Found 1118 diagnostics
+ Found 1116 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[invalid-argument-type] porcupine/pluginloader.py:298:40: Argument to function `_decide_loading_order` is incorrect: Expected `dict[Unknown, set[Unknown]]`, found `dict[PluginInfo, set[PluginInfo]]`
- Found 98 diagnostics
+ Found 97 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[invalid-argument-type] mkosi/__init__.py:4383:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-argument-type] mkosi/__init__.py:4424:9: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-argument-type] mkosi/burn.py:41:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-argument-type] mkosi/qemu.py:1002:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-argument-type] mkosi/qemu.py:1061:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-argument-type] mkosi/qemu.py:1090:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-argument-type] mkosi/qemu.py:1641:9: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- error[invalid-argument-type] mkosi/sysupdate.py:81:13: Argument to function `run` is incorrect: Expected `Mapping[str, str]`, found `dict[str | Unknown, str | Unknown]`
- Found 326 diagnostics
+ Found 318 diagnostics

Expression (https://github.com/cognitedata/Expression)
- error[invalid-argument-type] expression/collections/map.py:186:24: Argument to function `of_list` is incorrect: Expected `list[tuple[Unknown, Unknown]]`, found `list[tuple[_Key_, _Result]]`
- error[invalid-argument-type] expression/collections/map.py:454:32: Argument to function `of_list` is incorrect: Expected `Block[tuple[Unknown, Unknown]]`, found `Block[tuple[_Key, _Value]]`
- error[invalid-argument-type] expression/extra/result/traversable.py:50:21: Argument to function `traverse` is incorrect: Expected `(Unknown, /) -> Result[Unknown, Unknown]`, found `def identity(value: _A) -> _A`
+ error[invalid-argument-type] expression/extra/result/traversable.py:50:21: Argument to function `traverse` is incorrect: Expected `(Result[_TSource, _TError], /) -> Result[Unknown, Unknown]`, found `def identity(value: _A) -> _A`
- error[no-matching-overload] tests/test_parser.py:243:12: No overload of bound method `starmap` matches arguments
- Found 305 diagnostics
+ Found 302 diagnostics

altair (https://github.com/vega/altair)
- error[invalid-argument-type] tests/vegalite/v5/test_api.py:1708:25: Argument to bound method `delitem` is incorrect: Expected `Mapping[Literal["pandas"], Unknown]`, found `dict[str, ModuleType]`
- error[invalid-argument-type] tests/vegalite/v5/test_api.py:1709:25: Argument to bound method `delitem` is incorrect: Expected `Mapping[Literal["numpy"], Unknown]`, found `dict[str, ModuleType]`
- error[invalid-argument-type] tests/vegalite/v5/test_api.py:1710:25: Argument to bound method `delitem` is incorrect: Expected `Mapping[Literal["pyarrow"], Unknown]`, found `dict[str, ModuleType]`
- Found 1563 diagnostics
+ Found 1560 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-argument-type] testing/io/test_terminalwriter.py:188:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["PY_COLORS"], Literal["1"]]`, found `_Environ[str]`
- error[invalid-argument-type] testing/io/test_terminalwriter.py:193:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["PY_COLORS"], Literal["0"]]`, found `_Environ[str]`
- error[invalid-argument-type] testing/io/test_terminalwriter.py:198:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["NO_COLOR"], Literal["1"]]`, found `_Environ[str]`
- error[invalid-argument-type] testing/io/test_terminalwriter.py:203:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["FORCE_COLOR"], Literal["1"]]`, found `_Environ[str]`
- error[invalid-argument-type] testing/io/test_terminalwriter.py:221:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["NO_COLOR"], str]`, found `_Environ[str]`
- error[invalid-argument-type] testing/io/test_terminalwriter.py:222:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["FORCE_COLOR"], str]`, found `_Environ[str]`
- error[invalid-argument-type] testing/io/test_terminalwriter.py:227:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["NO_COLOR"], Literal[""]]`, found `_Environ[str]`
- error[invalid-argument-type] testing/io/test_terminalwriter.py:228:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["FORCE_COLOR"], Literal[""]]`, found `_Environ[str]`
- error[invalid-argument-type] testing/test_config.py:1371:25: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["mytestplugin"], PseudoPlugin]`, found `dict[str, ModuleType]`
- error[invalid-argument-type] testing/test_pathlib.py:319:29: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["pointsback123"], ModuleType]`, found `dict[str, ModuleType]`
- error[invalid-argument-type] testing/test_pluginmanager.py:349:29: Argument to bound method `setitem` is incorrect: Expected `Mapping[Literal["PYTEST_PLUGINS"], Literal["xy123"]]`, found `_Environ[str]`
+ error[invalid-argument-type] testing/test_pytester.py:318:29: Argument to bound method `setitem` is incorrect: Expected `Mapping[str | Unknown, ModuleType]`, found `dict[str, ModuleType]`
+ error[invalid-argument-type] testing/test_pytester.py:329:29: Argument to bound method `setitem` is incorrect: Expected `Mapping[str | Unknown, ModuleType]`, found `dict[str, ModuleType]`
+ error[invalid-argument-type] testing/test_pytester.py:342:33: Argument to bound method `setitem` is incorrect: Expected `Mapping[str | Unknown, ModuleType]`, found `dict[str, ModuleType]`
- Found 938 diagnostics
+ Found 930 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[type-assertion-failure] tests/annotations/declarations.py:951:5: Actual type `FullBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `FullBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 650 diagnostics
+ Found 649 diagnostics

mypy (https://github.com/python/mypy)
- error[invalid-argument-type] mypy/build.py:3486:26: Argument to function `topsort` is incorrect: Expected `dict[Unknown, set[Unknown]]`, found `dict[AbstractSet[Unknown], set[AbstractSet[Unknown]]]`
+ error[invalid-return-type] mypy/expandtype.py:163:16: Return type does not match returned value: Expected `T`, found `Type`
- error[invalid-argument-type] mypy/join.py:602:56: Argument to bound method `__init__` is incorrect: Expected `set[str]`, found `set[str | Unknown]`
- error[invalid-argument-type] mypy/plugins/singledispatch.py:102:47: Argument to function `get_first_arg` is incorrect: Expected `list[list[Unknown]]`, found `list[list[Type]]`
- error[invalid-argument-type] mypy/plugins/singledispatch.py:131:52: Argument to function `get_first_arg` is incorrect: Expected `list[list[Unknown]]`, found `list[list[Type]]`
+ error[no-matching-overload] mypy/plugins/singledispatch.py:102:17: No overload of function `get_proper_type` matches arguments
+ error[no-matching-overload] mypy/plugins/singledispatch.py:131:22: No overload of function `get_proper_type` matches arguments
- error[invalid-argument-type] mypy/plugins/singledispatch.py:208:30: Argument to function `get_first_arg` is incorrect: Expected `list[list[Unknown]]`, found `list[list[Type]]`
- error[invalid-argument-type] mypy/solve.py:147:58: Argument to function `strongly_connected_components` is incorrect: Expected `dict[Unknown, list[Unknown]]`, found `dict[TypeVarId, list[TypeVarId]]`
+ error[invalid-argument-type] mypy/solve.py:147:58: Argument to function `strongly_connected_components` is incorrect: Expected `dict[Unknown | TypeVarId, list[Unknown | TypeVarId]]`, found `dict[TypeVarId, list[TypeVarId]]`
- error[invalid-argument-type] mypy/solve.py:150:32: Argument to function `topsort` is incorrect: Expected `dict[Unknown, set[Unknown]]`, found `dict[AbstractSet[Unknown], set[AbstractSet[Unknown]]]`
- error[invalid-argument-type] mypy/solve.py:150:51: Argument to function `prepare_sccs` is incorrect: Expected `dict[Unknown, list[Unknown]]`, found `dict[TypeVarId, list[TypeVarId]]`
- Found 3334 diagnostics
+ Found 3330 diagnostics

rotki (https://github.com/rotki/rotki)
- error[invalid-argument-type] rotkehlchen/accounting/structures/balance.py:165:13: Argument is incorrect: Expected `defaultdict[Asset, Balance]`, found `dict[Unknown, Unknown]`
+ error[invalid-argument-type] rotkehlchen/accounting/structures/balance.py:165:13: Argument is incorrect: Expected `defaultdict[Asset, Balance]`, found `dict[Asset, Balance]`
- error[invalid-argument-type] rotkehlchen/accounting/structures/balance.py:166:13: Argument is incorrect: Expected `defaultdict[Asset, Balance]`, found `dict[Unknown, Unknown]`
+ error[invalid-argument-type] rotkehlchen/accounting/structures/balance.py:166:13: Argument is incorrect: Expected `defaultdict[Asset, Balance]`, found `dict[Asset, Balance]`
- error[invalid-argument-type] rotkehlchen/accounting/structures/balance.py:182:13: Argument is incorrect: Expected `defaultdict[Asset, Balance]`, found `dict[Unknown, Unknown]`
+ error[invalid-argument-type] rotkehlchen/accounting/structures/balance.py:182:13: Argument is incorrect: Expected `defaultdict[Asset, Balance]`, found `dict[Asset, Balance]`
- error[invalid-argument-type] rotkehlchen/accounting/structures/balance.py:183:13: Argument is incorrect: Expected `defaultdict[Asset, Balance]`, found `dict[Unknown, Unknown]`
+ error[invalid-argument-type] rotkehlchen/accounting/structures/balance.py:183:13: Argument is incorrect: Expected `defaultdict[Asset, Balance]`, found `dict[Asset, Balance]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[int | Unknown]`
+ error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[int | Unknown]`
+ error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[int | Unknown]`
+ error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[int | Unknown]`
+ error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`
- error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[int | Unknown]`
+ error[invalid-argument-type] rotkehlchen/chain/ethereum/modules/eth2/eth2.py:229:17: Argument to bound method `get_validators_profit` is incorrect: Expected `set[int] | None`, found `None | set[Unknown] | set[int] | set[Unknown | int]`

sympy (https://github.com/sympy/sympy)
- warning[possibly-unbound-attribute] sympy/utilities/codegen.py:700:27: Attribute `label` on type `property | Unknown` is possibly unbound
+ error[unresolved-attribute] sympy/utilities/codegen.py:700:27: Type `property` has no attribute `label`

```
</details>


---

_@AlexWaygood reviewed on 2025-05-12 01:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:52 on 2025-05-12 01:39_

hmm, the comment here says to keep this field private ;)

---

_@AlexWaygood reviewed on 2025-05-12 01:41_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/generics.rs`:683 on 2025-05-12 01:41_

what about generic protocol instances? Do we need to add a branch for them as well?

---

_@dcreager reviewed on 2025-05-12 13:47_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:52 on 2025-05-12 13:47_

Woof, talk about tunnel vision!  I had done this to be able to pattern-match on the field so that I could have a match arm that only applied to nominal instances of generic aliases.

I'll back this out and tweak the match arm — though keeping it that way might become more difficult in the future if/when the match becomes more complex, with fallthroughs being needed.

---

_@dcreager reviewed on 2025-05-12 13:49_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:52 on 2025-05-12 13:49_

Oh, actually there's another trick we can use here.  Let me push it up as a proposal to see what you think

---

_@dcreager reviewed on 2025-05-12 14:03_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/instance.rs`:52 on 2025-05-12 14:03_

Here you go — if we add an unused `PhantomData` field, we can mark _that_ as private to prevent construction, but still allow read/match access to the `class` field. wdyt? (If you're :+1: then I'll also remove the `class()` accessor method in favor of just reading the field, but didn't want to clutter the diff with that yet if you prefer the original)

---

_@AlexWaygood reviewed on 2025-05-12 14:04_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:68 on 2025-05-12 14:04_

oh, that's clever! It's a shame I'm only learning about this trick now -- much of the diff in #17525 came from me making the `class` field private, and I guess that was unnecessary

---

_@AlexWaygood reviewed on 2025-05-12 14:06_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/instance.rs`:52 on 2025-05-12 14:06_

Yeah, I like the way that it allows for easier pattern-matching while preserving the property that it's only possible to construct `NominalInstanceType`s from the `Type::instance()` method!

---

_@dcreager reviewed on 2025-05-12 14:33_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/generics.rs`:683 on 2025-05-12 14:33_

Good idea, though I'd like to tackle that as a follow-on PR

---

_@AlexWaygood approved on 2025-05-12 21:08_

---

_@carljm approved on 2025-05-12 22:42_

We adds the parentheses, then we takes the parentheses away again.

(Also, the actual fix here is excellent)

---

_Merged by @dcreager on 2025-05-13 02:12_

---

_Closed by @dcreager on 2025-05-13 02:12_

---

_Branch deleted on 2025-05-13 02:12_

---
