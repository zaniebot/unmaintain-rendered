```yaml
number: 17017
title: "[red-knot] Decorators and properties"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/decorators
created_at: 2025-03-27T18:20:48Z
updated_at: 2025-05-07T15:20:10Z
url: https://github.com/astral-sh/ruff/pull/17017
synced_at: 2026-01-10T18:57:02Z
```

# [red-knot] Decorators and properties

---

_Pull request opened by @sharkdp on 2025-03-27 18:20_

## Summary

This PR adds support for decorators on functions. It also adds support for properties by adding special handling for `@property` and `@<name of property>.setter`/`.getter` decorators.

closes https://github.com/astral-sh/ruff/issues/16987

## Ecosystem results

- :heavy_check_mark: A lot of false positives are fixed by our new understanding of properties
- :red_circle: A bunch of new false positives (typically `possibly-unbound-attribute` or `invalid-argument-type`) occur because we currently do not perform type narrowing on attributes. And with the new understanding of properties, this becomes even more relevant. In many cases, the narrowing occurs through an assertion, so this is also something that we need to implement to get rid of these false positives.
- :red_circle: A few new false positives occur because we do not understand generics, and therefore some calls to custom setters fail.
- :red_circle: Similarly, some false positives occur because we do not understand protocols yet.
- :heavy_check_mark: Seems like a true positive to me. [The setter](https://github.com/pypa/packaging/blob/e624d8edfaa28865de7b5a7da8bd59fd410e5331/src/packaging/specifiers.py#L752-L754) only accepts `bools`, but `None` is assigned in [this line](https://github.com/pypa/packaging/blob/e624d8edfaa28865de7b5a7da8bd59fd410e5331/tests/test_specifiers.py#L688).
  ```
  + error[lint:invalid-assignment] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:688:9: Invalid assignment to data descriptor attribute `prereleases` on type `SpecifierSet` with custom `__set__` method
  ```
- :heavy_check_mark: This is arguable also a true positive. The setter [here](https://github.com/Textualize/rich/blob/0c6c75644f80530de219dae3e94f0aeb999f9b4c/rich/table.py#L359-L363) returns `Table`, but typeshed wants [setters to return `None`](https://github.com/python/typeshed/blob/bf8d2a99126dcb155692c0ba56c4866fcd618393/stdlib/builtins.pyi#L1298).
  ```
  + error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/table.py:359:5: Object of type `Literal[padding]` cannot be assigned to parameter 2 (`fset`) of bound method `setter`; expected type `(Any, Any, /) -> None`
  ```  

## Follow ups

- Fix the `@no_type_check` regression
- Implement class decorators

## Test Plan

New Markdown test suites for decorators and properties.


---

_Label `red-knot` added by @sharkdp on 2025-03-27 18:20_

---

_Comment by @github-actions[bot] on 2025-03-27 18:26_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/utils.py:84:2: Type `Literal[canonicalize_version]` has no attribute `register`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:76:6: Type `Literal[prereleases]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:272:6: Type `Literal[prereleases]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:752:6: Type `Literal[prereleases]` has no attribute `setter`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:554:13: Implicit shadowing of function `prereleases`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:558:13: Implicit shadowing of function `prereleases`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:670:9: Implicit shadowing of function `prereleases`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:679:9: Implicit shadowing of function `prereleases`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:688:9: Implicit shadowing of function `prereleases`; annotate to make it explicit if this is intentional
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:32:5: Type `Literal[_get_musl_version]` has no attribute `cache_clear`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tests/test_manylinux.py:30:5: Type `Literal[_get_glibc_version]` has no attribute `cache_clear`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:327:39: Object of type `tuple[@Todo(return type of decorated function), @Todo(generics)]` cannot be assigned to parameter 1 (`version`) of function `_version_nodot`; expected type `Sequence`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:327:39: Object of type `tuple[@Todo(Support for `typing.TypeVar` instances in type expressions), @Todo(generics)]` cannot be assigned to parameter 1 (`version`) of function `_version_nodot`; expected type `Sequence`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/packaging/tests/test_specifiers.py:688:9: Invalid assignment to data descriptor attribute `prereleases` on type `SpecifierSet` with custom `__set__` method
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:443:9: Type `Literal[_get_glibc_version]` has no attribute `cache_clear`
- Found 121 diagnostics
+ Found 110 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/context_manager.py:76:17: Attribute `f_code` on type `FrameType | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/context_manager.py:76:50: Attribute `f_lineno` on type `FrameType | None` is possibly unbound
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/processors.py:260:17: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["<string>"]` with `str | None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/processors.py:268:62: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["<frozen runpy>"]` with `str | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_pstats_renderer.py:125:43: Object of type `Session | None` cannot be assigned to parameter 2 (`session`) of bound method `render`; expected type `Session`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:158:18: Attribute `root_frame` on type `None | @Todo(Support for `typing.TypeVar` instances in type expressions)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:88:13: Attribute `root_frame` on type `Session | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:90:12: Attribute `function` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:91:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:109:13: Attribute `root_frame` on type `Session | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:112:12: Attribute `function` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:113:16: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:115:31: Attribute `children` on type `Frame | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:334:12: Attribute `duration` on type `Session | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler.py:347:12: Attribute `duration` on type `Session | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/jsonrenderer.py:61:67: Object of type `str | None` cannot be assigned to parameter 1; expected type `str`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:158:18: Attribute `root_frame` on type `None | @Todo(return type of decorated function)` is possibly unbound
- Found 270 diagnostics
+ Found 287 diagnostics

arrow (https://github.com/arrow-py/arrow)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/arrow/arrow/formatter.py:128:49: Attribute `utcoffset` on type `tzutc | @Todo(Support for `typing.TypeAlias`) | None` is possibly unbound
- Found 46 diagnostics
+ Found 47 diagnostics

pybind11 (https://github.com/pybind/pybind11)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/pybind11/setup_helpers.py:171:6: Type `Literal[cxx_std]` has no attribute `setter`
- Found 269 diagnostics
+ Found 268 diagnostics

typeshed-stats (https://github.com/AlexWaygood/typeshed-stats)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/typeshed-stats/tests/test__cli.py:91:20: Type `<bound method `name` of `OutputOption`>` has no attribute `lower`
- Found 34 diagnostics
+ Found 33 diagnostics

rich (https://github.com/Textualize/rich)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_syntax.py:328:16: Attribute `name` on type `Unknown | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_syntax.py:352:16: Attribute `name` on type `Unknown | None` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_inspect.py:55:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_inspect.py:97:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule_in_table.py:31:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule_in_table.py:53:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule_in_table.py:75:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/align.py:311:64: Object of type `<bound method `height` of `Console`>` cannot be assigned to parameter `height` of bound method `center`; expected type `int | None`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_columns.py:60:21: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_columns_align.py:31:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/__main__.py:221:5: Implicit shadowing of function `file`; annotate to make it explicit if this is intentional
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:48:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_pretty.py:57:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_styled.py:13:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_card.py:25:31: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_markdown_no_hyperlinks.py:88:31: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_rich_print.py:23:9: Implicit shadowing of function `file`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_rich_print.py:29:9: Implicit shadowing of function `file`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_rich_print.py:68:9: Implicit shadowing of function `file`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_rich_print.py:74:9: Implicit shadowing of function `file`; annotate to make it explicit if this is intentional
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/panel.py:118:32: Attribute `replace` on type `<bound method `plain` of `Text`> | Literal[plain] | Unknown` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/panel.py:134:35: Attribute `replace` on type `<bound method `plain` of `Text`> | Literal[plain] | Unknown` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/panel.py:177:45: Object of type `<bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`text`) of function `cell_len`; expected type `str`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_progress.py:242:21: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_progress.py:267:21: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_progress.py:295:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_progress.py:326:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_progress.py:378:31: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_progress.py:562:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/table.py:291:6: Type `Literal[expand]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/table.py:359:6: Type `Literal[padding]` has no attribute `setter`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/table.py:977:9: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/table.py:987:9: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/table.py:994:9: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/table.py:1001:9: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/progress_bar.py:220:9: Type `<bound method `file` of `Console`>` has no attribute `write`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/highlighter.py:128:18: Type `<bound method `spans` of `Text`>` has no attribute `append`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/highlighter.py:133:32: Object of type `<bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`obj`) of function `len`; expected type `Sized`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/rich/rich/highlighter.py:134:24: Cannot subscript object of type `<bound method `plain` of `Text`> | Literal[plain]` with no `__getitem__` method
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/render.py:23:31: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_prompt.py:19:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_prompt.py:37:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_prompt.py:53:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_prompt.py:68:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_prompt.py:83:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_prompt.py:98:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_prompt.py:111:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tools/profile_pretty.py:22:7: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/syntax.py:785:17: Object of type `<bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`obj`) of function `len`; expected type `Sized`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_log.py:35:29: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:38:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:44:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:50:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:57:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:66:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:75:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:84:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:92:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:104:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:118:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_align.py:125:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/rule.py:70:28: Attribute `replace` on type `Unknown | <bound method `plain` of `Text`> | Literal[plain]` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/rule.py:82:44: Object of type `Unknown | <bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`text`) of function `cell_len`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/rule.py:85:45: Object of type `<bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`text`) of function `cell_len`; expected type `str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/rule.py:85:68: Object of type `Unknown | <bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`text`) of function `cell_len`; expected type `str`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/rich/rich/rule.py:88:30: Operator `+` is unsupported between objects of type `<bound method `plain` of `Text`> | Literal[plain]` and `Literal[" "]`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/rule.py:102:9: Implicit shadowing of function `plain`; annotate to make it explicit if this is intentional
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/rule.py:102:41: Object of type `<bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`text`) of function `set_cell_size`; expected type `str`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/rule.py:108:9: Implicit shadowing of function `plain`; annotate to make it explicit if this is intentional
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/rule.py:108:41: Object of type `<bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`text`) of function `set_cell_size`; expected type `str`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_segment.py:190:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/palette.py:87:22: Type `<bound method `size` of `Console`>` has no attribute `height`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/_log_render.py:47:9: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_logging.py:55:14: Attribute `getvalue` on type `Unknown | <bound method `file` of `Console`>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_logging.py:86:14: Attribute `getvalue` on type `Unknown | <bound method `file` of `Console`>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_logging.py:146:20: Attribute `getvalue` on type `Unknown | <bound method `file` of `Console`>` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_logging.py:151:5: Object of type `StringIO` is not assignable to attribute `file` on type `Unknown | Console`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_logging.py:153:21: Attribute `getvalue` on type `Unknown | <bound method `file` of `Console`>` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_logging.py:158:5: Object of type `StringIO` is not assignable to attribute `file` on type `Unknown | Console`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_logging.py:160:20: Attribute `getvalue` on type `Unknown | <bound method `file` of `Console`>` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/examples/top_lite_simulator.py:83:42: Type `<bound method `size` of `Console`>` has no attribute `height`
- error[lint:not-iterable] /tmp/mypy_primer/projects/rich/tests/test_console.py:43:21: Object of type `<bound method `size` of `Console`>` is not iterable because it doesn't have an `__iter__` method or a `__getitem__` method
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:51:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:not-iterable] /tmp/mypy_primer/projects/rich/tests/test_console.py:133:12: Object of type `<bound method `size` of `Console`>` is not iterable because it doesn't have an `__iter__` method or a `__getitem__` method
- error[lint:not-iterable] /tmp/mypy_primer/projects/rich/tests/test_console.py:137:12: Object of type `<bound method `size` of `Console`>` is not iterable because it doesn't have an `__iter__` method or a `__getitem__` method
- error[lint:not-iterable] /tmp/mypy_primer/projects/rich/tests/test_console.py:179:16: Object of type `<bound method `size` of `Console`>` is not iterable because it doesn't have an `__iter__` method or a `__getitem__` method
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:192:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:198:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:204:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:210:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:216:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:231:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:240:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:249:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:259:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:284:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:297:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:304:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:311:9: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:319:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:329:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:336:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:343:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:374:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:388:9: Attribute `write` on type `Unknown | <bound method `file` of `Console`>` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:400:9: Attribute `write` on type `Unknown | <bound method `file` of `Console`>` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:421:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:427:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:433:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:439:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:451:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:463:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:475:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:488:9: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:595:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:601:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:695:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:705:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:713:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_console.py:856:5: Implicit shadowing of function `size`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_console.py:858:5: Implicit shadowing of function `width`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_console.py:860:5: Implicit shadowing of function `height`; annotate to make it explicit if this is intentional
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:880:5: Unresolved attribute `isatty` on type `<bound method `file` of `Console`>`.
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:995:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:1077:12: Type `<bound method `file` of `Console`>` has no attribute `called_isatty`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:1084:16: Type `<bound method `file` of `Console`>` has no attribute `called_isatty`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:1091:12: Type `<bound method `file` of `Console`>` has no attribute `called_isatty`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:1098:12: Type `<bound method `file` of `Console`>` has no attribute `called_isatty`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:1105:12: Type `<bound method `file` of `Console`>` has no attribute `called_isatty`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:1112:16: Type `<bound method `file` of `Console`>` has no attribute `called_isatty`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:30:34: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:99:22: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:109:22: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:120:22: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:134:22: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:149:22: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:176:22: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:198:22: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:208:22: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:246:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_traceback.py:280:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_panel.py:35:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_panel.py:121:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_protocol.py:18:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_protocol.py:33:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_protocol.py:40:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_protocol.py:64:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_protocol.py:84:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_markdown.py:94:31: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/layout.py:401:73: Object of type `int & ~AlwaysFalsy | <bound method `width` of `Console`>` cannot be assigned to parameter 2 (`width`) of bound method `update_dimensions`; expected type `int`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/layout.py:401:80: Object of type `int & ~AlwaysFalsy | <bound method `height` of `Console`>` cannot be assigned to parameter 3 (`height`) of bound method `update_dimensions`; expected type `int`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/progress_bar.py:94:13: Attribute `get_truecolor` on type `Color | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/progress_bar.py:99:13: Attribute `get_truecolor` on type `Color | None` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule.py:27:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule.py:44:14: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule.py:61:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule.py:75:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule.py:81:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule.py:88:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule.py:102:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_rule.py:120:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/table.py:359:5: Object of type `Literal[padding]` cannot be assigned to parameter 2 (`fset`) of bound method `setter`; expected type `(Any, Any, /) -> None`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/text.py:409:6: Type `Literal[plain]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/text.py:425:6: Type `Literal[spans]` has no attribute `setter`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_table.py:34:5: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_table.py:40:5: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_table.py:65:5: Implicit shadowing of function `padding`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_table.py:70:5: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_table.py:72:5: Implicit shadowing of function `expand`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_table.py:81:5: Implicit shadowing of function `padding`; annotate to make it explicit if this is intentional
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_table.py:86:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:769:6: Type `Literal[file]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:784:6: Type `Literal[_buffer_index]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1046:6: Type `Literal[size]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1066:6: Type `Literal[width]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1084:6: Type `Literal[height]` has no attribute `setter`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_text.py:86:5: Implicit shadowing of function `plain`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/tests/test_text.py:90:5: Implicit shadowing of function `plain`; annotate to make it explicit if this is intentional
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_text.py:375:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_text.py:637:16: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_text.py:639:9: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_text.py:703:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_text.py:820:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_text.py:827:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/tests/test_text.py:834:12: Type `<bound method `file` of `Console`>` has no attribute `getvalue`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1921:20: Attribute `f_code` on type `FrameType | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1921:46: Attribute `f_lineno` on type `FrameType | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:1921:62: Attribute `f_locals` on type `FrameType | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:2358:44: Attribute `is_default` on type `Color | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:2359:22: Attribute `get_truecolor` on type `Color | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:2363:46: Attribute `is_default` on type `Color | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:2364:22: Attribute `get_truecolor` on type `Color | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:2466:30: Attribute `get_truecolor` on type `@Todo(map_with_boundness: intersections with negative contributions) | Color | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/rich/console.py:2474:30: Attribute `get_truecolor` on type `@Todo(map_with_boundness: intersections with negative contributions) | Color | None` is possibly unbound
- Found 577 diagnostics
+ Found 411 diagnostics

