```yaml
number: 17721
title: "[red-knot] Legacy generic classes"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/legacy-class
created_at: 2025-04-29T20:03:41Z
updated_at: 2025-05-03T15:43:22Z
url: https://github.com/astral-sh/ruff/pull/17721
synced_at: 2026-01-12T15:56:04Z
```

# [red-knot] Legacy generic classes

---

_@dcreager_

This adds support for legacy generic classes, which use a `typing.Generic` base class, or which inherit from another generic class that has been specialized with legacy typevars.


---

_Comment by @github-actions[bot] on 2025-04-29 20:06_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
dacite (https://github.com/konradhalas/dacite)
- warning[lint:unused-ignore-comment] dacite/cache.py:11:71: Unused blanket `type: ignore` directive
+ error[lint:invalid-assignment] dacite/core.py:48:5: Object of type `dict[Unknown, Unknown]` is not assignable to `MutableMapping[str, Any]`
+ error[lint:invalid-assignment] dacite/core.py:49:5: Object of type `dict[Unknown, Unknown]` is not assignable to `MutableMapping[str, Any]`
- Found 24 diagnostics
+ Found 25 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
+ error[lint:invalid-return-type] src/async_utils/bg_loop.py:64:16: Return type does not match returned value: Expected `Future[T]`, found `Future[_T]`
+ error[lint:invalid-argument-type] src/async_utils/bg_loop.py:165:67: Argument to this function is incorrect: Expected `Mapping[str, Any] | None`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-assignment] src/async_utils/priority_sem.py:31:1: Object of type `ContextVar[Literal[0]]` is not assignable to `ContextVar[int]`
- Found 22 diagnostics
+ Found 25 diagnostics

