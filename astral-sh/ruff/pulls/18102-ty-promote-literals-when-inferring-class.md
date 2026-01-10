```yaml
number: 18102
title: "[ty] Promote literals when inferring class specializations from constructors"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/promote-literals
created_at: 2025-05-14T18:08:48Z
updated_at: 2025-05-19T19:42:56Z
url: https://github.com/astral-sh/ruff/pull/18102
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Promote literals when inferring class specializations from constructors

---

_Pull request opened by @dcreager on 2025-05-14 18:08_

This implements the stopgap approach described in https://github.com/astral-sh/ty/issues/336#issuecomment-2880532213 for handling literal types in generic class specializations.

With this approach, we will promote any literal to its instance type, but _only_ when inferring a generic class specialization from a constructor call:

```py
class C[T]:
    def __init__(self, x: T) -> None: ...

reveal_type(C("string"))  # revealed: C[str]
```

If you specialize the class explicitly, we still use whatever type you provide, even if it's a literal:

```py
from typing import Literal

reveal_type(C[Literal[5]](5))  # revealed: C[Literal[5]]
```

And this doesn't apply at all to generic functions:

```py
def f[T](x: T) -> T:
    return x

reveal_type(f(5))  # revealed: Literal[5]
```

---

As part of making this happen, we also generalize the `TypeMapping` machinery.  This provides a way to apply a function to type, returning a new type.  Complicating matters is that for function literals, we have to apply the mapping lazily, since the function's signature is not created until (and if) someone calls its `signature` method.  That means we have to stash away the mappings that we want to apply to the signatures parameter/return annotations once we do create it.  This requires some minor `Cow` shenanigans to continue working for partial specializations.

---

_Review requested from @carljm by @dcreager on 2025-05-14 18:08_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-14 18:08_

---

_Label `ty` added by @dcreager on 2025-05-14 18:08_

---

_Review requested from @sharkdp by @dcreager on 2025-05-14 18:08_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/call/bind.rs`:1415 on 2025-05-14 18:12_

Everything else in the PR is setup for this one line

---

_@dcreager reviewed on 2025-05-14 18:12_

---

_Comment by @github-actions[bot] on 2025-05-14 19:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- error[invalid-assignment] src/async_utils/priority_sem.py:31:1: Object of type `ContextVar[Literal[0]]` is not assignable to `ContextVar[int]`
- Found 16 diagnostics
+ Found 15 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[invalid-return-type] chess/svg.py:162:12: Return type does not match returned value: expected `Element[str]`, found `Element[Literal["svg"]]`
- error[invalid-return-type] chess/svg.py:200:12: Return type does not match returned value: expected `Element[str]`, found `Element[Literal["g"]]`
- Found 39 diagnostics
+ Found 37 diagnostics

