```yaml
number: 17832
title: "[ty] Understand classes that inherit from subscripted `Protocol[]` as generic"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/generic-protocol-mro
created_at: 2025-05-04T13:35:09Z
updated_at: 2025-06-27T16:29:53Z
url: https://github.com/astral-sh/ruff/pull/17832
synced_at: 2026-01-10T18:51:01Z
```

# [ty] Understand classes that inherit from subscripted `Protocol[]` as generic

---

_Pull request opened by @AlexWaygood on 2025-05-04 13:35_

## Summary

- Understand that classes that inherit from `Protocol[]` are generic in exactly the same way as classes that inherit from `Generic[]`. This fixes our MRO inference for a large number of stdlib classes.
- Add the special case for displaying a specialisation of the `tuple` class: although `tuple` is only generic over a single type variable in typeshed, a homogeneous tuple is spelled `tuple[int, ...]` in type annotations rather than `tuple[int]`.
- Add TODO branches for various things that now lead to false-positive errors as a result of our improved understanding of the MROs for a whole range of stdlib classes. In particular:
  - Ensure we continue to not emit false positives on functional `NamedTuple` syntax, e.g. `Person = NamedTuple("Person", [("Name", int)])`
  - Ensure we continue to not emit false positives on classes that inherit from heterogeneous tuples, e.g. `tuple[int, str]`
  - Ensure we continue to not emit false positives on unpacked tuples in type annotations, e.g. `*tuple[int, ...]`

## Test Plan

Existing mdtests updated


---

_Label `ty` added by @AlexWaygood on 2025-05-04 13:37_

---

_Closed by @AlexWaygood on 2025-05-04 13:42_

---

_Reopened by @AlexWaygood on 2025-05-04 13:42_

---

_Comment by @AlexWaygood on 2025-05-04 13:49_

I obviously need to investigate the panic found in mypy_primer, but apart from that I think this is ready for review, so I'll take it out of draft.

---

_Marked ready for review by @AlexWaygood on 2025-05-04 13:49_

---

_Review requested from @carljm by @AlexWaygood on 2025-05-04 13:49_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-05-04 13:49_

---

_Review requested from @dcreager by @AlexWaygood on 2025-05-04 13:49_

---

_Comment by @AlexWaygood on 2025-05-04 13:54_

> I obviously need to investigate the panic found in mypy_primer

I haven't looked into it yet, but I'm removing `packaging` from the project selector for now so that I can see what the primer results are on all other projects...

---

_Renamed from "[ty] Understand classes that inherited from subscripted `Protocol[]` as generic" to "[ty] Understand classes that inherit from subscripted `Protocol[]` as generic" by @AlexWaygood on 2025-05-04 14:48_

---

_Comment by @github-actions[bot] on 2025-05-04 15:04_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
+ error[lint:invalid-argument-type] src/async_utils/bg_loop.py:81:42: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[_T]`
+ error[lint:invalid-argument-type] src/async_utils/corofunc_cache.py:117:50: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
+ error[lint:invalid-argument-type] src/async_utils/corofunc_cache.py:196:50: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
+ error[lint:invalid-argument-type] src/async_utils/task_cache.py:176:43: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
+ error[lint:invalid-return-type] src/async_utils/task_cache.py:177:24: Return type does not match returned value: Expected `Task[R]`, found `Task[_T]`
+ error[lint:invalid-argument-type] src/async_utils/task_cache.py:262:43: Argument to this function is incorrect: Expected `Future[_T] | Future[_T]`, found `Future[R]`
+ error[lint:invalid-return-type] src/async_utils/task_cache.py:263:24: Return type does not match returned value: Expected `Task[R]`, found `Task[_T]`
- Found 22 diagnostics
+ Found 29 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ error[lint:no-matching-overload] pytest_robotframework/__init__.py:302:22: No overload of bound method `__init__` matches arguments
- error[lint:unresolved-attribute] pytest_robotframework/_internal/pytest/plugin.py:452:21: Unresolved attribute `_stream` on type `_RedirectStream`.
+ error[lint:unresolved-attribute] pytest_robotframework/_internal/pytest/plugin.py:452:21: Unresolved attribute `_stream` on type `_RedirectStream[IO[str]]`.
- error[lint:type-assertion-failure] tests/type_tests.py:41:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> @Todo(specialized non-generic class)`
+ error[lint:type-assertion-failure] tests/type_tests.py:41:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> AbstractContextManager[None, bool | None]`
- error[lint:type-assertion-failure] tests/type_tests.py:49:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> @Todo(specialized non-generic class)`
+ error[lint:type-assertion-failure] tests/type_tests.py:49:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> _GeneratorContextManager[None]`
- Found 233 diagnostics
+ Found 234 diagnostics

com2ann (https://github.com/ilevkivskyi/com2ann)
+ error[lint:invalid-argument-type] src/com2ann.py:667:23: Argument to this function is incorrect: Expected `str`, found `str | None`
- Found 9 diagnostics
+ Found 10 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[lint:unsupported-operator] mypy_primer/main.py:168:23: Operator `*` is unsupported between objects of type `list` and `int | None`
+ error[lint:unsupported-operator] mypy_primer/main.py:168:23: Operator `*` is unsupported between objects of type `list[Unknown]` and `int | None`
+ error[lint:no-matching-overload] mypy_primer/utils.py:33:12: No overload of bound method `__init__` matches arguments
- Found 15 diagnostics
+ Found 16 diagnostics

parso (https://github.com/davidhalter/parso)
- error[lint:invalid-parameter-default] parso/grammar.py:53:15: Default value of type `None` is not assignable to annotated parameter type `PathLike | str`
+ error[lint:invalid-parameter-default] parso/grammar.py:53:15: Default value of type `None` is not assignable to annotated parameter type `PathLike[Unknown] | str`
- error[lint:invalid-parameter-default] parso/grammar.py:57:15: Default value of type `None` is not assignable to annotated parameter type `PathLike | str`
+ error[lint:invalid-parameter-default] parso/grammar.py:57:15: Default value of type `None` is not assignable to annotated parameter type `PathLike[Unknown] | str`
- error[lint:invalid-argument-type] parso/python/diff.py:884:35: Argument to this function is incorrect: Expected `tuple[int, int]`, found `tuple`
+ error[lint:invalid-argument-type] parso/python/diff.py:884:35: Argument to this function is incorrect: Expected `tuple[int, int]`, found `tuple[Unknown, ...]`
+ error[lint:no-matching-overload] parso/python/errors.py:650:22: No overload of bound method `__init__` matches arguments
- error[lint:invalid-argument-type] parso/python/tokenize.py:230:42: Argument to this function is incorrect: Expected `tuple[str]`, found `set`
+ error[lint:invalid-argument-type] parso/python/tokenize.py:230:42: Argument to this function is incorrect: Expected `tuple[str]`, found `set[Unknown]`
- error[lint:invalid-parameter-default] parso/python/tokenize.py:367:5: Default value of type `None` is not assignable to annotated parameter type `list`
+ error[lint:invalid-parameter-default] parso/python/tokenize.py:367:5: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
- Found 82 diagnostics
+ Found 83 diagnostics

paroxython (https://github.com/laowantong/paroxython)
+ error[lint:no-matching-overload] paroxython/map_taxonomy.py:168:47: No overload of function `dirname` matches arguments
+ error[lint:no-matching-overload] paroxython/parse_program.py:79:26: No overload of function `dirname` matches arguments
- Found 15 diagnostics
+ Found 17 diagnostics

beartype (https://github.com/beartype/beartype)
- error[lint:unsupported-operator] beartype/_cave/_cavemap.py:144:18: Operator `in` is not supported for types `type` and `str`, in comparing `type` with `(str & tuple & ~type & ~AlwaysFalsy) | (@Todo(Inference of subscript on special form) & tuple & ~type & ~AlwaysFalsy)`
+ error[lint:unsupported-operator] beartype/_cave/_cavemap.py:144:18: Operator `in` is not supported for types `type` and `str`, in comparing `type` with `(str & tuple[Unknown, ...] & ~type & ~AlwaysFalsy) | (@Todo(Inference of subscript on special form) & tuple[Unknown, ...] & ~type & ~AlwaysFalsy)`
- warning[lint:unused-ignore-comment] beartype/_check/error/_pep/pep484585/errpep484585mapping.py:120:59: Unused blanket `type: ignore` directive
+ error[lint:invalid-type-form] beartype/_data/hint/datahintpep.py:161:26: Variable of type `_SpecialForm` is not allowed in a type expression
+ error[lint:invalid-type-form] beartype/_data/hint/datahintpep.py:175:26: Variable of type `_SpecialForm` is not allowed in a type expression
+ error[lint:no-matching-overload] beartype/_util/error/utilerrwarn.py:65:14: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] beartype/_util/error/utilerrwarn.py:87:39: No overload of function `dirname` matches arguments
+ error[lint:unsupported-operator] beartype/claw/_clawstate.py:214:16: Operator `not in` is not supported for types `str` and `_S`, in comparing `Literal["."]` with `Unknown | _S`
+ error[lint:invalid-argument-type] beartype/claw/_importlib/clawimppath.py:142:23: Argument to this function is incorrect: Expected `(str, /) -> PathEntryFinderProtocol`, found `Unknown | None`
- Found 551 diagnostics
+ Found 556 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- error[lint:call-non-callable] nion/utils/Geometry.py:266:19: Object of type `GenericAlias` is not callable
- error[lint:call-non-callable] nion/utils/Geometry.py:267:26: Object of type `GenericAlias` is not callable
- error[lint:call-non-callable] nion/utils/ListModel.py:292:31: Object of type `GenericAlias` is not callable
- error[lint:call-non-callable] nion/utils/ListModel.py:293:24: Object of type `GenericAlias` is not callable
+ error[lint:invalid-argument-type] nion/utils/Stream.py:550:53: Argument to this function is incorrect: Expected `Queue[ValueChange[Unknown]]`, found `Queue[ValueChange[T]]`
- Found 25 diagnostics
+ Found 22 diagnostics

git-revise (https://github.com/mystor/git-revise)
+ warning[lint:possibly-unbound-attribute] gitrevise/merge.py:90:20: Attribute `decode` on type `Unknown | _S` is possibly unbound
- Found 10 diagnostics
+ Found 11 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:unsupported-operator] pyinstrument/__main__.py:52:31: Operator `+` is unsupported between objects of type `@Todo(specialized non-generic class) | None` and `@Todo(specialized non-generic class) | None`
+ error[lint:unsupported-operator] pyinstrument/__main__.py:52:31: Operator `+` is unsupported between objects of type `list[str] | None` and `list[str] | None`
- warning[lint:call-possibly-unbound-method] pyinstrument/__main__.py:53:9: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] pyinstrument/__main__.py:53:9: Method `__getitem__` of type `list[str] | None` is possibly unbound
- warning[lint:call-possibly-unbound-method] pyinstrument/__main__.py:54:9: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] pyinstrument/__main__.py:54:9: Method `__getitem__` of type `list[str] | None` is possibly unbound
+ error[lint:unsupported-operator] pyinstrument/__main__.py:342:20: Operator `+` is unsupported between objects of type `list[Unknown]` and `Unknown | list[str]`
- error[lint:not-iterable] pyinstrument/__main__.py:487:32: Object of type `@Todo(specialized non-generic class) | None` may not be iterable
+ error[lint:not-iterable] pyinstrument/__main__.py:487:32: Object of type `list[str] | None` may not be iterable
+ error[lint:no-matching-overload] pyinstrument/__main__.py:612:5: No overload of bound method `sort` matches arguments
+ error[lint:unsupported-operator] pyinstrument/magic/magic.py:315:13: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `str | None`
+ error[lint:unsupported-operator] pyinstrument/processors.py:35:32: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["<frozen importlib._bootstrap"]` with `str | None`
+ error[lint:no-matching-overload] pyinstrument/renderers/console.py:154:27: No overload of function `split` matches arguments
+ error[lint:invalid-argument-type] pyinstrument/session.py:109:13: Argument to this function is incorrect: Expected `list[str]`, found `Unknown | None`
- Found 95 diagnostics
+ Found 101 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ error[lint:invalid-assignment] aioredis/client.py:3432:13: Object of type `(Sequence[@Todo(Inference of subscript on special form)] & ~Mapping[Unknown, Unknown]) | (Mapping[AnyKeyT, int | float] & ~Mapping[Unknown, Unknown])` is not assignable to `Sequence[Unknown] | AbstractSet[AnyKeyT]`
- error[lint:invalid-exception-caught] aioredis/connection.py:283:16: Cannot catch object of type `Unknown | tuple` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[lint:invalid-exception-caught] aioredis/connection.py:283:16: Cannot catch object of type `Unknown | tuple[Unknown, ...]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- error[lint:invalid-exception-caught] aioredis/connection.py:511:16: Cannot catch object of type `Unknown | tuple` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
+ error[lint:invalid-exception-caught] aioredis/connection.py:511:16: Cannot catch object of type `Unknown | tuple[Unknown, ...]` in an exception handler (must be a `BaseException` subclass or a tuple of `BaseException` subclasses)
- Found 33 diagnostics
+ Found 34 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
- warning[lint:unused-ignore-comment] pyp.py:597:30: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] pyp.py:598:34: Unused blanket `type: ignore` directive
+ error[lint:unresolved-attribute] pyp.py:669:25: Unresolved attribute `colno` on type `FrameSummary`.
- warning[lint:unused-ignore-comment] pyp.py:668:63: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] pyp.py:670:53: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] pyp.py:672:62: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] pyp.py:673:53: Unused blanket `type: ignore` directive
- Found 17 diagnostics
+ Found 12 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
+ error[lint:unsupported-operator] src/pegen/python_generator.py:279:31: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["LOCATIONS"]` with `Unknown | str | None`
- Found 55 diagnostics
+ Found 56 diagnostics

