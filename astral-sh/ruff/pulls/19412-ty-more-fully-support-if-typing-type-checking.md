```yaml
number: 19412
title: "[ty] More fully support 'if typing.TYPE_CHECKING'"
type: pull_request
state: closed
author: carljm
labels:
  - ty
assignees: []
draft: true
base: main
head: cjm/iftc
created_at: 2025-07-17T20:32:22Z
updated_at: 2025-11-14T17:23:53Z
url: https://github.com/astral-sh/ruff/pull/19412
synced_at: 2026-01-10T16:53:55Z
```

# [ty] More fully support 'if typing.TYPE_CHECKING'

---

_Pull request opened by @carljm on 2025-07-17 20:32_

## Summary

#19372 added support for detecting whether a function is defined inside an `if TYPE_CHECKING` block, and if so, allowing it to be a "stub" function (empty body, even with non-None return type).

This PR improves that support by also supporting it for `if typing.TYPE_CHECKING` as well as `if TYPE_CHECKING`. In order to do so, it changes our basic approach to `TYPE_CHECKING` support. Rather than inferring a type of `Literal[True]` for all symbols named `TYPE_CHECKING`, instead we specially handle `if` conditions with purely syntactic matching (on any `Name` matching `TYPE_CHECKING` or any attribute expression with attribute `TYPE_CHECKING`). This means we no longer recognize aliasing of `TYPE_CHECKING` (e.g. `from typing import TYPE_CHECKING as TC`), and we'll recognize `foo.TYPE_CHECKING` no matter what `foo` is. (In both cases this matches the behavior of all other existing type checkers.) Arguably it's better to not support aliasing `TYPE_CHECKING`, given its unusual behavior of matching any variable named `TYPE_CHECKING`.

We currently don't handle conditions like `if TYPE_CHECKING or get_bool()` (other type checkers do). If this proves to be relevant, we can add it.

I think it would also be possible to remove the extra boolean flag on scopes and in SemanticIndexBuilder, and rely instead on reachability constraints (which we already track on scopes) for detecting when a scope is defined inside `if TYPE_CHECKING`. This would require adding new terminal reachability constraints for `TYPE_CHECKING` and probably `NOT_TYPE_CHECKING` as well, which would behave like `ALWAYS_TRUE` and `ALWAYS_FALSE`, but would also allow detecting that a reachability constraint is dependent on `TYPE_CHECKING`. I didn't choose to do that here, but it's a possible future improvement.

Another alternative to this entire PR would be to instead double down on our handling of `TYPE_CHECKING` in type inference rather than syntactically, add a new `SpecialForm` for "the type of `TYPE_CHECKING`", and then we could evaluate reachability constraints under "consider `TYPE_CHECKING` False" conditions in order to detect code that is only reachable due to `TYPE_CHECKING`.

## Test Plan

mdtests


---

_Label `ty` added by @carljm on 2025-07-17 20:32_

---

_Review requested from @AlexWaygood by @carljm on 2025-07-17 20:32_

---

_Review requested from @sharkdp by @carljm on 2025-07-17 20:32_

---

_Review requested from @dcreager by @carljm on 2025-07-17 20:32_

---

_Comment by @github-actions[bot] on 2025-07-17 20:36_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- error[invalid-return-type] bidict/_bidict.py:41:30: Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT, KT]`
- error[invalid-return-type] bidict/_bidict.py:44:26: Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT, KT]`
- error[invalid-return-type] bidict/_bidict.py:185:30: Function always implicitly returns `None`, which is not assignable to return type `bidict[VT, KT]`
- error[invalid-return-type] bidict/_bidict.py:188:26: Function always implicitly returns `None`, which is not assignable to return type `bidict[VT, KT]`
- error[invalid-return-type] bidict/_frozen.py:34:30: Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT, KT]`
- error[invalid-return-type] bidict/_frozen.py:37:26: Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT, KT]`
- error[invalid-return-type] bidict/_orderedbase.py:138:30: Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
- error[invalid-return-type] bidict/_orderedbase.py:141:26: Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
- error[invalid-return-type] bidict/_orderedbidict.py:39:30: Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
- error[invalid-return-type] bidict/_orderedbidict.py:42:26: Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
- Found 14 diagnostics
+ Found 4 diagnostics

