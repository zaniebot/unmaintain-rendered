```yaml
number: 17984
title: "[ty] bottom-up improvement of diagnostic messages for union type function calls"
type: pull_request
state: merged
author: BurntSushi
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: ag/diag-call-union-redux
created_at: 2025-05-09T14:13:35Z
updated_at: 2025-05-09T17:41:04Z
url: https://github.com/astral-sh/ruff/pull/17984
synced_at: 2026-01-10T18:57:03Z
```

# [ty] bottom-up improvement of diagnostic messages for union type function calls

---

_Pull request opened by @BurntSushi on 2025-05-09 14:13_

This PR is a redux of #17959. Namely, we still have the goal of
improving diagnostics for incorrect function calls through union types,
but we do it from a bottom-up approach. That is, instead of creating
one new aggregate "invalid union call" diagnostic, we keep all of the
existing diagnostics as-is but attach additional context to bring the
union type to the end user's attention.

The benefits of this PR I believe are:

1. We retain our existing taxonomy with no new "aggregate" diagnostics.
2. Errors on function calls through union types will now surface all
relevant diagnostics.
3. Errant union variants now get added `info`-level sub-diagnostics
that contextualize the union information.

One thing this _doesn't_ do that was discussed in #17959 is aggregate
errant union calls based on the diagnostic ID. That is, if there weren't
multiple incorrect union variants that are wrong for the same reason
(like `invalid-argument-type`), then it was suggested that we could
aggregate those into a single top-level diagnostic. I chose not go down
this path for a number of reasons:

1. It didn't seem plausibly straight-forward to do. I think it would
need a fair bit of refactoring.
2. I'm not convinced it's actually an improvement. It still has a
feeling of arbitrariness to it IMO. Moreover, it wasn't obvious to me
how to tie it all together in a way that was cohesive and made sense.
3. Time constraints. I've already spent a ton of time on this specific
diagnostic. :-)


---

_Review requested from @carljm by @BurntSushi on 2025-05-09 14:13_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-05-09 14:13_

---

_Review requested from @sharkdp by @BurntSushi on 2025-05-09 14:13_

---

_Review requested from @dcreager by @BurntSushi on 2025-05-09 14:13_

---

_Label `ty` added by @BurntSushi on 2025-05-09 14:13_

---

_Label `diagnostics` added by @BurntSushi on 2025-05-09 14:13_

---

_Comment by @github-actions[bot] on 2025-05-09 14:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
more-itertools (https://github.com/more-itertools/more-itertools)
- error[invalid-argument-type] more_itertools/recipes.py:675:29: Argument to this function is incorrect: Expected `Sequence[_T] | AbstractSet[_T]`, found `range`
+ error[invalid-argument-type] more_itertools/recipes.py:675:29: Argument to bound method `sample` is incorrect: Expected `Sequence[_T] | AbstractSet[_T]`, found `range`

async-utils (https://github.com/mikeshardmind/async-utils)
- error[invalid-argument-type] src/async_utils/bg_loop.py:81:42: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[_T]`
+ error[invalid-argument-type] src/async_utils/bg_loop.py:81:42: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[_T]`
- error[invalid-argument-type] src/async_utils/corofunc_cache.py:117:50: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
+ error[invalid-argument-type] src/async_utils/corofunc_cache.py:117:50: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
- error[invalid-argument-type] src/async_utils/corofunc_cache.py:196:50: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
+ error[invalid-argument-type] src/async_utils/corofunc_cache.py:196:50: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
- error[invalid-argument-type] src/async_utils/task_cache.py:176:43: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
+ error[invalid-argument-type] src/async_utils/task_cache.py:176:43: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
- error[invalid-argument-type] src/async_utils/task_cache.py:262:43: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
+ error[invalid-argument-type] src/async_utils/task_cache.py:262:43: Argument to function `wrap_future` is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ error[missing-argument] mypy_primer/main.py:62:12: No argument provided for required parameter `repo` of function `setup_pyright`
+ error[missing-argument] mypy_primer/main.py:62:12: No argument provided for required parameter `repo` of function `setup_ty`
- error[invalid-argument-type] mypy_primer/main.py:130:39: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] mypy_primer/main.py:130:39: Argument to bound method `from_location` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] mypy_primer/projects.py:55:21: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] mypy_primer/projects.py:55:21: Argument to function `context_diff` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] mypy_primer/projects.py:56:21: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] mypy_primer/projects.py:56:21: Argument to function `context_diff` is incorrect: Expected `str`, found `str | None`
- Found 16 diagnostics
+ Found 18 diagnostics