paroxython (https://github.com/laowantong/paroxython)
- error[lint:invalid-assignment] paroxython/derived_labels_db.py:180:9: Object of type `defaultdict` is not assignable to `dict`
+ error[lint:invalid-assignment] paroxython/derived_labels_db.py:180:9: Object of type `defaultdict[Unknown, Unknown]` is not assignable to `dict[Unknown, Unknown]`
- error[lint:invalid-assignment] paroxython/filter_programs.py:439:13: Object of type `defaultdict` is not assignable to `dict`
+ error[lint:invalid-assignment] paroxython/filter_programs.py:439:13: Object of type `defaultdict[Unknown, Unknown]` is not assignable to `dict[Unknown, Unknown]`
- error[lint:invalid-assignment] paroxython/goodies.py:33:5: Object of type `defaultdict` is not assignable to `dict`
+ error[lint:invalid-assignment] paroxython/goodies.py:33:5: Object of type `defaultdict[Unknown, Unknown]` is not assignable to `dict[Unknown, Unknown]`
- error[lint:no-matching-overload] paroxython/map_taxonomy.py:241:26: No overload of bound method `__init__` matches arguments
- error[lint:invalid-assignment] paroxython/recommend_programs.py:259:9: Object of type `defaultdict` is not assignable to `dict`
+ error[lint:invalid-assignment] paroxython/recommend_programs.py:259:9: Object of type `defaultdict[Unknown, Unknown]` is not assignable to `dict[Unknown, Unknown]`
- Found 22 diagnostics
+ Found 21 diagnostics

packaging (https://github.com/pypa/packaging)
+ error[lint:invalid-assignment] src/packaging/_manylinux.py:77:1: Object of type `defaultdict[Unknown, Unknown]` is not assignable to `dict[int, int]`
+ error[lint:invalid-argument-type] src/packaging/licenses/__init__.py:100:54: Argument to this function is incorrect: Expected `Mapping[str, object] | None`, found `dict[str, Any]`
- error[lint:unsupported-operator] src/packaging/_tokenizer.py:39:26: Operator `|` is unsupported between objects of type `Literal[str]` and `GenericAlias`
- error[lint:unsupported-operator] src/packaging/_tokenizer.py:102:26: Operator `|` is unsupported between objects of type `Literal[str]` and `GenericAlias`
- error[lint:unsupported-operator] src/packaging/markers.py:217:49: Operator `|` is unsupported between objects of type `Literal[str]` and `Literal[AbstractSet]`
- error[lint:invalid-return-type] src/packaging/markers.py:259:12: Return type does not match returned value: Expected `Environment`, found `dict`
+ error[lint:invalid-return-type] src/packaging/markers.py:259:12: Return type does not match returned value: Expected `Environment`, found `dict[Unknown, Unknown]`
+ error[lint:no-matching-overload] src/packaging/metadata.py:306:18: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/packaging/metadata.py:308:18: No overload of bound method `__init__` matches arguments
- error[lint:unsupported-operator] src/packaging/markers.py:353:20: Operator `|` is unsupported between objects of type `Literal[str]` and `Literal[AbstractSet]`
- error[lint:unsupported-operator] src/packaging/markers.py:354:16: Operator `|` is unsupported between objects of type `Literal[str]` and `Literal[AbstractSet]`
- error[lint:unsupported-operator] src/packaging/metadata.py:302:20: Operator `|` is unsupported between objects of type `Literal[str]` and `GenericAlias`
- warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `str | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `_HeaderRegistryT | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
- warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `str | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `_HeaderRegistryT | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
- warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `str | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `_HeaderRegistryT | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
- warning[lint:redundant-cast] src/packaging/utils.py:51:12: Value is already of type `Unknown`
- Found 19 diagnostics
+ Found 16 diagnostics

com2ann (https://github.com/ilevkivskyi/com2ann)
+ error[lint:no-matching-overload] src/com2ann.py:422:13: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/com2ann.py:433:19: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/com2ann.py:471:13: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/com2ann.py:485:23: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/com2ann.py:605:15: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/com2ann.py:611:14: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/com2ann.py:616:25: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/com2ann.py:654:12: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/com2ann.py:713:19: No overload of function `search` matches arguments
- error[lint:invalid-assignment] src/test_cli.py:49:9: Object of type `dict` is not assignable to `ParseResult`
+ error[lint:invalid-assignment] src/test_cli.py:49:9: Object of type `dict[Unknown, Unknown]` is not assignable to `ParseResult`
+ error[lint:no-matching-overload] src/test_com2ann.py:78:29: No overload of function `search` matches arguments
+ error[lint:no-matching-overload] src/test_com2ann.py:80:30: No overload of function `search` matches arguments
- Found 9 diagnostics
+ Found 20 diagnostics

parso (https://github.com/davidhalter/parso)
+ error[lint:invalid-assignment] parso/pgen2/generator.py:264:5: Object of type `dict[Unknown, Unknown]` is not assignable to `Mapping[str, ReservedString]`
+ error[lint:no-matching-overload] parso/python/errors.py:650:22: No overload of bound method `__init__` matches arguments
- warning[lint:possibly-unbound-attribute] parso/python/pep8.py:719:31: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] parso/python/pep8.py:719:31: Attribute `group` on type `Match[str] | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] parso/python/prefix.py:84:19: Attribute `group` on type `Unknown | Match[AnyStr] | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] parso/python/prefix.py:85:17: Attribute `group` on type `Unknown | Match[AnyStr] | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] parso/python/prefix.py:96:17: Attribute `end` on type `Unknown | Match[AnyStr] | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] parso/python/tree.py:259:16: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] parso/python/tree.py:259:16: Attribute `group` on type `Match[str] | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] parso/python/tree.py:267:16: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] parso/python/tree.py:267:16: Attribute `group` on type `Match[str] | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] parso/python/tree.py:267:37: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] parso/python/tree.py:267:37: Attribute `group` on type `Match[str] | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] parso/utils.py:95:27: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] parso/utils.py:95:27: Attribute `group` on type `Match[bytes] | None` is possibly unbound
- Found 81 diagnostics
+ Found 86 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
- error[lint:call-non-callable] mypy_primer/globals.py:60:7: Object of type `GenericAlias` is not callable
+ error[lint:no-matching-overload] mypy_primer/main.py:147:40: No overload of function `search` matches arguments
+ error[lint:call-non-callable] mypy_primer/model.py:192:9: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/model.py:194:25: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/model.py:195:9: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/model.py:197:13: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/model.py:200:13: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/model.py:268:13: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/model.py:317:13: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/model.py:319:9: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/model.py:378:13: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:invalid-assignment] mypy_primer/model.py:539:9: Object of type `defaultdict[Unknown, Unknown]` is not assignable to `dict[str, int]`
+ error[lint:call-non-callable] mypy_primer/type_checker.py:49:13: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/type_checker.py:50:13: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/type_checker.py:116:9: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:call-non-callable] mypy_primer/type_checker.py:153:9: Method `__getitem__` of type `bound method dict[AnyStr, AnyStr].__getitem__(key: AnyStr, /) -> AnyStr` is not callable on object of type `dict[AnyStr, AnyStr]`
+ error[lint:invalid-return-type] mypy_primer/utils.py:114:12: Return type does not match returned value: Expected `tuple[CompletedProcess[str], int | float]`, found `tuple[CompletedProcess[@Todo(map_with_boundness: intersections with negative contributions) | None], int | float]`
- Found 16 diagnostics
+ Found 31 diagnostics

nionutils (https://github.com/nion-software/nionutils)
- error[lint:call-non-callable] nion/utils/ListModel.py:294:33: Object of type `GenericAlias` is not callable
+ error[lint:invalid-assignment] nion/utils/Stream.py:335:13: Object of type `ConstantStream[Observable]` is not assignable to `AbstractStream[Observable]`
+ error[lint:invalid-argument-type] nion/utils/Stream.py:335:67: Argument to this function is incorrect: Expected `Observable | None`, found `(Observable & ~AbstractStream[Unknown]) | (AbstractStream[Observable] & ~AbstractStream[Unknown])`
+ error[lint:invalid-argument-type] nion/utils/test/Model_test.py:43:49: Argument to this function is incorrect: Expected `AbstractStream[() -> T]`, found `ValueStream[() -> Unknown]`
+ error[lint:invalid-argument-type] nion/utils/test/Model_test.py:48:45: Argument to this function is incorrect: Expected `AbstractStream[T]`, found `ValueStream[Literal[0]]`
- Found 26 diagnostics
+ Found 29 diagnostics

bidict (https://github.com/jab/bidict)
+ error[lint:invalid-parameter-default] bidict/_base.py:423:9: Default value of type `MappingProxyType[Unknown, Unknown]` is not assignable to annotated parameter type `Mapping[str, VT]`
+ error[lint:invalid-return-type] bidict/_base.py:519:16: Return type does not match returned value: Expected `BT`, found `BidictBase[Any, Any]`
+ error[lint:invalid-return-type] bidict/_bidict.py:41:30: Function can implicitly return `None`, which is not assignable to return type `MutableBidict[VT, KT]`
+ error[lint:invalid-return-type] bidict/_bidict.py:44:26: Function can implicitly return `None`, which is not assignable to return type `MutableBidict[VT, KT]`
+ error[lint:invalid-return-type] bidict/_bidict.py:185:30: Function can implicitly return `None`, which is not assignable to return type `bidict[VT, KT]`
+ error[lint:invalid-return-type] bidict/_bidict.py:188:26: Function can implicitly return `None`, which is not assignable to return type `bidict[VT, KT]`
+ error[lint:invalid-return-type] bidict/_frozen.py:34:30: Function can implicitly return `None`, which is not assignable to return type `frozenbidict[VT, KT]`
+ error[lint:invalid-return-type] bidict/_frozen.py:37:26: Function can implicitly return `None`, which is not assignable to return type `frozenbidict[VT, KT]`
+ error[lint:invalid-return-type] bidict/_orderedbase.py:138:30: Function can implicitly return `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
+ error[lint:invalid-return-type] bidict/_orderedbase.py:141:26: Function can implicitly return `None`, which is not assignable to return type `OrderedBidictBase[VT, KT]`
+ error[lint:invalid-return-type] bidict/_orderedbidict.py:39:30: Function can implicitly return `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
+ error[lint:invalid-return-type] bidict/_orderedbidict.py:42:26: Function can implicitly return `None`, which is not assignable to return type `OrderedBidict[VT, KT]`
- Found 4 diagnostics
+ Found 16 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:invalid-argument-type] pyinstrument/__main__.py:323:76: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[lint:invalid-argument-type] pyinstrument/__main__.py:392:95: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[lint:invalid-argument-type] pyinstrument/__main__.py:513:47: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO`
+ error[lint:invalid-argument-type] pyinstrument/__main__.py:514:43: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO`
+ error[lint:invalid-argument-type] pyinstrument/context_manager.py:92:43: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[lint:invalid-argument-type] pyinstrument/context_manager.py:93:47: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `TextIO | @Todo(Support for `typing.TypeAlias`)`
+ error[lint:unresolved-attribute] pyinstrument/magic/magic.py:286:20: Type `_T` has no attribute `result`
+ error[lint:no-matching-overload] pyinstrument/processors.py:123:52: No overload of function `match` matches arguments
+ error[lint:no-matching-overload] pyinstrument/processors.py:124:52: No overload of function `match` matches arguments
+ error[lint:no-matching-overload] pyinstrument/processors.py:252:17: No overload of function `match` matches arguments
+ error[lint:no-matching-overload] pyinstrument/processors.py:268:18: No overload of function `match` matches arguments
+ error[lint:invalid-argument-type] pyinstrument/profiler.py:312:45: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `IO[str] | @Todo(Support for `typing.TypeAlias`)`
+ error[lint:invalid-argument-type] pyinstrument/profiler.py:314:41: Argument to this function is incorrect: Expected `IO[AnyStr]`, found `IO[str] | @Todo(Support for `typing.TypeAlias`)`
+ error[lint:invalid-argument-type] pyinstrument/stack_sampler.py:29:37: Argument to this function is incorrect: Expected `str`, found `_VT | Unknown`
+ error[lint:invalid-assignment] pyinstrument/stack_sampler.py:49:1: Object of type `ContextVar[None]` is not assignable to `ContextVar[object]`
+ error[lint:no-matching-overload] pyinstrument/vendor/decorator.py:223:16: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] pyinstrument/vendor/decorator.py:263:16: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] pyinstrument/vendor/decorator.py:287:13: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] pyinstrument/vendor/decorator.py:428:13: No overload of bound method `__init__` matches arguments
- Found 87 diagnostics
+ Found 106 diagnostics

beartype (https://github.com/beartype/beartype)
+ error[lint:no-matching-overload] beartype/_conf/confmain.py:846:27: No overload of bound method `__init__` matches arguments
+ error[lint:call-non-callable] beartype/_decor/_type/_pep/_decortypepep557.py:435:13: Object of type `None` is not callable
- error[lint:invalid-assignment] beartype/_decor/_type/decortype.py:298:1: Object of type `defaultdict` is not assignable to `dict`
+ error[lint:invalid-assignment] beartype/_decor/_type/decortype.py:298:1: Object of type `defaultdict[Unknown, Unknown]` is not assignable to `dict[Unknown, Unknown]`
+ error[lint:invalid-return-type] beartype/_util/api/standard/utilfunctools.py:319:12: Return type does not match returned value: Expected `BeartypeableT`, found `_lru_cache_wrapper[_T]`
+ error[lint:invalid-argument-type] beartype/_util/cache/utilcachecall.py:572:32: Argument to this function is incorrect: Expected `Mapping[str, object] | None`, found `dict[Unknown, Unknown]`
+ error[lint:no-matching-overload] beartype/_util/error/utilerrwarn.py:65:14: No overload of bound method `__init__` matches arguments
+ error[lint:invalid-argument-type] beartype/_util/func/utilfuncmake.py:273:48: Argument to this function is incorrect: Expected `Mapping[str, object] | None`, found `(Unknown & dict[Unknown, Unknown]) | dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] beartype/_util/hint/pep/proposal/pep585.py:508:29: Argument to this function is incorrect: Expected `MutableMapping[Unknown, Unknown]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] beartype/_util/kind/map/utilmapset.py:313:39: Argument to this function is incorrect: Expected `Mapping[Unknown, Unknown]`, found `MutableMapping[Unknown, Unknown]`
+ error[lint:invalid-argument-type] beartype/vale/_is/_valeisobj.py:294:17: Argument to this function is incorrect: Expected `MutableMapping[Unknown, Unknown]`, found `dict[Unknown, Unknown]`
- Found 568 diagnostics
+ Found 577 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
+ warning[lint:unused-ignore-comment] more_itertools/more.pyi:531:45: Unused blanket `type: ignore` directive
- error[lint:inconsistent-mro] more_itertools/more.pyi:189:1: Cannot create a consistent method resolution order (MRO) for class `peekable` with bases list `[@Todo(`Generic[]` subscript), <class 'Iterator'>]`
- error[lint:inconsistent-mro] more_itertools/more.pyi:242:1: Cannot create a consistent method resolution order (MRO) for class `bucket` with bases list `[@Todo(`Generic[]` subscript), <class 'Container'>]`
- error[lint:inconsistent-mro] more_itertools/more.pyi:554:1: Cannot create a consistent method resolution order (MRO) for class `islice_extended` with bases list `[@Todo(`Generic[]` subscript), <class 'Iterator'>]`
- error[lint:inconsistent-mro] more_itertools/more.pyi:576:1: Cannot create a consistent method resolution order (MRO) for class `SequenceView` with bases list `[@Todo(`Generic[]` subscript), <class 'Sequence'>]`
- error[lint:inconsistent-mro] more_itertools/more.pyi:584:1: Cannot create a consistent method resolution order (MRO) for class `seekable` with bases list `[@Todo(`Generic[]` subscript), <class 'Iterator'>]`
- error[lint:inconsistent-mro] more_itertools/more.pyi:662:1: Cannot create a consistent method resolution order (MRO) for class `time_limited` with bases list `[@Todo(`Generic[]` subscript), <class 'Iterator'>]`
- error[lint:inconsistent-mro] more_itertools/more.pyi:724:1: Cannot create a consistent method resolution order (MRO) for class `callback_iter` with bases list `[@Todo(`Generic[]` subscript), <class 'Iterator'>]`
- error[lint:inconsistent-mro] more_itertools/more.pyi:772:1: Cannot create a consistent method resolution order (MRO) for class `countable` with bases list `[@Todo(`Generic[]` subscript), <class 'Iterator'>]`
- Found 70 diagnostics
+ Found 63 diagnostics

python-chess (https://github.com/niklasf/python-chess)
+ error[lint:invalid-argument-type] chess/engine.py:71:25: Argument to this function is incorrect: Expected `Coroutine[Any, Any, _T]`, found `Coroutine[Any, Any, None]`
- error[lint:invalid-assignment] chess/engine.py:1763:5: Object of type `dict` is not assignable to `InfoDict`
+ error[lint:invalid-assignment] chess/engine.py:1763:5: Object of type `dict[Unknown, Unknown]` is not assignable to `InfoDict`
- error[lint:invalid-assignment] chess/engine.py:2572:5: Object of type `dict` is not assignable to `InfoDict`
+ error[lint:invalid-assignment] chess/engine.py:2572:5: Object of type `dict[Unknown, Unknown]` is not assignable to `InfoDict`
+ error[lint:invalid-return-type] chess/engine.py:2909:16: Return type does not match returned value: Expected `MutableMapping[str, Option]`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2916:16: Return type does not match returned value: Expected `Mapping[str, str]`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2922:16: Return type does not match returned value: Expected `T`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2928:16: Return type does not match returned value: Expected `None`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2936:16: Return type does not match returned value: Expected `None`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2942:16: Return type does not match returned value: Expected `None`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2950:16: Return type does not match returned value: Expected `PlayResult`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2964:16: Return type does not match returned value: Expected `InfoDict | list`, found `_T`
+ error[lint:invalid-argument-type] chess/engine.py:2972:43: Argument to this function is incorrect: Expected `AnalysisResult`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2978:16: Return type does not match returned value: Expected `None`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:2984:16: Return type does not match returned value: Expected `None`, found `_T`
+ error[lint:invalid-argument-type] chess/engine.py:3014:34: Argument to this function is incorrect: Expected `(Future[T], /) -> Coroutine[Any, Any, None]`, found `def background(future: Future[SimpleEngine]) -> @Todo(generic types.CoroutineType)`
+ error[lint:invalid-return-type] chess/engine.py:3058:16: Return type does not match returned value: Expected `InfoDict`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:3065:16: Return type does not match returned value: Expected `list`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:3074:16: Return type does not match returned value: Expected `BestMove`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:3079:16: Return type does not match returned value: Expected `bool`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:3084:16: Return type does not match returned value: Expected `bool`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:3089:16: Return type does not match returned value: Expected `InfoDict`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:3094:16: Return type does not match returned value: Expected `InfoDict | None`, found `_T`
+ error[lint:invalid-return-type] chess/engine.py:3105:20: Return type does not match returned value: Expected `InfoDict`, found `_T`
- error[lint:invalid-return-type] chess/gaviota.py:1670:16: Return type does not match returned value: Expected `BinaryIO`, found `(Unknown & ~None) | TextIOWrapper`
+ error[lint:invalid-return-type] chess/gaviota.py:1670:16: Return type does not match returned value: Expected `BinaryIO`, found `(Unknown & ~None) | TextIOWrapper[_WrappedBuffer]`
+ error[lint:invalid-argument-type] chess/gaviota.py:1892:35: Argument to this function is incorrect: Expected `int`, found `@Todo(Support for `typing.TypeAlias`) | None | Literal[64]`
- error[lint:call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:invalid-return-type] chess/svg.py:162:12: Return type does not match returned value: Expected `Element[str]`, found `Element[Literal["svg"]]`
+ error[lint:invalid-return-type] chess/svg.py:200:12: Return type does not match returned value: Expected `Element[str]`, found `Element[Literal["g"]]`
- Found 51 diagnostics
+ Found 75 diagnostics

attrs (https://github.com/python-attrs/attrs)
- error[lint:no-matching-overload] bench/test_benchmarks.py:55:22: No overload of function `field` matches arguments
+ error[lint:invalid-argument-type] src/attr/_make.py:256:38: Argument to this function is incorrect: Expected `Mapping[str, object] | None`, found `dict[Unknown, Unknown] | Mapping[str, object]`
+ error[lint:invalid-return-type] src/attr/_make.py:258:12: Return type does not match returned value: Expected `dict[str, Any]`, found `dict[Unknown, Unknown] | Mapping[str, object]`
+ error[lint:invalid-argument-type] src/attr/_make.py:531:50: Argument to this function is incorrect: Expected `Mapping[str, object] | None`, found `dict[Unknown, Unknown]`
- error[lint:unresolved-attribute] tests/test_converters.py:32:16: Type `Converter` has no attribute `__call__`
+ error[lint:unresolved-attribute] tests/test_converters.py:32:16: Type `Converter[Unknown, Unknown]` has no attribute `__call__`
- error[lint:unresolved-attribute] tests/test_converters.py:61:26: Type `Converter` has no attribute `_fmt_converter_call`
+ error[lint:unresolved-attribute] tests/test_converters.py:61:26: Type `Converter[Unknown, Unknown]` has no attribute `_fmt_converter_call`
- error[lint:call-non-callable] tests/test_converters.py:77:9: Object of type `Converter` is not callable
+ error[lint:call-non-callable] tests/test_converters.py:77:9: Object of type `Converter[Unknown, Unknown]` is not callable
- error[lint:call-non-callable] tests/test_converters.py:81:9: Object of type `Converter` is not callable
+ error[lint:call-non-callable] tests/test_converters.py:81:9: Object of type `Converter[Unknown, Unknown]` is not callable
- error[lint:call-non-callable] tests/test_converters.py:85:9: Object of type `Converter` is not callable
+ error[lint:call-non-callable] tests/test_converters.py:85:9: Object of type `Converter[Unknown, Unknown]` is not callable
- error[lint:call-non-callable] tests/test_converters.py:89:9: Object of type `Converter` is not callable
+ error[lint:call-non-callable] tests/test_converters.py:89:9: Object of type `Converter[Unknown, Unknown]` is not callable
- error[lint:unresolved-attribute] tests/test_converters.py:106:25: Type `Converter` has no attribute `__call__`
+ error[lint:unresolved-attribute] tests/test_converters.py:106:25: Type `Converter[Unknown, Unknown]` has no attribute `__call__`
- error[lint:unresolved-attribute] tests/test_converters.py:112:25: Type `Converter` has no attribute `__call__`
+ error[lint:unresolved-attribute] tests/test_converters.py:112:25: Type `Converter[Unknown, Unknown]` has no attribute `__call__`
- error[lint:unresolved-attribute] tests/test_converters.py:113:24: Type `Converter` has no attribute `__call__`
+ error[lint:unresolved-attribute] tests/test_converters.py:113:24: Type `Converter[Unknown, Unknown]` has no attribute `__call__`
+ error[lint:no-matching-overload] tests/test_hooks.py:134:22: No overload of function `attrib` matches arguments
+ error[lint:no-matching-overload] tests/test_hooks.py:135:22: No overload of function `attrib` matches arguments
+ error[lint:no-matching-overload] tests/test_hooks.py:151:22: No overload of function `attrib` matches arguments
+ error[lint:no-matching-overload] tests/test_hooks.py:152:22: No overload of function `attrib` matches arguments
+ error[lint:no-matching-overload] tests/test_hooks.py:171:26: No overload of function `attrib` matches arguments
+ error[lint:no-matching-overload] tests/test_hooks.py:172:26: No overload of function `attrib` matches arguments
+ error[lint:no-matching-overload] tests/test_make.py:1421:22: No overload of function `field` matches arguments
+ error[lint:no-matching-overload] tests/test_make.py:1686:13: No overload of function `attrib` matches arguments
- error[lint:invalid-argument-type] tests/test_next_gen.py:444:25: Argument to this function is incorrect: Expected `int`, found `dict`
+ error[lint:invalid-argument-type] tests/test_next_gen.py:444:25: Argument to this function is incorrect: Expected `int`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_validators.py:185:68: Argument to this function is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[AnyStr] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
+ error[lint:invalid-argument-type] tests/test_validators.py:219:56: Argument to this function is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[AnyStr] | None) | None`, found `Overload[(pattern: str | Pattern[str], string: str, flags: Unknown = Literal[0]) -> Match[str] | None, (pattern: bytes | Pattern[bytes], string: @Todo(Support for `typing.TypeAlias`), flags: Unknown = Literal[0]) -> Match[bytes] | None]`
- error[lint:invalid-argument-type] tests/test_validators.py:228:32: Argument to this function is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> @Todo(specialized non-generic class) | None) | None`, found `() -> Unknown`
+ error[lint:invalid-argument-type] tests/test_validators.py:228:32: Argument to this function is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> Match[AnyStr] | None) | None`, found `() -> Unknown`
- Found 640 diagnostics
+ Found 652 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- error[lint:invalid-argument-type] src/pegen/parser.py:332:45: Argument to this function is incorrect: Expected `() -> str`, found `(bound method TextIO.readline(limit: int = Literal[-1], /) -> AnyStr) | @Todo(Support for `typing.TypeAlias`)`
- warning[lint:unused-ignore-comment] src/pegen/utils.py:51:69: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] src/pegen/utils.py:66:61: Unused blanket `type: ignore` directive
- Found 58 diagnostics
+ Found 55 diagnostics