trio (https://github.com/python-trio/trio)
- error[invalid-argument-type] src/trio/_core/_tests/test_run.py:2170:17: Argument to bound method `set` is incorrect: Expected `Literal["hmmm"]`, found `Literal["hmmmm"]`
- error[invalid-argument-type] src/trio/_core/_tests/test_run.py:2180:17: Argument to bound method `set` is incorrect: Expected `Literal["hmmm"]`, found `Literal["hmmmm"]`
- Found 1095 diagnostics
+ Found 1093 diagnostics

mkosi (https://github.com/systemd/mkosi)
- error[invalid-argument-type] mkosi/config.py:5087:23: Argument to bound method `set` is incorrect: Expected `Literal[False]`, found `bool`
- error[invalid-argument-type] mkosi/config.py:5089:29: Argument to bound method `set` is incorrect: Expected `Literal[False]`, found `bool`
- error[invalid-argument-type] mkosi/config.py:5091:31: Argument to bound method `set` is incorrect: Expected `Literal[False]`, found `bool`
- Found 250 diagnostics
+ Found 247 diagnostics

asynq (https://github.com/quora/asynq)
- error[invalid-argument-type] asynq/asynq_to_async.py:85:41: Argument to bound method `set` is incorrect: Expected `Literal[False]`, found `Literal[True]`
- error[unresolved-attribute] asynq/tests/test_generator.py:28:24: Type `Value[Literal["value"]]` has no attribute `value`
+ error[unresolved-attribute] asynq/tests/test_generator.py:28:24: Type `Value[str]` has no attribute `value`
- error[invalid-argument-type] asynq/tests/test_scoped_value.py:28:25: Argument to bound method `override` is incorrect: Expected `Literal["a"]`, found `Literal["c"]`
- error[invalid-argument-type] asynq/tests/test_scoped_value.py:51:13: Argument to bound method `set` is incorrect: Expected `Literal["capybara"]`, found `Literal["nutria"]`
- error[invalid-argument-type] asynq/tests/test_scoped_value.py:64:29: Argument to bound method `override` is incorrect: Expected `Literal["a"]`, found `Literal["b"]`
- Found 214 diagnostics
+ Found 210 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- error[invalid-argument-type] tornado/test/asyncio_test.py:277:26: Argument to bound method `set` is incorrect: Expected `Literal["default"]`, found `Literal["foo"]`
- Found 345 diagnostics
+ Found 344 diagnostics

vision (https://github.com/pytorch/vision)
- error[no-matching-overload] test/builtin_dataset_mocks.py:839:22: No overload of function `tostring` matches arguments
- error[no-matching-overload] test/test_datasets.py:747:22: No overload of function `tostring` matches arguments
- error[invalid-argument-type] test/test_onnx.py:241:13: Argument to bound method `__init__` is incorrect: Expected `dict[str, int]`, found `dict[str, Literal[2000, 1000]]`
- error[invalid-argument-type] test/test_onnx.py:242:13: Argument to bound method `__init__` is incorrect: Expected `dict[str, int]`, found `dict[str, Literal[2000, 1000]]`
- Found 1847 diagnostics
+ Found 1843 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[type-assertion-failure] tests/annotations/declarations.py:1208:5: Argument does not have asserted type `dict[str, int]`
- Found 639 diagnostics
+ Found 638 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- error[unsupported-operator] tests/types/test_range.py:475:16: Operator `not in` is not supported for types `int` and `Range[Literal[10]]`, in comparing `Literal[9]` with `Range[Literal[10]]`
- error[unsupported-operator] tests/types/test_range.py:476:16: Operator `in` is not supported for types `int` and `Range[Literal[10]]`, in comparing `Literal[10]` with `Range[Literal[10]]`
- error[unsupported-operator] tests/types/test_range.py:477:16: Operator `in` is not supported for types `int` and `Range[Literal[10]]`, in comparing `Literal[11]` with `Range[Literal[10]]`
- error[unsupported-operator] tests/types/test_range.py:480:16: Operator `not in` is not supported for types `int` and `Range[Literal[10]]`, in comparing `Literal[9]` with `Range[Literal[10]]`
- error[unsupported-operator] tests/types/test_range.py:481:16: Operator `not in` is not supported for types `int` and `Range[Literal[10]]`, in comparing `Literal[10]` with `Range[Literal[10]]`
- error[unsupported-operator] tests/types/test_range.py:482:16: Operator `in` is not supported for types `int` and `Range[Literal[10]]`, in comparing `Literal[11]` with `Range[Literal[10]]`
- error[unsupported-operator] tests/types/test_range.py:485:16: Operator `in` is not supported for types `int` and `Range[Literal[20]]`, in comparing `Literal[19]` with `Range[Literal[20]]`
- error[unsupported-operator] tests/types/test_range.py:486:16: Operator `not in` is not supported for types `int` and `Range[Literal[20]]`, in comparing `Literal[20]` with `Range[Literal[20]]`
- error[unsupported-operator] tests/types/test_range.py:487:16: Operator `not in` is not supported for types `int` and `Range[Literal[20]]`, in comparing `Literal[21]` with `Range[Literal[20]]`
- error[unsupported-operator] tests/types/test_range.py:490:16: Operator `in` is not supported for types `int` and `Range[Literal[20]]`, in comparing `Literal[19]` with `Range[Literal[20]]`
- error[unsupported-operator] tests/types/test_range.py:491:16: Operator `in` is not supported for types `int` and `Range[Literal[20]]`, in comparing `Literal[20]` with `Range[Literal[20]]`
- error[unsupported-operator] tests/types/test_range.py:492:16: Operator `not in` is not supported for types `int` and `Range[Literal[20]]`, in comparing `Literal[21]` with `Range[Literal[20]]`
- error[unsupported-operator] tests/types/test_range.py:495:16: Operator `not in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[9]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:496:16: Operator `in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[10]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:497:16: Operator `in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[11]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:498:16: Operator `in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[19]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:499:16: Operator `not in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[20]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:500:16: Operator `not in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[21]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:503:16: Operator `not in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[9]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:504:16: Operator `not in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[10]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:505:16: Operator `in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[11]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:506:16: Operator `in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[19]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:507:16: Operator `in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[20]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:508:16: Operator `not in` is not supported for types `int` and `Range[Literal[10, 20]]`, in comparing `Literal[21]` with `Range[Literal[10, 20]]`
- error[unsupported-operator] tests/types/test_range.py:511:16: Operator `not in` is not supported for types `int` and `Range[Literal[20, 10]]`, in comparing `Literal[9]` with `Range[Literal[20, 10]]`
- error[unsupported-operator] tests/types/test_range.py:512:16: Operator `not in` is not supported for types `int` and `Range[Literal[20, 10]]`, in comparing `Literal[10]` with `Range[Literal[20, 10]]`
- error[unsupported-operator] tests/types/test_range.py:513:16: Operator `not in` is not supported for types `int` and `Range[Literal[20, 10]]`, in comparing `Literal[11]` with `Range[Literal[20, 10]]`
- error[unsupported-operator] tests/types/test_range.py:514:16: Operator `not in` is not supported for types `int` and `Range[Literal[20, 10]]`, in comparing `Literal[19]` with `Range[Literal[20, 10]]`
- error[unsupported-operator] tests/types/test_range.py:515:16: Operator `not in` is not supported for types `int` and `Range[Literal[20, 10]]`, in comparing `Literal[20]` with `Range[Literal[20, 10]]`
- error[unsupported-operator] tests/types/test_range.py:516:16: Operator `not in` is not supported for types `int` and `Range[Literal[20, 10]]`, in comparing `Literal[21]` with `Range[Literal[20, 10]]`
- Found 1148 diagnostics
+ Found 1118 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-return-type] src/_pytest/junitxml.py:113:20: Return type does not match returned value: expected `Element[str] | None`, found `Element[Literal["properties"]]`
- error[invalid-return-type] src/_pytest/junitxml.py:152:16: Return type does not match returned value: expected `Element[str]`, found `Element[Literal["testcase"]]`
- error[no-matching-overload] src/_pytest/junitxml.py:675:27: No overload of function `tostring` matches arguments
- error[invalid-return-type] src/_pytest/junitxml.py:691:20: Return type does not match returned value: expected `Element[str] | None`, found `Element[Literal["properties"]]`
- Found 886 diagnostics
+ Found 882 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
- error[invalid-assignment] mitmproxy/addons/export.py:152:1: Object of type `dict[str, (def curl_command(f: Flow) -> str) | (def httpie_command(f: Flow) -> str) | (def raw(f: Flow, separator=Literal[b"\r\n\r\n"]) -> bytes) | (def raw_request(f: Flow) -> bytes) | (def raw_response(f: Flow) -> bytes)]` is not assignable to `dict[str, (Flow, /) -> str | bytes]`
+ error[invalid-assignment] mitmproxy/addons/export.py:152:1: Object of type `dict[str, (def curl_command(f: Flow) -> str) | (def httpie_command(f: Flow) -> str) | (def raw(f: Flow, separator=bytes) -> bytes) | (def raw_request(f: Flow) -> bytes) | (def raw_response(f: Flow) -> bytes)]` is not assignable to `dict[str, (Flow, /) -> str | bytes]`

colour (https://github.com/colour-science/colour)
- error[invalid-argument-type] colour/io/tm2714.py:1766:41: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["IESTM2714"]]`
- error[invalid-argument-type] colour/io/tm2714.py:1778:21: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal[""]] | Element[str]`
- error[no-matching-overload] colour/io/tm2714.py:1786:17: No overload of function `tostring` matches arguments
- Found 533 diagnostics
+ Found 530 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:628:35: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:642:37: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:648:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:651:37: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:660:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:661:42: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:689:37: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-return-type] mesonbuild/backend/vs2010backend.py:710:16: Return type does not match returned value: expected `tuple[Element[str], Element[str]]`, found `tuple[Element[Literal["Project"]], Element[str]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:1840:40: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:1841:38: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["Project"]]`
- error[invalid-argument-type] mesonbuild/backend/vs2010backend.py:1900:54: Argument to bound method `__init__` is incorrect: Expected `Element[str] | None`, found `Element[Literal["Project"]]`
- error[invalid-assignment] mesonbuild/interpreter/compiler.py:164:1: Object of type `KwargInfo[Literal[""]]` is not assignable to `KwargInfo[str]`
- error[invalid-assignment] mesonbuild/interpreter/type_checking.py:186:1: Object of type `KwargInfo[Literal[True]]` is not assignable to `KwargInfo[bool | UserFeatureOption]`
+ error[invalid-assignment] mesonbuild/interpreter/type_checking.py:186:1: Object of type `KwargInfo[bool]` is not assignable to `KwargInfo[bool | UserFeatureOption]`
- error[invalid-assignment] mesonbuild/interpreter/type_checking.py:193:1: Object of type `KwargInfo[Literal[False]]` is not assignable to `KwargInfo[bool]`
- error[invalid-assignment] mesonbuild/interpreter/type_checking.py:490:1: Object of type `KwargInfo[Literal[False]]` is not assignable to `KwargInfo[bool]`
- error[invalid-argument-type] mesonbuild/modules/_qt.py:776:35: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["RCC"]]`
- error[invalid-argument-type] mesonbuild/modules/_qt.py:785:31: Argument to bound method `__init__` is incorrect: Expected `Element[str] | None`, found `Element[Literal["RCC"]]`
- error[invalid-assignment] mesonbuild/modules/gnome.py:203:1: Object of type `KwargInfo[Literal[True]]` is not assignable to `KwargInfo[bool]`
- error[invalid-argument-type] mesonbuild/modules/i18n.py:352:27: Argument to bound method `evolve` is incorrect: Expected `Literal[False] | None | _NULL_T`, found `Literal[True]`
- error[invalid-argument-type] mesonbuild/mtest.py:869:42: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["testsuite"]]`
- error[invalid-argument-type] mesonbuild/mtest.py:891:37: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["testsuite"]]`
- error[invalid-argument-type] mesonbuild/mtest.py:894:37: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["testsuite"]]`
- error[invalid-argument-type] mesonbuild/mtest.py:905:38: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["testsuite"]] | Unknown`
- error[invalid-argument-type] packaging/createpkg.py:71:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:73:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:75:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:77:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:78:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:81:41: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:84:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:85:32: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:87:23: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] packaging/createpkg.py:90:24: Argument to bound method `__init__` is incorrect: Expected `Element[str] | None`, found `Element[Literal["installer-gui-script"]]`
- error[invalid-argument-type] run_project_tests.py:1251:39: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["testsuites"]]`
- error[invalid-argument-type] run_project_tests.py:1443:20: Argument to bound method `__init__` is incorrect: Expected `Element[str] | None`, found `Element[Literal["testsuites"]]`
- error[invalid-argument-type] unittests/internaltests.py:1546:22: Argument to bound method `evolve` is incorrect: Expected `Literal["foo"] | None | _NULL_T`, found `Literal["bar"]`
- Found 1403 diagnostics
+ Found 1368 diagnostics

cwltool (https://github.com/common-workflow-language/cwltool)
- error[invalid-assignment] cwltool/software_requirements.py:100:9: Object of type `dict[str, LiteralString]` is not assignable to `dict[str, str]`
- Found 298 diagnostics
+ Found 297 diagnostics

streamlit (https://github.com/streamlit/streamlit)
- error[invalid-assignment] lib/streamlit/runtime/scriptrunner_utils/script_run_context.py:63:1: Object of type `ContextVar[Literal[False]]` is not assignable to `ContextVar[bool]`
- Found 3258 diagnostics
+ Found 3257 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
- warning[call-possibly-unbound-method] benchmarks/set_http_meta/scenario.py:74:13: Method `__getitem__` of type `Unknown | LiteralString | Literal[200, 0] | dict[Unknown, Unknown]` is possibly unbound
- warning[call-possibly-unbound-method] benchmarks/set_http_meta/scenario.py:80:13: Method `__getitem__` of type `Unknown | LiteralString | Literal[200, 0] | dict[Unknown, Unknown]` is possibly unbound
+ warning[call-possibly-unbound-method] benchmarks/set_http_meta/scenario.py:74:13: Method `__getitem__` of type `Unknown | str | int | dict[Unknown, Unknown]` is possibly unbound
+ warning[call-possibly-unbound-method] benchmarks/set_http_meta/scenario.py:80:13: Method `__getitem__` of type `Unknown | str | int | dict[Unknown, Unknown]` is possibly unbound
- error[invalid-argument-type] ddtrace/internal/coverage/code.py:224:38: Argument to bound method `set` is incorrect: Expected `Literal[False]`, found `Literal[True]`
- error[invalid-argument-type] tests/tracer/test_compat.py:60:27: Argument to function `to_unicode` is incorrect: Argument type `dict[str, Literal["value"]]` does not satisfy constraints of type variable `AnyStr`
+ error[invalid-argument-type] tests/tracer/test_compat.py:60:27: Argument to function `to_unicode` is incorrect: Argument type `dict[str, str]` does not satisfy constraints of type variable `AnyStr`
- error[invalid-argument-type] tests/tracer/test_span.py:731:33: Argument to bound method `set_tag_str` is incorrect: Expected `str`, found `dict[str, Literal[1]]`
+ error[invalid-argument-type] tests/tracer/test_span.py:731:33: Argument to bound method `set_tag_str` is incorrect: Expected `str`, found `dict[str, int]`
- error[invalid-argument-type] tests/tracer/test_span.py:739:33: Argument to bound method `set_tag_str` is incorrect: Expected `str`, found `dict[str, Literal[1]]`
+ error[invalid-argument-type] tests/tracer/test_span.py:739:33: Argument to bound method `set_tag_str` is incorrect: Expected `str`, found `dict[str, int]`
- Found 6995 diagnostics
+ Found 6994 diagnostics

zulip (https://github.com/zulip/zulip)
- error[invalid-assignment] scripts/lib/check_rabbitmq_queue.py:47:1: Object of type `defaultdict[str, Literal[600, 1200, 120, 60, 3600]]` is not assignable to `defaultdict[str, int]`
- error[invalid-assignment] scripts/lib/check_rabbitmq_queue.py:55:1: Object of type `defaultdict[str, Literal[900, 180, 1800, 90, 4500]]` is not assignable to `defaultdict[str, int]`
- error[invalid-assignment] zerver/actions/realm_linkifiers.py:16:5: Object of type `dict[str, Literal["realm_linkifiers"] | list[LinkifierDict]]` is not assignable to `dict[str, object]`
+ error[invalid-assignment] zerver/actions/realm_linkifiers.py:16:5: Object of type `dict[str, str | list[LinkifierDict]]` is not assignable to `dict[str, object]`
- error[invalid-argument-type] zerver/actions/user_groups.py:599:49: Argument to function `do_send_user_group_update_event` is incorrect: Expected `dict[str, str | int | UserGroupMembersDict]`, found `dict[str, Literal[True]]`
+ error[invalid-argument-type] zerver/actions/user_groups.py:599:49: Argument to function `do_send_user_group_update_event` is incorrect: Expected `dict[str, str | int | UserGroupMembersDict]`, found `dict[str, bool]`
- error[invalid-argument-type] zerver/actions/user_groups.py:621:49: Argument to function `do_send_user_group_update_event` is incorrect: Expected `dict[str, str | int | UserGroupMembersDict]`, found `dict[str, Literal[False]]`
+ error[invalid-argument-type] zerver/actions/user_groups.py:621:49: Argument to function `do_send_user_group_update_event` is incorrect: Expected `dict[str, str | int | UserGroupMembersDict]`, found `dict[str, bool]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:641:42: Argument to bound method `insert` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:646:24: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]] | Element[str]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:682:38: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]] | Element[str]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1023:16: Return type does not match returned value: expected `Element[str]`, found `Element[Literal["p"]]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:1038:32: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:1057:31: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:1076:38: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1083:20: Return type does not match returned value: expected `Element[str] | None`, found `Element[Literal["div"]]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:1177:40: Argument to bound method `insert` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:1257:42: Argument to bound method `insert` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]]`
- error[invalid-argument-type] zerver/lib/markdown/__init__.py:1264:24: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]] | Element[str]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1458:20: Return type does not match returned value: expected `Element[str] | None`, found `Element[Literal["span"]]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1471:24: Return type does not match returned value: expected `Element[str] | None`, found `Element[Literal["span"]]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1478:16: Return type does not match returned value: expected `Element[str] | None`, found `Element[Literal["time"]]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1513:12: Return type does not match returned value: expected `Element[str]`, found `Element[Literal["span"]]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1522:12: Return type does not match returned value: expected `Element[str]`, found `Element[Literal["img"]]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1612:20: Return type does not match returned value: expected `str | Element[str]`, found `Element[Literal["span"]]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1678:12: Return type does not match returned value: expected `Element[str] | str`, found `Element[Literal["a"]]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:1978:20: Return type does not match returned value: expected `tuple[Element[str] | str | None, int | None, int | None]`, found `tuple[Element[Literal["span"]], int, int]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:2015:20: Return type does not match returned value: expected `tuple[Element[str] | str | None, int | None, int | None]`, found `tuple[Element[Literal["span"]], int, int]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:2050:16: Return type does not match returned value: expected `tuple[Element[str] | str | None, int | None, int | None]`, found `tuple[Element[Literal["a"]], int, int]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:2094:16: Return type does not match returned value: expected `tuple[Element[str] | str | None, int | None, int | None]`, found `tuple[Element[Literal["a"]], int, int]`
- error[invalid-return-type] zerver/lib/markdown/__init__.py:2126:16: Return type does not match returned value: expected `tuple[Element[str] | str | None, int | None, int | None]`, found `tuple[Element[Literal["a"]], int, int]`
- error[invalid-argument-type] zerver/lib/markdown/nested_code_blocks.py:67:26: Argument to function `SubElement` is incorrect: Expected `Element[str]`, found `Element[Literal["div"]]`
- error[invalid-return-type] zerver/lib/markdown/nested_code_blocks.py:69:16: Return type does not match returned value: expected `Element[str]`, found `Element[Literal["div"]]`
- error[invalid-assignment] zerver/lib/message.py:931:5: Object of type `dict[str, dict[Unknown, Unknown] | set[Unknown] | Literal[False]]` is not assignable to `RawUnreadMessagesResult`
+ error[invalid-assignment] zerver/lib/message.py:931:5: Object of type `dict[str, dict[Unknown, Unknown] | set[Unknown] | bool]` is not assignable to `RawUnreadMessagesResult`
- error[invalid-return-type] zerver/lib/test_helpers.py:748:12: Return type does not match returned value: expected `dict[str, str]`, found `dict[str, Literal["zulip", "zulip_extra_emoji"]]`
- error[invalid-argument-type] zerver/tests/test_user_topics.py:1104:35: Argument to function `participate_in_poll` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["vote", "1,1", 1]]`
+ error[invalid-argument-type] zerver/tests/test_user_topics.py:1104:35: Argument to function `participate_in_poll` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
- error[invalid-argument-type] zerver/tests/test_user_topics.py:1105:36: Argument to function `participate_in_poll` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["new_option", "maybe", 7]]`
+ error[invalid-argument-type] zerver/tests/test_user_topics.py:1105:36: Argument to function `participate_in_poll` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
- error[invalid-argument-type] zerver/tests/test_user_topics.py:1166:33: Argument to function `edit_todo_list` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["new_task", "eat", "", 7, False]]`
+ error[invalid-argument-type] zerver/tests/test_user_topics.py:1166:33: Argument to function `edit_todo_list` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
- error[invalid-argument-type] zerver/tests/test_user_topics.py:1167:31: Argument to function `edit_todo_list` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["strike", "5,9"]]`
+ error[invalid-argument-type] zerver/tests/test_user_topics.py:1167:31: Argument to function `edit_todo_list` is incorrect: Expected `dict[str, object]`, found `dict[str, str]`
- error[invalid-argument-type] zerver/tests/test_user_topics.py:1475:35: Argument to function `participate_in_poll` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["vote", "1,1", 1]]`
+ error[invalid-argument-type] zerver/tests/test_user_topics.py:1475:35: Argument to function `participate_in_poll` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
- error[invalid-argument-type] zerver/tests/test_user_topics.py:1476:36: Argument to function `participate_in_poll` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["new_option", "maybe", 7]]`
+ error[invalid-argument-type] zerver/tests/test_user_topics.py:1476:36: Argument to function `participate_in_poll` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
- error[invalid-argument-type] zerver/tests/test_user_topics.py:1542:33: Argument to function `edit_todo_list` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["new_task", "eat", "", 7, False]]`
+ error[invalid-argument-type] zerver/tests/test_user_topics.py:1542:33: Argument to function `edit_todo_list` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
- error[invalid-argument-type] zerver/tests/test_user_topics.py:1543:31: Argument to function `edit_todo_list` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["strike", "5,9"]]`
+ error[invalid-argument-type] zerver/tests/test_user_topics.py:1543:31: Argument to function `edit_todo_list` is incorrect: Expected `dict[str, object]`, found `dict[str, str]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:386:33: Argument to function `post` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["question", "Tabs or spaces?"]]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:386:33: Argument to function `post` is incorrect: Expected `dict[str, object]`, found `dict[str, str]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:389:31: Argument to function `post` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["question", "Tabs or spaces?"]]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:389:31: Argument to function `post` is incorrect: Expected `dict[str, object]`, found `dict[str, str]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:415:33: Argument to function `post` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["new_task_list_title", "School Work"]]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:415:33: Argument to function `post` is incorrect: Expected `dict[str, object]`, found `dict[str, str]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:418:31: Argument to function `post` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["new_task_list_title", "School Work"]]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:418:31: Argument to function `post` is incorrect: Expected `dict[str, object]`, found `dict[str, str]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:477:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:478:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:477:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["vote", "1,1", 1]]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:479:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, str]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:478:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["new_option", "maybe", 7]]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:533:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, str | int]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:479:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["question", "what's for dinner?"]]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:533:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["new_task", "eat", "", 7, False]]`
- error[invalid-argument-type] zerver/tests/test_widgets.py:534:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, Literal["strike", "5,9"]]`
+ error[invalid-argument-type] zerver/tests/test_widgets.py:534:24: Argument to function `assert_success` is incorrect: Expected `dict[str, object]`, found `dict[str, str]`
- error[invalid-return-type] zerver/tornado/event_queue.py:59:12: Return type does not match returned value: expected `dict[str, str]`, found `dict[str, Literal["heartbeat"]]`
- error[invalid-return-type] zproject/backends.py:2288:24: Return type does not match returned value: expected `dict[str, str]`, found `dict[str, Literal["GitHub user is not member of required team"]]`
- error[invalid-return-type] zproject/backends.py:2294:24: Return type does not match returned value: expected `dict[str, str]`, found `dict[str, Literal["GitHub user is not member of required organization"]]`
- Found 6905 diagnostics
+ Found 6874 diagnostics

