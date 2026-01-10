```yaml
number: 19381
title: "[ty] perform type narrowing for places marked `global` too"
type: pull_request
state: merged
author: oconnor663
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: jack/global_place_narrowing
created_at: 2025-07-16T01:40:15Z
updated_at: 2025-07-22T23:42:12Z
url: https://github.com/astral-sh/ruff/pull/19381
synced_at: 2026-01-10T17:58:13Z
```

# [ty] perform type narrowing for places marked `global` too

---

_Pull request opened by @oconnor663 on 2025-07-16 01:40_

This fixes a bug reported at:
https://github.com/astral-sh/ty/issues/311#issuecomment-3070834132

---

I don't understand the narrowing machinery very well yet, so I don't have much intuition for whether this is a Good Change, other than that it doesn't seem to break anything. We call `narrow_place_with_applicable_constraints` five separate times (previously four) in this one function, and that seems a little fishy. When would we _not_ want to do it?

---

_Review requested from @carljm by @oconnor663 on 2025-07-16 01:40_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-07-16 01:40_

---

_Review requested from @sharkdp by @oconnor663 on 2025-07-16 01:40_

---

_Review requested from @dcreager by @oconnor663 on 2025-07-16 01:40_

---

_Review comment by @oconnor663 on `crates/ty_python_semantic/resources/mdtest/scopes/global.md`:84 on 2025-07-16 01:40_

This fails on `main`, as per https://github.com/astral-sh/ty/issues/311#issuecomment-3070834132.

---

_@oconnor663 reviewed on 2025-07-16 01:40_

---

_Comment by @github-actions[bot] on 2025-07-16 01:43_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pyinstrument (https://github.com/joerick/pyinstrument)
- warning[possibly-unbound-attribute] pyinstrument/magic/magic.py:214:33: Attribute `is_running` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] pyinstrument/magic/magic.py:215:13: Attribute `stop` on type `Unknown | None` is possibly unbound
- error[invalid-return-type] pyinstrument/stack_sampler.py:344:12: Return type does not match returned value: expected `dict[Unknown, int | float]`, found `dict[Unknown, int | float] | None`
- Found 44 diagnostics
+ Found 41 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[invalid-return-type] src/anyio/_core/_asyncio_selector_thread.py:167:16: Return type does not match returned value: expected `Selector`, found `Selector | None`
- warning[possibly-unbound-attribute] src/anyio/pytest_plugin.py:63:13: Attribute `close` on type `ExitStack[bool | None] | None` is possibly unbound
- Found 93 diagnostics
+ Found 91 diagnostics

