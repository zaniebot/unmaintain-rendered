```yaml
number: 19544
title: Display generic function signature properly
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - ty
assignees: []
merged: true
base: main
head: display-generic-function-signature
created_at: 2025-07-24T23:54:49Z
updated_at: 2025-11-06T11:48:11Z
url: https://github.com/astral-sh/ruff/pull/19544
synced_at: 2026-01-10T16:53:55Z
```

# Display generic function signature properly

---

_Pull request opened by @MatthewMckee4 on 2025-07-24 23:54_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves https://github.com/astral-sh/ty/issues/817

Unsure if we want the upper bound to be displayed, but it makes sense to use this display struct already there

## Test Plan

Update mdtest


---

_Comment by @MatthewMckee4 on 2025-07-24 23:57_

This extra `DisplayOptionalGenericContext` struct may be a bit overkill, but i imaging we may want to use it for classes in the future, not sure.

---

_Comment by @github-actions[bot] on 2025-07-24 23:58_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
Expression (https://github.com/cognitedata/Expression)
- expression/collections/seq.py:637:41: error[invalid-argument-type] Argument to function `init_infinite` is incorrect: Expected `(int, /) -> Unknown`, found `def identity(value: _A@identity) -> _A@identity`
+ expression/collections/seq.py:637:41: error[invalid-argument-type] Argument to function `init_infinite` is incorrect: Expected `(int, /) -> Unknown`, found `def identity[_A](value: _A@identity) -> _A@identity`
- expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some(value: _T1@Some) -> Option[_T1@Some], Unknown, /) -> def Some(value: _T1@Some) -> Option[_T1@Some]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
+ expression/extra/option/pipeline.py:91:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Some[_T1](value: _T1@Some) -> Option[_T1@Some], Unknown, /) -> def Some[_T1](value: _T1@Some) -> Option[_T1@Some]`, found `def reducer(acc: (Any, /) -> Option[Any], fn: (Any, /) -> Option[Any]) -> (Any, /) -> Option[Any]`
- expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok(value: _TSource@Ok) -> Result[_TSource@Ok, Any], Unknown, /) -> def Ok(value: _TSource@Ok) -> Result[_TSource@Ok, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
+ expression/extra/result/pipeline.py:96:19: error[invalid-argument-type] Argument to function `reduce` is incorrect: Expected `(def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any], Unknown, /) -> def Ok[_TSource](value: _TSource@Ok) -> Result[_TSource@Ok, Any]`, found `def reducer(acc: (Any, /) -> Result[Any, Any], fn: (Any, /) -> Result[Any, Any]) -> (Any, /) -> Result[Any, Any]`
- expression/extra/result/traversable.py:50:21: error[invalid-argument-type] Argument to function `traverse` is incorrect: Expected `(Result[_TSource@sequence, _TError@sequence], /) -> Result[Unknown, Unknown]`, found `def identity(value: _A@identity) -> _A@identity`
+ expression/extra/result/traversable.py:50:21: error[invalid-argument-type] Argument to function `traverse` is incorrect: Expected `(Result[_TSource@sequence, _TError@sequence], /) -> Result[Unknown, Unknown]`, found `def identity[_A](value: _A@identity) -> _A@identity`
- tests/test_array.py:357:1: error[invalid-assignment] Object of type `def singleton(value: _TSource@singleton) -> TypedArray[_TSource@singleton]` is not assignable to `(int, /) -> TypedArray[int]`
+ tests/test_array.py:357:1: error[invalid-assignment] Object of type `def singleton[_TSource](value: _TSource@singleton) -> TypedArray[_TSource@singleton]` is not assignable to `(int, /) -> TypedArray[int]`
- tests/test_block.py:335:1: error[invalid-assignment] Object of type `def singleton(value: _TSource@singleton) -> Block[_TSource@singleton]` is not assignable to `(int, /) -> Block[int]`
+ tests/test_block.py:335:1: error[invalid-assignment] Object of type `def singleton[_TSource](value: _TSource@singleton) -> Block[_TSource@singleton]` is not assignable to `(int, /) -> Block[int]`
- tests/test_map.py:49:19: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Map[str, int], /) -> Unknown`, found `def to_seq(table: Map[_Key@to_seq, _Value@to_seq]) -> Iterable[tuple[_Key@to_seq, _Value@to_seq]]`
+ tests/test_map.py:49:19: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Map[str, int], /) -> Unknown`, found `def to_seq[_Key: SupportsLessThan, _Value](table: Map[_Key@to_seq, _Value@to_seq]) -> Iterable[tuple[_Key@to_seq, _Value@to_seq]]`
- tests/test_parser.py:232:9: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Parser[str], /) -> Unknown`, found `def many(parser: Parser[_A@many]) -> Parser[Block[_A@many]]`
+ tests/test_parser.py:232:9: error[invalid-argument-type] Argument to function `pipe` is incorrect: Expected `(Parser[str], /) -> Unknown`, found `def many[_A](parser: Parser[_A@many]) -> Parser[Block[_A@many]]`
- tests/test_result.py:487:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def swap(result: Result[_TSource@swap, _TError@swap]) -> Result[_TError@swap, _TSource@swap]`
+ tests/test_result.py:487:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def swap[_TSource, _TError](result: Result[_TSource@swap, _TError@swap]) -> Result[_TError@swap, _TSource@swap]`
- tests/test_result.py:573:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def merge(result: Result[_TSource@merge, _TSource@merge]) -> _TSource@merge`
+ tests/test_result.py:573:24: error[invalid-argument-type] Argument to bound method `pipe` is incorrect: Expected `(Result[Literal[42], Any], /) -> Unknown`, found `def merge[_TSource](result: Result[_TSource@merge, _TSource@merge]) -> _TSource@merge`
- tests/test_seq.py:308:20: error[invalid-argument-type] Argument to bound method `choose` is incorrect: Expected `(None | Literal[42], /) -> Option[Unknown]`, found `def of_optional(value: _TSource@of_optional | None) -> Option[_TSource@of_optional]`
+ tests/test_seq.py:308:20: error[invalid-argument-type] Argument to bound method `choose` is incorrect: Expected `(None | Literal[42], /) -> Option[Unknown]`, found `def of_optional[_TSource](value: _TSource@of_optional | None) -> Option[_TSource@of_optional]`
- tests/test_seq.py:321:1: error[invalid-assignment] Object of type `def singleton(item: _TSource@singleton) -> Seq[_TSource@singleton]` is not assignable to `(int, /) -> Seq[int]`
+ tests/test_seq.py:321:1: error[invalid-assignment] Object of type `def singleton[_TSource](item: _TSource@singleton) -> Seq[_TSource@singleton]` is not assignable to `(int, /) -> Seq[int]`

trio (https://github.com/python-trio/trio)
- src/trio/_tests/test_threads.py:89:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync(fn: (...) -> RetT@from_thread_run_sync, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run_sync`
+ src/trio/_tests/test_threads.py:89:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync[RetT](fn: (...) -> RetT@from_thread_run_sync, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run_sync`
- src/trio/_tests/test_threads.py:96:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync(fn: (...) -> RetT@from_thread_run_sync, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run_sync`
+ src/trio/_tests/test_threads.py:96:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run_sync[RetT](fn: (...) -> RetT@from_thread_run_sync, *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run_sync`
- src/trio/_tests/test_threads.py:104:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run(afn: (...) -> Awaitable[RetT@from_thread_run], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run`
+ src/trio/_tests/test_threads.py:104:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run[RetT](afn: (...) -> Awaitable[RetT@from_thread_run], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run`
- src/trio/_tests/test_threads.py:112:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run(afn: (...) -> Awaitable[RetT@from_thread_run], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run`
+ src/trio/_tests/test_threads.py:112:22: error[invalid-argument-type] Argument to function `check_case` is incorrect: Expected `(...) -> Thread`, found `def from_thread_run[RetT](afn: (...) -> Awaitable[RetT@from_thread_run], *args: @Todo(`Unpack[]` special form), *, trio_token: TrioToken | None = None) -> RetT@from_thread_run`

schemathesis (https://github.com/schemathesis/schemathesis)
- src/schemathesis/specs/openapi/negative/mutations.py:445:33: error[invalid-parameter-default] Default value of type `def ident(x: T@ident) -> T@ident` is not assignable to annotated parameter type `(T@ordered, /) -> Any`
+ src/schemathesis/specs/openapi/negative/mutations.py:445:33: error[invalid-parameter-default] Default value of type `def ident[T](x: T@ident) -> T@ident` is not assignable to annotated parameter type `(T@ordered, /) -> Any`

pwndbg (https://github.com/pwndbg/pwndbg)
- pwndbg/dbg/gdb/__init__.py:1663:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def exit(func: () -> T@exit, **kwargs: Any) -> () -> T@exit`
+ pwndbg/dbg/gdb/__init__.py:1663:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def exit[T](func: () -> T@exit, **kwargs: Any) -> () -> T@exit`
- pwndbg/dbg/gdb/__init__.py:1665:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def cont(func: () -> T@cont, **kwargs: Any) -> () -> T@cont`
+ pwndbg/dbg/gdb/__init__.py:1665:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def cont[T](func: () -> T@cont, **kwargs: Any) -> () -> T@cont`
- pwndbg/dbg/gdb/__init__.py:1667:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def start(func: () -> T@start, **kwargs: Any) -> () -> T@start`
+ pwndbg/dbg/gdb/__init__.py:1667:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def start[T](func: () -> T@start, **kwargs: Any) -> () -> T@start`
- pwndbg/dbg/gdb/__init__.py:1669:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def stop(func: () -> T@stop, **kwargs: Any) -> () -> T@stop`
+ pwndbg/dbg/gdb/__init__.py:1669:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def stop[T](func: () -> T@stop, **kwargs: Any) -> () -> T@stop`
- pwndbg/dbg/gdb/__init__.py:1671:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def new_objfile(func: () -> T@new_objfile, **kwargs: Any) -> () -> T@new_objfile`
+ pwndbg/dbg/gdb/__init__.py:1671:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def new_objfile[T](func: () -> T@new_objfile, **kwargs: Any) -> () -> T@new_objfile`
- pwndbg/dbg/gdb/__init__.py:1673:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def mem_changed(func: () -> T@mem_changed, **kwargs: Any) -> () -> T@mem_changed`
+ pwndbg/dbg/gdb/__init__.py:1673:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def mem_changed[T](func: () -> T@mem_changed, **kwargs: Any) -> () -> T@mem_changed`
- pwndbg/dbg/gdb/__init__.py:1675:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def reg_changed(func: () -> T@reg_changed, **kwargs: Any) -> () -> T@reg_changed`
+ pwndbg/dbg/gdb/__init__.py:1675:20: error[invalid-return-type] Return type does not match returned value: expected `((...) -> T@event_handler, /) -> (...) -> T@event_handler`, found `def reg_changed[T](func: () -> T@reg_changed, **kwargs: Any) -> () -> T@reg_changed`

pytest (https://github.com/pytest-dev/pytest)
- src/_pytest/pastebin.py:43:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method Stash.__setitem__(key: StashKey[T@__setitem__], value: T@__setitem__) -> None)` cannot be called with a key of type `Unknown | StashKey[IO[bytes]]` and a value of type `TextIOWrapper[_WrappedBuffer]` on object of type `Unknown | Stash`
+ src/_pytest/pastebin.py:43:13: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method Stash.__setitem__[T](key: StashKey[T@__setitem__], value: T@__setitem__) -> None)` cannot be called with a key of type `Unknown | StashKey[IO[bytes]]` and a value of type `TextIOWrapper[_WrappedBuffer]` on object of type `Unknown | Stash`
- src/_pytest/pytester.py:1129:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method Stash.__setitem__(key: StashKey[T@__setitem__], value: T@__setitem__) -> None)` cannot be called with a key of type `Unknown | StashKey[int]` and a value of type `Literal[0]` on object of type `Unknown | Stash`
+ src/_pytest/pytester.py:1129:21: error[invalid-assignment] Method `__setitem__` of type `Unknown | (bound method Stash.__setitem__[T](key: StashKey[T@__setitem__], value: T@__setitem__) -> None)` cannot be called with a key of type `Unknown | StashKey[int]` and a value of type `Literal[0]` on object of type `Unknown | Stash`

cwltool (https://github.com/common-workflow-language/cwltool)
- cwltool/process.py:1182:5: error[invalid-parameter-default] Default value of type `def urljoin(base: AnyStr@urljoin, url: AnyStr@urljoin | None, allow_fragments: bool = Literal[True]) -> AnyStr@urljoin` is not assignable to annotated parameter type `(str, str, /) -> str`
+ cwltool/process.py:1182:5: error[invalid-parameter-default] Default value of type `def urljoin[AnyStr: (str, bytes)](base: AnyStr@urljoin, url: AnyStr@urljoin | None, allow_fragments: bool = Literal[True]) -> AnyStr@urljoin` is not assignable to annotated parameter type `(str, str, /) -> str`

streamlit (https://github.com/streamlit/streamlit)
- lib/streamlit/elements/dialog_decorator.py:292:16: error[invalid-return-type] Return type does not match returned value: expected `F@dialog_decorator | ((F@dialog_decorator, /) -> F@dialog_decorator)`, found `def wrapper(f: F@wrapper) -> F@wrapper`
+ lib/streamlit/elements/dialog_decorator.py:292:16: error[invalid-return-type] Return type does not match returned value: expected `F@dialog_decorator | ((F@dialog_decorator, /) -> F@dialog_decorator)`, found `def wrapper[F: (...) -> Any](f: F@wrapper) -> F@wrapper`
- lib/streamlit/elements/dialog_decorator.py:333:16: error[invalid-return-type] Return type does not match returned value: expected `F@experimental_dialog_decorator | ((F@experimental_dialog_decorator, /) -> F@experimental_dialog_decorator)`, found `def wrapper(f: F@wrapper) -> F@wrapper`
+ lib/streamlit/elements/dialog_decorator.py:333:16: error[invalid-return-type] Return type does not match returned value: expected `F@experimental_dialog_decorator | ((F@experimental_dialog_decorator, /) -> F@experimental_dialog_decorator)`, found `def wrapper[F: (...) -> Any](f: F@wrapper) -> F@wrapper`

meson (https://github.com/mesonbuild/meson)
- unittests/helpers.py:228:16: error[invalid-return-type] Return type does not match returned value: expected `((...) -> R@xfail_if_jobname, /) -> (...) -> R@xfail_if_jobname`, found `def expectedFailure(test_item: _FT@expectedFailure) -> _FT@expectedFailure`
+ unittests/helpers.py:228:16: error[invalid-return-type] Return type does not match returned value: expected `((...) -> R@xfail_if_jobname, /) -> (...) -> R@xfail_if_jobname`, found `def expectedFailure[_FT: (...) -> Any](test_item: _FT@expectedFailure) -> _FT@expectedFailure`
- unittests/linuxliketests.py:712:48: error[invalid-argument-type] Argument to function `copytree` is incorrect: Expected `(str, str, /) -> object`, found `def copyfile(src: @Todo(Support for `typing.TypeAlias`), dst: _StrOrBytesPathT@copyfile, *, follow_symlinks: bool = Literal[True]) -> _StrOrBytesPathT@copyfile`
+ unittests/linuxliketests.py:712:48: error[invalid-argument-type] Argument to function `copytree` is incorrect: Expected `(str, str, /) -> object`, found `def copyfile[_StrOrBytesPathT: @Todo(Support for `typing.TypeAlias`)](src: @Todo(Support for `typing.TypeAlias`), dst: _StrOrBytesPathT@copyfile, *, follow_symlinks: bool = Literal[True]) -> _StrOrBytesPathT@copyfile`

static-frame (https://github.com/static-frame/static-frame)
- static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index[Any]].from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I@from_labels) | (bound method <class 'Index'>.from_labels(labels: Iterable[Unknown], *, /, name: Unknown = None) -> I@from_labels)`
+ static_frame/test/property/strategies.py:927:17: error[invalid-argument-type] Argument to function `get_index_hierarchy` is incorrect: Expected `(...) -> IndexHierarchy`, found `(bound method type[Index[Any]].from_labels[I: Index[Any]](labels: Iterable[Unknown], *, /, name: Unknown = None) -> I@from_labels) | (bound method <class 'Index'>.from_labels[I: Index[Any]](labels: Iterable[Unknown], *, /, name: Unknown = None) -> I@from_labels)`

```
</details>
No memory usage changes detected ✅


---

_Comment by @MatthewMckee4 on 2025-07-24 23:58_

I also don't think i need to consider `inherited_generic_context` at all

---

_Comment by @MatthewMckee4 on 2025-07-25 00:01_

hmm the `Self` generic context looks a bit unfortunate to include 

---

_Comment by @MatthewMckee4 on 2025-07-25 00:03_

will mark as ready for some feedback

---

_Marked ready for review by @MatthewMckee4 on 2025-07-25 00:03_

---

_Review requested from @carljm by @MatthewMckee4 on 2025-07-25 00:03_

---

_Review requested from @AlexWaygood by @MatthewMckee4 on 2025-07-25 00:03_

---

_Review requested from @sharkdp by @MatthewMckee4 on 2025-07-25 00:03_

---

_Review requested from @dcreager by @MatthewMckee4 on 2025-07-25 00:03_

---

_Review requested from @MichaReiser by @MatthewMckee4 on 2025-07-25 00:03_

---

_Review comment by @carljm on `crates/ty_ide/src/completion.rs`:1250 on 2025-07-25 00:12_

I think we should for sure always exclude the upper bound of `Self`, since it's implied by the definition of `Self`. I think probably we should exclude `Self` entirely. (This kind of special handling will probably be easiest to do if we give `Self` its own distinct `TypeVarKind`.)

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/named_tuple.md`:153 on 2025-07-25 00:14_

Yeah, I think we should skip `Self` when printing functions. Conceptually `Self` is really (implicitly) scoped to the class, not to the function.

---

_@carljm reviewed on 2025-07-25 00:17_

This is looking pretty good, thank you!

---

_Label `ty` added by @ntBre on 2025-07-25 01:22_

---

_Comment by @MatthewMckee4 on 2025-07-25 07:24_

Thanks for the feedback, I'll put this back in draft and get to it on Monday 

---

_Review requested from @carljm by @MatthewMckee4 on 2025-07-27 18:14_

---

_Review request for @sharkdp removed by @sharkdp on 2025-07-28 06:51_

---

_Comment by @github-actions[bot] on 2025-08-05 20:27_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Closed by @MatthewMckee4 on 2025-08-05 20:37_

---

_Reopened by @MatthewMckee4 on 2025-08-05 20:37_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6706 on 2025-08-05 21:20_

Is there a reason we need this two-layer enum structure? It's not clear to me what it gains us; it seems simpler and equally reasonable to me to just add `Self` as a third `TypeVarKind` alongside `Legacy` and `Pep695`. Are there other kinds of "Implicit" typevars you expect we'll need to handle in the future?

---

_@carljm reviewed on 2025-08-05 21:20_

The results here look great! Just one question inline about the implementation choices.

---

_Review comment by @MatthewMckee4 on `crates/ty_python_semantic/src/types.rs`:6706 on 2025-08-05 21:23_

No there's not really a good reason other than it made sense at the time to separate them. I'm happy to revert this.

---

_@MatthewMckee4 reviewed on 2025-08-05 21:23_

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6703 on 2025-08-05 23:06_

```suggestion
    /// T = TypeVar("T")
    Legacy,
    /// def foo[T](x: T) -> T: ...
    Pep695,
    /// `typing.Self`
    Implicit,
```

---

_@carljm reviewed on 2025-08-05 23:06_

---

_@carljm approved on 2025-08-05 23:07_

---

_Merged by @carljm on 2025-08-05 23:35_

---

_Closed by @carljm on 2025-08-05 23:35_

---

_Branch deleted on 2025-11-06 11:48_

---