bidict (https://github.com/jab/bidict)
+ error[lint:non-subscriptable] bidict/_typing.py:34:39: Cannot subscript object of type `<class 'Iterable[tuple[KT, VT]]'>` with no `__class_getitem__` method
- Found 14 diagnostics
+ Found 15 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- error[lint:unsupported-operator] more_itertools/more.pyi:162:18: Operator `|` is unsupported between objects of type `<class 'type'>` and `GenericAlias`
+ error[lint:unsupported-operator] more_itertools/more.pyi:162:18: Operator `|` is unsupported between objects of type `<class 'type'>` and `<class 'tuple[@Todo(Generic tuple specializations), ...]'>`
- error[lint:inconsistent-mro] more_itertools/more.pyi:164:1: Cannot create a consistent method resolution order (MRO) for class `_SizedIterable` with bases list `[@Todo(`Protocol[]` subscript), <class 'Sized'>, <class 'Iterable'>]`
- error[lint:inconsistent-mro] more_itertools/more.pyi:167:1: Cannot create a consistent method resolution order (MRO) for class `_SizedReversible` with bases list `[@Todo(`Protocol[]` subscript), <class 'Sized'>, <class 'Reversible'>]`
+ error[lint:invalid-argument-type] more_itertools/recipes.py:675:29: Argument to this function is incorrect: Expected `Sequence[_T] | AbstractSet[_T]`, found `range`
- Found 52 diagnostics
+ Found 51 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[lint:invalid-return-type] chess/engine.py:2964:16: Return type does not match returned value: Expected `InfoDict | list`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2964:16: Return type does not match returned value: Expected `InfoDict | list[Unknown]`, found `_T`
- error[lint:invalid-return-type] chess/engine.py:3065:16: Return type does not match returned value: Expected `list`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:3065:16: Return type does not match returned value: Expected `list[Unknown]`, found `_T`
- warning[lint:unused-ignore-comment] chess/pgn.py:529:37: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] chess/pgn.py:533:77: Unused blanket `type: ignore` directive
- error[lint:call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
- error[lint:call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> list[Unknown]]` is not callable on object of type `list[Unknown]`
- warning[lint:unused-ignore-comment] chess/svg.py:446:70: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] chess/svg.py:448:33: Unused blanket `type: ignore` directive
- Found 62 diagnostics
+ Found 58 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[lint:unresolved-attribute] src/anyio/_backends/_asyncio.py:854:37: Type `(...) -> @Todo(specialized non-generic class)` has no attribute `__qualname__`
+ error[lint:unresolved-attribute] src/anyio/_backends/_asyncio.py:854:37: Type `(...) -> Awaitable[Any]` has no attribute `__qualname__`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:1220:38: Argument to this function is incorrect: Expected `tuple[str, int, int, int] | tuple[str, int]`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...]`
- warning[lint:unused-ignore-comment] src/anyio/_backends/_asyncio.py:1131:52: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] src/anyio/_backends/_asyncio.py:1132:53: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] src/anyio/_backends/_asyncio.py:1133:53: Unused blanket `type: ignore` directive
+ error[lint:call-non-callable] src/anyio/_backends/_asyncio.py:2223:49: Object of type `GenericAlias` is not callable
+ error[lint:call-non-callable] src/anyio/_backends/_asyncio.py:2223:49: Object of type `GenericAlias` is not callable
- error[lint:call-non-callable] src/anyio/_backends/_asyncio.py:2434:26: Object of type `GenericAlias` is not callable
- error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2469:41: Argument to this function is incorrect: Expected `tuple[Context, (...) -> Unknown, tuple, Future, CancelScope] | None`, found `tuple[Context, (...) -> T_Retval, @Todo(full tuple[...] support), Unknown, CancelScope | None]`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2469:41: Argument to this function is incorrect: Expected `tuple[Context, (...) -> Unknown, tuple[Unknown, ...], Future[Unknown], CancelScope] | None`, found `tuple[Context, (...) -> T_Retval, @Todo(full tuple[...] support), Future[T_Retval], CancelScope | None]`
+ error[lint:no-matching-overload] src/anyio/_backends/_asyncio.py:2590:19: No overload of bound method `create_connection` matches arguments
+ error[lint:no-matching-overload] src/anyio/_backends/_asyncio.py:2590:19: No overload of bound method `create_connection` matches arguments
+ error[lint:no-matching-overload] src/anyio/_backends/_asyncio.py:2590:19: No overload of bound method `create_connection` matches arguments
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2635:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2636:13: Argument to this function is incorrect: Expected `tuple[str, int] | str | None`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...] | None`
- warning[lint:unused-ignore-comment] src/anyio/_backends/_asyncio.py:2889:44: Unused blanket `type: ignore` directive
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2700:53: Argument to this function is incorrect: Expected `tuple[str, int] | tuple[str, int, int, int]`, found `Unknown | tuple[@Todo(Generic tuple specializations), ...]`
- error[lint:invalid-return-type] src/anyio/_backends/_trio.py:553:32: Function can implicitly return `None`, which is not assignable to return type `tuple[bytes, Unknown]`
+ error[lint:invalid-return-type] src/anyio/_backends/_trio.py:553:32: Function can implicitly return `None`, which is not assignable to return type `tuple[bytes, Unknown | tuple[@Todo(Generic tuple specializations), ...]]`
+ error[lint:invalid-return-type] src/anyio/_core/_contextmanagers.py:72:16: Return type does not match returned value: Expected `_T_co`, found `object`
+ warning[lint:possibly-unbound-attribute] src/anyio/_core/_contextmanagers.py:92:32: Attribute `__exit__` on type `AbstractContextManager[object, bool | None] | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/anyio/_core/_contextmanagers.py:184:38: Attribute `__aexit__` on type `AbstractAsyncContextManager[object, bool | None] | None` is possibly unbound
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:38:42: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:41:48: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:95:68: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:124:47: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:127:60: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
- error[lint:unsupported-operator] src/anyio/_core/_contextmanagers.py:189:46: Operator `|` is unsupported between objects of type `<class 'bool'>` and `None`
+ error[lint:no-matching-overload] src/anyio/_core/_sockets.py:271:12: No overload of function `fspath` matches arguments
+ error[lint:no-matching-overload] src/anyio/_core/_sockets.py:535:19: No overload of function `fspath` matches arguments
- error[lint:invalid-assignment] src/anyio/_core/_sockets.py:756:9: Not enough values to unpack: Expected 4
+ error[lint:invalid-argument-type] src/anyio/from_thread.py:236:35: Argument to this function is incorrect: Expected `T_Retval`, found `@Todo(generic `typing.Awaitable` type) | Awaitable[T_Retval] | T_Retval`
- error[lint:call-non-callable] src/anyio/pytest_plugin.py:210:27: Object of type `GenericAlias` is not callable
- Found 123 diagnostics
+ Found 129 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ error[lint:invalid-assignment] src/attr/_make.py:1559:5: Object of type `tuple[Unknown, ...]` is not assignable to `list[Unknown]`
- error[lint:invalid-assignment] src/attr/exceptions.py:20:5: Object of type `list` is not assignable to `tuple[str]`
+ error[lint:invalid-assignment] src/attr/exceptions.py:20:5: Object of type `list[Unknown]` is not assignable to `tuple[str]`
- error[lint:invalid-argument-type] tests/test_converters.py:350:21: Argument to this function is incorrect: Expected `str | int`, found `list`
+ error[lint:invalid-argument-type] tests/test_converters.py:350:21: Argument to this function is incorrect: Expected `str | int`, found `list[Unknown]`
- error[lint:invalid-argument-type] tests/test_next_gen.py:146:29: Argument to this function is incorrect: Expected `list`, found `Literal[1]`
+ error[lint:invalid-argument-type] tests/test_next_gen.py:146:29: Argument to this function is incorrect: Expected `list[Unknown]`, found `Literal[1]`
- error[lint:invalid-argument-type] tests/test_next_gen.py:146:48: Argument to this function is incorrect: Expected `list`, found `Literal[1]`
+ error[lint:invalid-argument-type] tests/test_next_gen.py:146:48: Argument to this function is incorrect: Expected `list[Unknown]`, found `Literal[1]`
- error[lint:invalid-argument-type] tests/test_next_gen.py:148:26: Argument to this function is incorrect: Expected `list`, found `Literal[-1]`
+ error[lint:invalid-argument-type] tests/test_next_gen.py:148:26: Argument to this function is incorrect: Expected `list[Unknown]`, found `Literal[-1]`
- Found 601 diagnostics
+ Found 602 diagnostics