parso (https://github.com/davidhalter/parso)
- error[invalid-argument-type] parso/__init__.py:57:28: Argument to this function is incorrect: Expected `str`, found `None`
+ error[invalid-argument-type] parso/__init__.py:57:28: Argument to function `load_grammar` is incorrect: Expected `str`, found `None`
- error[invalid-argument-type] parso/python/diff.py:884:35: Argument to this function is incorrect: Expected `tuple[int, int]`, found `tuple[Unknown, ...]`
+ error[invalid-argument-type] parso/python/diff.py:884:35: Argument to bound method `__init__` is incorrect: Expected `tuple[int, int]`, found `tuple[Unknown, ...]`
- error[invalid-argument-type] parso/python/tokenize.py:230:42: Argument to this function is incorrect: Expected `tuple[str]`, found `set[Unknown]`
+ error[invalid-argument-type] parso/python/tokenize.py:230:42: Argument is incorrect: Expected `tuple[str]`, found `set[Unknown]`

com2ann (https://github.com/ilevkivskyi/com2ann)
- error[invalid-argument-type] src/com2ann.py:163:32: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] src/com2ann.py:163:32: Argument is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] src/com2ann.py:166:32: Argument to this function is incorrect: Expected `int`, found `int | None`
+ error[invalid-argument-type] src/com2ann.py:166:32: Argument is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] src/com2ann.py:166:50: Argument to this function is incorrect: Expected `int`, found `int | None`
+ error[invalid-argument-type] src/com2ann.py:166:50: Argument is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] src/com2ann.py:667:23: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] src/com2ann.py:667:23: Argument to bound method `add` is incorrect: Expected `str`, found `str | None`

nionutils (https://github.com/nion-software/nionutils)
- error[invalid-argument-type] nion/utils/Stream.py:335:67: Argument to this function is incorrect: Expected `Observable | None`, found `(Observable & ~AbstractStream[Unknown]) | (AbstractStream[Observable] & ~AbstractStream[Unknown])`
+ error[invalid-argument-type] nion/utils/Stream.py:335:67: Argument to bound method `__init__` is incorrect: Expected `Observable | None`, found `(Observable & ~AbstractStream[Unknown]) | (AbstractStream[Observable] & ~AbstractStream[Unknown])`
- error[invalid-argument-type] nion/utils/Stream.py:550:53: Argument to this function is incorrect: Expected `Queue[ValueChange[Unknown]]`, found `Queue[ValueChange[T]]`
+ error[invalid-argument-type] nion/utils/Stream.py:550:53: Argument to bound method `__init__` is incorrect: Expected `Queue[ValueChange[Unknown]]`, found `Queue[ValueChange[T]]`

beartype (https://github.com/beartype/beartype)
- error[invalid-argument-type] beartype/_check/error/errmain.py:504:9: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] beartype/_check/error/errmain.py:504:9: Argument to function `suffix_str_unless_suffixed` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] beartype/_decor/_nontype/decornontype.py:369:23: Argument to this function is incorrect: Argument type `BeartypeableT` does not satisfy upper bound of type variable `_F`
+ error[invalid-argument-type] beartype/_decor/_nontype/decornontype.py:369:23: Argument to function `no_type_check` is incorrect: Argument type `BeartypeableT` does not satisfy upper bound of type variable `_F`
- error[invalid-argument-type] beartype/_decor/_nontype/decornontype.py:409:9: Argument to this function is incorrect: Expected `((...) -> Unknown) | None`, found `BeartypeableT`
+ error[invalid-argument-type] beartype/_decor/_nontype/decornontype.py:409:9: Argument to function `make_func` is incorrect: Expected `((...) -> Unknown) | None`, found `BeartypeableT`
- error[invalid-argument-type] beartype/_util/api/standard/utilfunctools.py:292:29: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `BeartypeableT`
+ error[invalid-argument-type] beartype/_util/api/standard/utilfunctools.py:292:29: Argument to function `unwrap_func_once` is incorrect: Expected `(...) -> Unknown`, found `BeartypeableT`
- error[invalid-argument-type] beartype/_util/hint/pep/proposal/pep484585/pep484585ref.py:532:43: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `((...) -> Unknown) | None`
+ error[invalid-argument-type] beartype/_util/hint/pep/proposal/pep484585/pep484585ref.py:532:43: Argument to function `label_callable` is incorrect: Expected `(...) -> Unknown`, found `((...) -> Unknown) | None`
- error[invalid-argument-type] beartype/claw/_importlib/clawimppath.py:142:23: Argument to this function is incorrect: Expected `(str, /) -> PathEntryFinderProtocol`, found `Unknown | None`
+ error[invalid-argument-type] beartype/claw/_importlib/clawimppath.py:142:23: Argument to bound method `remove` is incorrect: Expected `(str, /) -> PathEntryFinderProtocol`, found `Unknown | None`
- error[invalid-argument-type] beartype/door/_func/infer/inferhint.py:244:36: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `~type`
+ error[invalid-argument-type] beartype/door/_func/infer/inferhint.py:244:36: Argument to function `infer_hint_callable` is incorrect: Expected `(...) -> Unknown`, found `~type`

