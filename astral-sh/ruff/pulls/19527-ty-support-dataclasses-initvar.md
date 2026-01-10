```yaml
number: 19527
title: "[ty] Support `dataclasses.InitVar`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/initvar
created_at: 2025-07-24T11:32:59Z
updated_at: 2025-07-24T19:01:32Z
url: https://github.com/astral-sh/ruff/pull/19527
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Support `dataclasses.InitVar`

---

_Pull request opened by @sharkdp on 2025-07-24 11:32_

## Summary

I saw that this creates a lot of false positives in the ecosystem, and it seemed to be relatively easy to add basic support for this.

Some preliminary work on this was done by @InSyncWithFoo — thank you.

part of https://github.com/astral-sh/ty/issues/111

## Ecosystem analysis

The results look good.

## Test Plan

New Markdown tests


---

_Label `ty` added by @sharkdp on 2025-07-24 11:33_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-24 11:33_

---

_Comment by @github-actions[bot] on 2025-07-24 11:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
anyio (https://github.com/agronholm/anyio)
- src/anyio/streams/text.py:45:5: error[invalid-assignment] Object of type `Literal["utf-8"]` is not assignable to `InitVar[str]`
- src/anyio/streams/text.py:46:5: error[invalid-assignment] Object of type `Literal["strict"]` is not assignable to `InitVar[str]`
- src/anyio/streams/text.py:85:5: error[invalid-assignment] Object of type `Literal["utf-8"]` is not assignable to `InitVar[str]`
- src/anyio/streams/text.py:124:5: error[invalid-assignment] Object of type `Literal["utf-8"]` is not assignable to `InitVar[str]`
- src/anyio/streams/text.py:125:5: error[invalid-assignment] Object of type `Literal["strict"]` is not assignable to `InitVar[str]`
- src/anyio/streams/text.py:131:36: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- src/anyio/streams/text.py:131:55: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- src/anyio/streams/text.py:134:36: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- Found 91 diagnostics
+ Found 83 diagnostics

kopf (https://github.com/nolar/kopf)
- kopf/_cogs/structs/references.py:269:5: error[invalid-assignment] Object of type `None` is not assignable to `InitVar[None | str | Marker]`
- kopf/_cogs/structs/references.py:270:5: error[invalid-assignment] Object of type `None` is not assignable to `InitVar[None | str | Marker]`
- kopf/_cogs/structs/references.py:271:5: error[invalid-assignment] Object of type `None` is not assignable to `InitVar[None | str | Marker]`
- kopf/_cogs/structs/references.py:272:5: error[invalid-assignment] Object of type `None` is not assignable to `InitVar[None]`
- kopf/_cogs/structs/references.py:394:17: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["apiextensions.k8s.io"]`
- kopf/_cogs/structs/references.py:394:41: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["customresourcedefinitions"]`
- kopf/_cogs/structs/references.py:395:19: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["v1"]`
- kopf/_cogs/structs/references.py:395:25: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["events"]`
- kopf/_cogs/structs/references.py:396:23: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["events.k8s.io"]`
- kopf/_cogs/structs/references.py:396:40: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["events"]`
- kopf/_cogs/structs/references.py:397:23: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["v1"]`
- kopf/_cogs/structs/references.py:397:29: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["namespaces"]`
- kopf/_cogs/structs/references.py:398:31: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["kopf.dev/v1"]`
- kopf/_cogs/structs/references.py:398:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["clusterkopfpeerings"]`
- kopf/_cogs/structs/references.py:399:31: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["zalando.org/v1"]`
- kopf/_cogs/structs/references.py:399:49: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["clusterkopfpeerings"]`
- kopf/_cogs/structs/references.py:400:34: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["kopf.dev/v1"]`
- kopf/_cogs/structs/references.py:400:49: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["kopfpeerings"]`
- kopf/_cogs/structs/references.py:401:34: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["zalando.org/v1"]`
- kopf/_cogs/structs/references.py:401:52: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["kopfpeerings"]`
- kopf/_cogs/structs/references.py:402:29: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["admissionregistration.k8s.io"]`
- kopf/_cogs/structs/references.py:402:61: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["mutatingwebhookconfigurations"]`
- kopf/_cogs/structs/references.py:403:31: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["admissionregistration.k8s.io"]`
- kopf/_cogs/structs/references.py:403:63: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `Literal["validatingwebhookconfigurations"]`
- kopf/on.py:184:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:184:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:184:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:244:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:244:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:244:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:301:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:301:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:301:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:357:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:357:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:357:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:415:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:415:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:472:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:472:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:472:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:530:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:530:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:530:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:586:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:586:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:586:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:636:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:636:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:636:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:694:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:694:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:694:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:756:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:756:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- kopf/on.py:756:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[None | str | Marker]`, found `str | Marker | None`
- Found 117 diagnostics
+ Found 60 diagnostics

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- src/hydra_zen/structured_configs/_implementations.py:665:5: error[invalid-assignment] Object of type `None` is not assignable to `InitVar[ZenConvert | None]`
- Found 570 diagnostics
+ Found 569 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- strawberry/schema/config.py:17:5: error[invalid-assignment] Object of type `None` is not assignable to `InitVar[bool]`
+ strawberry/schema/config.py:17:5: error[invalid-assignment] Object of type `None` is not assignable to `bool`
- strawberry/schema/schema.py:433:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | None]`, found `str | None`
- strawberry/types/execution.py:46:5: error[invalid-assignment] Object of type `None` is not assignable to `InitVar[str | None]`
- Found 365 diagnostics
+ Found 363 diagnostics