anyio (https://github.com/agronholm/anyio)
+ error[lint:missing-argument] src/anyio/_backends/_asyncio.py:183:29: No arguments provided for required parameters `_SendT_nd_contra`, `_ReturnT_nd_co` of class `Coroutine`
+ warning[lint:possibly-unbound-attribute] src/anyio/_backends/_asyncio.py:797:29: Attribute `_tasks` on type `Unknown | CancelScope | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/anyio/_backends/_asyncio.py:798:13: Attribute `_tasks` on type `Unknown | CancelScope | None` is possibly unbound
- error[lint:unresolved-import] src/anyio/_backends/_asyncio.py:2305:20: Cannot resolve import `uvloop`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2469:41: Argument to this function is incorrect: Expected `tuple[Context, (...) -> Unknown, tuple, Future, CancelScope] | None`, found `tuple[Context, (...) -> T_Retval, @Todo(full tuple[...] support), Unknown, CancelScope | None]`
+ error[lint:invalid-argument-type] src/anyio/_core/_streams.py:52:40: Argument to this function is incorrect: Expected `MemoryObjectStreamState[T_contra]`, found `MemoryObjectStreamState[T_Item]`
+ error[lint:invalid-argument-type] src/anyio/_core/_streams.py:52:74: Argument to this function is incorrect: Expected `MemoryObjectStreamState[T_co]`, found `MemoryObjectStreamState[T_Item]`
+ error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:349:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `Literal[SpooledTemporaryFile]` in `super(Literal[SpooledTemporaryFile], SpooledTemporaryFile[bytes])` call
+ error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:370:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `Literal[SpooledTemporaryFile]` in `super(Literal[SpooledTemporaryFile], SpooledTemporaryFile[bytes])` call
+ error[lint:invalid-super-argument] src/anyio/_core/_tempfile.py:377:22: `SpooledTemporaryFile[bytes]` is not an instance or subclass of `Literal[SpooledTemporaryFile]` in `super(Literal[SpooledTemporaryFile], SpooledTemporaryFile[bytes])` call
+ error[lint:no-matching-overload] src/anyio/_core/_tempfile.py:500:21: No overload of bound method `__init__` matches arguments
+ error[lint:invalid-return-type] src/anyio/_core/_typedattr.py:50:16: Return type does not match returned value: Expected `Mapping[T_Attr, () -> T_Attr]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-return-type] src/anyio/abc/_sockets.py:87:16: Return type does not match returned value: Expected `Mapping[Any, () -> Any]`, found `dict[Unknown, Unknown]`
- error[lint:inconsistent-mro] src/anyio/from_thread.py:84:1: Cannot create a consistent method resolution order (MRO) for class `_BlockingAsyncContextManager` with bases list `[@Todo(`Generic[]` subscript), <class 'AbstractContextManager'>]`
+ error[lint:invalid-argument-type] src/anyio/from_thread.py:219:50: Argument to this function is incorrect: Expected `(Future[_T], /) -> object`, found `def callback(f: Future[T_Retval]) -> None`
+ error[lint:invalid-argument-type] src/anyio/from_thread.py:376:29: Argument to this function is incorrect: Expected `(Future[_T], /) -> object`, found `def task_done(future: Future[T_Retval]) -> None`
- error[lint:inconsistent-mro] src/anyio/streams/memory.py:72:1: Cannot create a consistent method resolution order (MRO) for class `MemoryObjectReceiveStream` with bases list `[@Todo(`Generic[]` subscript), <class 'ObjectReceiveStream'>]`
- error[lint:invalid-return-type] src/anyio/streams/memory.py:124:24: Return type does not match returned value: Expected `T_co`, found `T_Item`
+ error[lint:invalid-return-type] src/anyio/streams/file.py:52:16: Return type does not match returned value: Expected `Mapping[Any, () -> Any]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-return-type] src/anyio/streams/stapled.py:52:16: Return type does not match returned value: Expected `Mapping[Any, () -> Any]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-return-type] src/anyio/streams/stapled.py:88:16: Return type does not match returned value: Expected `Mapping[Any, () -> Any]`, found `dict[Unknown, Unknown]`
- error[lint:inconsistent-mro] src/anyio/streams/memory.py:190:1: Cannot create a consistent method resolution order (MRO) for class `MemoryObjectSendStream` with bases list `[@Todo(`Generic[]` subscript), <class 'ObjectSendStream'>]`
- error[lint:inconsistent-mro] src/anyio/streams/stapled.py:58:1: Cannot create a consistent method resolution order (MRO) for class `StapledObjectStream` with bases list `[@Todo(`Generic[]` subscript), <class 'ObjectStream'>]`
- error[lint:inconsistent-mro] src/anyio/streams/stapled.py:94:1: Cannot create a consistent method resolution order (MRO) for class `MultiListener` with bases list `[@Todo(`Generic[]` subscript), <class 'Listener'>]`
- error[lint:unsupported-operator] src/anyio/streams/tls.py:44:41: Operator `|` is unsupported between objects of type `Literal[str]` and `Unknown | GenericAlias`
+ error[lint:invalid-return-type] src/anyio/streams/stapled.py:141:16: Return type does not match returned value: Expected `Mapping[Any, () -> Any]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-assignment] src/anyio/streams/text.py:37:5: Object of type `Literal["utf-8"]` is not assignable to `InitVar[str]`
+ error[lint:invalid-assignment] src/anyio/streams/text.py:38:5: Object of type `Literal["strict"]` is not assignable to `InitVar[str]`
+ error[lint:invalid-assignment] src/anyio/streams/text.py:77:5: Object of type `Literal["utf-8"]` is not assignable to `InitVar[str]`
+ error[lint:invalid-assignment] src/anyio/streams/text.py:116:5: Object of type `Literal["utf-8"]` is not assignable to `InitVar[str]`
+ error[lint:invalid-assignment] src/anyio/streams/text.py:117:5: Object of type `Literal["strict"]` is not assignable to `InitVar[str]`
+ error[lint:invalid-argument-type] src/anyio/streams/text.py:123:36: Argument to this function is incorrect: Expected `InitVar[str]`, found `str`
+ error[lint:invalid-argument-type] src/anyio/streams/text.py:123:55: Argument to this function is incorrect: Expected `InitVar[str]`, found `str`
+ error[lint:invalid-argument-type] src/anyio/streams/text.py:126:36: Argument to this function is incorrect: Expected `InitVar[str]`, found `str`
+ error[lint:invalid-return-type] src/anyio/streams/text.py:144:16: Return type does not match returned value: Expected `Mapping[Any, () -> Any]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-return-type] src/anyio/streams/tls.py:245:16: Return type does not match returned value: Expected `Mapping[Any, () -> Any]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-return-type] src/anyio/streams/tls.py:350:16: Return type does not match returned value: Expected `Mapping[Any, () -> Any]`, found `dict[Unknown, Unknown]`
- Found 129 diagnostics
+ Found 150 diagnostics

git-revise (https://github.com/mystor/git-revise)
+ error[lint:invalid-argument-type] gitrevise/merge.py:98:34: Argument to this function is incorrect: Expected `Mapping[bytes, Entry]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] gitrevise/merge.py:167:42: Argument to this function is incorrect: Expected `Mapping[bytes, Entry]`, found `dict[Unknown, Unknown]`
+ error[lint:no-matching-overload] gitrevise/odb.py:286:29: No overload of bound method `__init__` matches arguments
+ error[lint:invalid-assignment] gitrevise/odb.py:839:9: Object of type `dict[Unknown, Unknown]` is not assignable to `Mapping[str, str] | None`
+ warning[lint:call-possibly-unbound-method] gitrevise/odb.py:840:9: Method `__getitem__` of type `Mapping[str, str] | None` is possibly unbound
+ error[lint:invalid-argument-type] gitrevise/tui.py:126:40: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[lint:invalid-argument-type] gitrevise/tui.py:128:47: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[lint:invalid-argument-type] gitrevise/tui.py:128:47: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[lint:invalid-argument-type] gitrevise/tui.py:128:47: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ error[lint:invalid-argument-type] gitrevise/tui.py:180:39: Argument to this function is incorrect: Expected `Commit`, found `Commit | None`
+ warning[lint:possibly-unbound-attribute] gitrevise/tui.py:183:13: Attribute `tree` on type `Commit | None` is possibly unbound
+ error[lint:no-matching-overload] gitrevise/utils.py:102:25: No overload of function `match` matches arguments
+ warning[lint:possibly-unbound-attribute] gitrevise/utils.py:238:18: Attribute `oid` on type `Commit | None` is possibly unbound
- Found 4 diagnostics
+ Found 17 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
- error[lint:invalid-return-type] aioredis/client.py:3921:16: Return type does not match returned value: Expected `MonitorCommandInfo`, found `dict`
+ error[lint:invalid-return-type] aioredis/client.py:3921:16: Return type does not match returned value: Expected `MonitorCommandInfo`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-assignment] aioredis/connection.py:161:5: Object of type `dict[Unknown, Unknown]` is not assignable to `Mapping[str, type[Exception] | Mapping[str, type[Exception]]]`
- error[lint:invalid-assignment] aioredis/connection.py:460:9: Object of type `dict` is not assignable to `_HiredisReaderArgs`
+ error[lint:invalid-assignment] aioredis/connection.py:460:9: Object of type `dict[Unknown, Unknown]` is not assignable to `_HiredisReaderArgs`
+ error[lint:invalid-assignment] aioredis/connection.py:1166:1: Object of type `MappingProxyType[Unknown, Unknown]` is not assignable to `Mapping[str, (...) -> object]`
- error[lint:invalid-assignment] aioredis/connection.py:1192:5: Object of type `dict` is not assignable to `ConnectKwargs`
+ error[lint:invalid-assignment] aioredis/connection.py:1192:5: Object of type `dict[Unknown, Unknown]` is not assignable to `ConnectKwargs`
+ error[lint:invalid-argument-type] aioredis/connection.py:1208:38: Argument to this function is incorrect: Expected `str | bytes`, found `str | None`
+ error[lint:invalid-argument-type] aioredis/connection.py:1210:38: Argument to this function is incorrect: Expected `str | bytes`, found `str | None`
+ error[lint:invalid-argument-type] aioredis/connection.py:1220:38: Argument to this function is incorrect: Expected `str | bytes`, found `str | None`
- Found 28 diagnostics
+ Found 33 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
- warning[lint:unused-ignore-comment] test_htmlgen/element.py:218:60: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] test_htmlgen/element.py:224:60: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] test_htmlgen/element.py:225:54: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] test_htmlgen/element.py:254:28: Unused blanket `type: ignore` directive
- Found 102 diagnostics
+ Found 98 diagnostics

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ error[lint:invalid-argument-type] pytest_robotframework/__init__.py:96:60: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[str, Never]`
+ error[lint:invalid-argument-type] pytest_robotframework/__init__.py:97:62: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[str, Never]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:256:9: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:281:9: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:346:9: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:347:9: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:359:13: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[str, object]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:360:13: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/plugin.py:470:25: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/pytest/robot_file_support.py:58:63: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[str, Unknown]`
+ error[lint:invalid-argument-type] pytest_robotframework/_internal/robot/utils.py:158:15: `Unknown` is not a valid argument to `typing.Generic`
+ error[lint:no-matching-overload] pytest_robotframework/_internal/robot/utils.py:259:12: No overload of function `reduce` matches arguments
+ error[lint:invalid-argument-type] tests/fixtures/test_python/test_as_keyword_args_and_kwargs.py:7:46: Argument to this function is incorrect: Expected `Mapping[str, str] | None`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:7:32: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:7:54: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:11:32: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:11:56: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:15:32: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:15:44: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:19:32: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:19:56: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:23:32: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:23:55: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:27:32: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:27:56: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:31:32: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:31:54: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:31:66: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:35:32: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:35:56: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
+ error[lint:invalid-argument-type] tests/test_robot_utils.py:35:70: Argument to this function is incorrect: Expected `Mapping[str, object]`, found `dict[Unknown, Unknown]`
- Found 322 diagnostics
+ Found 353 diagnostics

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
+ error[lint:type-assertion-failure] test/test_generated_mypy.py:486:5: Actual type `Unknown` is not the same as asserted type `ScalarMap[Unknown, Email]`
+ error[lint:type-assertion-failure] test/test_generated_mypy.py:497:5: Actual type `Unknown` is not the same as asserted type `ScalarMap[Unknown, Email]`
- Found 79 diagnostics
+ Found 81 diagnostics

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
+ error[lint:no-matching-overload] src/check_jsonschema/cachedownloader.py:81:5: No overload of bound method `write` matches arguments
+ warning[lint:possibly-unbound-attribute] src/check_jsonschema/cli/param_types.py:90:15: Attribute `group` on type `Match[AnyStr] | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/check_jsonschema/cli/param_types.py:91:21: Attribute `group` on type `Match[AnyStr] | None` is possibly unbound
+ error[lint:no-matching-overload] src/check_jsonschema/parsers/yaml.py:61:14: No overload of bound method `__init__` matches arguments
- Found 62 diagnostics
+ Found 66 diagnostics

pyjwt (https://github.com/jpadilla/pyjwt)
- error[lint:invalid-return-type] jwt/api_jws.py:54:16: Return type does not match returned value: Expected `SigOptions`, found `dict`
+ error[lint:invalid-return-type] jwt/api_jws.py:54:16: Return type does not match returned value: Expected `SigOptions`, found `dict[Unknown, Unknown]`
- error[lint:invalid-assignment] jwt/api_jws.py:215:13: Object of type `dict` is not assignable to `SigOptions`
+ error[lint:invalid-assignment] jwt/api_jws.py:215:13: Object of type `dict[Unknown, Unknown]` is not assignable to `SigOptions`
- error[lint:invalid-return-type] jwt/api_jwt.py:51:16: Return type does not match returned value: Expected `FullOptions`, found `dict`
+ error[lint:invalid-return-type] jwt/api_jwt.py:51:16: Return type does not match returned value: Expected `FullOptions`, found `dict[Unknown, Unknown]`
- error[lint:invalid-return-type] jwt/api_jwt.py:77:16: Return type does not match returned value: Expected `FullOptions`, found `dict`
+ error[lint:invalid-return-type] jwt/api_jwt.py:77:16: Return type does not match returned value: Expected `FullOptions`, found `dict[Unknown, Unknown]`
- error[lint:invalid-assignment] jwt/api_jwt.py:246:9: Object of type `dict` is not assignable to `SigOptions`
+ error[lint:invalid-assignment] jwt/api_jwt.py:246:9: Object of type `dict[Unknown, Unknown]` is not assignable to `SigOptions`
- error[lint:invalid-argument-type] jwt/jwks_client.py:113:42: Argument to this function is incorrect: Expected `Options | None`, found `dict`
+ error[lint:invalid-argument-type] jwt/jwks_client.py:113:42: Argument to this function is incorrect: Expected `Options | None`, found `dict[Unknown, Unknown]`

kornia (https://github.com/kornia/kornia)
- error[lint:not-iterable] kornia/augmentation/container/image.py:380:18: Object of type `dict | list | None` may not be iterable
+ error[lint:not-iterable] kornia/augmentation/container/image.py:380:18: Object of type `dict[Unknown, Unknown] | list | None` may not be iterable
- error[lint:unsupported-operator] kornia/augmentation/container/image.py:382:10: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["output_size"]` with `dict | list | None`
+ error[lint:unsupported-operator] kornia/augmentation/container/image.py:382:10: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["output_size"]` with `dict[Unknown, Unknown] | list | None`
- error[lint:call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict.__getitem__(key: _KT, /) -> _VT) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict | list | None`
+ error[lint:call-non-callable] kornia/augmentation/container/image.py:383:17: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict[Unknown, Unknown] | list | None`
- error[lint:call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict.__getitem__(key: _KT, /) -> _VT) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict | list | None`
+ error[lint:call-non-callable] kornia/augmentation/container/image.py:388:32: Method `__getitem__` of type `(bound method dict[Unknown, Unknown].__getitem__(key: Unknown, /) -> Unknown) | (Overload[(i: SupportsIndex, /) -> _T, (s: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(specialized non-generic class)])` is not callable on object of type `dict[Unknown, Unknown] | list | None`
- error[lint:invalid-return-type] kornia/augmentation/container/ops.py:50:16: Return type does not match returned value: Expected `dict`, found `dict | list | None`
+ error[lint:invalid-return-type] kornia/augmentation/container/ops.py:50:16: Return type does not match returned value: Expected `dict[Unknown, Unknown]`, found `dict[Unknown, Unknown] | list | None`
- error[lint:invalid-return-type] kornia/augmentation/container/ops.py:58:16: Return type does not match returned value: Expected `list`, found `dict | list | None`
+ error[lint:invalid-return-type] kornia/augmentation/container/ops.py:58:16: Return type does not match returned value: Expected `list`, found `dict[Unknown, Unknown] | list | None`
+ error[lint:call-non-callable] kornia/contrib/models/efficient_vit/nn/act.py:43:19: Method `__getitem__` of type `bound method dict[str, @Todo(unsupported type[X] special form)].__getitem__(key: str, /) -> @Todo(unsupported type[X] special form)` is not callable on object of type `dict[str, @Todo(unsupported type[X] special form)]`
+ error[lint:invalid-argument-type] kornia/contrib/models/rt_detr/model.py:220:35: Argument to this function is incorrect: Expected `str`, found `str | None`
+ error[lint:no-matching-overload] kornia/feature/dedode/encoder.py:57:22: No overload of bound method `__init__` matches arguments
- error[lint:invalid-assignment] kornia/feature/keynet.py:39:1: Object of type `dict` is not assignable to `KeyNet_conf`
+ error[lint:invalid-assignment] kornia/feature/keynet.py:39:1: Object of type `dict[Unknown, Unknown]` is not assignable to `KeyNet_conf`
- error[lint:invalid-return-type] kornia/feature/scale_space_detector.py:275:12: Return type does not match returned value: Expected `Detector_config`, found `dict`
+ error[lint:invalid-return-type] kornia/feature/scale_space_detector.py:275:12: Return type does not match returned value: Expected `Detector_config`, found `dict[Unknown, Unknown]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice[Any, _StartT_co, _StartT_co | _StopT_co], /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/quaternion.py:156:43: Method `__getitem__` of type `Unknown | (bound method Quaternion.__getitem__(idx: int | slice) -> Quaternion)` is not callable on object of type `Unknown | Quaternion`
+ error[lint:call-non-callable] kornia/geometry/quaternion.py:156:43: Method `__getitem__` of type `Unknown | (bound method Quaternion.__getitem__(idx: int | slice[Any, _StartT_co, _StartT_co | _StopT_co]) -> Quaternion)` is not callable on object of type `Unknown | Quaternion`
- error[lint:unsupported-operator] kornia/models/detection/base.py:230:37: Operator `|` is unsupported between objects of type `Literal[str]` and `Literal[int]`
+ error[lint:no-matching-overload] kornia/x/utils.py:38:22: No overload of function `field` matches arguments
+ error[lint:no-matching-overload] kornia/x/utils.py:39:23: No overload of function `field` matches arguments
+ error[lint:no-matching-overload] kornia/x/utils.py:40:23: No overload of function `field` matches arguments
+ error[lint:no-matching-overload] kornia/x/utils.py:41:17: No overload of function `field` matches arguments
+ error[lint:no-matching-overload] kornia/x/utils.py:42:24: No overload of function `field` matches arguments
+ error[lint:no-matching-overload] kornia/x/utils.py:43:35: No overload of function `field` matches arguments
- Found 1005 diagnostics
+ Found 1013 diagnostics

python-sop (https://gitlab.com/dkg/python-sop)
- error[lint:invalid-assignment] sop/__init__.py:380:13: Object of type `TextIOWrapper` is not assignable to `BinaryIO`
+ error[lint:invalid-assignment] sop/__init__.py:380:13: Object of type `TextIOWrapper[_WrappedBuffer]` is not assignable to `BinaryIO`
- error[lint:invalid-assignment] sop/__init__.py:382:13: Object of type `TextIOWrapper` is not assignable to `BinaryIO`
+ error[lint:invalid-assignment] sop/__init__.py:382:13: Object of type `TextIOWrapper[_WrappedBuffer]` is not assignable to `BinaryIO`
+ error[lint:invalid-parameter-default] sop/__init__.py:489:14: Default value of type `dict[Unknown, Unknown]` is not assignable to annotated parameter type `MutableMapping[str, bytes]`
+ error[lint:invalid-parameter-default] sop/__init__.py:525:16: Default value of type `dict[Unknown, Unknown]` is not assignable to annotated parameter type `MutableMapping[str, bytes]`
+ error[lint:invalid-parameter-default] sop/__init__.py:571:17: Default value of type `dict[Unknown, Unknown]` is not assignable to an...*[Comment body truncated]*

---

_Label `red-knot` added by @AlexWaygood on 2025-04-29 20:10_

---

_Comment by @codspeed-hq[bot] on 2025-04-30 18:29_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flegacy-class)

### Merging #17721 will **degrade performances by 5.21%**

<sub>Comparing <code>dcreager/legacy-class</code> (36626a2) with <code>main</code> (f7cae4f)</sub>



### Summary

`❌ 2 (👁 2)` regressions  
`✅ 31` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_check_file[cold] `` | 96.8 ms | 102 ms | -5.07% |
| 👁 | `` red_knot_check_file[incremental] `` | 5.3 ms | 5.6 ms | -5.21% |


---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/decorators.md`:148 on 2025-05-01 01:05_

