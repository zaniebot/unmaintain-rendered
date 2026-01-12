```yaml
number: 17345
title: "[red-knot] Type narrowing for assertions (take 2)"
type: pull_request
state: merged
author: carljm
labels:
  - ty
assignees: []
merged: true
base: main
head: cjm/reapply-assert-narrowing
created_at: 2025-04-11T03:45:56Z
updated_at: 2025-04-18T15:11:09Z
url: https://github.com/astral-sh/ruff/pull/17345
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Type narrowing for assertions (take 2)

---

_@carljm_

## Summary

Fixes #17147.

This was landed in #17149 and then reverted in #17335 because it caused cycle panics in checking pybind11. #17456 fixed the cause of that panic.

## Test Plan

Add new narrow/assert.md test file

---

_Label `red-knot` added by @carljm on 2025-04-11 03:45_

---

_Comment by @github-actions[bot] on 2025-04-11 03:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:97:22: Attribute `rsplit` on type `str | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:97:22: Attribute `rsplit` on type `str | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:97:22: Attribute `rsplit` on type `str | None` is possibly unbound
- Found 336 diagnostics
+ Found 333 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:69:5: Attribute `self_check` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:71:12: Attribute `total_self_time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:72:12: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:73:16: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:74:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:75:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:76:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:77:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:79:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:80:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:81:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:123:5: Attribute `self_check` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:125:12: Attribute `total_self_time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:126:12: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:127:16: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:128:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:129:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:130:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:131:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:159:5: Attribute `self_check` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:161:12: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:162:16: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:163:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:164:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:165:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:166:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:167:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:168:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:207:5: Attribute `self_check` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:208:12: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:210:16: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:211:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:212:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:213:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:214:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:215:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:216:12: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:252:5: Attribute `self_check` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:253:12: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:255:16: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:256:60: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:288:5: Attribute `self_check` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:289:12: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:290:16: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:292:28: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:347:5: Attribute `self_check` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:348:12: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_processors.py:349:18: Attribute `children` on type `Frame | None` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline_main.py:52:12: Type `None` has no attribute `time`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline_main.py:76:12: Type `None` has no attribute `processor_options`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline_main.py:100:12: Type `None` has no attribute `processor_options`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:90:12: Attribute `function` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:91:16: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:112:12: Attribute `function` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:113:16: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_threading.py:20:48: Attribute `frame_records` on type `Unknown | ActiveProfilerSession | None` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_threading.py:44:12: Type `None` has no attribute `args`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_pstats_renderer.py:125:43: Argument to this function is incorrect: Expected `Session`, found `Session | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/context_manager.py:76:17: Attribute `f_code` on type `FrameType | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/context_manager.py:76:50: Attribute `f_lineno` on type `FrameType | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/stack_sampler.py:249:53: Argument to this function is incorrect: Expected `int | float`, found `int | float | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline.py:274:25: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline.py:275:20: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:51:12: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:52:12: Attribute `await_time` on type `Frame | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:54:47: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:74:16: Attribute `time` on type `Frame | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:75:16: Attribute `await_time` on type `Frame | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:77:51: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:158:18: Attribute `root_frame` on type `None | @Todo(Support for `typing.TypeVar` instances in type expressions)` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:166:44: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:199:12: Attribute `time` on type `Frame | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:201:44: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:231:12: Attribute `time` on type `Frame | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:233:44: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:238:32: Argument to this function is incorrect: Expected `Frame`, found `Frame | None`
- Found 276 diagnostics
+ Found 197 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/element.py:115:23: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/element.py:124:23: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/element.py:152:23: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- Found 501 diagnostics
+ Found 498 diagnostics