kornia (https://github.com/kornia/kornia)
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:63: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:63: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:69: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:69: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:77: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:77: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:84: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/gaussian_illumination.py:162:84: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:61: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:61: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:67: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:120:67: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:67: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:67: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:73: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/linear_illumination.py:227:73: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:56: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:56: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:64: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple & ~float) | (tuple[int | float, int | float] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
+ error[lint:invalid-argument-type] kornia/augmentation/_2d/intensity/salt_pepper_noise.py:127:64: Argument to this function is incorrect: Expected `tuple[int | float, int | float]`, found `(int & tuple[Unknown, ...] & ~float) | (tuple[int | float, int | float] & tuple[Unknown, ...] & ~float) | tuple[float, float] | tuple[@Todo(map_with_boundness: intersections with negative contributions), @Todo(map_with_boundness: intersections with negative contributions)]`
- error[lint:not-iterable] kornia/augmentation/container/image.py:380:18: Object of type `dict[Unknown, Unknown] | list | None` may not be iterable
+ error[lint:not-iterable] kornia/augmentation/container/image.py:380:18: Object of type `dict[Unknown, Unknown] | list[Unknown] | None` may not be iterable
- error[lint:unsupported-operator] kornia/augmentation/container/image.py:382:10: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["output_size"]` with `dict[Unknown, Unknown] | list | None`
+ error[lint:unsupported-operator] kornia/augmentation/container/image.py:382:10: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["output_size"]` with `dict[Unknown, Unknown] | list[Unknown] | None`
- error[lint:call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict[Unknown, Unknown] | list | None`
+ error[lint:call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> list[Unknown]])` is not callable on object of type `dict[Unknown, Unknown] | list[Unknown] | None`
- error[lint:call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict[Unknown, Unknown] | list | None`
+ error[lint:call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, Any, Any], /) -> list[Unknown]])` is not callable on object of type `dict[Unknown, Unknown] | list[Unknown] | None`
- error[lint:invalid-return-type] kornia/augmentation/container/ops.py:50:16: Return type does not match returned value: Expected `dict[Unknown, Unknown]`, found `dict[Unknown, Unknown] | list | None`
+ error[lint:invalid-return-type] kornia/augmentation/container/ops.py:50:16: Return type does not match returned value: Expected `dict[Unknown, Unknown]`, found `dict[Unknown, Unknown] | list[Unknown] | None`
- error[lint:invalid-return-type] kornia/augmentation/container/ops.py:58:16: Return type does not match returned value: Expected `list`, found `dict[Unknown, Unknown] | list | None`
+ error[lint:invalid-return-type] kornia/augmentation/container/ops.py:58:16: Return type does not match returned value: Expected `list[Unknown]`, found `dict[Unknown, Unknown] | list[Unknown] | None`
- warning[lint:possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:51:8: Attribute `dim` on type `Unknown | int | float | tuple[int | float, int | float] | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:51:8: Attribute `dim` on type `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` is possibly unbound
+ error[lint:unsupported-operator] kornia/augmentation/utils/param_validation.py:52:12: Operator `<` is not supported for types `list[Unknown]` and `int`, in comparing `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` with `Literal[0]`
- warning[lint:possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:24: Attribute `repeat` on type `Unknown | int | float | tuple[int | float, int | float] | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:24: Attribute `repeat` on type `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:70: Attribute `device` on type `Unknown | int | float | tuple[int | float, int | float] | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:70: Attribute `device` on type `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:91: Attribute `dtype` on type `Unknown | int | float | tuple[int | float, int | float] | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/augmentation/utils/param_validation.py:59:91: Attribute `dtype` on type `Unknown | int | float | tuple[int | float, int | float] | list[Unknown]` is possibly unbound
+ error[lint:invalid-argument-type] kornia/contrib/models/tiny_vit.py:307:21: Argument to this function is incorrect: Expected `int | float`, found `Unknown | (int & ~list[Unknown]) | (float & ~list[Unknown]) | (list[int | float] & ~list[Unknown])`
- warning[lint:possibly-unbound-attribute] kornia/core/mixin/image_module.py:193:20: Attribute `detach` on type `Unknown | list | tuple[Unknown]` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/core/mixin/image_module.py:193:20: Attribute `detach` on type `Unknown | list[Unknown] | tuple[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/core/module.py:204:20: Attribute `detach` on type `Unknown | tuple[Unknown]` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/core/module.py:204:20: Attribute `detach` on type `Unknown | list[Unknown] | tuple[Unknown]` is possibly unbound
+ error[lint:invalid-argument-type] kornia/enhance/adjust.py:370:22: Argument to this function is incorrect: Expected `Iterable[object]`, found `bool | (bool & Unknown) | Unknown`
- error[lint:invalid-return-type] kornia/feature/dedode/transformer/dinov2.py:309:20: Return type does not match returned value: Expected `tuple[Unknown | tuple[Unknown]]`, found `tuple`
+ error[lint:invalid-return-type] kornia/feature/dedode/transformer/dinov2.py:309:20: Return type does not match returned value: Expected `tuple[Unknown | tuple[Unknown]]`, found `tuple[Unknown, ...]`
- error[lint:invalid-return-type] kornia/feature/dedode/transformer/dinov2.py:310:16: Return type does not match returned value: Expected `tuple[Unknown | tuple[Unknown]]`, found `tuple`
+ error[lint:invalid-return-type] kornia/feature/dedode/transformer/dinov2.py:310:16: Return type does not match returned value: Expected `tuple[Unknown | tuple[Unknown]]`, found `tuple[Unknown, ...]`
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:38: Attribute `shape` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:54: Attribute `device` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list[Unknown]` is possibly unbound
- warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list` is possibly unbound
+ warning[lint:possibly-unbound-attribute] kornia/utils/draw.py:379:71: Attribute `dtype` on type `Unknown | list[Unknown]` is possibly unbound
- Found 960 diagnostics
+ Found 963 diagnostics

kopf (https://github.com/nolar/kopf)
+ warning[lint:possibly-unbound-attribute] kopf/_cogs/structs/dicts.py:241:25: Attribute `obj` on type `(_T & ~None) | Iterable[_T]` is possibly unbound
+ error[lint:invalid-return-type] kopf/_core/actions/lifecycles.py:40:12: Return type does not match returned value: Expected `Sequence[Handler]`, found `list[_T] | list[Unknown]`
+ error[lint:invalid-argument-type] kopf/_core/actions/lifecycles.py:40:26: Argument to this function is incorrect: Expected `Sequence[_T]`, found `Sequence[Handler] & ~AlwaysFalsy`
+ error[lint:no-matching-overload] kopf/_core/actions/throttlers.py:62:17: No overload of function `next` matches arguments
+ warning[lint:possibly-unbound-attribute] kopf/_core/engines/admission.py:429:20: Attribute `check` on type `Selector | None` is possibly unbound
+ warning[lint:redundant-cast] kopf/_core/engines/posting.py:99:20: Value is already of type `Iterator[Body]`
+ warning[lint:redundant-cast] kopf/_core/engines/posting.py:112:20: Value is already of type `Iterator[Body]`
+ warning[lint:redundant-cast] kopf/_core/engines/posting.py:125:20: Value is already of type `Iterator[Body]`
+ warning[lint:redundant-cast] kopf/_core/engines/posting.py:143:20: Value is already of type `Iterator[Body]`
+ error[lint:call-non-callable] kopf/_core/intents/callbacks.py:260:20: Object of type `_FnT` is not callable
+ error[lint:call-non-callable] kopf/_core/intents/callbacks.py:266:20: Object of type `_FnT` is not callable
+ error[lint:call-non-callable] kopf/_core/intents/callbacks.py:272:24: Object of type `_FnT` is not callable
+ error[lint:call-non-callable] kopf/_core/intents/registries.py:470:16: Object of type `str` is not callable
+ error[lint:unsupported-operator] kopf/_core/reactor/observation.py:221:9: Operator `|` is unsupported between objects of type `Unknown | frozenset[Selector]` and `Unknown | frozenset[Selector]`
+ error[lint:unsupported-operator] kopf/_core/reactor/observation.py:227:9: Operator `|` is unsupported between objects of type `Unknown | frozenset[Selector]` and `Unknown | frozenset[Selector]`
+ error[lint:unsupported-operator] kopf/_core/reactor/processing.py:161:9: Operator `|` is unsupported between objects of type `Unknown | set[Unknown | tuple[@Todo(Generic tuple specializations), ...]]` and `Unknown | set[Unknown | tuple[@Todo(Generic tuple specializations), ...]]`
+ warning[lint:redundant-cast] kopf/_kits/hierarchies.py:40:16: Value is already of type `Iterator[Unknown]`
+ warning[lint:redundant-cast] kopf/_kits/hierarchies.py:77:16: Value is already of type `Iterator[Unknown]`
+ warning[lint:redundant-cast] kopf/_kits/hierarchies.py:119:16: Value is already of type `Iterator[Unknown]`
+ warning[lint:redundant-cast] kopf/_kits/hierarchies.py:171:16: Value is already of type `Iterator[Unknown]`
+ warning[lint:redundant-cast] kopf/_kits/hierarchies.py:224:16: Value is already of type `Iterator[Unknown]`
- warning[lint:redundant-cast] kopf/_kits/runner.py:186:16: Value is already of type `Unknown`
+ error[lint:unresolved-attribute] kopf/cli.py:35:35: Type `_EnumMemberT` has no attribute `name`
- Found 169 diagnostics
+ Found 190 diagnostics

alerta (https://github.com/alerta/alerta)
- error[lint:invalid-parameter-default] alerta/models/alert.py:413:29: Default value of type `None` is not assignable to annotated parameter type `list`
+ error[lint:invalid-parameter-default] alerta/models/alert.py:413:29: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
- error[lint:invalid-parameter-default] alerta/models/blackout.py:225:29: Default value of type `None` is not assignable to annotated parameter type `list`
+ error[lint:invalid-parameter-default] alerta/models/blackout.py:225:29: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
- error[lint:invalid-parameter-default] alerta/models/heartbeat.py:28:44: Default value of type `None` is not assignable to annotated parameter type `list`
+ error[lint:invalid-parameter-default] alerta/models/heartbeat.py:28:44: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
- error[lint:invalid-parameter-default] alerta/models/heartbeat.py:161:29: Default value of type `None` is not assignable to annotated parameter type `list`
+ error[lint:invalid-parameter-default] alerta/models/heartbeat.py:161:29: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
- error[lint:invalid-parameter-default] alerta/models/heartbeat.py:170:28: Default value of type `None` is not assignable to annotated parameter type `list`
+ error[lint:invalid-parameter-default] alerta/models/heartbeat.py:170:28: Default value of type `None` is not assignable to annotated parameter type `list[Unknown]`
+ error[lint:call-non-callable] tests/helpers/utils.py:25:24: Method `__getitem__` of type `bound method _Environ[str].__getitem__(key: str) -> str` is not callable on object of type `_Environ[str]`
- error[lint:invalid-argument-type] tests/test_scopes.py:30:17: Argument to this function is incorrect: Expected `list`, found `Unknown | None`
+ error[lint:invalid-argument-type] tests/test_scopes.py:30:17: Argument to this function is incorrect: Expected `list[Unknown]`, found `Unknown | None`
- Found 482 diagnostics
+ Found 483 diagnostics

starlette (https://github.com/encode/starlette)
+ error[lint:unsupported-operator] starlette/datastructures.py:262:21: Operator `+` is unsupported between objects of type `list[tuple[Unknown, Unknown]]` and `list[tuple[Unknown, Unknown]]`
+ error[lint:invalid-argument-type] starlette/middleware/__init__.py:17:26: `ParamSpec` is not a valid argument to subscripted `typing.Protocol`
+ error[lint:invalid-assignment] starlette/routing.py:65:5: Object of type `((Request, /) -> Awaitable[Response] | Response) | partial[Unknown]` is not assignable to `(Request, /) -> Awaitable[Response]`
+ error[lint:no-matching-overload] tests/test_formparsers.py:420:51: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:448:51: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:476:51: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:503:51: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:530:51: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:559:51: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:589:13: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:619:13: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:671:51: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_formparsers.py:710:13: No overload of bound method `__init__` matches arguments
- Found 171 diagnostics
+ Found 184 diagnostics

pybind11 (https://github.com/pybind/pybind11)
+ error[lint:unsupported-operator] pybind11/setup_helpers.py:155:27: Operator `+` is unsupported between objects of type `list[str]` and `list[str]`
+ warning[lint:call-possibly-unbound-method] tests/extra_python_package/test_files.py:311:31: Method `__getitem__` of type `Unknown | _S` is possibly unbound
+ error[lint:no-matching-overload] tests/test_pytypes.py:778:14: No overload of bound method `__init__` matches arguments
- Found 241 diagnostics
+ Found 244 diagnostics

graphql-core (https://github.com/graphql-python/graphql-core)
+ error[lint:invalid-argument-type] src/graphql/execution/collect_fields.py:190:42: Argument to this function is incorrect: Expected `SelectionSetNode`, found `SelectionSetNode | None`
- warning[lint:unused-ignore-comment] src/graphql/execution/execute.py:833:59: Unused blanket `type: ignore` directive
+ error[lint:invalid-argument-type] src/graphql/execution/execute.py:1802:25: Argument to this function is incorrect: Expected `ExecutionContext`, found `(list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])`
+ error[lint:call-non-callable] src/graphql/execution/execute.py:2071:33: Object of type `None` is not callable
+ error[lint:invalid-argument-type] src/graphql/execution/execute.py:2176:56: Argument to this function is incorrect: Expected `ExecutionContext`, found `(list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])`
+ error[lint:invalid-argument-type] src/graphql/execution/execute.py:2249:44: Argument to this function is incorrect: Expected `ExecutionContext`, found `(@Todo(map_with_boundness: intersections with negative contributions) & ~list[Unknown]) | (list[GraphQLError] & ~list[Unknown]) | (ExecutionContext & ~list[Unknown])`
+ error[lint:no-matching-overload] src/graphql/language/visitor.py:223:28: No overload of bound method `__init__` matches arguments
+ error[lint:unresolved-attribute] src/graphql/type/validate.py:119:42: Type `_EnumMemberT` has no attribute `value`
+ error[lint:unresolved-attribute] src/graphql/type/validate.py:122:49: Type `_EnumMemberT` has no attribute `QUERY`
+ error[lint:invalid-argument-type] src/graphql/type/validate.py:127:57: Argument to this function is incorrect: Expected `OperationType`, found `_EnumMemberT`
+ error[lint:unsupported-operator] src/graphql/type/validate.py:408:21: Operator `+` is unsupported between objects of type `list[NamedTypeNode]` and `list[NamedTypeNode]`
+ error[lint:invalid-argument-type] src/graphql/utilities/coerce_input_value.py:150:25: Argument to this function is incorrect: Expected `list[str | int]`, found `@Todo(map_with_boundness: intersections with negative contributions) | list[_T]`
+ error[lint:unresolved-attribute] src/graphql/utilities/extend_schema.py:246:43: Type `_EnumMemberT` has no attribute `value`
- warning[lint:call-possibly-unbound-method] src/graphql/utilities/introspection_from_schema.py:50:15: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] src/graphql/utilities/introspection_from_schema.py:50:15: Method `__getitem__` of type `list[GraphQLError] | None` is possibly unbound
- error[lint:invalid-argument-type] src/graphql/validation/validate.py:100:68: Argument to this function is incorrect: Expected `(GraphQLError, /) -> None`, found `bound method list.append(object: _T, /) -> None`
- warning[lint:call-possibly-unbound-method] tests/error/test_graphql_error.py:260:16: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] tests/error/test_graphql_error.py:260:16: Method `__getitem__` of type `list[str | int] | None` is possibly unbound
- warning[lint:call-possibly-unbound-method] tests/error/test_graphql_error.py:261:9: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] tests/error/test_graphql_error.py:261:9: Method `__getitem__` of type `list[str | int] | None` is possibly unbound
- warning[lint:call-possibly-unbound-method] tests/execution/test_resolve.py:316:17: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] tests/execution/test_resolve.py:316:17: Method `__getitem__` of type `list[GraphQLError] | None` is possibly unbound
- error[lint:invalid-assignment] tests/execution/test_union_interface.py:137:1: Object of type `list` is not assignable to attribute `progeny` on type `Cat | None`
+ error[lint:invalid-assignment] tests/execution/test_union_interface.py:137:1: Object of type `list[Unknown]` is not assignable to attribute `progeny` on type `Cat | None`
- error[lint:invalid-assignment] tests/execution/test_union_interface.py:141:1: Object of type `list` is not assignable to attribute `progeny` on type `Dog | None`
+ error[lint:invalid-assignment] tests/execution/test_union_interface.py:141:1: Object of type `list[Unknown]` is not assignable to attribute `progeny` on type `Dog | None`
+ error[lint:missing-argument] tests/type/test_definition.py:1288:17: No argument provided for required parameter `name` of function `__new__`
+ error[lint:missing-argument] tests/type/test_definition.py:1288:17: No argument provided for required parameter `name` of bound method `__init__`
- warning[lint:unused-ignore-comment] tests/type/test_directives.py:152:54: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] tests/utilities/test_ast_to_dict.py:28:43: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] tests/utilities/test_ast_to_dict.py:29:39: Unused blanket `type: ignore` directive
+ error[lint:invalid-argument-type] tests/utilities/test_strip_ignored_characters_fuzz.py:94:58: Argument to this function is incorrect: Expected `str`, found `Unknown | _T`
+ error[lint:invalid-argument-type] tests/utilities/test_strip_ignored_characters_fuzz.py:95:58: Argument to this function is incorrect: Expected `str`, found `Unknown | _T`
+ error[lint:invalid-argument-type] tests/utilities/test_strip_ignored_characters_fuzz.py:98:72: Argument to this function is incorrect: Expected `str`, found `Unknown | _T`
+ error[lint:invalid-argument-type] tests/utilities/test_strip_ignored_characters_fuzz.py:99:80: Argument to this function is incorrect: Expected `str`, found `Unknown | _T`
+ error[lint:unsupported-operator] tests/utilities/test_strip_ignored_characters_fuzz.py:101:28: Operator `+` is unsupported between objects of type `LiteralString` and `Unknown | _T`
+ error[lint:invalid-argument-type] tests/utilities/test_strip_ignored_characters_fuzz.py:101:70: Argument to this function is incorrect: Expected `str`, found `Unknown | _T`
+ error[lint:unsupported-operator] tests/utilities/test_strip_ignored_characters_fuzz.py:102:28: Operator `+` is unsupported between objects of type `Unknown | _T` and `LiteralString`
+ error[lint:invalid-argument-type] tests/utilities/test_strip_ignored_characters_fuzz.py:102:70: Argument to this function is incorrect: Expected `str`, found `Unknown | _T`
- Found 406 diagnostics
+ Found 423 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- warning[lint:unused-ignore-comment] strawberry/codegen/query_codegen.py:688:33: Unused blanket `type: ignore` directive
+ error[lint:invalid-argument-type] strawberry/experimental/pydantic/conversion.py:47:38: Argument to this function is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~None) | Unknown | None`
+ error[lint:invalid-argument-type] strawberry/experimental/pydantic/conversion.py:47:38: Argument to this function is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~None) | Unknown | None`
+ error[lint:invalid-argument-type] strawberry/experimental/pydantic/conversion.py:47:38: Argument to this function is incorrect: Expected `Iterable[Unknown]`, found `(Unknown & ~None) | Unknown | None`
+ error[lint:unsupported-operator] strawberry/experimental/pydantic/error_type.py:128:26: Operator `+` is unsupported between objects of type `list[Field[Unknown]]` and `list[Field[Unknown]]`
+ error[lint:invalid-argument-type] strawberry/experimental/pydantic/object_type.py:176:13: Argument to this function is incorrect: Expected `set[str]`, found `set[Unknown | _S] | set[Unknown]`
+ error[lint:invalid-argument-type] strawberry/experimental/pydantic/object_type.py:193:17: Argument to this function is incorrect: Expected `set[str]`, found `set[Unknown | _S] | set[Unknown]`
+ error[lint:unsupported-operator] strawberry/experimental/pydantic/object_type.py:207:26: Operator `+` is unsupported between objects of type `list[Field[Unknown]]` and `list[Field[Unknown]]`
- warning[lint:unused-ignore-comment] strawberry/experimental/pydantic/utils.py:39:66: Unused blanket `type: ignore` directive
+ warning[lint:possibly-unbound-attribute] strawberry/extensions/query_depth_limiter.py:175:25: Attribute `value` on type `NameNode | None` is possibly unbound
- warning[lint:unused-ignore-comment] strawberry/federation/schema.py:274:46: Unused blanket `type: ignore` directive
- error[lint:not-iterable] strawberry/http/__init__.py:21:52: Object of type `@Todo(specialized non-generic class) | None` may not be iterable
+ error[lint:not-iterable] strawberry/http/__init__.py:21:52: Object of type `list[GraphQLError] | None` may not be iterable
- warning[lint:unused-ignore-comment] strawberry/printer/printer.py:513:75: Unused blanket `type: ignore` directive
- error[lint:conflicting-declarations] strawberry/relay/types.py:863:21: Conflicting declared types for `edges`: @Todo(specialized non-generic class), @Todo(specialized non-generic class)
+ error[lint:conflicting-declarations] strawberry/relay/types.py:863:21: Conflicting declared types for `edges`: list[Edge[Unknown]], list[Edge[Unknown]]
- error[lint:conflicting-declarations] strawberry/relay/types.py:869:21: Conflicting declared types for `edges`: @Todo(specialized non-generic class), @Todo(specialized non-generic class)
+ error[lint:conflicting-declarations] strawberry/relay/types.py:869:21: Conflicting declared types for `edges`: list[Edge[Unknown]], list[Edge[Unknown]]
+ error[lint:unresolved-attribute] strawberry/schema/schema.py:254:25: Type `type | StrawberryType` has no attribute `__strawberry_definition__`
- error[lint:invalid-return-type] strawberry/schema/schema_converter.py:531:24: Return type does not match returned value: Expected `(Any, GraphQLResolveInfo, Unknown, /) -> @Todo(specialized non-generic class) | str | None`, found `((Any, GraphQLResolveInfo, Unknown, /) -> str) | None`
+ error[lint:invalid-return-type] strawberry/schema/schema_converter.py:531:24: Return type does not match returned value: Expected `(Any, GraphQLResolveInfo, Unknown, /) -> Awaitable[str | None] | str | None`, found `((Any, GraphQLResolveInfo, Unknown, /) -> str) | None`
- error[lint:not-iterable] strawberry/subscriptions/protocols/graphql_transport_ws/handlers.py:405:64: Object of type `@Todo(specialized non-generic class) | None` may not be iterable
+ error[lint:not-iterable] strawberry/subscriptions/protocols/graphql_transport_ws/handlers.py:405:64: Object of type `list[GraphQLError] | None` may not be iterable
- error[lint:not-iterable] strawberry/subscriptions/protocols/graphql_ws/handlers.py:225:42: Object of type `@Todo(specialized non-generic class) | None` may not be iterable
+ error[lint:not-iterable] strawberry/subscriptions/protocols/graphql_ws/handlers.py:225:42: Object of type `list[GraphQLError] | None` may not be iterable
+ error[lint:unresolved-attribute] strawberry/types/enum.py:120:22: Type `_EnumMemberT` has no attribute `value`
+ error[lint:unresolved-attribute] strawberry/types/enum.py:121:21: Type `_EnumMemberT` has no attribute `name`
- warning[lint:unused-ignore-comment] strawberry/types/field.py:85:63: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/types/field.py:91:49: Unused blanket `type: ignore` directive
+ warning[lint:possibly-unbound-attribute] strawberry/types/object_type.py:180:28: Attribute `wrapped_func` on type `StrawberryResolver[Unknown] | None` is possibly unbound
+ error[lint:invalid-assignment] strawberry/types/object_type.py:188:17: Object of type `@Todo(Type::Intersection.call()) | ((...) -> Unknown)` is not assignable to attribute `wrapped_func` on type `StrawberryResolver[Unknown] | None`
+ error[lint:invalid-assignment] strawberry/types/object_type.py:191:17: Object of type `MethodType` is not assignable to attribute `wrapped_func` on type `StrawberryResolver[Unknown] | None`
- Found 450 diagnostics
...*[Comment body truncated]*

---

_Comment by @AlexWaygood on 2025-05-04 15:07_

Okay, so all's good here except that we panic on 5 new ecosystem projects and emit a catastrophic number of new false-positive errors on real-world code. And [some benchmark regressions of up to 2%](https://codspeed.io/astral-sh/ruff/branches/alex%2Fgeneric-protocol-mro).

---

_Closed by @AlexWaygood on 2025-05-05 13:13_

---

_Reopened by @AlexWaygood on 2025-05-05 13:13_

---

_Comment by @AlexWaygood on 2025-05-05 13:49_

The five new primer crashes are not new bugs introduced by this PR. I minimized the new `packaging` panic to this snippet, which the `main` branch also panics on:

```py
from typing import Union

class GenericClass[T]: ...
class Foo: ...
Bar = GenericClass[Union["Bar", Foo]]
```

The reason why we have five new primer projects panicking on this branch is that we suddenly understand many more stdlib classes as being generic, because we previously did not understand that classes inheriting from subscripted protocols were generic.

Note that `pip`, `packaging` and `setuptools` are all "the same panic": `pip` and `setuptools` both vendore `packaging`, and I think we're just panicking on the vendored `packaging` code [here](https://github.com/pypa/packaging/blob/d0d5ad8687f666bea942d1ab4ee2feb5fa019d04/src/packaging/_parser.py#L44-L47) when checking any of them.

---

_Comment by @AlexWaygood on 2025-05-05 14:38_

Here is a minimal repro of one of the new false-positive diagnostics we're emitting on `parso`:

```py
from typing import Any

parser_cache: dict[str, Any] = {}

for path_to_item_map in parser_cache.values():
    reveal_type(path_to_item_map)  # revealed: `_VT_co`, but should be `Any`

    # error: Type `_VT_co` has no attribute `items`
    # revealed: Unknown
    reveal_type(path_to_item_map.items())
```

and here's an even more minimal repro that doesn't depend on the complicated typeshed stubs for `dict`, and that also reproduces on `main`:

```py
from typing import Any

class Baz[T]:
    x: T

class Bar[T]:
    def method(self) -> Baz[T]: ...

def test2(b: Bar[Any]):
    reveal_type(b.method().x)  # revealed: T, but should be Any!
```

So it does look like most of the new false-positive diagnostics are also pre-existing issues being exposed by this PR rather than new bugs being introduced by this PR.

---

_Comment by @AlexWaygood on 2025-05-05 18:22_

Rebasing this PR on top of https://github.com/astral-sh/ruff/pull/17865 has hugely improved the primer report (thanks so much for the quick fix @dcreager!!). There are still a bunch of new false-positive diagnostics however, many along the lines of this:

```diff
+ error[lint:invalid-parameter-default] mypy_primer/model.py:295:36: Default value of type `tuple[()]` is not assignable to annotated parameter type `Sequence[str]`
```

or this:

```diff
+ error[lint:invalid-return-type] nion/utils/ListModel.py:983:16: Return type does not match returned value: Expected `Sequence[Any]`, found `list[Unknown]`
```

These suggest to me that we might have some gaps in our assignability logic for generic instance types? `tuple[()]` should be considered assignable to `Sequence[str]`; `list[Unknown]` should be considered assignable to `Sequence[Any]`.

---

_Comment by @AlexWaygood on 2025-05-05 18:25_

And there are also a bunch of new false-positive diagnostics like this on generator functions: possibly we don't yet understand that all generator functions implicitly return an instance of `types.GeneratorType`?

```diff
+ error[lint:invalid-return-type] paroxython/label_programs.py:55:54: Function can implicitly return `None`, which is not assignable to return type `Iterator[Unknown]`
+ error[lint:invalid-return-type] paroxython/list_programs.py:99:55: Function can implicitly return `None`, which is not assignable to return type `Iterator[Program]`
+ error[lint:no-matching-overload] paroxython/map_taxonomy.py:168:47: No overload of function `dirname` matches arguments
+ error[lint:no-matching-overload] paroxython/parse_program.py:79:26: No overload of function `dirname` matches arguments
+ error[lint:invalid-return-type] paroxython/parse_program.py:84:6: Function can implicitly return `None`, which is not assignable to return type `Iterator[tuple[Unknown, Span]]`
```

---

_Comment by @AlexWaygood on 2025-05-05 18:30_

> These suggest to me that we might have some gaps in our assignability logic for generic instance types? `tuple[()]` should be considered assignable to `Sequence[str]`; `list[Unknown]` should be considered assignable to `Sequence[Any]`.

Here's a minimal repro for this that reproduces on `main`:

```py
from ty_extensions import static_assert, is_assignable_to

class Foo[T]:
    def method(self) -> T: ...

class Bar[T](Foo[T]): ...

def f(x: Foo[int]): ...

f(Bar[int]())  # Argument to this function is incorrect: Expected `Foo[int]`, found `Bar[int]`
static_assert(is_assignable_to(Bar[int], Foo[int]))  # static-assert-error
```

https://types.ruff.rs/a1117a09-4fad-4846-b5a3-07e7cb4464e0

---

_Comment by @carljm on 2025-05-05 18:32_

> possibly we don't yet understand that all generator functions implicitly return an instance of `types.GeneratorType`

We don't. Would be great to have an issue for this.

---

_Comment by @AlexWaygood on 2025-05-05 18:34_

> We don't. Would be great to have an issue for this.

I was going to try to fix the false positives as part of this PR, since we don't emit them on `main`. Possibly just with a quick hack, though.

---

_Comment by @carljm on 2025-05-05 18:42_

I think the proper fix here might not be very hard -- just need to identify functions containing `yield` (we can do this easily in semantic indexing) and then special-case their return type checks. Still might make more sense to do it in a separate PR and then rebase this on top of it?

---

_Comment by @AlexWaygood on 2025-05-05 18:45_

Sure, I can do that

---

_Comment by @AlexWaygood on 2025-05-05 19:03_

And in fact, I _can_ repro the generator-function thing on `main`, you just have to be careful not to use type arguments to anything in the return-type signature (because if you do, it gets inferred as `@Todo` prior to this PR, and everything is assignable to `Todo`):

```py
import types
import typing

def f() -> types.GeneratorType:  # error: [invalid-return-type]
    yield 42

def g() -> typing.Generator:  # error: [invalid-return-type]
    yield 42

def h() -> typing.Iterator:  # error: [invalid-return-type]
    yield 42

def i() -> typing.Iterable:  # error: [invalid-return-type]
    yield 42
```

the reason why this hasn't come up before now is that return-type annotations like that are just really rare in real life -- you'd almost always specify type arguments to any of those classes.

https://types.ruff.rs/a9ee8a58-c99a-481d-9a89-785179820cba

---

_Comment by @AlexWaygood on 2025-05-05 21:05_

> ```diff
> + error[lint:missing-argument] src/anyio/_core/_signals.py:12:6: No argument provided for required parameter `_ExitT_co` of class `AbstractContextManager`
> ```

This looks like we don't yet understand legacy TypeVars that use the `default` keyword argument to specify that it's okay to omit them when supplying type arguments to a generic class. I.e., this:

```py
from typing import TypeVar, Generic

T = TypeVar("T", default=int)

class Foo(Generic[T]):
    x: T
```

is equivalent to this with the new syntax:

```py
class Foo[T = int]:
    x: T
```

---

_Closed by @dcreager on 2025-05-05 21:17_

---

_Reopened by @AlexWaygood on 2025-05-05 21:21_

---

_Comment by @dcreager on 2025-05-05 21:58_

> This looks like we don't yet understand legacy TypeVars that use the `default` keyword argument to specify that it's okay to omit them when supplying type arguments to a generic class

We weren't handling this for PEP-695 typevars either!  Fixed in https://github.com/astral-sh/ruff/pull/17872

---

_Comment by @AlexWaygood on 2025-05-06 12:17_

> ```diff
> mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
> + error[lint:unresolved-attribute] mypy_protobuf/main.py:122:45: Type `_T` has no attribute `name`
> + error[lint:unresolved-attribute] mypy_protobuf/main.py:123:45: Type `_T` has no attribute `name`
> + error[lint:unresolved-attribute] mypy_protobuf/main.py:131:40: Type `_T` has no attribute `name`
> + error[lint:unresolved-attribute] mypy_protobuf/main.py:132:45: Type `_T` has no attribute `name`
> + error[lint:unresolved-attribute] mypy_protobuf/main.py:133:39: Type `_T` has no attribute `name`
> + error[lint:unresolved-attribute] mypy_protobuf/main.py:134:31: Type `_T` has no attribute `nested_type`
> + error[lint:unresolved-attribute] mypy_protobuf/main.py:135:28: Type `_T` has no attribute `enum_type`
> - Found 82 diagnostics
> + Found 89 diagnostics
> ```

Here's a minimal repro for these diagnostics that also repros on `main`:

```py
class Bar[T]:
    x: T

class Baz[T](Bar[T]): ...
class Spam[T](Baz[T]): ...

reveal_type(Spam[int]().x) # revealed: `T`, but should be `int`
```

https://play.ty.dev/fe18277e-de4b-4d16-92d3-3a7c467b5798

@dcreager this looks very similar to the issue you fixed in https://github.com/astral-sh/ruff/pull/17865?

---

_Comment by @AlexWaygood on 2025-05-06 17:46_

There are lots of primer hits like this:

```diff
+ error[lint:call-non-callable] scipy/stats/tests/test_multivariate.py:121:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_multivariate.py:171:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_multivariate.py:2971:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_multivariate.py:4369:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_qmc.py:542:17: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_qmc.py:730:9: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_qmc.py:733:9: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_qmc.py:736:9: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_qmc.py:890:9: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_qmc.py:893:9: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_qmc.py:896:9: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_quantile.py:132:17: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_quantile.py:134:17: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_quantile.py:137:17: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_quantile.py:194:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_resampling.py:585:9: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_sampling.py:383:13: Object of type `_WithException[Unknown, Literal[XFailed]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_sampling.py:649:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_sampling.py:659:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_sampling.py:840:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
+ error[lint:call-non-callable] scipy/stats/tests/test_sampling.py:842:13: Object of type `_WithException[Unknown, Literal[Skipped]]` is not callable
```

But... I think our behaviour is correct for these! They're all to do with the fact that we don't think the object `pytest.skip` is callable on this PR branch, and it's obviously very common in tests for `pytest.skip()` to be called in order to skip a test.

Why don't we think `pytest.skip()` is callable on this branch? Well, `skip()` is declared [here](https://github.com/pytest-dev/pytest/blob/89b84cb56295c46e1d8834b599e858bc65c15a5b/src/_pytest/outcomes.py#L125-L130), and we can see that it's decorated with `@_with_exception()`. `_with_exception` is declared [here](https://github.com/pytest-dev/pytest/blob/89b84cb56295c46e1d8834b599e858bc65c15a5b/src/_pytest/outcomes.py#L92-L98), and the TL;DR is that `pytest.skip` is inferred as being an instance of this protocol:

```py
class _WithException(Protocol[_F, _ET]):
    Exception: _ET
    __call__: _F
```

Where `_F` is

```py
_F = TypeVar("_F", bound=Callable[..., object])
```

So this actually has nothing to do with generics, or in fact protocols. It has to do with our behaviour (which we're consistent about) with how dunders are looked up on instances. Namely, it's insufficient for an object to have a `__call__` instance attribute in order for it to be callable -- the `__call__` attribute must be accessible on the class object. And according to this protocol definition, that's not necessarily the case for instances of `_WithException`; ergo, `pytest.skip`, which is annotated as being an instance of `_WithException`, is not callable.

Justifying the theoretical basis for these diagnostics is one thing; issuing a huge number of diagnostics for users of Python's dominant testing library is another. It's a bit concerning that we'll emit so many diagnostics for users of pytest.

---

_Comment by @dcreager on 2025-05-06 18:26_

> Here's a minimal repro for these diagnostics that also repros on `main`:

Fixed in #17892 

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:89 on 2025-05-06 18:29_

question: should `Generic` appear in the MRO at all?  It doesn't at runtime:

```pycon
$ python
Python 3.13.3 (main, Apr  9 2025, 07:44:25) [GCC 14.2.1 20250207] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> from collections import Counter
>>> Counter[int].__mro__
(<class 'collections.Counter'>, <class 'dict'>, <class 'object'>)
```

And we don't need it to detect legacy generic classes — for that we look in the _base class_ list, so we could filter it out when constructing the MRO.

---

_@dcreager reviewed on 2025-05-06 18:29_

---

_Comment by @AlexWaygood on 2025-05-06 18:30_

> ```diff
> + error[lint:invalid-type-form] beartype/_data/hint/datahintpep.py:161:26: Variable of type `_SpecialForm` is not allowed in a type expression
> + error[lint:invalid-type-form] beartype/_data/hint/datahintpep.py:175:26: Variable of type `_SpecialForm` is not allowed in a type expression
> ```

These are expected: beartype is using `typing_extensions.TypeForm`, which we don't support yet (and isn't standardised yet)

Edit: this one also looks like it falls into the bucket of "beartype doing weird stuff":

```diff
+ error[lint:unresolved-attribute] beartype/_data/hint/pep/datapeprepr.py:331:36: Type `_T_co` has no attribute `name`
```

---

_@dcreager reviewed on 2025-05-06 18:32_

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:89 on 2025-05-06 18:32_

Protocols arguably don't need to be there either, since we have to use structural checks to determine if a class implements a protocol.  (Though we presumably need them in the MRO of _other protocols_ so that we can synthesize the union of the methods correctly?)

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:89 on 2025-05-06 18:33_

> It doesn't at runtime

That's correct, but the MRO that typeshed gives for `Counter` is very different to the MRO that `Counter` actually has at runtime. [We can debate](https://github.com/python/typeshed/issues/1595) the pros and cons of that, but we should definitely just be trusting typeshed here and not trying to "fix things up" on our end. And according to typeshed, `Counter` is a class that has `Protocol` in its MRO, which means that it must also have `Generic` in its MRO.

---

_@AlexWaygood reviewed on 2025-05-06 18:33_

---

_@AlexWaygood reviewed on 2025-05-06 18:36_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:89 on 2025-05-06 18:36_

> (Though we presumably need them in the MRO of _other protocols_ so that we can synthesize the union of the methods correctly?)

Uhmm, a class is only a protocol class if it has `Protocol` directly in its bases (it's not sufficient for `Protocol` to be somewhere in its MRO. So I don't think that, in itself, is a good reason for having `Protocol` be included in MROs of classes that inherit from `Protocol`

---

_Review comment by @dcreager on `crates/ty_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:89 on 2025-05-06 18:42_

Ah yes, I thought `Generic` was being excluded at runtime becuase the interpreter was actively removing it, not because it isn't actually listed as a base!

```pycon
>>> from typing import Protocol, TypeVar
>>> T = TypeVar("T")
>>> class Foo(Protocol[T]):
...     pass
...
>>> Foo.__mro__
(<class '__main__.Foo'>, <class 'typing.Protocol'>, <class 'typing.Generic'>, <class 'object'>)
```

---

_@dcreager reviewed on 2025-05-06 18:42_

---

_Comment by @dcreager on 2025-05-07 01:49_

I think a lot of the new false positives will go away with https://github.com/astral-sh/ruff/pull/17897

---

_Comment by @AlexWaygood on 2025-05-07 10:17_

@dcreager I'm not sure https://github.com/astral-sh/ruff/pull/17897 fixes all the issues... I still see lots of lines like this in the primer report here after rebasing on top of #17897:

> ```diff
> + error[lint:invalid-return-type] nion/utils/ListModel.py:33:60: Return type does not match returned value: Expected `Sequence[Any]`, found `list[Unknown]`
> ```

or like this:

> ```diff
> + error[lint:invalid-return-type] src/pegen/grammar.py:150:16: Return type does not match returned value: Expected `AbstractSet[str]`, found `set[Unknown]`
> ```

but `list[Unknown]` should be considered assignable to `Sequence[Any]`, and `set[Unknown]` should be considered assignable to `AbstractSet[str]`

---

_Comment by @dcreager on 2025-05-07 12:47_

> but `list[Unknown]` should be considered assignable to `Sequence[Any]`, and `set[Unknown]` should be considered assignable to `AbstractSet[str]`

I think I see the issue for this:

https://github.com/astral-sh/ruff/blob/0d9b6a097574e49f40f31758b1b8af420da458c2/crates/ty_python_semantic/src/types/class.rs#L293-L294

It's the "same generic class" part.  I decided to go ahead and merge #17897 first and work on a fix for this as a separate PR.

---

_Comment by @carljm on 2025-05-07 13:56_

> `list[Unknown]` should be considered assignable to `Sequence[Any]`

Maybe the same issue as in https://github.com/astral-sh/ruff/issues/17904 ?

---

_Comment by @dcreager on 2025-05-07 14:14_

> Maybe the same issue as in #17904 ?

Yes I think it is

---

_Comment by @AlexWaygood on 2025-05-07 22:10_

Wowzer. I think @dcreager's latest generics fix took many thousands of lines off the primer diff from this PR!

---

_Comment by @github-actions[bot] on 2025-05-07 22:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2025-05-07 22:13_

> ```diff
> pyp (https://github.com/hauntsaninja/pyp)
> - warning[lint:unused-ignore-comment] pyp.py:597:30: Unused blanket `type: ignore` directive
> - warning[lint:unused-ignore-comment] pyp.py:598:34: Unused blanket `type: ignore` directive
> + error[lint:unresolved-attribute] pyp.py:669:25: Unresolved attribute `colno` on type `FrameSummary`.
> ```

The new error here is weird. Want to look into it tomorrow.

---

_Comment by @AlexWaygood on 2025-05-07 22:15_

> ```diff
> isort (https://github.com/pycqa/isort)
> + error[lint:invalid-return-type] isort/api.py:516:6: Function can implicitly return `None`, which is not assignable to return type `Iterator[Import]`
> + error[lint:invalid-return-type] isort/api.py:585:6: Function can implicitly return `None`, which is not assignable to return type `Iterator[Import]`
> + error[lint:invalid-return-type] isort/api.py:617:6: Function can implicitly return `None`, which is not assignable to return type `Iterator[Import]`
> ```

Oops. We recognise functions that have `yield` expressions as being generator functions, but not yet functions that have `yield from` expressions in them. I can fix this real quick. (Edit: fixed in https://github.com/astral-sh/ruff/pull/17930)

---

_Comment by @AlexWaygood on 2025-05-07 22:16_

> ```diff
> + error[lint:unsupported-operator] strawberry/experimental/pydantic/error_type.py:128:26: Operator `+` is unsupported between objects of type `list[Field[Unknown]]` and `list[Field[Unknown]]`
> ```

@dcreager I think the new diagnostics we're now seeing on the tomllib benchmarks are similar to this -- any idea what's going on? The `list` class has an `__add__` method in typeshed

---

_@AlexWaygood reviewed on 2025-05-09 09:16_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/ty.rs`:1 on 2025-05-09 09:16_

The new tomllib diagnostics are:

```
error: lint:invalid-argument-type: Argument to this function is incorrect
   --> /Users/alexw/dev/tomllib/tomllib/_parser.py:270:33
    |
268 |     if char == "#":
269 |         return skip_until(
270 |             src, pos + 1, "\n", error_on=ILLEGAL_COMMENT_CHARS, error_on_eof=False
    |                                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Expected `frozenset[str]`, found `Unknown | frozenset[Unknown | _S]`
271 |         )
272 |     return pos
    |
info: Function defined here
   --> /Users/alexw/dev/tomllib/tomllib/_parser.py:241:5
    |
241 | def skip_until(
    |     ^^^^^^^^^^
242 |     src: str,
243 |     pos: Pos,
244 |     expect: str,
245 |     *,
246 |     error_on: frozenset[str],
    |     ------------------------ Parameter declared here
247 |     error_on_eof: bool,
248 | ) -> Pos:
    |
info: `lint:invalid-argument-type` is enabled by default

error: lint:invalid-argument-type: Argument to this function is incorrect
   --> /Users/alexw/dev/tomllib/tomllib/_parser.py:516:24
    |
514 |     start_pos = pos
515 |     pos = skip_until(
516 |         src, pos, "'", error_on=ILLEGAL_LITERAL_STR_CHARS, error_on_eof=True
    |                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Expected `frozenset[str]`, found `Unknown | frozenset[Unknown | _S]`
517 |     )
518 |     return pos + 1, src[start_pos:pos]  # Skip ending apostrophe
    |
info: Function defined here
   --> /Users/alexw/dev/tomllib/tomllib/_parser.py:241:5
    |
241 | def skip_until(
    |     ^^^^^^^^^^
242 |     src: str,
243 |     pos: Pos,
244 |     expect: str,
245 |     *,
246 |     error_on: frozenset[str],
    |     ------------------------ Parameter declared here
247 |     error_on_eof: bool,
248 | ) -> Pos:
    |
info: `lint:invalid-argument-type` is enabled by default

error: lint:invalid-argument-type: Argument to this function is incorrect
   --> /Users/alexw/dev/tomllib/tomllib/_parser.py:532:13
    |
530 |             pos,
531 |             "'''",
532 |             error_on=ILLEGAL_MULTILINE_LITERAL_STR_CHARS,
    |             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Expected `frozenset[str]`, found `Unknown | frozenset[Unknown | _S]`
533 |             error_on_eof=True,
534 |         )
    |
info: Function defined here
   --> /Users/alexw/dev/tomllib/tomllib/_parser.py:241:5
    |
241 | def skip_until(
    |     ^^^^^^^^^^
242 |     src: str,
243 |     pos: Pos,
244 |     expect: str,
245 |     *,
246 |     error_on: frozenset[str],
    |     ------------------------ Parameter declared here
247 |     error_on_eof: bool,
248 | ) -> Pos:
    |
info: `lint:invalid-argument-type` is enabled by default

Found 3 diagnostics
```

I wouldn't expect us to be able to infer `frozenset[str]` for [the ILLEGAL_COMMENT_CHARS frozenset](https://github.com/python/cpython/blob/f52de8a937e89a4d1cf314f12ee5e7bbaa79e7da/Lib/tomllib/_parser.py#L35) just yet (we haven't implemented inferring generator expressions as evaluating to generic `types.GeneratorType` instances yet). But I also wouldn't expect us to infer the frozenset as `frozenset[Unknown | _S]` -- shouldn't it just be `frozenset[Unknown]` if we can't infer what the TypeVar should be solved as? Cc. @dcreager

---

_@AlexWaygood reviewed on 2025-05-09 11:34_

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/ty.rs`:1 on 2025-05-09 11:34_

This is pretty interesting. The bug appears to be specific to legacy generics:

```py
from typing import Any, TypeVar, reveal_type, AbstractSet

_T_co = TypeVar("_T_co", covariant=True)
_S = TypeVar("_S")

class Foo(AbstractSet[_T_co]):
    def __or__(self, value: AbstractSet[_S], /) -> "Foo[_T_co | _S]": ...

def f(x: Foo[Any], y: Foo[Any]):
    reveal_type(x | y)  # revealed: `Foo[Any | _S]`

class Bar[T](AbstractSet[T]):
    def __or__[S](self, value: AbstractSet[S], /) -> "Bar[T | S]": ...

def g(x: Bar[Any], y: Bar[Any]):
    reveal_type(x | y)  # revealed: `Bar[Any]`
```

But `Foo` and `Bar` should be equivalent classes here

---

_Review comment by @AlexWaygood on `crates/ruff_benchmark/benches/ty.rs`:1 on 2025-05-09 11:45_

Okay, here is (finally!) a minimal repro for the new `tomllib` diagnostics, that repros on `main`:

```py
from __future__ import annotations
from typing import TypeVar, Any, reveal_type

S = TypeVar("S")

class Foo[T]:
    def method(self, other: Foo[S]) -> Foo[T | S]: ...  # type: ignore[invalid-return-type]

def f(x: Foo[Any], y: Foo[Any]):
    reveal_type(x.method(y))  # revealed: `Foo[Any | S]`, but should be `Foo[Any]`
```

https://play.ty.dev/24de2879-e71b-45d5-a80b-3aa3248d1022

---

_@AlexWaygood reviewed on 2025-05-09 11:45_

---

_@dcreager reviewed on 2025-05-09 15:15_

---

_Review comment by @dcreager on `crates/ruff_benchmark/benches/ty.rs`:1 on 2025-05-09 15:15_

Thanks for the minimal repro, I can take a look!  That's strange that the PEP-695 equivalent works correctly.

---

_Merged by @AlexWaygood on 2025-05-09 16:39_

---

_Closed by @AlexWaygood on 2025-05-09 16:39_

---

_Branch deleted on 2025-05-09 16:39_

---

_@dcreager reviewed on 2025-05-12 19:09_

---

_Review comment by @dcreager on `crates/ruff_benchmark/benches/ty.rs`:1 on 2025-05-12 19:09_

This should be fixed by https://github.com/astral-sh/ruff/pull/18052

---

_Comment by @karlicoss on 2025-05-26 23:28_

@AlexWaygood as a workaround for `"Object of type _WithException[Unknown, Literal[Skipped]] is not callable"` when using `pytest.skip` -- is it worth fixing the type in `pytest`? 

First through was wrapping `__call__` into a `ClassVar` here: https://github.com/pytest-dev/pytest/blob/89b84cb56295c46e1d8834b599e858bc65c15a5b/src/_pytest/outcomes.py#L89, but then I remembered `ClassVar` can't be generic -- filed an issue here https://github.com/astral-sh/ty/issues/518 . So ClassVar wouldn't work.

Next I tried messing with `_WithException`'s definition -- and seems like `def __call__` is treated differently by than `__call__` as an attribute.


```python
from collections.abc import Callable
from typing import Protocol, reveal_type

class WithCallBad(Protocol):
    __call__: Callable[..., str]


class WithCallGood(Protocol):
    def __call__(self, *args, **kwargs) -> str: ...


def check(c: WithCallBad) -> str:
    reveal_type(c)
    res = c()
    reveal_type(res)
    return res
```

`c: WithCallBad` results in `Object of type WithCallBad is not callable`, whereas `c: WithCallGood` works as expected and infers `res` type correctly.

Is that part of `Protocol`'s spec? I.e. definiting a dunder as method means it's accessible from the class object? 

If that works, we could use `ParamSpec` to type it properly in pytest as `def __call__`, I gave it a go [here](https://github.com/pytest-dev/pytest/commit/837097131fd8d8d96b98b6b9ef7112162946867d), and for mypy it gives identical results of `reveal_type(pytest.skip)`.
For `ty`, seems like `ParamSpec` isn't supported yet: https://github.com/astral-sh/ty/issues/157 , but hopefully it will be!

Of course doesn't solve the bigger issue discussed [here](https://discuss.python.org/t/when-should-we-assume-callable-types-are-method-descriptors/92938), but I tried `ty` on 10+ of my repos, and so far only `pytest.skip` resulted in that sort of issue, so perhaps it will help many people. 
If you think it makes sense, happy to follow up with `pytest`.


---

_Comment by @AlexWaygood on 2025-05-27 11:06_

> Is that part of `Protocol`'s spec? I.e. definiting a dunder as method means it's accessible from the class object?

Attributes are looked up on instances of protocol classes in the same way as they're looked up on instances of nominal classes! Any implicit dunder call in Python always looks up the dunder on the class rather than on the instance. If `__call__` is only annotated as existing on instances, the type checker cannot assume that it will exist on the class object. But a method defined directly in a class namespace is of course accessible from the class object.

> If that works, we could use `ParamSpec` to type it properly in pytest as `def __call__`, I gave it a go [here](https://github.com/pytest-dev/pytest/commit/837097131fd8d8d96b98b6b9ef7112162946867d), and for mypy it gives identical results of `reveal_type(pytest.skip)`.

Yeah, that looks like a less ambiguous way of annotating `pytest.skip` to me; I think that would be a worthwhile change to pytest's annotations. But I'm pretty sure that pytest isn't the only package that does this kind of thing with callable types, so fixing this one symptom won't fix the broader problem.

My only quibble with https://github.com/pytest-dev/pytest/commit/837097131fd8d8d96b98b6b9ef7112162946867d is that I think the `_R` type variable should be covariant ;)

---

_Comment by @karlicoss on 2025-06-27 16:29_

Btw the pytest.skip fix is merged, so should be in the next release! https://github.com/pytest-dev/pytest/pull/13445

---