python-chess (https://github.com/niklasf/python-chess)
- error[invalid-argument-type] chess/engine.py:2393:64: Argument to this function is incorrect: Expected `int`, found `int | None`
+ error[invalid-argument-type] chess/engine.py:2393:64: Argument to bound method `__init__` is incorrect: Expected `int`, found `int | None`
- error[invalid-argument-type] chess/engine.py:2972:43: Argument to this function is incorrect: Expected `AnalysisResult`, found `_T`
+ error[invalid-argument-type] chess/engine.py:2972:43: Argument to bound method `__init__` is incorrect: Expected `AnalysisResult`, found `_T`
- error[invalid-argument-type] chess/gaviota.py:1892:35: Argument to this function is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None | Literal[64]`
+ error[invalid-argument-type] chess/gaviota.py:1892:35: Argument to bound method `__init__` is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None | Literal[64]`
- error[invalid-argument-type] chess/polyglot.py:262:59: Argument to this function is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None`
+ error[invalid-argument-type] chess/polyglot.py:262:59: Argument to function `square_file` is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None`
- error[invalid-argument-type] chess/svg.py:170:19: Argument to this function is incorrect: Expected `str`, found `Unknown | None`
+ error[invalid-argument-type] chess/svg.py:170:19: Argument to function `_color` is incorrect: Expected `str`, found `Unknown | None`

pyinstrument (https://github.com/joerick/pyinstrument)
- error[invalid-argument-type] pyinstrument/__main__.py:56:32: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] pyinstrument/__main__.py:56:32: Argument to function `setattr` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] pyinstrument/__main__.py:323:76: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] pyinstrument/__main__.py:323:76: Argument to function `file_is_a_tty` is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/__main__.py:332:49: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] pyinstrument/__main__.py:332:49: Argument to function `load_report_from_temp_storage` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] pyinstrument/__main__.py:392:95: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] pyinstrument/__main__.py:392:95: Argument to function `file_is_a_tty` is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/__main__.py:445:40: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] pyinstrument/__main__.py:445:40: Argument to function `translate` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] pyinstrument/__main__.py:458:40: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] pyinstrument/__main__.py:458:40: Argument to function `translate` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] pyinstrument/__main__.py:513:47: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO`
+ error[invalid-argument-type] pyinstrument/__main__.py:513:47: Argument to function `file_supports_unicode` is incorrect: Expected `IO[AnyStr]`, found `TextIO`
- error[invalid-argument-type] pyinstrument/__main__.py:514:43: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO`
+ error[invalid-argument-type] pyinstrument/__main__.py:514:43: Argument to function `file_supports_color` is incorrect: Expected `IO[AnyStr]`, found `TextIO`
- error[invalid-argument-type] pyinstrument/__main__.py:533:48: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] pyinstrument/__main__.py:533:48: Argument to function `guess_renderer_from_outfile` is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] pyinstrument/context_manager.py:92:43: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] pyinstrument/context_manager.py:92:43: Argument to function `file_supports_color` is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/context_manager.py:93:47: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] pyinstrument/context_manager.py:93:47: Argument to function `file_supports_unicode` is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/profiler.py:312:45: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `IO[str] | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] pyinstrument/profiler.py:312:45: Argument to function `file_supports_unicode` is incorrect: Expected `IO[AnyStr]`, found `IO[str] | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/profiler.py:314:41: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `IO[str] | @Todo(Support for `typing.TypeAlias`)`
+ error[invalid-argument-type] pyinstrument/profiler.py:314:41: Argument to function `file_supports_color` is incorrect: Expected `IO[AnyStr]`, found `IO[str] | @Todo(Support for `typing.TypeAlias`)`
- error[invalid-argument-type] pyinstrument/renderers/html.py:104:25: Argument to this function is incorrect: Expected `str`, found `Literal[b""]`
+ error[invalid-argument-type] pyinstrument/renderers/html.py:104:25: Argument to function `open` is incorrect: Expected `str`, found `Literal[b""]`
- error[invalid-argument-type] pyinstrument/renderers/jsonrenderer.py:61:67: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[invalid-argument-type] pyinstrument/renderers/jsonrenderer.py:61:67: Argument is incorrect: Expected `str`, found `str | None`
- error[invalid-argument-type] pyinstrument/session.py:109:13: Argument to this function is incorrect: Expected `list[str]`, found `Unknown | None`
+ error[invalid-argument-type] pyinstrument/session.py:109:13: Argument to bound method `__init__` is incorrect: Expected `list[str]`, found `Unknown | None`

