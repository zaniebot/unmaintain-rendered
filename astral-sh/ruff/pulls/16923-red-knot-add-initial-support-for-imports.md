```yaml
number: 16923
title: "[red-knot] Add initial support for `*` imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/star-imports-redux
created_at: 2025-03-22T22:51:41Z
updated_at: 2025-05-07T15:20:59Z
url: https://github.com/astral-sh/ruff/pull/16923
synced_at: 2026-01-12T15:55:59Z
```

# [red-knot] Add initial support for `*` imports

---

_@AlexWaygood_

## Summary

This PR adds initial support for `*` imports to red-knot. The approach is to implement a standalone query, called from semantic indexing, that visits the module referenced by the `*` import and collects all global-scope public names that will be imported by the `*` import. The `SemanticIndexBuilder` then adds separate definitions for each of these names, all keyed to the same `ast::Alias` node that represents the `*` import.

There are many pieces of `*`-import semantics that are still yet to be done, even with this PR:
- This PR does not attempt to implement any of the semantics to do with `__all__`. (If a module defines `__all__`, then only the symbols included in `__all__` are imported, _not_ all public global-scope symbols.
- With the logic implemented in this PR as it currently stands, we sometimes incorrectly consider a symbol bound even though it is defined in a branch that is statically known to be dead code, e.g. (assuming the target Python version is set to 3.11):

  ```py
  # a.py

  import sys

  if sys.version_info < (3, 10):
      class Foo: ...

  ```

  ```py
  # b.py

  from a import *

  print(Foo)  # this is unbound at runtime on 3.11,
              # but we currently consider it bound with the logic in this PR
  ```

Implementing these features is important, but is for now deferred to followup PRs.

Many thanks to @ntBre, who contributed to this PR in a pairing session on Friday!

## Test Plan

Assertions in existing mdtests are adjusted, and several new ones are added.

~~TODO: the `collections.abc` integration test at the bottom of `star.md` is still failing. Not sure why...~~ (I figured out the bug here and fixed it!)

---

_Label `red-knot` added by @AlexWaygood on 2025-03-22 22:51_

---

_Comment by @github-actions[bot] on 2025-03-22 23:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_elffile.py:92:16: Type `<module 'struct'>` has no attribute `unpack`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/_elffile.py:92:48: Type `<module 'struct'>` has no attribute `calcsize`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tasks/paths.py:7:11: Type `<module 'os.path'>` has no attribute `abspath`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tasks/paths.py:7:27: Type `<module 'os.path'>` has no attribute `dirname`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tasks/paths.py:7:43: Type `<module 'os.path'>` has no attribute `dirname`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tasks/paths.py:9:9: Type `<module 'os.path'>` has no attribute `join`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tests/test_elffile.py:97:19: Type `<module 'struct'>` has no attribute `unpack`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tests/test_elffile.py:98:28: Type `<module 'struct'>` has no attribute `pack`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tasks/check.py:28:18: Type `<module 'os.path'>` has no attribute `join`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tasks/check.py:51:21: Type `<module 'os.path'>` has no attribute `dirname`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:39:23: Type `<module 'struct'>` has no attribute `calcsize`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:811:44: Type `<module 'collections.abc'>` has no attribute `Iterator`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:1075:44: Type `<module 'collections.abc'>` has no attribute `Iterator`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:1278:44: Type `<module 'collections.abc'>` has no attribute `Iterator`
- Found 137 diagnostics
+ Found 123 diagnostics

zipp (https://github.com/jaraco/zipp)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/zipp/zipp/__init__.py:419:16: Type `<module 'stat'>` has no attribute `S_ISLNK`
- Found 10 diagnostics
+ Found 9 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/low_level/test_context.py:48:19: Object of type `Literal[busy_wait]` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/low_level/test_context.py:49:19: Object of type `Literal[busy_wait]` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:158:18: Attribute `root_frame` on type `None | @Todo` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/fake_time_util.py:58:20: Type `<module 'asyncio'>` has no attribute `get_running_loop`
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pyinstrument/test/fake_time_util.py:63:26: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] /tmp/mypy_primer/projects/pyinstrument/test/fake_time_util.py:65:84: Unused blanket `type: ignore` directive
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/magic/magic.py:279:20: Type `<module 'asyncio'>` has no attribute `get_event_loop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/magic/magic.py:280:16: Type `<module 'asyncio'>` has no attribute `new_event_loop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/magic/magic.py:283:13: Type `<module 'asyncio'>` has no attribute `set_event_loop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/magic/magic.py:285:22: Type `<module 'asyncio'>` has no attribute `run_coroutine_threadsafe`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/magic/magic.py:289:13: Type `<module 'asyncio'>` has no attribute `set_event_loop`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/pyinstrument/examples/litestar_hello.py:3:21: Module `asyncio` has no member `sleep`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:107:9: Object of type `<bound method `subscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:110:9: Object of type `<bound method `subscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:125:19: Object of type `<bound method `unsubscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/test/test_stack_sampler.py:126:19: Object of type `<bound method `unsubscribe` of `StackSampler`>` cannot be assigned to parameter 2 (`callable`) of bound method `run`; expected type `(...) -> Unknown`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/util.py:75:18: Type `<module 'codecs'>` has no attribute `lookup`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/examples/async_example_simple.py:10:15: Type `<module 'asyncio'>` has no attribute `sleep`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/examples/async_example_simple.py:15:1: Type `<module 'asyncio'>` has no attribute `run`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:42:15: Type `<module 'asyncio'>` has no attribute `sleep`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:101:19: Type `<module 'asyncio'>` has no attribute `sleep`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:115:16: Type `<module 'asyncio'>` has no attribute `new_event_loop`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_profiler_async.py:158:18: Attribute `root_frame` on type `None | Unknown` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/examples/async_experiment_1.py:27:11: Type `<module 'asyncio'>` has no attribute `sleep`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pyinstrument/examples/async_experiment_1.py:37:8: Type `<module 'asyncio'>` has no attribute `get_event_loop`
- Found 287 diagnostics
+ Found 276 diagnostics

arrow (https://github.com/arrow-py/arrow)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/factory.py:232:26: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/factory.py:340:18: Type `<module 'dateutil.tz'>` has no attribute `tzlocal`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:160:22: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:195:22: Type `<module 'dateutil.tz'>` has no attribute `tzlocal`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:223:30: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:252:22: Type `<module 'dateutil.tz'>` has no attribute `tzlocal`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:296:13: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:320:26: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:347:22: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:917:16: Type `<module 'dateutil.tz'>` has no attribute `datetime_ambiguous`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:923:20: Type `<module 'dateutil.tz'>` has no attribute `datetime_exists`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1041:36: Type `<module 'dateutil.tz'>` has no attribute `datetime_exists`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1042:23: Type `<module 'dateutil.tz'>` has no attribute `resolve_imaginary`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1154:64: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/arrow.py:1795:20: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/formatter.py:124:18: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/parser.py:730:57: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/parser.py:737:20: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/parser.py:910:22: Type `<module 'dateutil.tz'>` has no attribute `tzlocal`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/parser.py:913:22: Type `<module 'dateutil.tz'>` has no attribute `tzutc`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/parser.py:928:26: Type `<module 'dateutil.tz'>` has no attribute `tzoffset`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/arrow/arrow/parser.py:931:26: Type `<module 'dateutil.tz'>` has no attribute `gettz`
- Found 74 diagnostics
+ Found 52 diagnostics

itsdangerous (https://github.com/pallets/itsdangerous)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/encoding.py:44:17: Type `<module 'struct'>` has no attribute `Struct`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:112:35: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:112:56: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:128:35: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:128:56: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:144:35: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:144:56: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:163:35: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:163:56: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:179:35: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:179:56: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:194:35: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:194:56: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/serializer.py:289:66: Type `<module 'collections.abc'>` has no attribute `Iterator`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/timed.py:179:10: Type `<module 'collections.abc'>` has no attribute `Iterator`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/timed.py:180:24: Type `<module 'collections.abc'>` has no attribute `Iterator`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/signer.py:68:31: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/signer.py:68:52: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/signer.py:131:35: Type `<module 'collections.abc'>` has no attribute `Iterable`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/itsdangerous/src/itsdangerous/signer.py:131:56: Type `<module 'collections.abc'>` has no attribute `Iterable`
- Found 55 diagnostics
+ Found 35 diagnostics

black (https://github.com/psf/black)
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:15:29: Module `collections.abc` has no member `Iterable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pytree.py:15:39: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/trans.py:8:29: Module `collections.abc` has no member `Callable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/trans.py:8:39: Module `collections.abc` has no member `Collection`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/trans.py:8:51: Module `collections.abc` has no member `Iterable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/trans.py:8:61: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/trans.py:8:71: Module `collections.abc` has no member `Sequence`
+ error[lint:non-subscriptable] /tmp/mypy_primer/projects/black/src/black/trans.py:49:31: Cannot subscript object of type `Literal[Collection]` with no `__class_getitem__` method
+ error[lint:non-subscriptable] /tmp/mypy_primer/projects/black/src/black/trans.py:49:59: Cannot subscript object of type `Literal[Iterator]` with no `__class_getitem__` method
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/black/trans.py:2482:12: Object of type `Literal[insert_str_child]` is not assignable to return type `(@Todo, /) -> None`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/black/src/black/trans.py:2509:12: Object of type `Literal[is_valid_index]` is not assignable to return type `(int, /) -> bool`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/lines.py:3:29: Module `collections.abc` has no member `Callable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/lines.py:3:39: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/lines.py:3:49: Module `collections.abc` has no member `Sequence`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/lines.py:684:56: Object of type `Literal[is_import]` cannot be assigned to parameter `first_leaf_matches` of bound method `is_fmt_pass_converted`; expected type `((Leaf, /) -> bool) | None`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/driver.py:24:29: Module `collections.abc` has no member `Iterable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/driver.py:24:39: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/tokenize.py:31:29: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/comments.py:2:29: Module `collections.abc` has no member `Collection`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/comments.py:2:41: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/files.py:4:29: Module `collections.abc` has no member `Iterable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/files.py:4:39: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/files.py:4:49: Module `collections.abc` has no member `Sequence`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/cache.py:8:29: Module `collections.abc` has no member `Iterable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/pgen.py:5:29: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/pgen.py:5:39: Module `collections.abc` has no member `Sequence`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/parse.py:12:29: Module `collections.abc` has no member `Callable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/parse.py:12:39: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/parsing.py:8:29: Module `collections.abc` has no member `Collection`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/parsing.py:8:41: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/__init__.py:9:5: Module `collections.abc` has no member `Collection`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/__init__.py:10:5: Module `collections.abc` has no member `Generator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/__init__.py:11:5: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/__init__.py:12:5: Module `collections.abc` has no member `MutableMapping`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/__init__.py:13:5: Module `collections.abc` has no member `Sequence`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/__init__.py:14:5: Module `collections.abc` has no member `Sized`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/debug.py:1:29: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/concurrency.py:13:29: Module `collections.abc` has no member `Iterable`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:1280:48: Object of type `<bound method `readline` of `BytesIO`> | @Todo` cannot be assigned to parameter 1 (`readline`) of function `detect_encoding`; expected type `() -> bytes | bytearray`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:1280:48: Object of type `<bound method `readline` of `BytesIO`> | @Todo` cannot be assigned to parameter 1 (`readline`) of function `detect_encoding`; expected type `() -> bytes | bytearray`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:1280:48: Object of type `<bound method `readline` of `BytesIO`> | @Todo` cannot be assigned to parameter 1 (`readline`) of function `detect_encoding`; expected type `() -> bytes | bytearray`
+ error[lint:non-subscriptable] /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:8:31: Cannot subscript object of type `Literal[Awaitable]` with no `__class_getitem__` method
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:42:29: Type `<module 'asyncio'>` has no attribute `Future`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:49:20: Type `<module 'asyncio'>` has no attribute `AbstractEventLoop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:53:39: Type `<module 'asyncio'>` has no attribute `all_tasks`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:59:33: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:99:12: Type `<module 'asyncio'>` has no attribute `new_event_loop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:100:5: Type `<module 'asyncio'>` has no attribute `set_event_loop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:117:13: Type `<module 'asyncio'>` has no attribute `set_event_loop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:128:11: Type `<module 'asyncio'>` has no attribute `AbstractEventLoop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:155:9: Type `<module 'asyncio'>` has no attribute `ensure_future`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:170:25: Type `<module 'asyncio'>` has no attribute `wait`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:170:25: Type `<module 'asyncio'>` has no attribute `wait`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:170:25: Type `<module 'asyncio'>` has no attribute `wait`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:170:59: Type `<module 'asyncio'>` has no attribute `FIRST_COMPLETED`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:170:59: Type `<module 'asyncio'>` has no attribute `FIRST_COMPLETED`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:170:59: Type `<module 'asyncio'>` has no attribute `FIRST_COMPLETED`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/black/concurrency.py:189:15: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:1:29: Module `collections.abc` has no member `Awaitable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:1:40: Module `collections.abc` has no member `Callable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/blackd/middlewares.py:1:50: Module `collections.abc` has no member `Iterable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/nodes.py:6:29: Module `collections.abc` has no member `Iterator`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/nodes.py:783:43: Object of type `<bound method `prefix` of `Node`> | Literal[prefix]` cannot be assigned to parameter 1 (`obj`) of function `len`; expected type `Sized`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/brackets.py:3:29: Module `collections.abc` has no member `Iterable`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/brackets.py:3:39: Module `collections.abc` has no member `Sequence`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/blackd/__init__.py:27:16: Type `<module 'asyncio'>` has no attribute `Event`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/blackd/__init__.py:127:16: Type `<module 'asyncio'>` has no attribute `get_event_loop`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/black/src/blackd/__init__.py:150:20: Type `<module 'asyncio'>` has no attribute `get_event_loop`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/ranges.py:4:29: Module `collections.abc` has no member `Collection`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/ranges.py:4:41: Module `collections.abc` has no member `Iterator`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/ranges.py:4:51: Module `collections.abc` has no member `Sequence`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/linegen.py:7:29: Module `collections.abc` has no member `Collection`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/black/src/black/linegen.py:7:41: Module `collections.abc` has no member `Iterator`
- Found 288 diagnostics
+ Found 235 diagnostics

rich (https://github.com/Textualize/rich)
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/highlighter.py:133:32: Object of type `<bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`obj`) of function `len`; expected type `Sized`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/syntax.py:391:26: Type `<module 'os.path'>` has no attribute `splitext`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/syntax.py:391:26: Type `<module 'os.path'>` has no attribute `splitext`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/rich/syntax.py:391:26: Type `<module 'os.path'>` has no attribute `splitext`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/syntax.py:785:17: Object of type `<bound method `plain` of `Text`> | Literal[plain]` cannot be assigned to parameter 1 (`obj`) of function `len`; expected type `Sized`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/pretty.py:226:9: Object of type `Literal[display_hook]` is not assignable to attribute `displayhook` of type `(object, /) -> Any`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/rich/rich/traceback.py:173:9: Object of type `Literal[excepthook]` is not assignable to attribute `excepthook` of type `(type[BaseException], BaseException, TracebackType | None, /) -> Any`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/rich/rich/traceback.py:688:34: Object of type `None | range` cannot be assigned to parameter 1 (`obj`) of function `len`; expected type `Sized`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/rich/examples/downloader.py:70:29: Type `<module 'os.path'>` has no attribute `join`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/rich/rich/scope.py:1:29: Module `collections.abc` has no member `Mapping`

pybind11 (https://github.com/pybind/pybind11)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:112:12: Type `<module 'struct'>` has no attribute `unpack_from`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:113:12: Type `<module 'struct'>` has no attribute `unpack_from`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:152:17: Type `<module 'struct'>` has no attribute `unpack`
- error[lint:unresolved-import] /tmp/mypy_primer/projects/pybind11/setup.py:13:29: Module `collections.abc` has no member `Generator`
- Found 276 diagnostics
+ Found 272 diagnostics

isort (https://github.com/pycqa/isort)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/settings.py:545:16: Type `<module 'stat'>` has no attribute `S_ISFIFO`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:97:29: Type `<module 'os.path'>` has no attribute `sep`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:100:44: Type `<module 'os.path'>` has no attribute `join`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:101:20: Type `<module 'os.path'>` has no attribute `isdir`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:101:34: Type `<module 'os.path'>` has no attribute `join`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:124:20: Type `<module 'os.path'>` has no attribute `abspath`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:131:32: Type `<module 'os.path'>` has no attribute `realpath`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:142:20: Type `<module 'os.path'>` has no attribute `isdir`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:148:30: Type `<module 'os.path'>` has no attribute `realpath`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:157:34: Type `<module 'os.path'>` has no attribute `normcase`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:178:66: Type `<module 'os.path'>` has no attribute `isdir`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:186:20: Type `<module 'os.path'>` has no attribute `normcase`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:194:20: Type `<module 'os.path'>` has no attribute `normcase`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:228:16: Type `<module 'os.path'>` has no attribute `dirname`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:229:16: Type `<module 'os.path'>` has no attribute `join`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:250:20: Type `<module 'os.path'>` has no attribute `dirname`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:254:16: Type `<module 'os.path'>` has no attribute `abspath`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:255:12: Type `<module 'os.path'>` has no attribute `isfile`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:256:20: Type `<module 'os.path'>` has no attribute `dirname`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:305:25: Type `<module 'os.path'>` has no attribute `join`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:308:16: Type `<module 'os.path'>` has no attribute `isdir`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:311:25: Type `<module 'os.path'>` has no attribute `join`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:318:16: Type `<module 'os.path'>` has no attribute `isfile`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:335:20: Type `<module 'os.path'>` has no attribute `dirname`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/main.py:1126:77: Object of type `Literal[_preconvert]` cannot be assigned to parameter `default` of function `dumps`; expected type `((Any, /) -> Any) | None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/isort/isort/main.py:1199:17: Object of type `partial` cannot be assigned to parameter 2 (`func`) of bound method `imap`; expected type `(Unknown | @Todo, /) -> Unknown | @Todo`
- Found 76 diagnostics
+ Found 54 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/model.py:303:40: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/model.py:303:40: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/model.py:303:40: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:69:13: Type `<module 'asyncio'>` has no attribute `Semaphore`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:89:22: Type `<module 'asyncio'>` has no attribute `BoundedSemaphore`
+ error[lint:missing-argument] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:104:26: No argument provided for required parameter `program` of function `create_subprocess_exec`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:101:26: Type `<module 'asyncio'>` has no attribute `create_subprocess_shell`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:104:26: Type `<module 'asyncio'>` has no attribute `create_subprocess_exec`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:30:32: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:30:32: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:30:32: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:46:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:46:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:46:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:46:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:46:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:59:38: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:59:38: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:59:38: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:73:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:73:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:73:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:73:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:73:52: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:172:30: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:205:21: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:241:16: Type `<module 'asyncio'>` has no attribute `as_completed`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:287:11: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:294:25: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:325:29: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:359:23: Type `<module 'asyncio'>` has no attribute `gather`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:411:23: Type `<module 'asyncio'>` has no attribute `as_completed`
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/main.py:459:23: Type `<module 'asyncio'>` has no attribute `run`
- Found 49 diagnostics
+ Found 18 diagnostics

```
</details>


---

_Comment by @AlexWaygood on 2025-03-22 23:54_

Still not sure why the `collections.abc` integration test is still failing, but other than that I'm pretty happy with this now!

---

_Marked ready for review by @AlexWaygood on 2025-03-22 23:54_

---

_Review requested from @carljm by @AlexWaygood on 2025-03-22 23:54_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-03-22 23:54_

---

_Review requested from @dcreager by @AlexWaygood on 2025-03-22 23:54_

---

_@AlexWaygood reviewed on 2025-03-22 23:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:663 on 2025-03-22 23:56_

It's sort-of unfortunate that we need to keep track of what index the alias is in the names list for `*` imports, since it's invalid syntax to do something like `from foo import *, X` -- if `*` is present in the names list of a `StmtImportFrom` node, it must be the _only_ name in the list. However, if we _don't_ keep track of the index the `*` is in the names list, we end up panicking on various kinds of invalid syntax.

---

_@MichaReiser reviewed on 2025-03-23 06:41_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:663 on 2025-03-23 06:41_

Could we just search for the first alias with a * name? Or we could not  create a star import definition if the syntax is invalid

---

_@AlexWaygood reviewed on 2025-03-23 09:48_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:663 on 2025-03-23 09:48_

Ohh of course, thanks! I'll make that change

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/definition.rs`:663 on 2025-03-23 10:36_

https://github.com/astral-sh/ruff/pull/16923/commits/a6f41525f2f0b9a175b6250a20c9319c72a40242

---

_@AlexWaygood reviewed on 2025-03-23 10:36_

---

_Comment by @AlexWaygood on 2025-03-23 11:49_

> Still not sure why the `collections.abc` integration test is still failing, but other than that I'm pretty happy with this now!

I figured out the bug here and fixed it. Unfortunately, fixing the bug completely broke all our special casing for the builtin `len()` function. This is because `len()` has a parameter annotated with the `collections.abc.Sized` protocol in typeshed's `builtins.pyi` stub, and with my PR we now half-understand the collections.abc.Sized type (enough to emit false-positive errors 🙃) instead of not understanding it at all.

There were a huge amount of test failures in `len.md` resulting from this, so for now I've added a `KnownClass::Sized` variant and some special-casing for `KnownClass::Sized` to `Type::is_assignable_to()`. This is very similar to the existing special-casing @sharkdp had to add for `typing.SupportsIndex` in an earlier PR.

---

_Comment by @github-actions[bot] on 2025-03-23 11:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:80 on 2025-03-23 20:31_

Don't we need to use the `asname` here instead if it's Some?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:376 on 2025-03-23 20:36_

I see that you added debug assertions where we read definitions to ensure that we just have 1 in the cases where we should. But I think practically if we introduce a bug that adds multiple definitions for the same node key, it will be easier to debug if an assertion is raised on insert rather than on read. Couldn't we add an `allow_multiple` flag argument to `add_definition`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:43 on 2025-03-23 20:41_

There's a related test case (which I don't think would yet pass with this PR, which is fine, but I don't want to lose track of the case), where the re-exports visitor (which, as we've discussed, has to be an over-approximation in some cases) identifies a name as a possible re-export, but in fact in type inference the name turns out to not exist, and it was previously defined in the importing module; we need to be sure that we see the prior definition of that name.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1012 on 2025-03-23 20:54_

Might be worth a `self.is_module_global_scope()` for this, just for readability.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1055 on 2025-03-23 20:59_

I had to remove this line and see which tests failed and why, to understand why it's needed (to ensure we at least have an empty `Definitions` inserted for star imports from empty modules or with invalid syntax. Maybe comment to clarify?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:84 on 2025-03-23 21:02_

How much of the code in this visitor could we remove, without changing behavior, if we looked for all `Name` nodes with `Store` context?

I know imports don't use `Name` nodes and would still need dedicated handling (not to mention that we need to consider re-export logic). And I know we'd still need special handling for any node which creates a sub-scope, to avoid visiting the sub-scope. But maybe we wouldn't have to enumerate every possible place a target can appear?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:4177 on 2025-03-23 21:14_

I think it would be worth a helper method on the semantic index for getting a single definition, when that's what we expect, rather than repeating the debug assertion and indexing at every call site.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:2996 on 2025-03-23 21:17_

This comment doesn't quite make sense to me as written. If `foo` can't be resolved, then it can't be an "empty module" -- it's a nonexistent module. It seems like more what we want to say is, unresolved modules won't have any definitions associated with them, but we still need to emit a diagnostic for their unresolvability here.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:3124 on 2025-03-23 21:21_

Not sure why we need this `star_import_info` variable and two separate `if let`, when the variable is only used once? Why not just make this `let name = if let DefinitionKind::StarImport(star_import) = definition.kind(self.db()) {` and put all the logic in a single if-let?

---

_@carljm approved on 2025-03-23 21:21_

Nice work!

---

_@AlexWaygood reviewed on 2025-03-23 21:49_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3124 on 2025-03-23 21:49_

Borrow checker says "no" IIRC, but I'll double check!

---

_@AlexWaygood reviewed on 2025-03-23 21:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:84 on 2025-03-23 21:50_

Ooh, interesting idea — I hadn't thought of that! Will try it out

---

_@AlexWaygood reviewed on 2025-03-24 01:56_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:80 on 2025-03-24 01:56_

argh, indeed! I actually added a test that was meant to check this... but there was a bug in the test 🙃

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/module_name.rs`:281 on 2025-03-24 02:18_

Nit: Should this be a method: `ModuleName::from_import_statement` 

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:18 on 2025-03-24 02:20_

Nit: `find_exports` is a somewhat unusual name for a salsa query. We normally name these after what they return rather than using a verb. Maybe just `exports` or `exported_names`?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:42 on 2025-03-24 02:23_

The `contains` + `insert` check has the downside that `name` gets hashed twice. I assume you do it this way to avoid cloning `name` when the name already exists (although that's probably unlikely?). https://doc.rust-lang.org/std/collections/struct.HashSet.html#method.get_or_insert_with would be nice but it's nightly only.

I'm leaning towards always cloning. Or you can use a `FxHashMap<Name, ()>` with the entry API?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:73 on 2025-03-24 02:24_

Nit: It may be worth to cache the `is_stub` lookup

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:234 on 2025-03-24 02:27_

You can use `self.visit_body(body)` here

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:37 on 2025-03-24 02:30_

Not saying this is wrong, I'm just trying to understand the rational. 

What's the motivation for this being its own query vs. computing the exports as part of the semantic index builder?

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3126 on 2025-03-24 02:34_

```suggestion
                Some((star_import, self.index.symbol_table(self.scope())))
```

---

_@MichaReiser reviewed on 2025-03-24 02:35_

This is great!

---

_@AlexWaygood reviewed on 2025-03-24 04:09_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3126 on 2025-03-24 04:09_

hmm, that doesn't compile. This does compile:

```suggestion
                Some((
                    star_import,
                    self.index
                        .symbol_table(self.scope().file_scope_id(self.db())),
                ))
```

but it doesn't really seem simpler anymore ;-)

---

_@AlexWaygood reviewed on 2025-03-24 04:18_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:42 on 2025-03-24 04:18_

Yeah, I thought you might have things to say about this ;)

I also wasn't sure if this was worth it. I switched it to just do the clone every time.

I tried the `FxHashMap` idea but as far as I can tell the `entry` API also requires the value passed in to be owned rather than be a reference? So I'm not sure that helps us here

---

_Comment by @AlexWaygood on 2025-03-24 04:19_

(haven't finished addressing/responding to review comments yet, will continue tomorrow!)

---

_@AlexWaygood reviewed on 2025-03-24 04:24_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1077 on 2025-03-24 04:24_

I'm wondering if we should cache the resolved module in the `DefinitionKind` for `*` import definitions? It feels slightly silly to re-resolve the module name and module for these definitions in `infer.rs`, when we know that we wouldn't have created any definitions for the node during semantic indexing unless the module name and module were both resolvable.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:37 on 2025-03-24 11:34_

The trouble is that if `foo` has `from bar import *` in it, then we need to know the complete list of symbols exported from the global scope of `bar` in order to be able to complete semantic indexing of `foo`. I discussed this with @carljm and he was reluctant to have semantic indexing of one module be blocked on semantic indexing of another module, since semantic indexing can be expensive. (And the chain can go arbitrarily deep if `bar` also has `from baz import *` in it: now semantic indexing of `foo` is blocked on semantic indexing of `bar`, but semantic indexing of `bar` is blocked on semantic indexing of `baz`.)

Instead we decided to go down the route of a simpler, faster query called by semantic indexing that _only_ collects the names exported by the global scope. But the approach obviously has the disadvantage that we don't have all the tools available to us in this query that the `SemanticIndexBuilder` has.

---

_@AlexWaygood reviewed on 2025-03-24 11:34_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:271 on 2025-03-24 11:35_

mimimal repro that shows the bug here is pre-existing, rather than an issue with this PR: [playknot.ruff.rs/6f9f0942-db9f-481f-b868-690be1d1176e](https://playknot.ruff.rs/6f9f0942-db9f-481f-b868-690be1d1176e)

---

_@AlexWaygood reviewed on 2025-03-24 11:35_

---

_@MichaReiser reviewed on 2025-03-24 13:33_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/infer.rs`:3126 on 2025-03-24 13:33_

Oh, this isn't about simplifications. It's to avoid an extra salsa lookup (and dependency).

---

_@MichaReiser reviewed on 2025-03-24 13:34_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:42 on 2025-03-24 13:34_

ah right. That's unfortunate. 

---

_@AlexWaygood reviewed on 2025-03-24 13:35_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3126 on 2025-03-24 13:35_

Oh I see — I'll make the change, thanks

---

_@MichaReiser reviewed on 2025-03-24 13:46_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:37 on 2025-03-24 13:46_

Oh, I see. It recursively resolves star imports. That makes sense then

---

_@AlexWaygood reviewed on 2025-03-24 14:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:37 on 2025-03-24 14:13_

It's my fault for not documenting this better -- I'll add a paragraph to the module doc-comment in `re_exports.rs` 👍

---

_@AlexWaygood reviewed on 2025-03-24 14:29_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3124 on 2025-03-24 14:29_

Yeah, so if I make this change:

```diff
diff --git a/crates/red_knot_python_semantic/src/types/infer.rs b/crates/red_knot_python_semantic/src/types/infer.rs
index 4160a6aec..132001ec4 100644
--- a/crates/red_knot_python_semantic/src/types/infer.rs
+++ b/crates/red_knot_python_semantic/src/types/infer.rs
@@ -3161,21 +3161,16 @@ impl<'db> TypeInferenceBuilder<'db> {
             return;
         };
 
-        let star_import_info = definition
+        let name = definition
             .kind(self.db())
             .as_star_import()
             .map(|star_import| {
-                let symbol_table = self
-                    .index
-                    .symbol_table(self.scope().file_scope_id(self.db()));
-                (star_import, symbol_table)
-            });
-
-        let name = if let Some((star_import, symbol_table)) = star_import_info.as_ref() {
-            symbol_table.symbol(star_import.symbol_id()).name()
-        } else {
-            &alias.name.id
-        };
+                self.index
+                    .symbol_table(self.scope().file_scope_id(self.db()))
+                    .symbol(star_import.symbol_id())
+                    .name()
+            })
+            .unwrap_or(&alias.name.id);
```

then the borrow checker complains:

```
error[E0515]: cannot return value referencing temporary value
    --> crates/red_knot_python_semantic/src/types/infer.rs:3168:17
     |
3168 | //                 self.index
3169 | ||                     .symbol_table(self.scope().file_scope_id(self.db()))
     | ||________________________________________________________________________- temporary value created here
3170 | |                      .symbol(star_import.symbol_id())
3171 | |                      .name()
     | |____________________________^ returns a value referencing data owned by the current function
```

The borrow checker has to be able to prove that the value returned by `.symbol_table()` outlasts the `name` variable, since the `name` variable borrows from the value returned by `.symbol_table()`.

I'll add a comment saying why the indirection is necessary.

---

_@AlexWaygood reviewed on 2025-03-24 14:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:376 on 2025-03-24 14:46_

I had a go at addressing this in https://github.com/astral-sh/ruff/pull/16923/commits/210b860387cb6333b3319f2ce814660eec70a0eb -- is that the kind of thing you were thinking of?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:271 on 2025-03-24 15:36_

Wroteup an issue for this: https://github.com/astral-sh/ruff/issues/16954

---

_@AlexWaygood reviewed on 2025-03-24 15:36_

---

_@AlexWaygood reviewed on 2025-03-24 15:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1077 on 2025-03-24 15:50_

Hrm, this involves quite an annoying amount of refactoring unless we also cache the resolved `ModuleName` as well as the `File` on `StarImportDefinitionKind`. Leaning towards leaning this as-is for now, even though the duplication of logic between this file and `infer.rs` is somewhat annoying.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:265 on 2025-03-24 16:05_

probably `Unknown | int` since none of them are declared?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1052 on 2025-03-24 16:19_

```suggestion
                        // - If the import statement has invalid syntax: multiple `*` names in the `names` list
                        //   (e.g. `from foo import *, bar, *`)
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:16 on 2025-03-24 16:21_

Not sure if we have to say it here, but a main concern is also making semantic indexing potentially cyclic, meaning we'd have to fixpoint iterate it, which would be even more expensive.

---

_@carljm approved on 2025-03-24 16:27_

Looks good to me!

---

_@carljm reviewed on 2025-03-24 16:28_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:376 on 2025-03-24 16:28_

Yep, that looks great!

---

_@AlexWaygood reviewed on 2025-03-24 16:30_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:43 on 2025-03-24 16:30_

Thanks -- I'm tackling this in https://github.com/astral-sh/ruff/pull/16955. I'll rebase this PR on top of that branch

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/import/star.md`:265 on 2025-03-24 16:37_

Thanks, made this change in https://github.com/astral-sh/ruff/pull/16955

---

_@AlexWaygood reviewed on 2025-03-24 16:37_

---

_@AlexWaygood reviewed on 2025-03-24 17:07_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:16 on 2025-03-24 17:07_

I'll add cycle handling as a followup and add a note to this effect at the same time 👍

---

_Merged by @AlexWaygood on 2025-03-24 17:15_

---

_Closed by @AlexWaygood on 2025-03-24 17:15_

---

_Branch deleted on 2025-03-24 17:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/re_exports.rs`:16 on 2025-03-24 18:00_

https://github.com/astral-sh/ruff/pull/16958

---

_@AlexWaygood reviewed on 2025-03-24 18:00_

---