trio (https://github.com/python-trio/trio)
+ error[unresolved-attribute] src/trio/_core/_generated_io_kqueue.py:35:25: Type `<module 'select'>` has no attribute `kqueue`
+ error[unresolved-attribute] src/trio/_core/_generated_io_kqueue.py:41:16: Type `EpollIOManager` has no attribute `current_kqueue`
+ error[unresolved-attribute] src/trio/_core/_generated_io_kqueue.py:49:50: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_core/_generated_io_kqueue.py:55:16: Type `EpollIOManager` has no attribute `monitor_kevent`
+ error[unresolved-attribute] src/trio/_core/_generated_io_kqueue.py:69:22: Type `EpollIOManager` has no attribute `wait_kevent`
+ error[unresolved-attribute] src/trio/_core/_generated_io_windows.py:125:16: Type `EpollIOManager` has no attribute `register_with_iocp`
+ error[unresolved-attribute] src/trio/_core/_generated_io_windows.py:138:22: Type `EpollIOManager` has no attribute `wait_overlapped`
+ error[unresolved-attribute] src/trio/_core/_generated_io_windows.py:155:22: Type `EpollIOManager` has no attribute `write_overlapped`
+ error[unresolved-attribute] src/trio/_core/_generated_io_windows.py:172:22: Type `EpollIOManager` has no attribute `readinto_overlapped`
+ error[unresolved-attribute] src/trio/_core/_generated_io_windows.py:187:16: Type `EpollIOManager` has no attribute `current_iocp`
+ error[unresolved-attribute] src/trio/_core/_generated_io_windows.py:202:16: Type `EpollIOManager` has no attribute `monitor_completion_key`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:38:14: Type `<module 'select'>` has no attribute `kqueue`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:38:44: Type `<module 'select'>` has no attribute `kqueue`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:40:62: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:47:30: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:49:13: Type `<module 'select'>` has no attribute `KQ_FILTER_READ`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:50:13: Type `<module 'select'>` has no attribute `KQ_EV_ADD`
+ error[invalid-type-form] src/trio/_core/_io_kqueue.py:72:45: Variable of type `Literal["list[select.kevent]"]` is not allowed in a type expression
+ error[invalid-type-form] src/trio/_core/_io_kqueue.py:89:38: Variable of type `Literal["list[select.kevent]"]` is not allowed in a type expression
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:96:30: Type `<module 'select'>` has no attribute `KQ_EV_ONESHOT`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:115:33: Type `<module 'select'>` has no attribute `kqueue`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:128:40: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:138:34: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:179:17: Type `<module 'select'>` has no attribute `KQ_EV_ADD`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:179:36: Type `<module 'select'>` has no attribute `KQ_EV_ONESHOT`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:180:17: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:184:21: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:184:47: Type `<module 'select'>` has no attribute `KQ_EV_DELETE`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:230:37: Type `<module 'select'>` has no attribute `KQ_FILTER_READ`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:245:37: Type `<module 'select'>` has no attribute `KQ_FILTER_WRITE`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:276:25: Type `<module 'select'>` has no attribute `KQ_FILTER_READ`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:276:48: Type `<module 'select'>` has no attribute `KQ_FILTER_WRITE`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:284:25: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_core/_io_kqueue.py:284:52: Type `<module 'select'>` has no attribute `KQ_EV_DELETE`
+ error[unresolved-attribute] src/trio/_core/_io_windows.py:368:16: Type `OSError` has no attribute `winerror`
+ error[unresolved-attribute] src/trio/_core/_io_windows.py:557:16: Type `OSError` has no attribute `winerror`
+ error[invalid-argument-type] src/trio/_core/_io_windows.py:598:48: Argument is incorrect: Expected `bool`, found `CompletionKeyEventInfo`
+ error[unresolved-attribute] src/trio/_core/_io_windows.py:677:20: Type `OSError` has no attribute `winerror`
+ error[unresolved-attribute] src/trio/_core/_io_windows.py:726:20: Type `OSError` has no attribute `winerror`
+ error[unresolved-attribute] src/trio/_core/_io_windows.py:875:20: Type `OSError` has no attribute `winerror`
+ error[unresolved-attribute] src/trio/_core/_io_windows.py:945:16: Type `OSError` has no attribute `winerror`
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:45:28: Attribute `getwinerror` on type `Unknown | FFI` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:96:42: Attribute `current_iocp` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:104:10: Attribute `monitor_completion_key` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:152:27: Attribute `readinto_overlapped` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:158:17: Attribute `register_with_iocp` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:166:27: Attribute `readinto_overlapped` on type `<module 'src.trio._core'>` is possibly unbound
+ error[unresolved-import] src/trio/_core/_tests/test_windows.py:174:39: Module `asyncio.windows_utils` has no member `pipe`
+ error[unresolved-attribute] src/trio/_core/_tests/test_windows.py:178:20: Type `<module 'msvcrt'>` has no attribute `open_osfhandle`
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:198:25: Attribute `readinto_overlapped` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:227:9: Attribute `register_with_iocp` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:231:32: Attribute `readinto_overlapped` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_core/_tests/test_windows.py:247:22: Attribute `readinto_overlapped` on type `<module 'src.trio._core'>` is possibly unbound
+ error[unresolved-attribute] src/trio/_socket.py:330:35: Type `<module 'socket'>` has no attribute `fromshare`
+ warning[possibly-unbound-attribute] src/trio/_subprocess_platform/kqueue.py:13:14: Attribute `current_kqueue` on type `<module 'src.trio._core'>` is possibly unbound
+ error[unresolved-import] src/trio/_subprocess_platform/kqueue.py:15:28: Module `select` has no member `KQ_NOTE_EXIT`
+ error[unresolved-attribute] src/trio/_subprocess_platform/kqueue.py:22:35: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_subprocess_platform/kqueue.py:23:16: Type `<module 'select'>` has no attribute `kevent`
+ error[unresolved-attribute] src/trio/_subprocess_platform/kqueue.py:25:20: Type `<module 'select'>` has no attribute `KQ_FILTER_PROC`
+ error[unresolved-attribute] src/trio/_subprocess_platform/kqueue.py:31:36: Type `<module 'select'>` has no attribute `KQ_EV_ADD`
+ error[unresolved-attribute] src/trio/_subprocess_platform/kqueue.py:31:55: Type `<module 'select'>` has no attribute `KQ_EV_ONESHOT`
+ error[unresolved-attribute] src/trio/_subprocess_platform/kqueue.py:45:36: Type `<module 'select'>` has no attribute `KQ_EV_DELETE`
+ warning[possibly-unbound-attribute] src/trio/_subprocess_platform/kqueue.py:48:11: Attribute `wait_kevent` on type `<module 'src.trio._core'>` is possibly unbound
+ error[unresolved-attribute] src/trio/_subprocess_platform/kqueue.py:48:42: Type `<module 'select'>` has no attribute `KQ_FILTER_PROC`
+ warning[possibly-unbound-attribute] src/trio/_tests/test_socket.py:354:14: Attribute `fromshare` on type `<module 'src.trio.socket'>` is possibly unbound
+ error[invalid-argument-type] src/trio/_tests/test_unix_pipes.py:105:18: Argument to bound method `__init__` is incorrect: Expected `int`, found `None`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:122:12: Type `BaseException` has no attribute `errno`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:126:12: Type `BaseException` has no attribute `errno`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:139:12: Type `BaseException` has no attribute `errno`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:143:12: Type `BaseException` has no attribute `errno`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:198:26: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:201:15: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:207:25: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:229:26: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:232:15: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[unresolved-attribute] src/trio/_tests/test_unix_pipes.py:238:25: Type `<module 'src.trio._core'>` has no attribute `_run`
+ error[invalid-type-form] src/trio/_tests/test_windows_pipes.py:25:32: Variable of type `Never` is not allowed in a type expression
+ error[invalid-type-form] src/trio/_tests/test_windows_pipes.py:25:48: Variable of type `Never` is not allowed in a type expression
+ warning[possibly-unbound-attribute] src/trio/_windows_pipes.py:25:9: Attribute `register_with_iocp` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_windows_pipes.py:65:33: Attribute `write_overlapped` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-attribute] src/trio/_windows_pipes.py:114:30: Attribute `readinto_overlapped` on type `<module 'src.trio._core'>` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:67:9: Member `current_iocp` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:68:9: Member `monitor_completion_key` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:69:9: Member `readinto_overlapped` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:70:9: Member `register_with_iocp` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:71:9: Member `wait_overlapped` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:72:9: Member `write_overlapped` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:90:13: Member `current_kqueue` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:91:13: Member `monitor_kevent` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/lowlevel.py:92:13: Member `wait_kevent` of module `src.trio._core` is possibly unbound
+ warning[possibly-unbound-import] src/trio/socket.py:55:30: Member `fromshare` of module `src.trio._socket` is possibly unbound
- Found 746 diagnostics
+ Found 837 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- error[invalid-return-type] pydantic/_internal/_utils.py:307:70: Function always implicitly returns `None`, which is not assignable to return type `T`
- Found 747 diagnostics
+ Found 746 diagnostics

