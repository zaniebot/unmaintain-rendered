```yaml
number: 21123
title: "[ty] Infer type of `self` for decorated methods and properties"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: david/fix-1448
created_at: 2025-10-29T13:45:20Z
updated_at: 2025-10-29T21:33:43Z
url: https://github.com/astral-sh/ruff/pull/21123
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Infer type of `self` for decorated methods and properties

---

_Pull request opened by @sharkdp on 2025-10-29 13:45_

## Summary

Infer a type of unannotated `self` parameters in decorated methods / properties.

closes https://github.com/astral-sh/ty/issues/1448

## Test Plan

Existing tests, some new tests.


---

_Label `ty` added by @sharkdp on 2025-10-29 13:45_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-29 13:45_

---

_Comment by @github-actions[bot] on 2025-10-29 13:47_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-29 13:48_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
+ bidict/_base.py:200:9: error[invalid-assignment] Object of type `ReferenceType[Self@inverse]` is not assignable to attribute `_invweak` of type `ReferenceType[BidictBase[KT@BidictBase, VT@BidictBase]] | None`
- Found 25 diagnostics
+ Found 26 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
+ more_itertools/more.py:4049:16: warning[possibly-missing-attribute] Attribute `result` may be missing on object of type `Unknown | None`
- Found 23 diagnostics
+ Found 24 diagnostics

parso (https://github.com/davidhalter/parso)
+ parso/python/errors.py:415:18: warning[possibly-missing-attribute] Attribute `add_block` may be missing on object of type `Unknown | None | _Context`
+ parso/python/errors.py:416:24: warning[possibly-missing-attribute] Attribute `blocks` may be missing on object of type `Unknown | None | _Context`
+ parso/python/errors.py:431:28: warning[possibly-missing-attribute] Attribute `parent_context` may be missing on object of type `Unknown | None | _Context`
+ parso/python/errors.py:432:13: warning[possibly-missing-attribute] Attribute `close_child_context` may be missing on object of type `Unknown | None`
+ parso/python/errors.py:470:32: warning[possibly-missing-attribute] Attribute `add_context` may be missing on object of type `Unknown | None | _Context`
+ parso/python/errors.py:489:9: warning[possibly-missing-attribute] Attribute `finalize` may be missing on object of type `Unknown | None | _Context`
+ parso/python/pep8.py:258:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:259:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:263:17: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:270:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:271:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:276:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/pep8.py:277:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:290:22: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:362:17: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:363:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:385:37: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:414:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:415:20: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:418:35: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:419:54: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:431:16: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:433:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:436:41: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:460:49: warning[possibly-missing-attribute] Attribute `bracket_indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:462:49: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:464:29: warning[possibly-missing-attribute] Attribute `get_latest_suite_node` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:471:36: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:486:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:492:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:498:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:507:40: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:513:42: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:537:24: warning[possibly-missing-attribute] Attribute `type` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:538:41: warning[possibly-missing-attribute] Attribute `parent` may be missing on object of type `Unknown | None | IndentationNode`
- parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | IndentationNode | None`
+ parso/python/pep8.py:541:27: warning[possibly-missing-attribute] Attribute `indentation` may be missing on object of type `Unknown | None | IndentationNode`
+ parso/python/tree.py:856:30: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:971:16: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1105:24: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Unknown | NodeOrLeaf`
+ parso/python/tree.py:1161:17: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:1163:34: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:1163:61: error[invalid-argument-type] Argument to bound method `index` is incorrect: Expected `NodeOrLeaf`, found `Literal["*"]`
+ parso/python/tree.py:1170:34: warning[possibly-missing-attribute] Attribute `children` may be missing on object of type `BaseNode | None`
+ parso/python/tree.py:1170:61: error[invalid-argument-type] Argument to bound method `index` is incorrect: Expected `NodeOrLeaf`, found `Literal["/"]`
- Found 162 diagnostics
+ Found 183 diagnostics