dulwich (https://github.com/dulwich/dulwich)
- error[invalid-assignment] dulwich/merge_drivers.py:244:9: Object of type `Config` is not assignable to attribute `_config` on type `MergeDriverRegistry | None`
- error[invalid-return-type] dulwich/merge_drivers.py:245:12: Return type does not match returned value: expected `MergeDriverRegistry`, found `MergeDriverRegistry | None`
- Found 161 diagnostics
+ Found 159 diagnostics

kopf (https://github.com/nolar/kopf)
- error[invalid-return-type] kopf/_core/intents/registries.py:576:12: Return type does not match returned value: expected `OperatorRegistry`, found `OperatorRegistry | None`
- Found 118 diagnostics
+ Found 117 diagnostics

rich (https://github.com/Textualize/rich)
- error[invalid-return-type] rich/__init__.py:36:12: Return type does not match returned value: expected `Console`, found `Console | None`
- error[invalid-return-type] rich/console.py:575:16: Return type does not match returned value: expected `WindowsConsoleFeatures`, found `None | WindowsConsoleFeatures`
- Found 314 diagnostics
+ Found 312 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- warning[possibly-unbound-attribute] pydantic/plugin/_loader.py:57:12: Attribute `values` on type `dict[Unknown, Unknown] | dict[str, PydanticPluginProtocol] | None` is possibly unbound
- error[invalid-return-type] pydantic/v1/networks.py:120:12: Return type does not match returned value: expected `Pattern[str]`, found `Pattern[str] | Unknown | None`
- error[invalid-return-type] pydantic/v1/networks.py:138:12: Return type does not match returned value: expected `Pattern[str]`, found `Pattern[str] | Unknown | None`
- error[invalid-return-type] pydantic/v1/networks.py:149:12: Return type does not match returned value: expected `Pattern[str]`, found `Pattern[str] | Unknown | None`
- error[invalid-return-type] pydantic/v1/networks.py:158:12: Return type does not match returned value: expected `Pattern[str]`, found `Pattern[str] | Unknown | None`
- error[invalid-return-type] pydantic/v1/networks.py:168:12: Return type does not match returned value: expected `Pattern[str]`, found `Pattern[str] | Unknown | None`
- Found 777 diagnostics
+ Found 771 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- warning[possibly-unbound-attribute] porcupine/pluginmanager.py:235:31: Attribute `winfo_exists` on type `Toplevel | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/pluginmanager.py:236:9: Attribute `lift` on type `Toplevel | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/pluginmanager.py:237:9: Attribute `focus` on type `Toplevel | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/filemanager.py:212:32: Attribute `path` on type `PasteState | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/filemanager.py:219:27: Attribute `path` on type `PasteState | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/filemanager.py:225:8: Attribute `is_cut` on type `PasteState | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/filemanager.py:226:43: Attribute `path` on type `PasteState | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/filemanager.py:231:25: Attribute `path` on type `PasteState | None` is possibly unbound
- warning[possibly-unbound-attribute] porcupine/plugins/filemanager.py:233:57: Attribute `path` on type `PasteState | None` is possibly unbound
- Found 31 diagnostics
+ Found 22 diagnostics

optuna (https://github.com/optuna/optuna)
- error[invalid-argument-type] optuna/logging.py:91:43: Argument to bound method `removeHandler` is incorrect: Expected `Handler`, found `Handler | None`
- Found 570 diagnostics
+ Found 569 diagnostics

poetry (https://github.com/python-poetry/poetry)
- error[invalid-return-type] src/poetry/config/config.py:432:16: Return type does not match returned value: expected `Config`, found `Unknown | Config | None`
- error[invalid-return-type] src/poetry/utils/authenticator.py:469:12: Return type does not match returned value: expected `Authenticator`, found `Authenticator | None`
- Found 937 diagnostics
+ Found 935 diagnostics

pwndbg (https://github.com/pwndbg/pwndbg)
- error[unsupported-operator] pwndbg/commands/context.py:409:27: Operator `>=` is not supported for types `None` and `int`, in comparing `int | None` with `int`
- warning[possibly-unbound-attribute] pwndbg/exception.py:36:9: Attribute `print_exception` on type `(Unknown & ~EllipsisType) | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] pwndbg/gdblib/ptmalloc2_tracking.py:709:13: Attribute `delete` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] pwndbg/gdblib/ptmalloc2_tracking.py:712:13: Attribute `delete` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] pwndbg/integration/binja.py:124:16: Attribute `args` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] pwndbg/integration/ida.py:97:16: Attribute `args` on type `BaseException | None` is possibly unbound
- Found 2266 diagnostics
+ Found 2260 diagnostics