attrs (https://github.com/python-attrs/attrs)
- error[invalid-argument-type] tests/dataclass_transform_example.py:23:17: Argument to this function is incorrect: Expected `int`, found `Literal[b"42"]`
+ error[invalid-argument-type] tests/dataclass_transform_example.py:23:17: Argument is incorrect: Expected `int`, found `Literal[b"42"]`
- error[invalid-argument-type] tests/test_annotations.py:49:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:49:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:50:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:50:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:51:36: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:51:36: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:79:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:79:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:80:52: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:80:52: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:94:37: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:94:37: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:123:28: Argument to this function is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:123:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:125:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:125:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:127:50: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:127:50: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:128:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:128:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:130:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:130:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:131:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:131:33: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:133:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:133:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:135:42: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:135:42: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:416:28: Argument to this function is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:416:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:518:28: Argument to this function is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:518:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:520:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:520:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:521:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:521:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:522:36: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:522:36: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:540:28: Argument to this function is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:540:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'C'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:542:56: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_annotations.py:542:56: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_annotations.py:548:28: Argument to this function is incorrect: Argument type `<class 'D'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:548:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'D'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:550:37: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'D'>`
+ error[invalid-argument-type] tests/test_annotations.py:550:37: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'D'>`
- error[invalid-argument-type] tests/test_annotations.py:565:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:565:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:567:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:567:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:568:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:568:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:569:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:569:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:583:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:583:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:584:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:584:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:585:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:585:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:599:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:599:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:601:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:601:33: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:602:50: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:602:50: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:619:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:619:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:620:28: Argument to this function is incorrect: Argument type `<class 'B'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:620:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'B'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:622:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:622:46: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:623:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
+ error[invalid-argument-type] tests/test_annotations.py:623:33: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[invalid-argument-type] tests/test_annotations.py:625:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:625:46: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:626:33: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
+ error[invalid-argument-type] tests/test_annotations.py:626:33: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[invalid-argument-type] tests/test_annotations.py:667:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:667:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:668:28: Argument to this function is incorrect: Argument type `<class 'B'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:668:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'B'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:670:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:670:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:671:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
+ error[invalid-argument-type] tests/test_annotations.py:671:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[invalid-argument-type] tests/test_annotations.py:683:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:683:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:685:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:685:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_annotations.py:687:28: Argument to this function is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_annotations.py:687:28: Argument to function `resolve_types` is incorrect: Argument type `<class 'A'>` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_annotations.py:689:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_annotations.py:689:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_converters.py:297:29: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_converters.py:297:29: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_converters.py:350:21: Argument to this function is incorrect: Expected `str | int`, found `list[Unknown]`
+ error[invalid-argument-type] tests/test_converters.py:350:21: Argument to function `to_bool` is incorrect: Expected `str | int`, found `list[Unknown]`
- error[invalid-argument-type] tests/test_filters.py:33:31: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:33:31: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:34:46: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:34:46: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:47:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:47:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:48:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:48:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:52:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:52:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:60:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:60:25: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:67:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:67:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:68:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:68:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:72:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:72:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:80:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:80:25: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:93:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:93:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:94:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:94:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:98:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:98:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:106:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:106:25: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:113:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:113:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:114:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:114:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:118:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:118:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_filters.py:126:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_filters.py:126:25: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_functional.py:167:25: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1'>`
+ error[invalid-argument-type] tests/test_functional.py:167:25: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1'>`
- error[invalid-argument-type] tests/test_functional.py:218:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `type`
+ error[invalid-argument-type] tests/test_functional.py:218:26: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `type`
- error[invalid-argument-type] tests/test_functional.py:545:42: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_functional.py:545:42: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_functional.py:546:44: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_functional.py:546:44: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_functional.py:547:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_functional.py:547:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_functional.py:646:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_functional.py:646:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_functional.py:647:35: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_functional.py:647:35: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_hooks.py:70:15: Argument to this function is incorrect: Expected `int`, found `Literal["3"]`
+ error[invalid-argument-type] tests/test_hooks.py:70:15: Argument is incorrect: Expected `int`, found `Literal["3"]`
- error[invalid-argument-type] tests/test_hooks.py:70:22: Argument to this function is incorrect: Expected `int | float`, found `Literal["3.14"]`
+ error[invalid-argument-type] tests/test_hooks.py:70:22: Argument is incorrect: Expected `int | float`, found `Literal["3.14"]`
- error[invalid-argument-type] tests/test_hooks.py:88:44: Argument to this function is incorrect: Expected `int`, found `float`
+ error[invalid-argument-type] tests/test_hooks.py:88:44: Argument is incorrect: Expected `int`, found `float`
- error[invalid-argument-type] tests/test_hooks.py:216:40: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_hooks.py:216:40: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_hooks.py:234:54: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Base'>`
+ error[invalid-argument-type] tests/test_hooks.py:234:54: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Base'>`
- error[invalid-argument-type] tests/test_hooks.py:262:21: Argument to this function is incorrect: Expected `datetime`, found `Literal[1]`
+ error[invalid-argument-type] tests/test_hooks.py:262:21: Argument is incorrect: Expected `datetime`, found `Literal[1]`
- error[invalid-argument-type] tests/test_hooks.py:263:22: Argument to this function is incorrect: Expected `datetime`, found `Literal[2]`
+ error[invalid-argument-type] tests/test_hooks.py:263:22: Argument is incorrect: Expected `datetime`, found `Literal[2]`
- error[invalid-argument-type] tests/test_hooks.py:264:30: Argument to this function is incorrect: Expected `datetime`, found `Literal[3]`
+ error[invalid-argument-type] tests/test_hooks.py:264:30: Argument is incorrect: Expected `datetime`, found `Literal[3]`
- error[invalid-argument-type] tests/test_make.py:204:28: Argument to this function is incorrect: Expected `type`, found `tuple[()]`
+ error[invalid-argument-type] tests/test_make.py:204:28: Argument is incorrect: Expected `type`, found `tuple[()]`
- error[invalid-argument-type] tests/test_make.py:462:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
+ error[invalid-argument-type] tests/test_make.py:462:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'A'>`
- error[invalid-argument-type] tests/test_make.py:464:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
+ error[invalid-argument-type] tests/test_make.py:464:26: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[invalid-argument-type] tests/test_make.py:465:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
+ error[invalid-argument-type] tests/test_make.py:465:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'B'>`
- error[invalid-argument-type] tests/test_make.py:467:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_make.py:467:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_make.py:468:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_make.py:468:26: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_make.py:469:27: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_make.py:469:27: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_make.py:793:45: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_make.py:793:45: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_make.py:830:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'BaseClass'>`
+ error[invalid-argument-type] tests/test_make.py:830:26: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'BaseClass'>`
- error[invalid-argument-type] tests/test_make.py:831:26: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'SubClass'>`
+ error[invalid-argument-type] tests/test_make.py:831:26: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'SubClass'>`
- error[invalid-argument-type] tests/test_make.py:1242:32: Argument to this function is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_make.py:1242:32: Argument to function `resolve_types` is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_make.py:1253:32: Argument to this function is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/test_make.py:1253:32: Argument to function `resolve_types` is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
- error[invalid-argument-type] tests/test_make.py:2013:34: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Cases'>`
+ error[invalid-argument-type] tests/test_make.py:2013:34: Argument to function `fields_dict` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Cases'>`
- error[invalid-argument-type] tests/test_next_gen.py:41:36: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `type`
+ error[invalid-argument-type] tests/test_next_gen.py:41:36: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `type`
- error[invalid-argument-type] tests/test_next_gen.py:68:23: Argument to this function is incorrect: Expected `int`, found `None`
+ error[invalid-argument-type] tests/test_next_gen.py:68:23: Argument is incorrect: Expected `int`, found `None`
- error[invalid-argument-type] tests/test_next_gen.py:146:29: Argument to this function is incorrect: Expected `list[Unknown]`, found `Literal[1]`
+ error[invalid-argument-type] tests/test_next_gen.py:146:29: Argument is incorrect: Expected `list[Unknown]`, found `Literal[1]`
- error[invalid-argument-type] tests/test_next_gen.py:146:48: Argument to this function is incorrect: Expected `list[Unknown]`, found `Literal[1]`
+ error[invalid-argument-type] tests/test_next_gen.py:146:48: Argument is incorrect: Expected `list[Unknown]`, found `Literal[1]`
- error[invalid-argument-type] tests/test_next_gen.py:148:26: Argument to this function is incorrect: Expected `list[Unknown]`, found `Literal[-1]`
+ error[invalid-argument-type] tests/test_next_gen.py:148:26: Argument is incorrect: Expected `list[Unknown]`, found `Literal[-1]`
- error[invalid-argument-type] tests/test_next_gen.py:149:39: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NewSchool'>`
+ error[invalid-argument-type] tests/test_next_gen.py:149:39: Argument to function `fields_dict` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NewSchool'>`
- error[invalid-argument-type] tests/test_next_gen.py:164:39: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NewSchool'>`
+ error[invalid-argument-type] tests/test_next_gen.py:164:39: Argument to function `fields_dict` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NewSchool'>`
- error[invalid-argument-type] tests/test_next_gen.py:216:15: Argument to this function is incorrect: Expected `str`, found `Literal[1]`
+ error[invalid-argument-type] tests/test_next_gen.py:216:15: Argument is incorrect: Expected `str`, found `Literal[1]`
- error[invalid-argument-type] tests/test_next_gen.py:444:25: Argument to this function is incorrect: Expected `int`, found `dict[Unknown, Unknown]`
+ error[invalid-argument-type] tests/test_next_gen.py:444:25: Argument is incorrect: Expected `int`, found `dict[Unknown, Unknown]`
- error[invalid-argument-type] tests/test_slots.py:103:24: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1Slots'>`
+ error[invalid-argument-type] tests/test_slots.py:103:24: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1Slots'>`
- error[invalid-argument-type] tests/test_slots.py:103:48: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1'>`
+ error[invalid-argument-type] tests/test_slots.py:103:48: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C1'>`
- error[invalid-argument-type] tests/test_slots.py:545:41: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
+ error[invalid-argument-type] tests/test_slots.py:545:41: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'C'>`
- error[invalid-argument-type] tests/test_validators.py:185:68: Argument to this function is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
+ error[invalid-argument-type] tests/test_validators.py:185:68: Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
- error[invalid-argument-type] tests/test_validators.py:219:56: Argument to this function is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
+ error[invalid-argument-type] tests/test_validators.py:219:56: Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
- error[invalid-argument-type] tests/test_validators.py:228:32: Argument to this function is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `() -> Unknown`
+ error[invalid-argument-type] tests/test_validators.py:228:32: Argument to function `matches_re` is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[Literal["a"]] | None) | None`, found `() -> Unknown`
- error[invalid-argument-type] tests/test_validators.py:818:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
+ error[invalid-argument-type] tests/test_validators.py:818:23: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- error[invalid-argument-type] tests/test_validators.py:890:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
+ error[invalid-argument-type] tests/test_validators.py:890:23: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- error[invalid-argument-type] tests/test_validators.py:963:23: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
+ error[invalid-argument-type] tests/test_validators.py:963:23: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'Tester'>`
- error[invalid-argument-type] tests/typing_example.py:133:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'AliasExample'>`
+ error[invalid-argument-type] tests/typing_example.py:133:13: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'AliasExample'>`
- error[invalid-argument-type] tests/typing_example.py:134:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'AliasExample'>`
+ error[invalid-argument-type] tests/typing_example.py:134:13: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'AliasExample'>`
- error[invalid-argument-type] tests/typing_example.py:177:13: Argument to this function is incorrect: Expected `int`, found `Literal["on"]`
+ error[invalid-argument-type] tests/typing_example.py:177:13: Argument is incorrect: Expected `int`, found `Literal["on"]`
- error[invalid-argument-type] tests/typing_example.py:178:13: Argument to this function is incorrect: Expected `int`, found `Literal["yes"]`
+ error[invalid-argument-type] tests/typing_example.py:178:13: Argument is incorrect: Expected `int`, found `Literal["yes"]`
- error[invalid-argument-type] tests/typing_example.py:181:13: Argument to this function is incorrect: Expected `int`, found `Literal["n"]`
+ error[invalid-argument-type] tests/typing_example.py:181:13: Argument is incorrect: Expected `int`, found `Literal["n"]`
- error[invalid-argument-type] tests/typing_example.py:396:13: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
+ error[invalid-argument-type] tests/typing_example.py:396:13: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- error[invalid-argument-type] tests/typing_example.py:397:17: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
+ error[invalid-argument-type] tests/typing_example.py:397:17: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- error[invalid-argument-type] tests/typing_example.py:401:14: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
+ error[invalid-argument-type] tests/typing_example.py:401:14: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- error[invalid-argument-type] tests/typing_example.py:402:18: Argument to this function is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
+ error[invalid-argument-type] tests/typing_example.py:402:18: Argument to function `fields` is incorrect: Expected `type[AttrsInstance]`, found `<class 'NGFrozen'>`
- error[invalid-argument-type] tests/typing_example.py:490:28: Argument to this function is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
+ error[invalid-argument-type] tests/typing_example.py:490:28: Argument to function `resolve_types` is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`

git-revise (https://github.com/mystor/git-revise)
- error[invalid-argument-type] gitrevise/tui.py:126:40: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[invalid-argument-type] gitrevise/tui.py:126:40: Argument to function `commit_range` is incorrect: Expected `Commit`, found `Commit | None`
- error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to function `local_commits` is incorrect: Expected `Commit`, found `Commit | None`
- error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to function `local_commits` is incorrect: Expected `Commit`, found `Commit | None`
- error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[invalid-argument-type] gitrevise/tui.py:128:47: Argument to function `local_commits` is incorrect: Expected `Commit`, found `Commit | None`
- error[invalid-argument-type] gitrevise/tui.py:180:39: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[invalid-argument-type] gitrevise/tui.py:180:39: Argument to function `commit_range` is incorrect: Expected `Commit`, found `Commit | None`

aioredis (https://github.com/aio-libs/aioredis)
- error[invalid-argument-type] aioredis/connection.py:1208:38: Argument to this function is incorrect: Expected `str | bytes`, found `str | None`
+ error[invalid-argument-type] aioredis/connection.py:1208:38: Argument to function `unquote` is incorrect: Expected `str | bytes`, found `str | None`
- error[invalid-argument-type] aioredis/connection.py:1210:38: Argument to this function is incorrect: Expected `str | bytes`, found `str | None`
+ error[invalid-argument-type] aioredis/connection.py:1210:38: Argument to function `unquote` is incorrect: Expected `str | bytes`, found `str | None`
- error[invalid-argument-type] aioredis/connection.py:1220:38: Argument to this function is incorrect: Expected `str | bytes`, found `str | None`
+ error[invalid-argument-type] aioredis/connection.py:1220:38: Argument to function `unquote` is incorrect: Expected `str | bytes`, found `str | None`

anyio (https://github.com/agronholm/anyio)
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:1220:38: Argument to this function is incorrect: Expected `tuple[str, int, int, int] | tuple[str, int]`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...]`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:1220:38: Argument to function `convert_ipv6_sockaddr` is incorrect: Expected `tuple[str, int, int, int] | tuple[str, int]`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...]`
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2469:41: Argument to this function is incorrect: Expected `tuple[Context, (...) -> Unknown, tuple[Unknown, ...], Future[Unknown], CancelScope] | None`, found `tuple[Context, (...) -> T_Retval, @Todo(full tuple[...] support), Future[T_Retval], CancelScope | None]`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2469:41: Argument to bound method `put_nowait` is incorrect: Expected `tuple[Context, (...) -> Unknown, tuple[Unknown, ...], Future[Unknown], CancelScope] | None`, found `tuple[Context, (...) -> T_Retval, @Todo(full tuple[...] support), Future[T_Retval], CancelScope | None]`
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to bound method `create_datagram_endpoint` is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to bound method `create_datagram_endpoint` is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to bound method `create_datagram_endpoint` is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to bound method `create_datagram_endpoint` is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to bound method `create_datagram_endpoint` is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to bound method `create_datagram_endpoint` is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
- error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2700:53: Argument to this function is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...]`
+ error[invalid-argument-type] src/anyio/_backends/_asyncio.py:2700:53: Argument to bound method `getnameinfo` is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...]`
- error[invalid-argument-type] src/anyio/_core/_streams.py:52:40: Argument to this function is incorrect: Expected `MemoryObjectStreamState[T_contra]`, found `MemoryObjectStreamState[T_Item]`
+ error[invalid-argument-type] src/anyio/_core/_streams.py:52:40: Argument is incorrect: Expected `MemoryObjectStreamState[T_contra]`, found `MemoryObjectStreamState[T_Item]`
- error[invalid-argument-type] src/anyio/_core/_streams.py:52:74: Argument to this function is incorrect: Expected `MemoryObjectStreamState[T_co]`, found `MemoryObjectStreamState[T_Item]`
+ error[invalid-argument-type] src/anyio/_core/_streams.py:52:74: Argument is incorrect: Expected `MemoryObjectStreamState[T_co]`, found `MemoryObjectStreamState[T_Item]`
- error[invalid-argument-type] src/anyio/from_thread.py:236:35: Argument to this function is incorrect: Expected `T_Retval`, found `@Todo(generic `typing.Awaitable` type) | Awaitable[T_Retval] | T_Retval`
+ error[invalid-argument-type] src/anyio/from_thread.py:236:35: Argument to bound method `set_result` is incorrect: Expected `T_Retval`, found `@Todo(generic `typing.Awaitable` type) | Awaitable[T_Retval] | T_Retval`
- error[invalid-argument-type] src/anyio/streams/text.py:123:36: Argument to this function is incorrect: Expected `InitVar[str]`, found `str`
+ error[invalid-argument-type] src/anyio/streams/text.py:123:36: Argument is incorrect: Expected `InitVar[str]`, found `str`
- error[invalid-argument-type] src/anyio/streams/text.py:123:55: Argument to this function is incorrect: Expected `InitVar[str]`, found `str`
+ error[invalid-argument-type] src/anyio/streams/text.py:123:55: Argument is incorrect: Expected `InitVar[str]`, found `str`
- error[invalid-argument-type] src/anyio/streams/text.py:126:36: Argument to this function is incorrect: Expected `InitVar[str]`, found `str`
+ error[invalid-argument-type] src/anyio/streams/text.py:126:36: Argument is incorrect: Expected `InitVar[str]`, found `str`

kornia (https://github.com/kornia/kornia)
- error[invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:63: Argument to this function is incorrect: Expected `tuple[int | float, int | f...*[Comment body truncated]*

---

_Comment by @MichaReiser on 2025-05-09 15:03_

There are a few diagnostics that now look like duplicates (but maybe they're not?). Could you take a look at them

```
mypy (https://github.com/python/mypy)
+ error[lint:invalid-argument-type] mypy/checker.py:6610:45: Argument to this function is incorrect: Expected `ProperType`, found `None`
+ error[lint:invalid-argument-type] mypy/checker.py:6610:45: Argument to this function is incorrect: Expected `ProperType`, found `None`
```


---

_Comment by @BurntSushi on 2025-05-09 15:12_

@MichaReiser Only the top-level diagnostic is duplicative. So in concise output mode, they look like duplicates. But once you get more context, it's not duplicative:

```
$ cat main.py
def f1(name: str) -> int: return 0
def f2(name: str) -> int: return 0

def _(flag: bool):
    if flag:
        f = f1
    else:
        f = f2
    x = f(3)

$ run-ty -q pr2 -- check main.py
error: lint:invalid-argument-type: Argument to this function is incorrect
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^ Expected `str`, found `Literal[3]`
  |
info: Function defined here
 --> main.py:1:5
  |
1 | def f1(name: str) -> int: return 0
  |     ^^ --------- Parameter declared here
2 | def f2(name: str) -> int: return 0
  |
info: Union variant `def f1(name: str) -> int` is incompatible with this call site
info: Union type `(def f1(name: str) -> int) | (def f2(name: str) -> int)` is not callable
info: `lint:invalid-argument-type` is enabled by default

error: lint:invalid-argument-type: Argument to this function is incorrect
 --> main.py:9:11
  |
7 |     else:
8 |         f = f2
9 |     x = f(3)
  |           ^ Expected `str`, found `Literal[3]`
  |
info: Function defined here
 --> main.py:2:5
  |
1 | def f1(name: str) -> int: return 0
2 | def f2(name: str) -> int: return 0
  |     ^^ --------- Parameter declared here
3 |
4 | def _(flag: bool):
  |
info: Union variant `def f2(name: str) -> int` is incompatible with this call site
info: Union type `(def f1(name: str) -> int) | (def f2(name: str) -> int)` is not callable
info: `lint:invalid-argument-type` is enabled by default

Found 2 diagnostics
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_function_types_-_A_smaller_scale_example.snap`:34 on 2025-05-09 15:15_

I think one thing that would make the concise diagnostic for `invalid-argument-type` more informative (and less duplicate-looking in some cases) would be if we name the target function here, like we do below for `too-many-positional-arguments` (and for many of the other call diagnostics)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_function_types_-_A_smaller_scale_example.snap`:52 on 2025-05-09 15:24_

I think I mentioned this in a comment on the previous PR: I think we should reserve "not callable" to describe "objects that don't support the Python calling protocol" (that's what the `call-not-callable` diagnostic describes). So I would rather give this context in a way that doesn't use the phrase "not callable". One option could be "Attempted to call union type `...`" or "Error calling union type `...`"

---

_@carljm approved on 2025-05-09 15:33_

This looks great, thank you!

---

_Comment by @MichaReiser on 2025-05-09 16:26_

I don't think I'll get to reviewing this before Monday but you shouldn't feel blocked by my review.

---

_@BurntSushi reviewed on 2025-05-09 16:49_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_function_types_-_A_smaller_scale_example.snap`:34 on 2025-05-09 16:49_

Nice. This is done. And _super_ annoying to update mdtest for. Hah. But I think the messages are better.

---

_@BurntSushi reviewed on 2025-05-09 16:50_

---

_Review comment by @BurntSushi on `crates/ty_python_semantic/resources/mdtest/snapshots/union_call.md_-_Calling_a_union_of_function_types_-_A_smaller_scale_example.snap`:52 on 2025-05-09 16:50_

Ah right, thanks for repeating that here.

I went with your "Attempt to call union type `...`" verbiage here. I think it's still not great, but I get want to keep the "not callable" verbiage reserved.

---

_Comment by @github-actions[bot] on 2025-05-09 17:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@carljm approved on 2025-05-09 17:17_

Ship it!

---

_Merged by @BurntSushi on 2025-05-09 17:40_

---

_Closed by @BurntSushi on 2025-05-09 17:40_

---

_Branch deleted on 2025-05-09 17:40_

---

_Comment by @BurntSushi on 2025-05-09 17:41_

@MichaReiser Happy to do follow-ups. :-)

---