jinja (https://github.com/pallets/jinja)
- error[invalid-return-type] src/jinja2/ext.py:34:59: Function always implicitly returns `None`, which is not assignable to return type `str`
- error[invalid-return-type] src/jinja2/ext.py:38:14: Function always implicitly returns `None`, which is not assignable to return type `str`
- Found 196 diagnostics
+ Found 194 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
+ error[missing-argument] src/typeshed_stats/gather.py:645:20: No argument provided for required parameter `url` of function `get`
+ error[missing-argument] src/typeshed_stats/gather.py:645:20: No argument provided for required parameter `url` of function `get`
- Found 24 diagnostics
+ Found 26 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- error[invalid-return-type] src/_pytest/capture.py:968:16: Return type does not match returned value: expected `CaptureResult[AnyStr]`, found `CaptureResult[str]`
+ warning[unused-ignore-comment] src/_pytest/capture.py:709:41: Unused blanket `type: ignore` directive

sphinx (https://github.com/sphinx-doc/sphinx)
+ warning[unused-ignore-comment] sphinx/ext/autodoc/importer.py:202:43: Unused blanket `type: ignore` directive
- Found 531 diagnostics
+ Found 532 diagnostics

meson (https://github.com/mesonbuild/meson)
- error[invalid-return-type] mesonbuild/compilers/d.py:110:44: Function always implicitly returns `None`, which is not assignable to return type `list[str]`
- error[invalid-return-type] mesonbuild/linkers/linkers.py:625:68: Function always implicitly returns `None`, which is not assignable to return type `list[str]`
- error[invalid-return-type] mesonbuild/linkers/linkers.py:1334:68: Function always implicitly returns `None`, which is not assignable to return type `list[str]`
- error[invalid-return-type] mesonbuild/options.py:401:36: Function always implicitly returns `None`, which is not assignable to return type `int`
- Found 903 diagnostics
+ Found 899 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Converted to draft by @carljm on 2025-07-17 20:39_

---

_Comment by @carljm on 2025-07-17 20:39_

Primer results suggest there's something necessary missing in the new version of our TYPE_CHECKING support here.

---

_Comment by @MatthewMckee4 on 2025-07-17 21:01_

The diff from `https://github.com/python-trio/trio` is very interesting

---

_Comment by @AlexWaygood on 2025-07-21 13:39_

it appears as though we're no longer viewing the remainder of the module as unreachable code (in which most diagnostics should be suppressed) following this `assert not TYPE_CHECKING` line here: https://github.com/python-trio/trio/blob/3326972d70569b5867773c5fbd79223708e0f59d/src/trio/_core/_generated_io_kqueue.py#L21. That and similar assertions in trio appear to be causing the new diagnostics.

---

_Comment by @AlexWaygood on 2025-11-14 13:12_

https://github.com/astral-sh/ruff/pull/21449 looks like it got rid of the same set of false positives without adding any of the new false positives. Is this PR still needed, do you think?

---

_Comment by @carljm on 2025-11-14 17:23_

No, I think we can close this. There are some remaining inconsistencies in our support for `TYPE_CHECKING` (we recognize it when aliased in some circumstances but not in others) but I think any further work there should be driven by actual user reports. Thanks for following up on this!

---

_Closed by @carljm on 2025-11-14 17:23_

---