apprise (https://github.com/caronc/apprise)
- warning[possibly-unbound-attribute] apprise/emojis.py:2269:16: Attribute `sub` on type `Pattern[str] | Unknown | None` is possibly unbound
- Found 4316 diagnostics
+ Found 4315 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
- error[invalid-return-type] dragonchain/lib/keys.py:73:12: Return type does not match returned value: expected `DCKeys`, found `DCKeys | Unknown | None`
- Found 305 diagnostics
+ Found 304 diagnostics

cloud-init (https://github.com/canonical/cloud-init)
- error[unsupported-operator] cloudinit/util.py:1331:16: Operator `not in` is not supported for types `str` and `None`, in comparing `str | int` with `set[Unknown] | Unknown | None`
- Found 596 diagnostics
+ Found 595 diagnostics

pywin32 (https://github.com/mhammond/pywin32)
- warning[possibly-unbound-attribute] Pythonwin/pywin/framework/editor/vss.py:94:16: Attribute `VSSItem` on type `CDispatch | Unknown | None` is possibly unbound
- error[invalid-argument-type] Pythonwin/pywin/framework/help.py:26:72: Argument to function `HtmlHelp` is incorrect: Expected `str | tuple[int] | int`, found `Unknown | None`
- warning[possibly-unbound-attribute] Pythonwin/pywin/framework/help.py:164:34: Attribute `items` on type `(dict[Unknown, Unknown] & ~AlwaysFalsy) | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/framework/interact.py:942:29: Attribute `currentView` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/framework/interact.py:943:12: Attribute `currentView` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/framework/interact.py:948:13: Attribute `Close` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/framework/interact.py:955:29: Attribute `currentView` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/framework/interact.py:956:12: Attribute `currentView` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/framework/interact.py:963:13: Attribute `currentView` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/scintilla/find.py:54:13: Attribute `DestroyWindow` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/scintilla/find.py:57:13: Attribute `SetFocus` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] Pythonwin/pywin/tools/TraceCollector.py:74:5: Attribute `write` on type `WindowOutput | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] com/win32com/test/util.py:195:15: Attribute `handlers` on type `Logger | Unknown | None` is possibly unbound
- error[invalid-assignment] com/win32com/test/util.py:196:5: Object of type `list[Unknown]` is not assignable to attribute `emitted` on type `Handler | Unknown`
+ error[invalid-assignment] com/win32com/test/util.py:196:5: Object of type `list[Unknown]` is not assignable to attribute `emitted` on type `Handler | @Todo(map_with_boundness: intersections with negative contributions)`
- warning[possibly-unbound-attribute] com/win32com/test/util.py:197:12: Attribute `emitted` on type `Handler | Unknown` is possibly unbound
+ warning[possibly-unbound-attribute] com/win32com/test/util.py:197:12: Attribute `emitted` on type `Handler | @Todo(map_with_boundness: intersections with negative contributions)` is possibly unbound
- Found 2011 diagnostics
+ Found 1998 diagnostics

colour (https://github.com/colour-science/colour)
- error[invalid-return-type] colour/characterisation/aces_it.py:334:12: Return type does not match returned value: expected `MultiSpectralDistributions`, found `MultiSpectralDistributions | None`
- error[invalid-return-type] colour/characterisation/aces_it.py:404:12: Return type does not match returned value: expected `CanonicalMapping`, found `CanonicalMapping | None`
- error[invalid-return-type] colour/models/rgb/transfer_functions/filmic_pro.py:121:12: Return type does not match returned value: expected `Extrapolator`, found `Extrapolator | None`
- Found 480 diagnostics
+ Found 477 diagnostics

paasta (https://github.com/yelp/paasta)
- error[call-non-callable] paasta_tools/api/api.py:238:12: Object of type `None` is not callable
- Found 876 diagnostics
+ Found 875 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[invalid-argument-type] scrapy/utils/log.py:141:36: Argument to bound method `removeHandler` is incorrect: Expected `Handler`, found `Handler | None`
- Found 1081 diagnostics
+ Found 1080 diagnostics

schema_salad (https://github.com/common-workflow-language/schema_salad)
- error[invalid-return-type] schema_salad/schema.py:78:16: Return type does not match returned value: expected `tuple[Names, list[dict[str, str]], Loader]`, found `tuple[Names, list[dict[str, str]], Loader] | None`
- Found 145 diagnostics
+ Found 144 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- error[invalid-return-type] src/bokeh/io/state.py:233:12: Return type does not match returned value: expected `State`, found `State | None`
- error[invalid-return-type] src/bokeh/util/compiler.py:421:12: Return type does not match returned value: expected `Path`, found `Path | None`
- error[invalid-return-type] src/bokeh/util/compiler.py:428:12: Return type does not match returned value: expected `Path`, found `Unknown | Path | None`
- Found 839 diagnostics
+ Found 836 diagnostics

pytest (https://github.com/pytest-dev/pytest)
+ warning[unused-ignore-comment] src/_pytest/doctest.py:243:27: Unused blanket `type: ignore` directive
- error[call-non-callable] src/_pytest/doctest.py:698:12: Object of type `None` is not callable

cwltool (https://github.com/common-workflow-language/cwltool)
- error[invalid-return-type] cwltool/docker.py:54:12: Return type does not match returned value: expected `list[str]`, found `list[Unknown] | @Todo(list comprehension type) | list[str] | None`
- error[invalid-return-type] cwltool/singularity.py:62:12: Return type does not match returned value: expected `tuple[list[int], str]`, found `tuple[@Todo(list comprehension type) | list[int] | None, str | @Todo(Support for `typing.TypeAlias`)]`
- error[invalid-return-type] cwltool/singularity_utils.py:30:12: Return type does not match returned value: expected `bool`, found `bool | None`
- error[invalid-return-type] cwltool/utils.py:227:12: Return type does not match returned value: expected `str`, found `None | str`
- error[invalid-return-type] tests/util.py:83:12: Return type does not match returned value: expected `bool`, found `bool | None`
- Found 131 diagnostics
+ Found 126 diagnostics

openlibrary (https://github.com/internetarchive/openlibrary)
- warning[possibly-unbound-attribute] openlibrary/core/helpers.py:288:12: Attribute `sub` on type `Pattern[str] | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/coverstore/db.py:24:12: Attribute `get` on type `dict[Unknown, Unknown] | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/plugins/openlibrary/code.py:1111:49: Attribute `rsplit` on type `str | Unknown | None` is possibly unbound
- error[invalid-return-type] openlibrary/solr/utils.py:69:12: Return type does not match returned value: expected `bool`, found `bool | None`
- warning[possibly-unbound-attribute] openlibrary/utils/sentry.py:196:38: Attribute `config` on type `Sentry | None` is possibly unbound
- warning[possibly-unbound-attribute] openlibrary/utils/sentry.py:199:20: Attribute `config` on type `Sentry | None` is possibly unbound
- error[invalid-return-type] openlibrary/utils/sentry.py:201:16: Return type does not match returned value: expected `Sentry`, found `Sentry | None`
- Found 712 diagnostics
+ Found 705 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[invalid-return-type] lib/streamlit/config.py:1887:20: Return type does not match returned value: expected `dict[str, Unknown]`, found `dict[str, Unknown] | None`
- error[call-non-callable] lib/streamlit/watcher/local_sources_watcher.py:172:25: Object of type `None` is not callable
- Found 3289 diagnostics
+ Found 3287 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[unsupported-operator] run_tests.py:324:47: Operator `+` is unsupported between objects of type `Unknown | list[str] | None` and `list[str]`
- Found 886 diagnostics
+ Found 885 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- error[invalid-return-type] ddtrace/internal/packages.py:60:12: Return type does not match returned value: expected `Mapping[str, list[str]]`, found `Unknown | Mapping[str, list[str]] | None`
- warning[possibly-unbound-attribute] ddtrace/profiling/_asyncio.py:96:12: Attribute `get_object` on type `Unknown | None` is possibly unbound
- Found 6472 diagnostics
+ Found 6470 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
- warning[possibly-unbound-attribute] src/integrations/prefect-kubernetes/prefect_kubernetes/observer.py:146:26: Attribute `request` on type `PrefectClient | None` is possibly unbound
- warning[possibly-unbound-attribute] src/integrations/prefect-kubernetes/prefect_kubernetes/observer.py:188:11: Attribute `emit` on type `EventsClient | None` is possibly unbound
- warning[possibly-unbound-attribute] src/integrations/prefect-kubernetes/prefect_kubernetes/observer.py:269:26: Attribute `read_flow_run` on type `PrefectClient | None` is possibly unbound
- warning[possibly-unbound-attribute] src/integrations/prefect-kubernetes/prefect_kubernetes/observer.py:310:30: Attribute `read_flow_run` on type `PrefectClient | None` is possibly unbound
- error[invalid-argument-type] src/integrations/prefect-kubernetes/prefect_kubernetes/observer.py:327:13: Argument to function `propose_state` is incorrect: Expected `PrefectClient`, found `PrefectClient | None`
- warning[possibly-unbound-attribute] src/integrations/prefect-kubernetes/prefect_kubernetes/observer.py:451:9: Attribute `wait` on type `Event | None` is possibly unbound
- warning[possibly-unbound-attribute] src/integrations/prefect-kubernetes/prefect_kubernetes/observer.py:462:9: Attribute `set` on type `Event | None` is possibly unbound
- warning[possibly-unbound-attribute] src/integrations/prefect-kubernetes/prefect_kubernetes/observer.py:464:9: Attribute `join` on type `Thread | None` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/_internal/concurrency/threads.py:268:16: Attribute `thread` on type `EventLoopThread | None` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/_internal/concurrency/threads.py:269:16: Attribute `running` on type `EventLoopThread | None` is possibly unbound
- error[invalid-return-type] src/prefect/_internal/concurrency/threads.py:274:12: Return type does not match returned value: expected `EventLoopThread`, found `EventLoopThread | None`
- warning[possibly-unbound-attribute] src/prefect/_internal/concurrency/threads.py:299:16: Attribute `thread` on type `EventLoopThread | None` is possibly unbound
- warning[possibly-unbound-attribute] src/prefect/_internal/concurrency/threads.py:300:16: Attribute `running` on type `EventLoopThread | None` is possibly unbound
- error[invalid-return-type] src/prefect/_internal/concurrency/threads.py:305:12: Return type does not match returned value: expected `EventLoopThread`, found `EventLoopThread | None`
- error[invalid-return-type] src/prefect/plugins.py:51:16: Return type does not match returned value: expected `dict[str, ModuleType | Exception]`, found `None | dict[str, ModuleType | Exception]`
- warning[possibly-unbound-attribute] src/prefect/server/events/stream.py:132:5: Attribute `cancel` on type `Task[None] | None` is possibly unbound
- error[invalid-return-type] src/prefect/server/events/triggers.py:433:12: Return type does not match returned value: expected `Lock`, found `Lock | None`
- error[unsupported-operator] src/prefect/server/events/triggers.py:450:33: Operator `>=` is not supported for types `int` and `None`, in comparing `@Todo(Support for `typing.TypeAlias`) | int | float` with `int | float | None`
- error[unsupported-operator] src/prefect/server/events/triggers.py:472:19: Operator `-` is unsupported between objects of type `int | float | None` and `int | float`
- error[unsupported-operator] src/prefect/server/events/triggers.py:472:43: Operator `-` is unsupported between objects of type `int | float` and `int | float | None`
- error[invalid-return-type] src/prefect/server/events/triggers.py:655:12: Return type does not match returned value: expected `Lock`, found `Lock | None`
- warning[possibly-unbound-attribute] src/prefect/server/logs/stream.py:213:5: Attribute `cancel` on type `Task[None] | None` is possibly unbound
- error[invalid-context-manager] src/prefect/server/utilities/messaging/memory.py:69:10: Object of type `LockType | None` cannot be used with `with` because the methods `__enter__` and `__exit__` are possibly unbound
- error[invalid-return-type] src/prefect/utilities/importtools.py:29:12: Return type does not match returned value: expected `LockType`, found `LockType | None`
- error[not-iterable] src/prefect/utilities/services.py:182:26: Object of type `None` is not iterable
- Found 3713 diagnostics
+ Found 3688 diagnostics

zulip (https://github.com/zulip/zulip)
- error[invalid-return-type] zerver/lib/narrow_helpers.py:63:12: Return type does not match returned value: expected `list[str]`, found `Unknown | list[str] | None`
- warning[possibly-unbound-attribute] zerver/lib/sqlalchemy_utils.py:41:10: Attribute `connect` on type `Unknown | None` is possibly unbound
- error[not-iterable] zerver/lib/templates.py:159:14: Object of type `list[Unknown] | list[Extension] | None` may not be iterable
- error[not-iterable] zerver/lib/templates.py:175:44: Object of type `list[Unknown] | list[Extension] | None` may not be iterable
- warning[possibly-unbound-attribute] zproject/computed_settings.py:123:5: Attribute `append` on type `list[Unknown] | list[str] | None` is possibly unbound
- Found 7278 diagnostics
+ Found 7273 diagnostics

materialize (https://github.com/MaterializeInc/materialize)
- error[unsupported-operator] ci/mkpipeline.py:929:16: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `@Todo(set comprehension type) | set[str] | None`
- error[invalid-return-type] misc/python/materialize/checks/scenarios_upgrade.py:47:12: Return type does not match returned value: expected `list[Unknown]`, found `@Todo(list comprehension type) | list[Unknown] | None`
- error[unsupported-operator] misc/python/materialize/mzbuild.py:235:8: Operator `in` is not supported for types `str` and `None`, in comparing `str` with `set[Unknown] | set[str] | None`
- warning[possibly-unbound-attribute] misc/python/materialize/mzbuild.py:291:13: Attribute `add` on type `set[Unknown] | set[str] | None` is possibly unbound
- warning[possibly-unbound-attribute] misc/python/materialize/parallel_workload/executor.py:115:13: Attribute `flush` on type `TextIO | None` is possibly unbound
- error[invalid-return-type] misc/python/materialize/util.py:59:12: Return type does not match returned value: expected `list[str]`, found `Any | None`
- Found 3250 diagnostics
+ Found 3244 diagnostics

sympy (https://github.com/sympy/sympy)
- error[unsupported-operator] sympy/holonomic/holonomic.py:2277:12: Operator `in` is not supported for types `tuple[type[Basic], ...]` and `None`, in comparing `tuple[type[Basic], ...]` with `dict[Unknown, Unknown] | Unknown | None`
- error[non-subscriptable] sympy/holonomic/holonomic.py:2278:17: Cannot subscript object of type `None` with no `__getitem__` method
- error[unsupported-operator] sympy/integrals/meijerint.py:1502:8: Operator `in` is not supported for types `tuple[type[Basic], ...]` and `None`, in comparing `tuple[type[Basic], ...]` with `dict[Unknown, Unknown] | Unknown | None`
- error[non-subscriptable] sympy/integrals/meijerint.py:1503:13: Cannot subscript object of type `None` with no `__getitem__` method
- error[unsupported-operator] sympy/integrals/transforms.py:861:47: Operator `not in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `set[Unknown] | Unknown | None`
- warning[possibly-unbound-attribute] sympy/simplify/hyperexpand.py:2038:15: Attribute `lookup_origin` on type `FormulaCollection | Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] sympy/simplify/hyperexpand.py:2229:9: Attribute `lookup_origin` on type `MeijerFormulaCollection | Unknown | None` is possibly unbound
- error[non-subscriptable] sympy/solvers/solveset.py:423:20: Cannot subscript object of type `None` with no `__getitem__` method
- error[non-subscriptable] sympy/solvers/solveset.py:495:20: Cannot subscript object of type `None` with no `__getitem__` method
- Found 12972 diagnostics
+ Found 12963 diagnostics

manticore (https://github.com/trailofbits/manticore)
- warning[possibly-unbound-attribute] tests/native/test_aarch64cpu.py:35:12: Attribute `asm` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/native/test_armv7cpu.py:269:16: Attribute `asm` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/native/test_armv7cpu.py:272:16: Attribute `asm` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/native/test_armv7unicorn.py:174:16: Attribute `asm` on type `Unknown | None` is possibly unbound
- warning[possibly-unbound-attribute] tests/native/test_armv7unicorn.py:176:16: Attribute `asm` on type `Unknown | None` is possibly unbound
- Found 1099 diagnostics
+ Found 1094 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @oconnor663 on 2025-07-16 01:52_

Quite a lot of removed diagnostics in `mypy_primer`. I'll kick off `ecosystem-analyzer` too.

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-16 01:52_

---

_Comment by @github-actions[bot] on 2025-07-16 02:01_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `possibly-unbound-attribute` | 0 | 61 | 1 |
| `invalid-return-type` | 0 | 39 | 0 |
| `unsupported-operator` | 0 | 11 | 0 |
| `invalid-argument-type` | 0 | 4 | 0 |
| `non-subscriptable` | 0 | 4 | 0 |
| `call-non-callable` | 0 | 3 | 0 |
| `not-iterable` | 0 | 3 | 0 |
| `invalid-assignment` | 0 | 1 | 1 |
| `invalid-context-manager` | 0 | 1 | 0 |
| `unused-ignore-comment` | 1 | 0 | 0 |
| **Total** | **1** | **127** | **2** |

**[Full report with detailed diff](https://jack-global-place-narrowing.ecosystem-663.pages.dev/diff)**


---

_Comment by @mtshiba on 2025-07-16 03:19_

> We call `narrow_place_with_applicable_constraints` five separate times (previously four) in this one function, and that seems a little fishy. When would we not want to do it?

If a place that is not `Unbound` is found, it should be applied (narrowing `Unbound` is meaningless).
Since `Place(AndQualifiers)::map_type` does nothing with `Unbound`, it would be fine to just call `narrow_place_with_applicable_constraints` once at the end of `infer_place_load`. We have multiple early returns in this method, but all within closures.

---

_Label `ty` added by @AlexWaygood on 2025-07-16 06:18_

---

_Comment by @MichaReiser on 2025-07-18 13:44_

@sharkdp is this a PR you could review?

---

_@sharkdp approved on 2025-07-21 07:44_

Thank you, this looks great. @oconnor663 I'll leave it up to you if you want to include the suggested refactoring around `narrow_place_with_applicable_constraints` here, or not.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-21 13:40_

---

_@carljm approved on 2025-07-22 01:51_

---

_Label `ecosystem-analyzer` removed by @oconnor663 on 2025-07-22 22:36_

---

_Label `ecosystem-analyzer` added by @oconnor663 on 2025-07-22 22:36_

---

_Comment by @codspeed-hq[bot] on 2025-07-22 22:45_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/jack%2Fglobal_place_narrowing?runnerMode=Instrumentation)

### Merging #19381 will **not alter performance**

<sub>Comparing <code>jack/global_place_narrowing</code> (7357a3f) with <code>main</code> (dc10ab8)</sub>



### Summary

`✅ 41` untouched benchmarks  





---

_Comment by @codspeed-hq[bot] on 2025-07-22 22:47_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/jack%2Fglobal_place_narrowing?runnerMode=WallTime)

### Merging #19381 will **not alter performance**

<sub>Comparing <code>jack/global_place_narrowing</code> (7357a3f) with <code>main</code> (dc10ab8)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Comment by @oconnor663 on 2025-07-22 23:13_

The 14-18% performance regression in `many_tuple_assignments` is real, and I can repro it locally. I haven't looked into exactly how [the refactoring](https://github.com/astral-sh/ruff/pull/19381/commits/fbdcc1c48f2ed16b06b1812a4c95acf0ae4518bf) causes it, but instead of getting into that I'm going to land the original version of this PR, which just adds an extra call to `narrow_place_with_applicable_constraints` in the global case and doesn't cause any regressions.

---

_Merged by @oconnor663 on 2025-07-22 23:42_

---

_Closed by @oconnor663 on 2025-07-22 23:42_

---

_Branch deleted on 2025-07-22 23:42_

---