poetry (https://github.com/python-poetry/poetry)
- src/poetry/console/exceptions.py:101:5: error[invalid-assignment] Object of type `Literal[""]` is not assignable to `InitVar[str]`
- src/poetry/console/exceptions.py:211:57: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["    | "]`
- src/poetry/vcs/git/backend.py:212:33: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[Unknown | Path]`, found `Unknown | Path`
- Found 935 diagnostics
+ Found 932 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/fixtures.py:1219:5: error[invalid-assignment] Object of type `Literal[False]` is not assignable to `InitVar[bool]`
- src/_pytest/fixtures.py:1389:9: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- testing/example_scripts/dataclasses/test_compare_initvar.py:15:16: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[int]`, found `Literal[1]`
- testing/example_scripts/dataclasses/test_compare_initvar.py:15:29: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[int]`, found `Literal[1]`
- Found 496 diagnostics
+ Found 492 diagnostics

bokeh (https://github.com/bokeh/bokeh)
- src/bokeh/plotting/gmap.py:78:67: error[invalid-argument-type] Argument to function `_get_num_minor_ticks` is incorrect: Expected `int | Literal["auto"] | None`, found `InitVar[Unknown | int]`
- src/bokeh/plotting/gmap.py:84:67: error[invalid-argument-type] Argument to function `_get_num_minor_ticks` is incorrect: Expected `int | Literal["auto"] | None`, found `InitVar[Unknown | int]`
- src/bokeh/plotting/gmap.py:87:55: error[invalid-argument-type] Argument to function `process_tools_arg` is incorrect: Expected `Sequence[Tool | str]`, found `InitVar[Sequence[str | Tool]]`
- src/bokeh/plotting/gmap.py:87:67: error[invalid-argument-type] Argument to function `process_tools_arg` is incorrect: Expected `str | list[tuple[str, str]] | None`, found `InitVar[Template | str | list[tuple[str, str]] | None]`
+ src/bokeh/plotting/gmap.py:87:67: error[invalid-argument-type] Argument to function `process_tools_arg` is incorrect: Expected `str | list[tuple[str, str]] | None`, found `Template | str | list[tuple[str, str]] | None`
- Found 837 diagnostics
+ Found 834 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/cargo/interpreter.py:295:5: error[invalid-assignment] Object of type `None` is not assignable to `InitVar[str | None]`
- mesonbuild/compilers/c.py:132:17: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `Unknown | list[Unknown]`
- mesonbuild/compilers/c.py:264:17: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `Unknown | list[Unknown]`
- mesonbuild/compilers/c.py:415:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `Unknown | list[Unknown]`
- mesonbuild/compilers/cpp.py:245:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default"]`
- mesonbuild/compilers/cpp.py:252:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/compilers/cpp.py:258:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/compilers/cpp.py:265:17: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `Unknown | list[Unknown]`
- mesonbuild/compilers/cpp.py:418:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default"]`
- mesonbuild/compilers/cpp.py:469:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default"]`
- mesonbuild/compilers/cpp.py:476:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/compilers/cpp.py:482:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/compilers/cpp.py:489:17: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `Unknown | list[Unknown]`
- mesonbuild/compilers/cpp.py:606:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default"]`
- mesonbuild/compilers/cpp.py:613:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/compilers/cpp.py:695:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default"]`
- mesonbuild/compilers/cpp.py:702:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/compilers/cpp.py:708:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/compilers/cpp.py:804:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default"]`
- mesonbuild/compilers/cpp.py:811:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/compilers/cpp.py:817:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `Unknown | list[Unknown]`
- mesonbuild/compilers/cuda.py:643:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["none"]`
- mesonbuild/compilers/cuda.py:650:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal[""]`
- mesonbuild/compilers/cython.py:77:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["3"]`
- mesonbuild/compilers/cython.py:84:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["c"]`
- mesonbuild/compilers/fortran.py:124:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["none"]`
- mesonbuild/compilers/mixins/emscripten.py:66:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[int]`, found `Literal[4]`
- mesonbuild/compilers/rust.py:241:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["none"]`
- mesonbuild/compilers/rust.py:248:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/compilers/swift.py:127:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["none"]`
- mesonbuild/coredata.py:387:17: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[int]`, found `Literal[0]`
- mesonbuild/coredata.py:393:17: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal[""]`
- mesonbuild/msubprojects.py:747:33: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[Resolver]`, found `Resolver`
- mesonbuild/options.py:688:59: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:689:60: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["bin"]`
- mesonbuild/options.py:690:60: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:691:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:692:60: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:693:57: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:694:62: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal[""]`
- mesonbuild/options.py:695:72: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:696:64: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:697:72: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["var"]`
- mesonbuild/options.py:698:61: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:699:68: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:700:87: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["com"]`
- mesonbuild/options.py:701:66: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `str`
- mesonbuild/options.py:707:85: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["auto"]`
- mesonbuild/options.py:708:54: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["ninja"]`
- mesonbuild/options.py:713:13: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["vs2022"]`
- mesonbuild/options.py:716:59: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["debug"]`
- mesonbuild/options.py:717:82: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/options.py:718:68: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["shared"]`
- mesonbuild/options.py:720:94: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["shared"]`
- mesonbuild/options.py:722:88: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/options.py:723:102: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[Literal["preserve"] | OctalInt]`, found `OctalInt`
- mesonbuild/options.py:724:61: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["mirror"]`
- mesonbuild/options.py:725:63: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["0"]`
- mesonbuild/options.py:726:99: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:727:79: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/options.py:728:64: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:729:49: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["off"]`
- mesonbuild/options.py:730:61: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[int]`, found `Literal[4]`
- mesonbuild/options.py:731:75: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["1"]`
- mesonbuild/options.py:733:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:734:51: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default"]`
- mesonbuild/options.py:735:93: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `list[Unknown]`
- mesonbuild/options.py:736:74: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:739:95: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:742:80: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[int]`, found `Literal[0]`
- mesonbuild/options.py:743:89: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["prefix"]`
- mesonbuild/options.py:745:104: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal[""]`
- mesonbuild/options.py:746:108: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal[""]`
- mesonbuild/options.py:747:105: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/options.py:755:103: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `list[Unknown]`
- mesonbuild/options.py:756:103: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `list[Unknown]`
- mesonbuild/options.py:774:63: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/options.py:775:66: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:776:95: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[int]`, found `Literal[0]`
- mesonbuild/options.py:777:78: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default"]`
- mesonbuild/options.py:778:104: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:779:93: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal[""]`
- mesonbuild/options.py:780:70: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str | list[str]]`, found `list[Unknown]`
- mesonbuild/options.py:781:78: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/options.py:782:77: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/options.py:784:57: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["off"]`
- mesonbuild/options.py:785:70: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:787:49: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["always"]`
- mesonbuild/options.py:789:44: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["false"]`
- mesonbuild/options.py:790:92: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- mesonbuild/options.py:791:81: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:792:92: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- mesonbuild/options.py:794:60: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["from_buildtype"]`
- unittests/optiontests.py:20:63: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["somevalue"]`
- unittests/optiontests.py:32:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["somevalue"]`
- unittests/optiontests.py:45:78: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["/usr"]`
- unittests/optiontests.py:47:64: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["ninja"]`
- unittests/optiontests.py:64:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["false"]`
- unittests/optiontests.py:91:63: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["somevalue"]`
- unittests/optiontests.py:100:60: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["original"]`
- unittests/optiontests.py:104:61: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["reset"]`
- unittests/optiontests.py:114:59: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["top"]`
- unittests/optiontests.py:118:61: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["sub"]`
- unittests/optiontests.py:128:59: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["top"]`
- unittests/optiontests.py:137:59: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["top"]`
- unittests/optiontests.py:141:61: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["sub"]`
- unittests/optiontests.py:153:63: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["top"]`
- unittests/optiontests.py:157:65: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["sub"]`
- unittests/optiontests.py:171:59: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default1"]`
- unittests/optiontests.py:177:61: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["default2"]`
- unittests/optiontests.py:197:30: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["c++11"]`
- unittests/optiontests.py:240:30: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["c++11"]`
- unittests/optiontests.py:274:78: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["/usr"]`
- unittests/optiontests.py:276:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[False]`
- unittests/optiontests.py:278:46: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- unittests/optiontests.py:295:78: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["/usr"]`
- unittests/optiontests.py:297:57: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["0"]`
- unittests/optiontests.py:316:78: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["/usr"]`
- unittests/optiontests.py:318:57: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["0"]`
- unittests/optiontests.py:336:78: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["/usr"]`
- unittests/optiontests.py:338:57: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["0"]`
- unittests/optiontests.py:357:78: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["/usr"]`
- unittests/optiontests.py:359:54: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["both"]`
- unittests/optiontests.py:382:82: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["/usr"]`
- unittests/optiontests.py:384:67: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["debug"]`
- unittests/optiontests.py:386:71: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["0"]`
- unittests/optiontests.py:388:90: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- unittests/optiontests.py:401:72: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[str]`, found `Literal["0"]`
- Found 885 diagnostics
+ Found 757 diagnostics

rotki (https://github.com/rotki/rotki)
- rotkehlchen/assets/asset.py:77:5: error[invalid-assignment] Object of type `Literal[False]` is not assignable to `InitVar[bool]`
- rotkehlchen/assets/asset.py:345:50: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- rotkehlchen/assets/asset.py:396:52: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- rotkehlchen/assets/asset.py:567:49: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- rotkehlchen/assets/asset.py:669:44: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- rotkehlchen/assets/asset.py:739:52: error[invalid-argument-type] Argument is incorrect: Expected `InitVar[bool]`, found `Literal[True]`
- Found 1570 diagnostics
+ Found 1564 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-07-24 11:42_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-argument-type` | 0 | 197 | 1 |
| `invalid-assignment` | 0 | 15 | 1 |
| **Total** | **0** | **212** | **2** |

**[Full report with detailed diff](https://david-initvar.ecosystem-663.pages.dev/diff)**


---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-24 12:01_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-24 12:01_

---

_Marked ready for review by @sharkdp on 2025-07-24 12:08_

---

_Review requested from @carljm by @sharkdp on 2025-07-24 12:08_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-07-24 12:08_

---

_Review requested from @dcreager by @sharkdp on 2025-07-24 12:08_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-24 12:08_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-24 12:08_

---

_@AlexWaygood reviewed on 2025-07-24 12:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:27 on 2025-07-24 12:09_

```suggestion
We can see in the signature of `__init__` that `db` is included as an argument:
```

---

_@AlexWaygood reviewed on 2025-07-24 12:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:55 on 2025-07-24 12:10_

```suggestion
## `InitVar` with default value
```

---

_@AlexWaygood reviewed on 2025-07-24 12:10_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:57 on 2025-07-24 12:10_

```suggestion
An `InitVar` can also have a default value:
```

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:99 on 2025-07-24 12:13_

I don't think it's hugely important for us to support this, but note that if you provide a default value for an `InitVar` then it _is_ accessible from both classes and instances:

```pycon
>>> from dataclasses import InitVar, dataclass
... 
... @dataclass
... class Person:
...     name: str
...     age: int
... 
...     metadata: InitVar[str] = "default"
...     
>>> Person.metadata
'default'
>>> p = Person("alice", 42)
>>> p.metadata
'default'
```

but it's still not a _true_ dataclass field: it's not considered for equality, etc.:

```pycon
>>> p = Person("alice", 42)
>>> p2 = Person("alice", 42)
>>> p2.metadata = 'whatever'
>>> p == p2
True
```

---

_@AlexWaygood reviewed on 2025-07-24 12:14_

---

_@sharkdp reviewed on 2025-07-24 12:17_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:99 on 2025-07-24 12:17_

> I don't think it's hugely important for us to support this, but note that if you provide a default value for an `InitVar` then it _is_ accessible from both classes and instances:

Oh! Maybe that is important to reflect, see the ecosystem changes in bokeh.

---

_Comment by @AlexWaygood on 2025-07-24 12:20_

Here are some mypy issues regarding `InitVar` edge cases, which it might be useful to add tests for:
- https://github.com/python/mypy/issues/12046
- https://github.com/python/mypy/issues/12877

There are also quite a few dataclass/`InitVar` crashes mypy has had in the past, though I suspect most of them aren't something that could happen to us, so it may not be useful for us to add tests for most of them: https://github.com/python/mypy/issues?q=is%3Aissue%20state%3Aclosed%20InitVar

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:7 on 2025-07-24 12:22_

Ideally we would also verify that the signature of `__post_init__` is correct: that it includes all dataclass fields _and_ all `InitVar` pseudo-fields in the parameters list (https://github.com/python/mypy/issues/15498). Doesn't need to be done in this PR, of course!

---

_@AlexWaygood reviewed on 2025-07-24 12:22_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-24 12:24_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-24 12:24_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:7 on 2025-07-24 12:25_

I don't think there's really any practical use for having an `InitVar` field if you _don't_ have a `__post_init__` method, right? So we could possibly even consider having a `WARN`-severity lint that complains if you do that.

I don't know if any other type checkers do that, though. And it could also make a good Ruff rule; it doesn't necessarily need to be done by ty.

---

_@AlexWaygood reviewed on 2025-07-24 12:25_

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-24 12:25_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-24 12:25_

---

_@sharkdp reviewed on 2025-07-24 12:27_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:7 on 2025-07-24 12:27_

> I don't think there's really any practical use for having an `InitVar` field if you _don't_ have a `__post_init__` method, right? So we could possibly even consider having a `WARN`-severity lint that complains if you do that.

I think you could also just have a custom `__init__` instead? In that case, the `InitVar` declaration would only be documentation, I guess.

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:58 on 2025-07-24 12:28_

though note that if it is a slotted dataclass, the `InitVar` will not be added to the class's `__slots__` (because it is not treated as a real dataclass field), so it will be readable from the class and instances of the class but it will not be _writable_ on instances of the class:

```pycon
>>> from dataclasses import InitVar, dataclass
... 
... @dataclass(slots=True)
... class Person:
...     name: str
...     age: int
... 
...     metadata: InitVar[str] = "default"
...     
>>> p = Person('bob', 42)
>>> p.metadata
'default'
>>> p.metadata = 'something else'
Traceback (most recent call last):
  File "<python-input-11>", line 1, in <module>
    p.metadata = 'something else'
    ^^^^^^^^^^
AttributeError: 'Person' object attribute 'metadata' is read-only
```

---

_@AlexWaygood reviewed on 2025-07-24 12:28_

---

_@sharkdp reviewed on 2025-07-24 12:28_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:99 on 2025-07-24 12:28_

This should be fixed now. And the new diagnostics on bokeh are gone — thank you Alex!

---

_@AlexWaygood reviewed on 2025-07-24 12:30_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:7 on 2025-07-24 12:30_

Ah, right, good point! I forgot that you can manually supply an `__init__` method but still have lots of other methods generated by dataclasses. It would not just be documentation -- the field would not be included in the generated `__repr__` of instances, it would not be taken into account by the generated `__eq__` method of the class, and (if it's a slotted dataclass) it won't be added to the dataclass's `__slots__`.

---

_@sharkdp reviewed on 2025-07-24 12:30_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:58 on 2025-07-24 12:30_

Yeah, we don't have support for `slots` yet, see https://github.com/astral-sh/ty/issues/111. But I guess this PR provides the necessary metadata now to treat this properly.

---

_@sharkdp reviewed on 2025-07-24 12:31_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:7 on 2025-07-24 12:31_

Added to https://github.com/astral-sh/ty/issues/111

---

_@AlexWaygood reviewed on 2025-07-24 12:33_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:99 on 2025-07-24 12:33_

Excellent! 😃

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/class.rs`:884 on 2025-07-24 12:35_

```suggestion
/// Metadata regarding a dataclass field/attribute.
```

---

_@sharkdp reviewed on 2025-07-24 12:36_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:7 on 2025-07-24 12:36_

> It would not just be documentation -- the field would not be included in the generated `__repr__` of instances, it would not be taken into account by the generated `__eq__` method of the class, and (if it's a slotted dataclass) it won't be added to the dataclass's `__slots__`.

Right, but you could also just not mention the init-only field at all in that case, right? :-)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:2673 on 2025-07-24 12:47_

or you could do this?

```diff
diff --git a/crates/ty_python_semantic/src/types.rs b/crates/ty_python_semantic/src/types.rs
index 71c6eed57e..df778234b7 100644
--- a/crates/ty_python_semantic/src/types.rs
+++ b/crates/ty_python_semantic/src/types.rs
@@ -6142,6 +6142,17 @@ bitflags! {
     }
 }
 
+impl TypeQualifiers {
+    const fn name(self) -> &'static str {
+        match self {
+            TypeQualifiers::CLASS_VAR => "ClassVar",
+            TypeQualifiers::FINAL => "Final",
+            TypeQualifiers::INIT_VAR => "InitVar",
+            _ => "",
+        }
+    }
+}
+
 impl get_size2::GetSize for TypeQualifiers {}
 
 /// When inferring the type of an annotation expression, we can also encounter type qualifiers
diff --git a/crates/ty_python_semantic/src/types/infer.rs b/crates/ty_python_semantic/src/types/infer.rs
index f0699bc2af..11c214ae90 100644
--- a/crates/ty_python_semantic/src/types/infer.rs
+++ b/crates/ty_python_semantic/src/types/infer.rs
@@ -2654,16 +2654,17 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             let annotated = self.infer_annotation_expression(returns, deferred_expression_state);
 
             if !annotated.qualifiers.is_empty() {
-                for (qualifier, name) in [
-                    (TypeQualifiers::FINAL, "Final"),
-                    (TypeQualifiers::CLASS_VAR, "ClassVar"),
-                    (TypeQualifiers::INIT_VAR, "InitVar"),
+                for qualifier in [
+                    TypeQualifiers::FINAL,
+                    TypeQualifiers::CLASS_VAR,
+                    TypeQualifiers::INIT_VAR,
                 ] {
                     if annotated.qualifiers.contains(qualifier) {
                         if let Some(builder) = self.context.report_lint(&INVALID_TYPE_FORM, returns)
                         {
                             builder.into_diagnostic(format!(
-                                "`{name}` is not allowed in function return type annotations",
+                                "`{}` is not allowed in function return type annotations",
+                                qualifier.name()
                             ));
                         }
                     }
@@ -2709,17 +2710,18 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
 
         if let Some(qualifiers) = annotated.map(|annotated| annotated.qualifiers) {
             if !qualifiers.is_empty() {
-                for (qualifier, name) in [
-                    (TypeQualifiers::FINAL, "Final"),
-                    (TypeQualifiers::CLASS_VAR, "ClassVar"),
-                    (TypeQualifiers::INIT_VAR, "InitVar"),
+                for qualifier in [
+                    TypeQualifiers::FINAL,
+                    TypeQualifiers::CLASS_VAR,
+                    TypeQualifiers::INIT_VAR,
                 ] {
                     if qualifiers.contains(qualifier) {
                         if let Some(builder) =
                             self.context.report_lint(&INVALID_TYPE_FORM, parameter)
                         {
                             builder.into_diagnostic(format!(
-                                "`{name}` is not allowed in function parameter annotations",
+                                "`{}` is not allowed in function parameter annotations",
+                                qualifier.name()
                             ));
                         }
                     }
@@ -4272,17 +4274,15 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
                 self.infer_annotation_expression(annotation, DeferredExpressionState::None);
 
             if !annotated.qualifiers.is_empty() {
-                for (qualifier, name) in [
-                    (TypeQualifiers::CLASS_VAR, "ClassVar"),
-                    (TypeQualifiers::INIT_VAR, "InitVar"),
-                ] {
+                for qualifier in [TypeQualifiers::CLASS_VAR, TypeQualifiers::INIT_VAR] {
                     if annotated.qualifiers.contains(qualifier) {
                         if let Some(builder) = self
                             .context
                             .report_lint(&INVALID_TYPE_FORM, annotation.as_ref())
                         {
                             builder.into_diagnostic(format_args!(
-                                "`{name}` annotations are not allowed for non-name targets",
+                                "`{}` annotations are not allowed for non-name targets",
+                                qualifier.name()
                             ));
                         }
                     }
@@ -4324,16 +4324,14 @@ impl<'db, 'ast> TypeInferenceBuilder<'db, 'ast> {
             let current_scope_id = self.scope().file_scope_id(self.db());
             let current_scope = self.index.scope(current_scope_id);
             if current_scope.kind() != ScopeKind::Class {
-                for (qualifier, name) in [
-                    (TypeQualifiers::CLASS_VAR, "ClassVar"),
-                    (TypeQualifiers::INIT_VAR, "InitVar"),
-                ] {
+                for qualifier in [TypeQualifiers::CLASS_VAR, TypeQualifiers::INIT_VAR] {
                     if declared.qualifiers.contains(qualifier) {
                         if let Some(builder) =
                             self.context.report_lint(&INVALID_TYPE_FORM, annotation)
                         {
                             builder.into_diagnostic(format_args!(
-                                "`{name}` annotations are only allowed in class-body scopes"
+                                "`{}` annotations are only allowed in class-body scopes",
+                                qualifier.name()
                             ));
                         }
                     }
```

---

_@AlexWaygood approved on 2025-07-24 12:48_

---

_@AlexWaygood reviewed on 2025-07-24 12:48_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/type_qualifiers/initvar.md`:7 on 2025-07-24 12:48_

...true!

---

_@sharkdp reviewed on 2025-07-24 13:06_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/infer.rs`:2673 on 2025-07-24 13:06_

```diff
+impl TypeQualifiers {
+    const fn name(self) -> &'static str {
```

I thought about that quickly, but I don't like it too much. A bit set is not an enum. If multiple qualifiers are set, that won't work. But if you think the duplication here is ugly, I can try to think about a solution.

---

_@AlexWaygood reviewed on 2025-07-24 13:09_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/infer.rs`:2673 on 2025-07-24 13:09_

Right, I guess it should really return `Option<&'static str>` and return `None` if more than one flag is set. And then you'd have to unwrap it to use it in `format!()` strings, which is at least frustrating.

I don't _love_ the duplication, but it's obviously not a huge issue -- please don't spend a significant amount of time trying to fix it!

---

_@MichaReiser reviewed on 2025-07-24 13:13_

---

_Review comment by @MichaReiser on `crates/ty_python_semantic/src/types/infer.rs`:2673 on 2025-07-24 13:13_

Without having read or studied the code. I discovered `bitset::bitflags_match` yesterday https://docs.rs/bitflags/latest/bitflags/macro.bitflags_match.html. Not sure if this is relevant or useful here

---

_Comment by @sharkdp on 2025-07-24 14:01_

> [Error on instance property and init-only variable with the same name in a dataclass python/mypy#12046](https://github.com/python/mypy/issues/12046)

Hm.. that would be difficult to support. And that example doesn't even appear to work at runtime for me (*TypeError: non-default argument '_prop' follows default argument 'prop'*)
 
> [Assigning to an attribute with the same name as a dataclass `InitVar` raises `has no attribute` on 0.960 python/mypy#12877](https://github.com/python/mypy/issues/12877)

Okay. Made some changes to support that scenario, and added a test.

---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-07-24 14:01_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-07-24 14:01_

---

_Merged by @sharkdp on 2025-07-24 14:33_

---

_Closed by @sharkdp on 2025-07-24 14:33_

---

_Branch deleted on 2025-07-24 14:33_

---

_Comment by @AlexWaygood on 2025-07-24 14:33_

> > [Assigning to an attribute with the same name as a dataclass `InitVar` raises `has no attribute` on 0.960 python/mypy#12877](https://github.com/python/mypy/issues/12877)
> 
> Okay. Made some changes to support that scenario, and added a test.

Oh, thank you! Though I wasn't necessarily asking for any changes to the implementation here, just for an explicit test so it was obvious what our behaviour was. Sorry if I was unclear there.

Mypy decided that its behaviour here was a feature rather than a bug, so I think sticking with what you had before would have been defensible -- you could argue that creating an implicit instance variable by assigning to that attribute in an instance method contradicts the explicit declaration of the variable as `InitVar` (and `InitVar` only?) in the class body.

That said, I also think the new behaviour you've implemented is fine. It obviously works fine at runtime, and we're generally more permissive than other type checkers when it comes to our understanding of the set of implicit instance variables a class has -- being more permissive here feels in line with our general philosophy there.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6148 on 2025-07-24 16:02_

Incomplete sentence?

---

_@carljm reviewed on 2025-07-24 16:09_

Nice!

---

_@sharkdp reviewed on 2025-07-24 19:01_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types.rs`:6148 on 2025-07-24 19:01_

fixed, thanks.

---