scipy (https://github.com/scipy/scipy)
- error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:694:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, Literal[0]].__getitem__(key: str, /) -> Literal[0])` is not callable on object of type `tuple[Unknown] | dict[str, Literal[0]]`
+ error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:694:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, int].__getitem__(key: str, /) -> int)` is not callable on object of type `tuple[Unknown] | dict[str, int]`
- error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:698:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, Literal[0]].__getitem__(key: str, /) -> Literal[0])` is not callable on object of type `tuple[Unknown] | dict[str, Literal[0]]`
+ error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:698:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, int].__getitem__(key: str, /) -> int)` is not callable on object of type `tuple[Unknown] | dict[str, int]`
- error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:706:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, Literal[0]].__getitem__(key: str, /) -> Literal[0])` is not callable on object of type `tuple[Unknown] | dict[str, Literal[0]]`
+ error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:706:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, int].__getitem__(key: str, /) -> int)` is not callable on object of type `tuple[Unknown] | dict[str, int]`
- error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:709:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, Literal[0]].__getitem__(key: str, /) -> Literal[0])` is not callable on object of type `tuple[Unknown] | dict[str, Literal[0]]`
+ error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:709:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, int].__getitem__(key: str, /) -> int)` is not callable on object of type `tuple[Unknown] | dict[str, int]`
- error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:717:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, Literal[0]].__getitem__(key: str, /) -> Literal[0])` is not callable on object of type `tuple[Unknown] | dict[str, Literal[0]]`
+ error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:717:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, int].__getitem__(key: str, /) -> int)` is not callable on object of type `tuple[Unknown] | dict[str, int]`
- error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:720:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, Literal[0]].__getitem__(key: str, /) -> Literal[0])` is not callable on object of type `tuple[Unknown] | dict[str, Literal[0]]`
+ error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:720:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, int].__getitem__(key: str, /) -> int)` is not callable on object of type `tuple[Unknown] | dict[str, int]`
- error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:726:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, Literal[0]].__getitem__(key: str, /) -> Literal[0])` is not callable on object of type `tuple[Unknown] | dict[str, Literal[0]]`
+ error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:726:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, int].__getitem__(key: str, /) -> int)` is not callable on object of type `tuple[Unknown] | dict[str, int]`
- error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:731:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, Literal[0]].__getitem__(key: str, /) -> Literal[0])` is not callable on object of type `tuple[Unknown] | dict[str, Literal[0]]`
+ error[call-non-callable] scipy/optimize/tests/test_chandrupatla.py:731:9: Method `__getitem__` of type `(Overload[(key: SupportsIndex, /) -> Unknown, (key: slice[Any, Any, Any], /) -> tuple[Unknown, ...]]) | (bound method dict[str, int].__getitem__(key: str, /) -> int)` is not callable on object of type `tuple[Unknown] | dict[str, int]`

```
</details>