black (https://github.com/psf/black)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/ranges.py:244:21: Attribute `type` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/comments.py:390:17: Attribute `type` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/ranges.py:330:5: Object of type `Literal[""]` is not assignable to attribute `prefix` on type `Leaf & ~AlwaysFalsy`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/ranges.py:356:5: Object of type `Literal[""]` is not assignable to attribute `prefix` on type `Leaf & ~AlwaysFalsy`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/ranges.py:361:9: Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/black/src/black/comments.py:324:23: Cannot subscript object of type `<bound method `prefix` of `Leaf`>` with no `__getitem__` method
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/comments.py:379:9: Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/black/src/black/lines.py:79:13: Operator `+=` is unsupported between objects of type `<bound method `prefix` of `Leaf`>` and `str`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/lines.py:386:13: Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/lines.py:394:13: Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/lines.py:410:17: Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/linegen.py:176:13: Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/linegen.py:177:13: Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/black/src/black/linegen.py:317:17: Implicit shadowing of function `prefix`; annotate to make it explicit if this is intentional
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/brackets.py:282:16: Attribute `value` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:15:39: Operator `in` is not supported for types `str` and `<bound method `headers` of `Request`>`, in comparing `Literal["Access-Control-Request-Method"]` with `<bound method `headers` of `Request`> | @Todo(instance attribute on class with dynamic base)`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:21:18: Attribute `get` on type `<bound method `headers` of `Request`> | @Todo(instance attribute on class with dynamic base)` is possibly unbound
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:35:12: Object of type `Literal[impl]` is not assignable to return type `Middleware`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:319:6: Type `Literal[prefix]` has no attribute `setter`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:470:6: Type `Literal[prefix]` has no attribute `setter`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/lines.py:365:24: Attribute `type` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/nodes.py:794:43: Object of type `<bound method `prefix` of `Node`> | Literal[prefix]` cannot be assigned to parameter 1 (`obj`) of function `len`; expected type `Sized`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:829:8: Type `<bound method `prefix` of `Node`> | Literal[prefix]` has no attribute `strip`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:917:13: Attribute `type` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/nodes.py:1021:38: Attribute `type` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/linegen.py:168:35: Attribute `type` on type `@Todo(Inference of subscript on special form) | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:511:1: Object of type `Literal[main]` cannot be assigned to parameter 1 (`f`) of function `pass_context`; expected type `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:557:13: Type `Literal[main]` has no attribute `get_usage`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:562:13: Type `Literal[main]` has no attribute `get_usage`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:568:13: Type `Literal[main]` has no attribute `get_usage`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:672:17: Object of type `@Todo(return type of decorated function) | None` cannot be assigned to parameter `root` of function `get_sources`; expected type `Path`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:672:17: Object of type `@Todo(return type of overloaded function) | None` cannot be assigned to parameter `root` of function `get_sources`; expected type `Path`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blackd/__init__.py:104:12: Attribute `get` on type `<bound method `headers` of `Request`> | @Todo(instance attribute on class with dynamic base)` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blackd/__init__.py:110:12: Attribute `get` on type `<bound method `headers` of `Request`> | @Todo(instance attribute on class with dynamic base)` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blackd/__init__.py:116:27: Attribute `read` on type `<bound method `content` of `Request`> | @Todo(instance attribute on class with dynamic base)` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blackd/__init__.py:145:26: Attribute `get` on type `<bound method `headers` of `Request`> | @Todo(instance attribute on class with dynamic base)` is possibly unbound
- Found 209 diagnostics
+ Found 191 diagnostics

```
</details>


---

_Renamed from "[red-knot] Decorators" to "[red-knot] Decorators and properties" by @sharkdp on 2025-03-28 21:37_

---

_@sharkdp reviewed on 2025-03-31 09:23_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/call/getattr_static.md`:62 on 2025-03-31 09:23_

In an example like
```py
import inspect

class C:
    @property
    def p(self):
        return 1

reveal_type(inspect.getattr_static(C(), "p"))
```
we would previously get `Literal[p]`, and now we get `property`, which is exactly what the runtime version of `getattr_static` does as well (return the property instance).

So this change looks fine to me, since `real` is a `@property` on class `int`. At runtime, `getattr_static(1, "real")` returns `<attribute 'real' of 'int' objects>` of type `getset_descriptor`; but I don't think we need to model that here. Revealing `property` seems closer to what we had before, in any case.

---

_@sharkdp reviewed on 2025-04-01 19:48_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no_type_check.md`:70 on 2025-04-01 19:48_

These are regressions, but after talking about it, we consider them fairly low priority. I will open a ticket to address them as soon as this PR is merged.

---

_@sharkdp reviewed on 2025-04-01 19:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1328 on 2025-04-01 19:56_

This is probably not ideal. Should I introduce a new diagnostic for this? Note: it's currently hard to see them, without sub-diagnostics. They only surface if you desugar the property descriptor method calls: `C.attr.__set__(C(), "wrong argument type")`.

---

_Marked ready for review by @sharkdp on 2025-04-01 20:02_

---

_Review requested from @carljm by @sharkdp on 2025-04-01 20:02_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-04-01 20:02_

---

_Review requested from @dcreager by @sharkdp on 2025-04-01 20:02_

---

_@sharkdp reviewed on 2025-04-01 20:04_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/signatures.rs`:1035 on 2025-04-01 20:04_

If someone thinks that we need a replacement for this test, please let me know. The distinction between internal and external signature might still make sense for things like `@classmethod` that we handle by just flipping a bit.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/decorators.md`:157 on 2025-04-02 00:27_

Wow, the GitHub Python syntax highlighter and I have something in common: neither of us knew that this was valid syntax.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/decorators.md`:161 on 2025-04-02 00:29_

Doesn't really matter, but is the missing feature here bidirectional type inference? I wouldn't have said so, I would have said it was inlining.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/decorators.md`:185 on 2025-04-02 00:30_

Any reason to to assert here what the type of `f` is?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/decorators.md`:217 on 2025-04-02 00:31_

```suggestion
Decorators need to be callable with a single argument. If they are not, we emit a diagnostic:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:1035 on 2025-04-02 00:34_

Nope, as the author of this test, I approve its deletion :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/type_ordering.rs`:263 on 2025-04-02 00:35_

Do we also need to ensure a consistent ordering between two `PropertyInstance` types?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/properties.md`:81 on 2025-04-02 00:41_

I guess "support" just entails emitting a diagnostic if you `del` a property on an instance that doesn't define a deleter?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1458 on 2025-04-02 00:54_

Is this true even if some setter argument or getter return type is not fully static?

Currently I think effectively so, since those don't affect subtyping or assignability of the property type? It's always treated as just an instance of `builtins.property`. But in future when we add `Protocol` support, that won't necessarily be the case anymore.

I think it would probably be best to only call it fully static if its internal types are also all fully static?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:2582 on 2025-04-02 01:00_

```suggestion
                // For `builtins.property.__get__`, we use the same signature. The return types are not
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1328 on 2025-04-02 02:14_

I wouldn't bother adding a new diagnostic kind for this. I think what you have here is fine for this PR. I suspect that in future with sub-diagnostics we will want to do something a bit more sophisticated here, where instead of just having an opaque string we can store the binding from the internal call.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:1331 on 2025-04-02 02:15_

I think we might want to avoid the "Internal error" language in user-facing messages, as I suspect users may interpret it as "there's a bug in red-knot". Maybe just "Call{} failed: {reason}"?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:1445 on 2025-04-02 02:19_

Is this needed? It looks like if we encounter `overload` above we don't push it onto `decorator_types_and_nodes`, so how would we then encounter it here?

---

_@carljm approved on 2025-04-02 02:20_

Excellent work as always, thank you!

---

_@sharkdp reviewed on 2025-04-02 06:51_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1458 on 2025-04-02 06:51_

Oh, interesting — I hadn't thought about non-fully-static types in the context of structural subtyping. Does a class with an attribute/method that has a non-fully static type somewhere in its signature automatically become non-fully static?

The logical conclusion of that would be that we would have to consider types like `int` non-fully static, because `int.__pow__` is annotated as returning `Any`.

---

_Merged by @sharkdp on 2025-04-02 07:27_

---

_Closed by @sharkdp on 2025-04-02 07:27_

---

_Branch deleted on 2025-04-02 07:27_

---

_@sharkdp reviewed on 2025-04-02 07:31_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/properties.md`:81 on 2025-04-02 07:31_

https://github.com/astral-sh/ruff/issues/17141

---

_@sharkdp reviewed on 2025-04-02 07:32_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:1458 on 2025-04-02 07:32_

I left things as they were for now, but happy to make adjustments once I understand the situation more clearly.

---

_@sharkdp reviewed on 2025-04-02 07:36_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/suppressions/no_type_check.md`:70 on 2025-04-02 07:36_

https://github.com/astral-sh/ruff/issues/17142

---

_@carljm reviewed on 2025-04-02 15:48_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1458 on 2025-04-02 15:48_

Oh, yeah, good points. This could get ugly... I think you made the right choice, we'll have to consider this more deeply in implementing protocols.

---