nionutils (https://github.com/nion-software/nionutils)
+ nion/utils/ListModel.py:792:55: error[unresolved-attribute] Object of type `object` has no attribute `listen`
+ nion/utils/ListModel.py:793:53: error[unresolved-attribute] Object of type `object` has no attribute `listen`
- Found 10 diagnostics
+ Found 12 diagnostics

pip (https://github.com/pypa/pip)
+ src/pip/_internal/index/package_finder.py:693:32: error[invalid-argument-type] Argument to function `build_netloc` is incorrect: Expected `str`, found `Unknown | str | int | None`
+ src/pip/_internal/index/package_finder.py:693:32: error[invalid-argument-type] Argument to function `build_netloc` is incorrect: Expected `int | None`, found `Unknown | str | int | None`
+ src/pip/_internal/metadata/pkg_resources.py:179:25: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | EmptyProvider`
+ src/pip/_vendor/distlib/util.py:1386:17: warning[division-by-zero] Cannot divide object of type `float` by zero
+ src/pip/_vendor/requests/models.py:91:22: error[unresolved-attribute] Object of type `Self@path_url` has no attribute `url`
+ src/pip/_vendor/requests/models.py:935:41: error[invalid-argument-type] Argument to class `str` is incorrect: Expected `str`, found `Unknown | None`
+ src/pip/_vendor/rich/style.py:478:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
+ src/pip/_vendor/rich/style.py:653:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
+ src/pip/_vendor/rich/style.py:734:41: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- Found 573 diagnostics
+ Found 582 diagnostics

spack (https://github.com/spack/spack)
+ lib/spack/spack/config.py:612:9: error[invalid-assignment] Object of type `ConfigScope` is not assignable to `str | None`
+ lib/spack/spack/config.py:615:40: error[unresolved-attribute] Object of type `str | None` has no attribute `sections`
+ lib/spack/spack/config.py:615:59: error[unresolved-attribute] Object of type `str | None` has no attribute `sections`
+ lib/spack/spack/config.py:617:47: error[unresolved-attribute] Object of type `str | None` has no attribute `sections`
+ lib/spack/spack/config.py:620:9: error[unresolved-attribute] Object of type `str | None` has no attribute `sections`
+ lib/spack/spack/config.py:622:32: error[unresolved-attribute] Object of type `str | None` has no attribute `sections`
+ lib/spack/spack/config.py:624:9: error[unresolved-attribute] Object of type `str | None` has no attribute `_write_section`
+ lib/spack/spack/fetch_strategy.py:430:21: warning[possibly-missing-attribute] Attribute `save_filename` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:469:12: warning[possibly-missing-attribute] Attribute `save_filename` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:470:25: warning[possibly-missing-attribute] Attribute `save_filename` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:471:28: warning[possibly-missing-attribute] Attribute `save_filename` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:499:26: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:524:16: warning[possibly-missing-attribute] Attribute `archive_file` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:535:40: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:538:20: warning[possibly-missing-attribute] Attribute `expanded` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:539:24: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:540:33: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:555:12: warning[possibly-missing-attribute] Attribute `expanded` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:556:55: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:596:36: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:597:36: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:624:20: warning[possibly-missing-attribute] Attribute `save_filename` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:652:16: warning[possibly-missing-attribute] Attribute `save_filename` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:706:16: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:706:50: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:708:34: warning[possibly-missing-attribute] Attribute `srcdir` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:708:72: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:710:26: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:767:26: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:784:45: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:785:32: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:789:26: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:919:12: warning[possibly-missing-attribute] Attribute `expanded` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:920:42: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1142:26: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1236:12: warning[possibly-missing-attribute] Attribute `expanded` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1237:52: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1254:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `srcdir` on type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1255:36: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1273:26: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1329:12: warning[possibly-missing-attribute] Attribute `expanded` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1330:52: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1343:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `srcdir` on type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1344:36: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1366:26: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1425:44: error[unresolved-attribute] Object of type `Self@cachable` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1438:12: warning[possibly-missing-attribute] Attribute `expanded` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1439:52: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1443:12: error[unresolved-attribute] Object of type `Self@fetch` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1444:44: error[unresolved-attribute] Object of type `Self@fetch` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1452:12: error[unresolved-attribute] Object of type `Self@fetch` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1453:32: error[unresolved-attribute] Object of type `Self@fetch` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1460:13: error[invalid-assignment] Object of type `Unknown` is not assignable to attribute `srcdir` on type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1461:36: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1468:26: warning[possibly-missing-attribute] Attribute `path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1469:27: warning[possibly-missing-attribute] Attribute `source_path` may be missing on object of type `Unknown | None`
+ lib/spack/spack/fetch_strategy.py:1473:16: error[unresolved-attribute] Object of type `Self@reset` has no attribute `revision`
+ lib/spack/spack/fetch_strategy.py:1474:32: error[unresolved-attribute] Object of type `Self@reset` has no attribute `revision`
- lib/spack/spack/install_test.py:305:45: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/mirrors/mirror.py:114:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Literal[True] | Unknown | str`
+ lib/spack/spack/mirrors/mirror.py:120:16: error[invalid-return-type] Return type does not match returned value: expected `bool`, found `Unknown | str`
+ lib/spack/spack/package_base.py:830:21: error[unresolved-attribute] Object of type `Self@fullnames` has no attribute `__mro__`
+ lib/spack/spack/package_base.py:850:45: error[unresolved-attribute] Object of type `Self@name` has no attribute `__qualname__`
+ lib/spack/spack/package_base.py:2292:25: error[invalid-argument-type] Argument to bound method `append` is incorrect: Expected `str`, found `~AlwaysFalsy`
+ lib/spack/spack/spec.py:4535:16: warning[possibly-missing-attribute] Attribute `platform` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:4539:16: warning[possibly-missing-attribute] Attribute `os` may be missing on object of type `Unknown | None | ArchSpec`
+ lib/spack/spack/spec.py:4543:16: warning[possibly-missing-attribute] Attribute `target` may be missing on object of type `Unknown | None | ArchSpec`
- lib/spack/spack/vendor/jinja2/compiler.py:1779:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/runtime.py:489:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- lib/spack/spack/vendor/jinja2/runtime.py:650:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:71:39: error[unresolved-attribute] Object of type `Self@right` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:71:57: error[unresolved-attribute] Object of type `Self@right` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:78:39: error[unresolved-attribute] Object of type `Self@left` has no attribute `_left_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:78:56: error[unresolved-attribute] Object of type `Self@left` has no attribute `_right_list`
+ lib/spack/spack/vendor/pyrsistent/_pdeque.py:103:16: error[unresolved-attribute] Object of type `Self@maxlen` has no attribute `_maxlen`
+ lib/spack/spack/vendor/ruamel/yaml/composer.py:43:16: warning[possibly-missing-attribute] Attribute `_parser` may be missing on object of type `Unknown | None`
+ lib/spack/spack/vendor/ruamel/yaml/composer.py:51:16: warning[possibly-missing-attribute] Attribute `_resolver` may be missing on object of type `Unknown | None`
+ lib/spack/spack/vendor/ruamel/yaml/constructor.py:83:20: warning[possibly-missing-attribute] Attribute `_composer` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/constructor.py:86:48: warning[possibly-missing-attribute] Attribute `_composer` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/constructor.py:95:16: warning[possibly-missing-attribute] Attribute `_resolver` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/constructor.py:103:16: warning[possibly-missing-attribute] Attribute `_scanner` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/emitter.py:236:20: warning[possibly-missing-attribute] Attribute `_serializer` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/main.py:183:28: error[call-non-callable] Object of type `None` is not callable
+ lib/spack/spack/vendor/ruamel/yaml/main.py:192:29: error[call-non-callable] Object of type `None` is not callable
+ lib/spack/spack/vendor/ruamel/yaml/main.py:230:20: error[call-non-callable] Object of type `None` is not callable
+ lib/spack/spack/vendor/ruamel/yaml/main.py:287:17: error[call-non-callable] Object of type `None` is not callable
+ lib/spack/spack/vendor/ruamel/yaml/main.py:303:22: error[call-non-callable] Object of type `None` is not callable
+ lib/spack/spack/vendor/ruamel/yaml/representer.py:74:20: warning[possibly-missing-attribute] Attribute `_serializer` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/resolver.py:392:23: warning[possibly-missing-attribute] Attribute `_scanner` may be missing on object of type `Unknown | None`
+ lib/spack/spack/vendor/ruamel/yaml/resolver.py:398:31: warning[possibly-missing-attribute] Attribute `_serializer` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/scanner.py:159:40: warning[possibly-missing-attribute] Attribute `_reader` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/scanner.py:167:16: warning[possibly-missing-attribute] Attribute `processing_version` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/serializer.py:70:16: warning[possibly-missing-attribute] Attribute `_emitter` may be missing on object of type `(Unknown & ~<Protocol with members 'typ'>) | None`
+ lib/spack/spack/vendor/ruamel/yaml/serializer.py:77:16: warning[possibly-missing-attribute] Attribute `_resolver` may be missing on object of type `Unknown | None`
- Found 7771 diagnostics
+ Found 7857 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/compiler.py:1814:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/jinja2/runtime.py:445:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/jinja2/runtime.py:606:49: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 187 diagnostics
+ Found 184 diagnostics

isort (https://github.com/pycqa/isort)
+ isort/settings.py:642:9: error[invalid-assignment] Property `_known_patterns` defined in `Self@known_patterns` is read-only
+ isort/settings.py:666:9: error[invalid-assignment] Property `_section_comments` defined in `Self@section_comments` is read-only
+ isort/settings.py:674:9: error[invalid-assignment] Property `_section_comments_end` defined in `Self@section_comments_end` is read-only
+ isort/settings.py:682:9: error[invalid-assignment] Property `_skips` defined in `Self@skips` is read-only
+ isort/settings.py:690:9: error[invalid-assignment] Property `_skip_globs` defined in `Self@skip_globs` is read-only
+ isort/settings.py:699:13: error[invalid-assignment] Property `_sorting_function` defined in `Self@sorting_function` is read-only
+ isort/settings.py:701:13: error[invalid-assignment] Property `_sorting_function` defined in `Self@sorting_function` is read-only
+ isort/settings.py:707:21: error[invalid-assignment] Property `_sorting_function` defined in `Self@sorting_function` is read-only
- Found 35 diagnostics
+ Found 43 diagnostics

stone (https://github.com/dropbox/stone)
+ stone/ir/data_types.py:1439:35: error[not-iterable] Object of type `Unknown | None` may not be iterable
- Found 660 diagnostics
+ Found 661 diagnostics

werkzeug (https://github.com/pallets/werkzeug)
+ src/werkzeug/datastructures/auth.py:205:9: error[unresolved-attribute] Can not assign object of type `CallbackDict[Unknown, Unknown]` to attribute `_parameters` on type `Self@parameters` with custom `__setattr__` method.
- src/werkzeug/wrappers/response.py:380:33: warning[unused-ignore-comment] Unused blanket `type: ignore` directive

paasta (https://github.com/yelp/paasta)
+ paasta_tools/api/tweens/auth.py:90:17: error[invalid-argument-type] Argument to bound method `post` is incorrect: Expected `str | bytes`, found `Unknown | str | None`
- Found 978 diagnostics
+ Found 979 diagnostics

aiortc (https://github.com/aiortc/aiortc)
+ src/aiortc/rtcrtpreceiver.py:184:16: error[unsupported-operator] Operator `+` is unsupported between objects of type `Unknown | Literal[0]` and `int | None`
+ src/aiortc/rtcrtpreceiver.py:311:16: error[invalid-return-type] Return type does not match returned value: expected `MediaStreamTrack`, found `RemoteStreamTrack | None`
+ src/aiortc/rtcrtpsender.py:147:16: error[invalid-return-type] Return type does not match returned value: expected `MediaStreamTrack`, found `Unknown | MediaStreamTrack | None`
- Found 187 diagnostics
+ Found 190 diagnostics

scrapy (https://github.com/scrapy/scrapy)
+ scrapy/core/downloader/handlers/__init__.py:114:23: error[call-non-callable] Object of type `object` is not callable
- scrapy/http/cookies.py:77:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- scrapy/http/response/__init__.py:84:44: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- scrapy/http/response/__init__.py:94:39: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_closespider.py:29:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:39:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:58:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:76:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:86:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:88:36: warning[possibly-missing-attribute] Attribute `exception_cls` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:98:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_closespider.py:108:18: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_contracts.py:534:16: warning[possibly-missing-attribute] Attribute `visited` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:79:20: warning[possibly-missing-attribute] Attribute `urls_visited` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:127:16: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:128:16: warning[possibly-missing-attribute] Attribute `t2` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:129:16: warning[possibly-missing-attribute] Attribute `t2` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:129:36: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:135:16: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:136:16: warning[possibly-missing-attribute] Attribute `t2` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:137:16: warning[possibly-missing-attribute] Attribute `t2_err` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:138:16: warning[possibly-missing-attribute] Attribute `t2_err` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:138:40: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:143:16: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:144:16: warning[possibly-missing-attribute] Attribute `t2` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:145:16: warning[possibly-missing-attribute] Attribute `t2_err` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:146:16: warning[possibly-missing-attribute] Attribute `t2_err` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:146:40: warning[possibly-missing-attribute] Attribute `t1` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:243:16: warning[possibly-missing-attribute] Attribute `visited` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:252:16: warning[possibly-missing-attribute] Attribute `visited` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:323:31: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:324:34: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:326:39: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:329:39: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:332:39: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:335:39: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:343:42: error[invalid-argument-type] Argument to function `get_engine_status` is incorrect: Expected `ExecutionEngine`, found `Unknown | ExecutionEngine | None`
+ tests/test_crawl.py:351:43: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:359:45: error[invalid-argument-type] Argument to function `format_engine_status` is incorrect: Expected `ExecutionEngine`, found `Unknown | ExecutionEngine | None`
+ tests/test_crawl.py:374:43: warning[possibly-missing-attribute] Attribute `name` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:628:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:635:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:648:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:658:22: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:667:22: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:675:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:676:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:677:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:677:56: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:681:17: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:682:15: warning[possibly-missing-attribute] Attribute `full_response_length` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:689:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:690:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:691:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:692:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:693:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:695:34: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:697:17: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:698:15: warning[possibly-missing-attribute] Attribute `full_response_length` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:705:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:706:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:707:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:707:59: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:715:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:716:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:717:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:718:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:719:16: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_crawl.py:721:37: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_downloader_handlers.py:322:16: error[unresolved-attribute] Object of type `Response` has no attribute `encoding`
+ tests/test_downloader_handlers.py:330:16: error[unresolved-attribute] Object of type `Response` has no attribute `encoding`
+ tests/test_downloader_handlers.py:338:16: error[unresolved-attribute] Object of type `Response` has no attribute `encoding`
+ tests/test_downloader_handlers.py:350:16: error[unresolved-attribute] Object of type `Response` has no attribute `encoding`
- tests/test_downloader_handlers_http_base.py:720:51: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_downloader_handlers_http_base.py:732:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_downloader_handlers_http_base.py:734:55: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_downloaderslotssettings.py:69:17: warning[possibly-missing-attribute] Attribute `downloader` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_downloaderslotssettings.py:70:17: warning[possibly-missing-attribute] Attribute `times` may be missing on object of type `Unknown | Spider | None`
+ tests/test_engine_loop.py:48:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:48:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:48:17: warning[possibly-missing-attribute] Attribute `pause` may be missing on object of type `Unknown | BaseScheduler`
+ tests/test_engine_loop.py:49:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:49:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:55:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:55:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:55:17: warning[possibly-missing-attribute] Attribute `unpause` may be missing on object of type `Unknown | BaseScheduler`
+ tests/test_engine_loop.py:64:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:64:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:64:17: warning[possibly-missing-attribute] Attribute `pause` may be missing on object of type `Unknown | BaseScheduler`
+ tests/test_engine_loop.py:65:17: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:65:17: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:71:44: warning[possibly-missing-attribute] Attribute `_slot` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_engine_loop.py:71:44: warning[possibly-missing-attribute] Attribute `scheduler` may be missing on object of type `Unknown | _Slot | None`
+ tests/test_engine_loop.py:71:44: warning[possibly-missing-attribute] Attribute `unpause` may be missing on object of type `Unknown | BaseScheduler`
+ tests/test_feedexport.py:1679:20: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `Unknown | str | None | bytes | int` on object of type `dict[str, Any]`
+ tests/test_feedexport.py:1778:20: error[invalid-argument-type] Method `__getitem__` of type `bound method dict[str, Any].__getitem__(key: str, /) -> Any` cannot be called with key of type `Unknown | str | dict[Unknown | str, Unknown | bool]` on object of type `dict[str, Any]`
+ tests/test_pipeline_crawl.py:196:16: warning[possibly-missing-attribute] Attribute `get_value` may be missing on object of type `StatsCollector | None`
+ tests/test_proxy_connect.py:115:27: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_attribute_binding.py:80:20: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_attribute_binding.py:89:23: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_attribute_binding.py:106:19: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_attribute_binding.py:139:20: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_attribute_binding.py:172:20: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_attribute_binding.py:195:20: warning[possibly-missing-attribute] Attribute `meta` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_cb_kwargs.py:166:20: warning[possibly-missing-attribute] Attribute `checks` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_cb_kwargs.py:167:20: warning[possibly-missing-attribute] Attribute `checks` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_left.py:40:16: warning[possibly-missing-attribute] Attribute `caught_times` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_left.py:46:16: warning[possibly-missing-attribute] Attribute `caught_times` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_left.py:52:16: warning[possibly-missing-attribute] Attribute `caught_times` may be missing on object of type `Unknown | Spider | None`
+ tests/test_request_left.py:58:16: warning[possibly-missing-attribute] Attribute `caught_times` may be missing on object of type `Unknown | Spider | None`
+ tests/test_scheduler.py:119:9: warning[possibly-missing-attribute] Attribute `downloader` may be missing on object of type `Unknown | ExecutionEngine | None`
+ tests/test_scheduler.py:372:20: warning[possibly-missing-attribute] Attribute `get_value` may be missing on object of type `Unknown | StatsCollector | None`
+ tests/test_scheduler_base.py:137:29: warning[possibly-missing-attribute] Attribute `url` may be missing on object of type `Unknown | Request | None`
- tests/test_spidermiddleware_httperror.py:104:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/test_spidermiddleware_httperror.py:144:59: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tests/test_spidermiddleware_httperror.py:208:20: warning[possibly-missing-attribute] Attribute `skipped` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:209:16: warning[possibly-missing-attribute] Attribute `parsed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:210:16: warning[possibly-missing-attribute] Attribute `failed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:223:16: warning[possibly-missing-attribute] Attribute `parsed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:224:16: warning[possibly-missing-attribute] Attribute `skipped` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:225:16: warning[possibly-missing-attribute] Attribute `failed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:238:16: warning[possibly-missing-attribute] Attribute `parsed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:239:16: warning[possibly-missing-attribute] Attribute `failed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:250:16: warning[possibly-missing-attribute] Attribute `parsed` may be missing on object of type `Unknown | Spider | None`
+ tests/test_spidermiddleware_httperror.py:251:16: warning[possibly-missing-attribute] Attribute `failed` may be missing on object of type `Unknown | Spider | None`
- Found 1579 diagnostics
+ Found 1688 diagnostics

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/capture.py:296:22: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ src/_pytest/capture.py:200:26: error[unresolved-attribute] Object of type `object` has no attribute `replace`
+ src/_pytest/fixtures.py:1032:16: warning[possibly-missing-attribute] Attribute `value` may be missing on object of type `Scope | (@Todo & ~str) | (((str, Config, /) -> @Todo) & ~(() -> object) & ~str)`
- Found 483 diagnostics
+ Found 484 diagnostics

alerta (https://github.com/alerta/alerta)
+ alerta/models/history.py:105:34: error[unsupported-operator] Operator `+` is unsupported between objects of type `Literal["/alert/"]` and `Unknown | None`
- Found 524 diagnostics
+ Found 525 diagnostics

kopf (https://github.com/nolar/kopf)
+ kopf/_cogs/configs/configuration.py:331:13: error[invalid-assignment] Object of type `int` is not assignable to attribute `_max_workers` on type `Executor & <Protocol with members '_max_workers'>`
- Found 46 diagnostics
+ Found 47 diagnostics

httpx-caching (https://github.com/johtso/httpx-caching)
- httpx_caching/_async/_transport.py:105:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- httpx_caching/_sync/_transport.py:105:38: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 27 diagnostics
+ Found 25 diagnostics

ignite (https://github.com/pytorch/ignite)
- ignite/metrics/precision.py:151:63: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- ignite/metrics/precision.py:153:68: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 2157 diagnostics
+ Found 2155 diagnostics

rich (https://github.com/Textualize/rich)
+ rich/style.py:478:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
+ rich/style.py:653:37: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
+ rich/style.py:734:41: error[too-many-positional-arguments] Too many positional arguments to bound method `__new__`: expected 1, got 2
- Found 318 diagnostics
+ Found 321 diagnostics

sockeye (https://github.com/awslabs/sockeye)
+ sockeye/training.py:835:69: warning[possibly-missing-attribute] Attribute `checkpoint` may be missing on object of type `Unknown | None | TrainState`
+ sockeye/utils.py:168:20: warning[division-by-zero] Cannot divide object of type `float` by zero
- Found 405 diagnostics
+ Found 407 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
- github/GithubObject.py:465:52: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/GithubObject.py:472:61: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/PaginatedList.py:242:88: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/PaginatedList.py:265:92: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/PaginatedList.py:266:92: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/PaginatedList.py:268:35: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- github/RequiredPullRequestReviews.py:221:16: error[invalid-return-type] Return type does not match returned value: expected `list[Team]`, found `Unknown | None`
- github/RequiredPullRequestReviews.py:226:16: error[invalid-return-type] Return type does not match returned value: expected `list[NamedUser]`, found `Unknown | None`
- tests/Framework.py:439:47: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/Framework.py:442:69: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tests/Framework.py:447:97: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 350 diagnostics
+ Found 339 diagnostics

pydantic (https://github.com/pydantic/pydantic)
+ pydantic/_internal/_model_construction.py:346:52: error[unresolved-attribute] Object of type `Self@__pydantic_fields_complete__` has no attribute `__pydantic_fields__`
+ pydantic/networks.py:771:16: error[unresolved-attribute] Object of type `MultiHostUrl` has no attribute `host`
+ pydantic/networks.py:794:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | None`
+ pydantic/networks.py:827:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | None`
+ pydantic/networks.py:959:16: error[invalid-return-type] Return type does not match returned value: expected `str`, found `str | None`
- Found 772 diagnostics
+ Found 777 diagnostics

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/engine/errors.py:129:41: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 290 diagnostics
+ Found 289 diagnostics

tornado (https://github.com/tornadoweb/tornado)
- tornado/httpclient.py:558:31: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- tornado/test/asyncio_test.py:42:43: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ tornado/test/gen_test.py:583:13: error[unresolved-attribute] Unresolved attribute `local_ref` on type `Self@test_coroutine_refcounting`.
+ tornado/test/gen_test.py:600:27: error[unresolved-attribute] Object of type `Self@test_coroutine_refcounting` has no attribute `local_ref`
+ tornado/test/http1connection_test.py:25:13: error[unresolved-attribute] Unresolved attribute `server_stream` on type `Self@asyncSetUp`.
+ tornado/test/http1connection_test.py:26:29: error[unresolved-attribute] Object of type `Self@asyncSetUp` has no attribute `server_stream`
+ tornado/test/http1connection_test.py:41:9: error[unresolved-attribute] Object of type `Self@test_http10_no_content_length` has no attribute `server_stream`
+ tornado/test/http1connection_test.py:42:9: error[unresolved-attribute] Object of type `Self@test_http10_no_content_length` has no attribute `server_stream`
+ tornado/test/netutil_test.py:35:26: warning[possibly-missing-attribute] Attribute `resolve` may be missing on object of type `Unknown | None`
+ tornado/test/netutil_test.py:55:19: warning[possibly-missing-attribute] Attribute `resolve` may be missing on object of type `Unknown | None`
+ tornado/test/netutil_test.py:100:24: warning[possibly-missing-attribute] Attribute `resolve` may be missing on object of type `Unknown | None | OverrideResolver`
+ tornado/test/netutil_test.py:103:24: warning[possibly-missing-attribute] Attribute `resolve` may be missing on object of type `Unknown | None | OverrideResolver`
+ tornado/websocket.py:1623:16: warning[possibly-missing-attribute] Attribute `selected_subprotocol` may be missing on object of type `Unknown | None | WebSocketProtocol`
- Found 343 diagnostics
+ Found 352 diagnostics

dragonchain (https://github.com/dragonchain/dragonchain)
+ dragonchain/job_processor/contract_job_utest.py:136:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dragonchain/job_processor/contract_job_utest.py:137:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dragonchain/job_processor/contract_job_utest.py:146:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dragonchain/job_processor/contract_job_utest.py:147:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dragonchain/job_processor/contract_job_utest.py:148:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dragonchain/job_processor/contract_job_utest.py:149:26: error[non-subscriptable] Cannot subscript object of type `None` with no `__getitem__` method
+ dragonchain/job_processor/contract_job_utest.py:263:13: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `set_state` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:264:13: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `save` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:269:13: warning[possibly-missing-attribute] Attribute `assert_called_once` may be missing on object of type `Unknown | (bound method SmartContractModel.set_state(state: str | ContractState, msg: str = Literal[""]) -> None) | MagicMock`
+ dragonchain/job_processor/contract_job_utest.py:274:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `docker_login_if_necessary` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:280:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `pull_image` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:281:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `create_dockerfile` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:307:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `set_state` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:308:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `save` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:309:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `send_report` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:330:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `set_state` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:331:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `save` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:342:9: error[invalid-assignment] Object of type `Literal["update"]` is not assignable to attribute `task_type` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:355:9: error[invalid-assignment] Object of type `Literal["update"]` is not assignable to attribute `task_type` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:356:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `set_state` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:357:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `save` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:375:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `set_state` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:376:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `save` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:390:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `contract_service` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:416:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `set_state` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:417:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `save` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:431:9: warning[possibly-missing-attribute] Attribute `update` may be missing on object of type `Unknown | None`
+ dragonchain/job_processor/contract_job_utest.py:453:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `schedule_contract` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:454:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `build_contract_image` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:455:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `deploy_to_openfaas` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:456:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `create_openfaas_secrets` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:458:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `save` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:459:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `populate_api_keys` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:465:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `deploy_to_openfaas` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:466:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `schedule_contract` on type `Unknown | ContractJob`
+ dragonchain/job_processor/contract_job_utest.py:468:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `save` on type `Unknown | SmartContractModel`
+ dragonchain/job_processor/contract_job_utest.py:469:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `create_openfaas_secrets` on type `Unknown | ContractJob`
+ dragonchain/lib/dto/bnb_utest.py:109:9: error[invalid-assignment] Object of type `MagicMock` is not assignable to attribute `_fetch_account` on type `Unknown | BinanceNetwork`
- Found 405 diagnostics
+ Found 443 diagnostics

optuna (https://github.com/optuna/optuna)
+ optuna/importance/_fanova/_tree.py:45:16: error[invalid-return-type] Return type does not match returned value: expected `int | float`, found `(Unknown & ~None) | floating[Any]`
- Found 580 diagnostics
+ Found 581 diagnostics

pandera (https://github.com/pandera-dev/pandera)
- pandera/api/dataframe/container.py:195:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/api/function_dispatch.py:32:22: error[unresolved-attribute] Object of type `(...) -> Unknown` has no attribute `__code__`
- pandera/api/pandas/array.py:38:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/api/pyspark/column_schema.py:80:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- pandera/api/pyspark/container.py:255:29: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
+ pandera/backends/pandas/hypotheses.py:96:20: warning[possibly-missing-attribute] Attribute `samples` may be missing on object of type `Unknown | Check`
- Found 1631 diagnostics
+ Found 1629 diagnostics

mitmproxy (https://github.com/mitmproxy/mitmproxy)
+ docs/scripts/clirecording/clidirector.py:186:22: error[unsupported-operator] Operator `-` is unsupported between objects of type `int | float` and `Unknown | None | int | float`
+ mitmproxy/tools/console/flowview.py:346:9: error[invalid-assignment] Object of type `Unknown | None` is not assignable to `Message`
+ test/mitmproxy/addons/test_tlsconfig.py:361:38: error[invalid-argument-type] Argument to bound method `do_handshake` is incorrect: Expected `SSLTest | Connection`, found `Connection | None`
+ test/mitmproxy/addons/test_tlsconfig.py:506:38: error[invalid-argument-type] Argument to bound method `do_handshake` is incorrect: Expected `SSLTest | Connection`, found `Connection | None`
- Found 1886 diagnostics
+ Found 1890 diagnostics

cki-lib (https://gitlab.com/cki-project/cki-lib)
+ tests/test_cronjob.py:80:9: error[invalid-assignment] Object of type `Mock` is not assignable to attribute `run` on type `Unknown | CronJob`
+ tests/test_cronjob.py:83:9: warning[possibly-missing-attribute] Attribute `join` may be missing on object of type `Unknown | Thread | None`
- Found 185 diagnostics
+ Found 187 diagnostics

antidote (https://github.com/Finistere/antidote)
- src/antidote/core/_catalog.py:392:54: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 307 diagnostics
+ Found 306 diagnostics

urllib3 (https://github.com/urllib3/urllib3)
- src/urllib3/response.py:728:85: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- src/urll...*[Comment body truncated]*

---

_Comment by @github-actions[bot] on 2025-10-29 13:52_

<!-- generated-comment ty ecosystem-analyzer -->

## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `unresolved-attribute` | 10,151 | 0 | 0 |
| `possibly-missing-attribute` | 607 | 0 | 23 |
| `invalid-argument-type` | 245 | 0 | 2 |
| `unused-ignore-comment` | 0 | 222 | 0 |
| `invalid-return-type` | 134 | 3 | 2 |
| `invalid-assignment` | 106 | 0 | 0 |
| `non-subscriptable` | 103 | 0 | 0 |
| `unsupported-operator` | 53 | 0 | 2 |
| `call-non-callable` | 51 | 0 | 0 |
| `possibly-unresolved-reference` | 0 | 43 | 0 |
| `not-iterable` | 24 | 0 | 0 |
| `no-matching-overload` | 13 | 0 | 0 |
| `too-many-positional-arguments` | 6 | 0 | 0 |
| `division-by-zero` | 4 | 0 | 0 |
| `unknown-argument` | 3 | 0 | 0 |
| `invalid-context-manager` | 2 | 0 | 0 |
| `invalid-await` | 1 | 0 | 0 |
| `missing-argument` | 1 | 0 | 0 |
| **Total** | **11,504** | **268** | **29** |

**[Full report with detailed diff](https://david-fix-1448.ecosystem-663.pages.dev/diff)** ([timing results](https://david-fix-1448.ecosystem-663.pages.dev/timing))


---

_Comment by @github-actions[bot] on 2025-10-29 15:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `ecosystem-analyzer` removed by @sharkdp on 2025-10-29 16:13_

---

_Label `ecosystem-analyzer` added by @sharkdp on 2025-10-29 16:13_

---

_Renamed from "[ty] Infer type of implicit `self` for properties" to "[ty] Infer type of implicit `self` for decorated methods and properties" by @sharkdp on 2025-10-29 16:29_

---

_Renamed from "[ty] Infer type of implicit `self` for decorated methods and properties" to "[ty] Infer type of `self` for decorated methods and properties" by @sharkdp on 2025-10-29 16:30_

---

_@sharkdp reviewed on 2025-10-29 16:32_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/snapshots/overloads.md_-_Overloads_-_Invalid_-_Inconsistent_decorat…_-_`@classmethod`_(aaa04d4cfa3adaba).snap`:128 on 2025-10-29 16:32_

Not sure if this is a regression or an improvement.

---

_Marked ready for review by @sharkdp on 2025-10-29 16:36_

---

_Review requested from @carljm by @sharkdp on 2025-10-29 16:36_

---

_Review requested from @AlexWaygood by @sharkdp on 2025-10-29 16:36_

---

_Review requested from @dcreager by @sharkdp on 2025-10-29 16:36_

---

_Review requested from @MichaReiser by @sharkdp on 2025-10-29 16:36_

---

_Review comment by @dcreager on `crates/ty_python_semantic/src/types/function.rs`:190 on 2025-10-29 19:44_

It always trips me up that `__new__` is a _static_ method even though its first parameter is named `cls`

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/overloads.md`:601 on 2025-10-29 19:48_

nit: Since there's a snapshot test, consider not asserting the text of the error message. That makes less churn on the snapshot and means you're not double-checking the error message.

---

_@dcreager approved on 2025-10-29 19:50_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/self.md`:184 on 2025-10-29 19:54_

we should actually reject this on Python <3.10, because instances of `staticmethod` are not callable on Python <3.10, and `some_decorator` states that it only accepts `Callable`s. Worth a TODO comment?

---

_@AlexWaygood approved on 2025-10-29 19:55_

---

_Merged by @sharkdp on 2025-10-29 21:22_

---

_Closed by @sharkdp on 2025-10-29 21:22_

---

_Branch deleted on 2025-10-29 21:22_

---