This requires inducting into the structure of a callable type during inference, since `_T` needs to be mapped to the return type of the callable that's being decorated.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/annotations/stdlib_typing_aliases.md`:35 on 2025-05-01 01:07_

All of these (and many more below) require hard-coding that certain `KnownInstance`s (which are `_Alias`es in typeshed) are generic, and are valid origins of a `GenericAlias`.  I plan to tackle that in a follow-on PR.

---

_@dcreager reviewed on 2025-05-01 02:13_

This is in a good spot, marking it ready for review

---

_Marked ready for review by @dcreager on 2025-05-01 02:15_

---

_Review requested from @carljm by @dcreager on 2025-05-01 02:15_

---

_Review requested from @AlexWaygood by @dcreager on 2025-05-01 02:15_

---

_Review requested from @sharkdp by @dcreager on 2025-05-01 02:15_

---

_Review requested from @MichaReiser by @dcreager on 2025-05-01 02:15_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:499 on 2025-05-01 05:53_

Is it worth to have this as a dedicated query, considering that `explicit_bases` is a query (which caches gives us caching for both `legacy_generic` and `inherited_legacy`

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/generics.rs`:156 on 2025-05-01 05:56_

This is `O(n^2)`. Can we short circuit, e.g. compare the lengths first?

It might also be worth to cache `other.variables(db)` and move it outside the loop

---

_@MichaReiser reviewed on 2025-05-01 05:56_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/generics.rs`:156 on 2025-05-01 13:49_

Yeah I hadn't liked the quadratic nature of this ever since I first wrote it.  Turns out we've been using `FxOrderSet` to build up the variable list in most places already, so I've updated this to just store that set in `GenericContext`.  Now we can use its [`is_subset`](https://docs.rs/ordermap/latest/ordermap/set/struct.OrderSet.html#method.is_subset) method to do the work.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class.rs`:499 on 2025-05-01 13:51_

Hmm, several commits back on this branch I had to add this because of the cycle handling. But removing it no longer causes any cycle-related panics...

We're also post-processing the results of `explicit_bases` in both cases, and I thought the extra cache would prevent us from having to duplicate that work. Are you thinking that the overhead of the extra Salsa query will dominate that post-processing work?

---

_@dcreager reviewed on 2025-05-01 13:51_

---

_@MichaReiser reviewed on 2025-05-01 13:57_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:499 on 2025-05-01 13:57_

> Are you thinking that the overhead of the extra Salsa query will dominate that post-processing work?

Hard to tell but quiet possibly. It's also a balance between memory usage and perf where fewer queries means less memory usage.


I did note that you call a method that uses the AST node in at least one of the two cases. So removing the tracked might, after all, not be possible.

---

_Comment by @dcreager on 2025-05-01 23:48_

The current ecosystem failure is a "too many cycle iterations", coming from an [implicit type alias](https://github.com/pylint-dev/pylint/blob/77481a6e36fa7fa6b8530e9a300af840867e4df4/pylint/typing.py#L132) of the form:

```py
Foo = dict[str, "Foo"]
```



---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md`:57 on 2025-05-02 03:24_

Is this TODO really correct? Given that the default for the `BaseExceptionGroup` typevar is `BaseException`, it seems to me that what we emit here now is right, and we can probably remove this TODO. @AlexWaygood ?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md`:63 on 2025-05-02 03:25_

Again here I think maybe what we have below is correct, and we don't need this TODO anymore.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/expression/lambda.md`:86 on 2025-05-02 03:29_

Drive-by since we're here anyway:

```suggestion
Using a keyword-variadic parameter:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/legacy/classes.md`:5 on 2025-05-02 03:32_

```suggestion
At its simplest, to define a generic class using the legacy syntax, you inherit from the
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/legacy/classes.md`:116 on 2025-05-02 03:52_

Should we have a TODO here to improve this diagnostic?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/legacy/classes.md`:279 on 2025-05-02 04:33_

```suggestion
### Both present, `__new__` inherited from a generic base class
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695/classes.md`:240 on 2025-05-02 04:38_

```suggestion
### Both present, `__new__` inherited from a generic base class
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/protocols.md`:73 on 2025-05-02 04:39_

```suggestion
# TODO: should not have `Protocol` multiple times
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:599 on 2025-05-02 04:40_

🎉 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:2287 on 2025-05-02 04:45_

What happens if the specialization has the wrong arity?

---

_@carljm approved on 2025-05-02 04:49_

This is huge! Great work.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/legacy/classes.md`:279 on 2025-05-02 14:19_

Haha whoops!  This is my hack for running a single mdtest without having to remember its name :joy: 

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/pep695/classes.md`:240 on 2025-05-02 14:20_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md`:63 on 2025-05-02 14:22_

Is the thought here that the type in the `except*` clause should produce a more specific specialization?

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/legacy/classes.md`:116 on 2025-05-02 14:24_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/class.rs`:2287 on 2025-05-02 14:25_

~Currently we panic:~

https://github.com/astral-sh/ruff/blob/7f2d22b391140f490e79773efe66e3778cb3c6ac/crates/red_knot_python_semantic/src/types/generics.rs#L159

My thinking was that this is used to specialize known classes like `dict`, so mismatched arity would be a programmer error.

But that means a user could induce a panic by providing a custom typeshed where `dict` doesn't have two generic parameters!  I updated this to print out an INFO logging message, just like we do if a class can't be found at all in a custom typeshed.  (And added a test case)

---

_@dcreager reviewed on 2025-05-02 15:21_

---

_Comment by @dcreager on 2025-05-02 15:23_

Still seeing a 4-5% performance regression, though it's for both cold and incremental. We're genuinely doing more work now, so I think this is a reasonable dip.

---

_@AlexWaygood reviewed on 2025-05-02 15:25_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md`:57 on 2025-05-02 15:25_

Oh, I missed that the `TypeVar` had a default. Yes, I think we can remove the TODO now!

---

_@AlexWaygood reviewed on 2025-05-02 15:27_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md`:63 on 2025-05-02 15:27_

I think `BaseExceptionGroup[AttributeError | Unknown]` would be fine as an inferred type, but that `BaseExceptionGroup[BaseException]` is also fine. I agree with Carl that we can probably remove the TODO here

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md`:57 on 2025-05-02 15:29_

:+1: Sounds good, thanks for confirming!  Removed

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md`:63 on 2025-05-02 15:29_

Removed

---

_@dcreager reviewed on 2025-05-02 15:29_

---

_Comment by @dcreager on 2025-05-03 00:22_

This `trio` ecosystem failure was due to this definition:

https://github.com/python-trio/trio/blob/9c909fa10d7a9d9e1cfd019725f826848d05bf81/src/trio/_abc.py#L220-L226

```py
    @abstractmethod
    def socket(
        self,
        family: socket.AddressFamily | int = socket.AF_INET,
        type: socket.SocketKind | int = socket.SOCK_STREAM,
        proto: int = 0,
    ) -> SocketType:
```

which has a type annotation that is intended to resolve at the global scope, but that we resolve at the containing class scope. (That is, `socket.AddressFamily` looks for `AddressFamily` on the newly defined method.)  After discussion in Discord, we think that our behavior is correct, especially once PEP 649 lands.

Our behavior currently causes a cycle panic. For now, I'm moving `trio` to the bad list to unblock this PR.

(Complicating things is that the cycle panic is nondeterministic — it only shows up when `FunctionType::signature` is detected as the cycle head. It was also nondeterministic when setting `RAYON_NUM_THREADS=1`, which was surprising. It turns out that our [directory watcher](https://github.com/astral-sh/ruff/blob/a467e7c8d33128b8ebd77278fd73ca8da019a1a4/crates/ruff_db/src/system/os.rs#L430-L434) spins up multiple worker threads, too, independently of `RAYON_NUM_THREADS`, which can introduce nondeterminism based on which file we end up analyzing first.)

---

_Merged by @dcreager on 2025-05-03 00:34_

---

_Closed by @dcreager on 2025-05-03 00:34_

---

_Branch deleted on 2025-05-03 00:34_

---

_@carljm reviewed on 2025-05-03 15:43_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/exception/except_star.md`:63 on 2025-05-03 15:43_

Just for future reference, my thought process here (which I didn't fully spell out), is that the source of the `Unknown` in `BaseExceptionGroup[AttributeError | Unknown]` would be the default specialization (from the nonsense exception type), same as in the above case. Which means that it really should be `BaseExceptionGroup[AttributeError | BaseException]` instead, due to the typevar default, which simplifies to `BaseExceptionGroup[BaseException]`, since `AttributeError` is a subtype of `BaseException`.

---