porcupine (https://github.com/Akuli/porcupine)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/porcupine/porcupine/_logs.py:52:25: Function can implicitly return `None`, which is not assignable to return type `TextIO`
- error[lint:not-iterable] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/jump_to_definition.py:65:32: Object of type `None` is not iterable because it doesn't have an `__iter__` method or a `__getitem__` method
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:46:8: Attribute `indent` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:48:24: Attribute `indent` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:50:50: Attribute `indent` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:51:13: Object of type `None` is not assignable to attribute `indent` on type `Unknown & ~None | Path | AutoIndentRegexes`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:53:8: Attribute `dedent` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:55:24: Attribute `dedent` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:57:50: Attribute `dedent` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:58:13: Object of type `None` is not assignable to attribute `dedent` on type `Unknown & ~None | Path | AutoIndentRegexes`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:61:9: Attribute `indent` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:62:9: Attribute `dedent` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autoindent.py:63:9: Attribute `dedent_prev_line` on type `Unknown & ~None | Path | AutoIndentRegexes` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/sort.py:29:31: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/aboutdialog.py:160:40: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/python_venv.py:123:21: Argument to this function is incorrect: Expected `Path`, found `Path | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/python_venv.py:147:18: Argument to this function is incorrect: Expected `Path`, found `Unknown | Path | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/python_venv.py:150:46: Argument to this function is incorrect: Expected `Path`, found `Path | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:246:20: Attribute `is_dir` on type `Path | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:290:25: Attribute `iterdir` on type `Path | None` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/porcupine/scripts/build-exe-installer.py:56:12: Type `<module 'hashlib'>` has no attribute `sha256`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/tests/test_menubar.py:127:27: Argument to this function is incorrect: Expected `str | int`, found `int | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/tests/test_menubar.py:136:27: Argument to this function is incorrect: Expected `str | int`, found `int | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:445:30: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:678:43: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:682:36: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:694:73: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:700:53: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:706:43: Argument to this function is incorrect: Expected `LangServerConfig`, found `Unknown & ~None | Path`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:708:32: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:268:21: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:271:32: Operator `+` is unsupported between objects of type `int | None` and `Literal[1]`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:364:12: Return type does not match returned value: Expected `FileTab`, found `Tab | None`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:447:13: Type `Tab | None` has no attribute `save_as`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:449:13: Type `Tab | None` has no attribute `save`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:454:12: Attribute `can_be_closed` on type `Tab | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:455:41: Argument to this function is incorrect: Expected `Tab`, found `Tab | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:437:20: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/textutils.py:892:9: Type `Widget` has no attribute `config`
- Found 453 diagnostics
+ Found 414 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/polyglot.py:477:152: Function can implicitly return `None`, which is not assignable to return type `Entry`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/chess/pgn.py:1668:28: Attribute `variant` on type `None | Any | @Todo(Support for `typing.TypeVar` instances in type expressions) | Headers` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/chess/pgn.py:1674:15: Attribute `get` on type `None | Any | @Todo(Support for `typing.TypeVar` instances in type expressions) | Headers` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/chess/pgn.py:1676:48: Attribute `is_chess960` on type `None | Any | @Todo(Support for `typing.TypeVar` instances in type expressions) | Headers` is possibly unbound
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:815:16: Name `bb` used when possibly not defined
- Found 408 diagnostics
+ Found 403 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/werkzeug/tests/sansio/test_multipart.py:127:16: Type `Event` has no attribute `more_data`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_http.py:728:12: Attribute `to_header` on type `@Todo(Support for `typing.Self`) | None` is possibly unbound
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:208:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["int("]` with `@Todo(generics) & Unknown & str | @Todo(generics) & Any & str | @Todo(generics) & str | @Todo(generics) & Unknown & None | @Todo(generics) & Any & None | @Todo(generics) & None`
- Found 702 diagnostics
+ Found 701 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:323:54: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:334:54: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- Found 292 diagnostics
+ Found 290 diagnostics

black (https://github.com/psf/black)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/comments.py:95:28: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/comments.py:95:28: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/comments.py:95:28: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:135:41: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:135:41: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:135:41: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:135:41: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:141:42: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:141:42: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:141:42: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:149:34: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:149:34: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:149:34: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:156:41: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:156:41: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:156:41: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:156:41: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:170:21: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:175:22: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:176:45: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:176:45: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:176:45: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:176:45: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:176:45: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:186:30: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:202:23: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:207:20: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:207:20: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:207:20: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:224:21: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:231:23: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:236:21: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:564:17: Attribute `update` on type `@Todo(Support for `typing.GenericAlias` instances in type expressions) | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/driver.py:291:13: Argument to this function is incorrect: Expected `bytes`, found `bytes | None`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/pgen.py:187:16: Return type does not match returned value: Expected `tuple[@Todo(generics), str]`, found `tuple[dict, None | Unknown]`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:203:29: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:210:30: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:267:41: Attribute `parent` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:267:54: Attribute `parent` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:277:8: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:282:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:287:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:304:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:311:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:323:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:336:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:340:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:348:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:355:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:358:20: Attribute `parent` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:359:16: Attribute `parent` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:370:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:375:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:380:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:402:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:414:10: Attribute `type` on type `Node | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:417:10: Attribute `type` on type `Node | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:672:17: Argument to this function is incorrect: Expected `Path`, found `@Todo(generics) | None`
- Found 228 diagnostics
+ Found 170 diagnostics

psycopg (https://github.com/psycopg/psycopg)
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/fix_crdb.py:38:31: Name `spec` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/fix_crdb.py:44:36: Name `reason` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/fix_faker.py:409:25: Name `s` used when possibly not defined
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/psycopg/tests/test_concurrency.py:346:5: Type `None` has no attribute `send_signal`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/psycopg/tests/test_concurrency.py:347:5: Type `None` has no attribute `wait`
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/test_concurrency.py:350:78: Name `pid` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/test_cursor_server_async.py:375:16: Name `getter` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/test_cursor_server_async.py:376:25: Name `getter` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/test_cursor_server_async.py:377:25: Name `getter` used when possibly not defined
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/pq/pq_ctypes.py:794:16: Return type does not match returned value: Expected `bytes`, found `bytes | None`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/psycopg/tests/test_concurrency_async.py:260:5: Type `None` has no attribute `send_signal`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/psycopg/tests/test_concurrency_async.py:261:5: Type `None` has no attribute `wait`
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/test_concurrency_async.py:264:78: Name `pid` used when possibly not defined
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/psycopg/tests/pool/test_pool.py:557:16: Operator `-` is unsupported between objects of type `None` and `int | float`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/psycopg/tests/pool/test_pool_async.py:560:16: Operator `-` is unsupported between objects of type `None` and `int | float`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/psycopg/psycopg/psycopg/pq/_pq_ctypes.py:751:59: Function can implicitly return `None`, which is not assignable to return type `str`
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/test_cursor_server.py:369:16: Name `getter` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/test_cursor_server.py:370:25: Name `getter` used when possibly not defined
- warning[lint:possibly-unresolved-reference] /tmp/mypy_primer/projects/psycopg/tests/test_cursor_server.py:371:25: Name `getter` used when possibly not defined
- Found 1626 diagnostics
+ Found 1607 diagnostics

rich (https://github.com/Textualize/rich)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_logging.py:122:25: Type `None` has no attribute `msg`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/traceback.py:760:34: Argument to this function is incorrect: Expected `Sized`, found `None | range`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1921:20: Attribute `f_code` on type `FrameType | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1921:46: Attribute `f_lineno` on type `FrameType | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1921:62: Attribute `f_locals` on type `FrameType | None` is possibly unbound
- Found 568 diagnostics
+ Found 563 diagnostics

scrapy (https://github.com/scrapy/scrapy)
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/scrapy/scrapy/pipelines/images.py:171:16: Return type does not match returned value: Expected `str`, found `None | str`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:292:16: Attribute `dont_filter` on type `Request | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:294:16: Attribute `meta` on type `Request | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:295:16: Attribute `priority` on type `Request | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:342:16: Attribute `dont_filter` on type `Request | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:344:16: Attribute `meta` on type `Request | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:345:16: Attribute `priority` on type `Request | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:372:20: Attribute `dont_filter` on type `Request | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:374:20: Attribute `meta` on type `Request | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:375:20: Attribute `priority` on type `Request | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/tests/test_downloadermiddleware_retry.py:391:17: Argument to this function is incorrect: Expected `Request`, found `Request | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/debug.py:79:25: Attribute `f_back` on type `FrameType | None` is possibly unbound
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:339:32: No overload of class `str` matches arguments
- error[lint:no-matching-overload] /tmp/mypy_primer/projects/scrapy/tests/test_http2_client_protocol.py:575:32: No overload of class `str` matches arguments
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/contracts/__init__.py:40:20: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | @Todo(Inference of subscript on special form) | None`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/scrapy/scrapy/contracts/__init__.py:52:29: Object of type `None` is not callable
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/contracts/__init__.py:66:20: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | @Todo(Inference of subscript on special form) | None`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/scrapy/scrapy/contracts/__init__.py:68:29: Object of type `None` is not callable
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/contracts/__init__.py:180:16: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | @Todo(Inference of subscript on special form) | None`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/scrapy/scrapy/contracts/__init__.py:183:26: Object of type `None` is not callable
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/scrapy/scrapy/utils/iterators.py:62:24: Method `__getitem__` of type `tuple[int, int] | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/httpcache.py:80:41: Argument to this function is incorrect: Expected `bytes`, found `bytes | None | @Todo(instance attribute on class with dynamic base)`
- Found 1609 diagnostics
+ Found 1587 diagnostics

```
</details>


---

_Marked ready for review by @carljm on 2025-04-18 13:47_

---

_Review requested from @AlexWaygood by @carljm on 2025-04-18 13:47_

---

_Review requested from @sharkdp by @carljm on 2025-04-18 13:47_

---

_Review requested from @dcreager by @carljm on 2025-04-18 13:47_

---

_Comment by @carljm on 2025-04-18 15:11_

This was already reviewed once, so going ahead and landing it.

---

_Merged by @carljm on 2025-04-18 15:11_

---

_Closed by @carljm on 2025-04-18 15:11_

---

_Branch deleted on 2025-04-18 15:11_

---