---

_@AlexWaygood reviewed on 2025-05-14 19:39_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:960 on 2025-05-14 19:39_

there are other promotions we could do too:
- Should class-literal types be promoted to `type[]` types?
- `Type::FunctionLiteral`, `Type::BoundMethod`, `Type::MethodWrapper`, `Type::WrapperDescriptor`, `Type::DataclassDecorator`, `Type::DataclassTransformer`: should any/all of these types be promoted to `Callable` types?
- Should heterogeneous tuple types be promoted to homogeneous tuple types? (Easy with `TupleType::homogeneous_supertype()`)

I'm not saying we necessarily should or shouldn't do any of these... but what's the framework for thinking about which ones we should do and which we shouldn't?

---

_@dcreager reviewed on 2025-05-14 20:21_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:960 on 2025-05-14 20:21_



> but what's the framework for thinking about which ones we should do and which we shouldn't?

Those are good examples of other things we'll need to support with full bidirectional type inference, but I don't think I'd use the mechanism from this PR for any of them. This was meant to be a much more focused stop-gap.  So tbh, the decision tree was just that this was the minimal change that had a nice diff on the ecosystem report!

---

_@AlexWaygood reviewed on 2025-05-14 20:22_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types.rs`:960 on 2025-05-14 20:22_

I see, that makes sense -- it could be worth recording that in a comment here?

---

_@dcreager reviewed on 2025-05-14 20:44_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types.rs`:960 on 2025-05-14 20:44_

Done

---

_@carljm approved on 2025-05-19 19:28_

Looks good, thank you.

---

_Merged by @dcreager on 2025-05-19 19:42_

---

_Closed by @dcreager on 2025-05-19 19:42_

---

_Branch deleted on 2025-05-19 19:42_

---
