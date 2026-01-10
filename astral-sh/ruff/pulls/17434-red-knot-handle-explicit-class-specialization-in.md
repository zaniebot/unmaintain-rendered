```yaml
number: 17434
title: "[red-knot] Handle explicit class specialization in type expressions"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/subscript-types
created_at: 2025-04-16T19:29:43Z
updated_at: 2025-04-18T15:51:13Z
url: https://github.com/astral-sh/ruff/pull/17434
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Handle explicit class specialization in type expressions

---

_Pull request opened by @dcreager on 2025-04-16 19:29_

You can now use subscript expressions in a type expression to explicitly specialize generic classes, just like you could already do in value expressions.

This still does not implement bidirectional checking, so a type annotation on an assignment does not influence how we infer a specialization for a (not explicitly specialized) constructor call. You might get an `invalid-assignment` error if (a) we cannot infer a class specialization from the constructor call (in which case you end up e.g. trying to assign `C[Unknown]` to `C[int]`) or if (b) we can infer a specialization, but it doesn't match the annotation.

Closes https://github.com/astral-sh/ruff/issues/17432

---

_Label `red-knot` added by @dcreager on 2025-04-16 19:29_

---

_Review requested from @carljm by @dcreager on 2025-04-16 19:29_

---

_Review requested from @AlexWaygood by @dcreager on 2025-04-16 19:29_

---

_Review requested from @sharkdp by @dcreager on 2025-04-16 19:29_

---

_Comment by @github-actions[bot] on 2025-04-16 19:31_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- error[lint:type-assertion-failure] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:541:5: Actual type `frozenbidict` is not the same as asserted type `@Todo(generics)`
+ error[lint:type-assertion-failure] /tmp/mypy_primer/projects/bidict/tests/test_bidict.py:541:5: Actual type `frozenbidict` is not the same as asserted type `@Todo(specialized non-generic class)`

porcupine (https://github.com/Akuli/porcupine)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/tests/test_docs.py:19:64: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/tests/test_docs.py:29:12: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:529:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method AutoCompleter.on_tab(event: @Todo(specialized non-generic class), shifted: bool) -> str | None`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/tests/test_docs.py:19:64: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/tests/test_docs.py:29:12: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/aboutdialog.py:154:46: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> (() -> object) | None`, found `def get_link_opener(match: @Todo(specialized non-generic class)) -> () -> object`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:231:16: Attribute `value` on type `@Todo(specialized non-generic class) | Unknown | str` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/tabs2spaces.py:31:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def on_tab_key(event: @Todo(generics), shift_pressed: bool) -> str`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/tabs2spaces.py:31:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def on_tab_key(event: @Todo(specialized non-generic class), shift_pressed: bool) -> str`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:529:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `bound method AutoCompleter.on_tab(event: @Todo(generics), shifted: bool) -> str | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/indent_block.py:39:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def on_tab_key(event: @Todo(generics), shifted: bool) -> None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/aboutdialog.py:154:46: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> (() -> object) | None`, found `def get_link_opener(match: @Todo(generics)) -> () -> object`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/pluginloader.py:298:55: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> None`, found `def _handle_circular_dependency(cycle: @Todo(generics)) -> None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:231:16: Attribute `value` on type `@Todo(generics) | Unknown | str` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/indent_block.py:39:40: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Any`, found `def on_tab_key(event: @Todo(specialized non-generic class), shifted: bool) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/pluginloader.py:298:55: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> None`, found `def _handle_circular_dependency(cycle: @Todo(specialized non-generic class)) -> None`

pyinstrument (https://github.com/joerick/pyinstrument)
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:52:31: Operator `+` is unsupported between objects of type `@Todo(generics) | None` and `@Todo(generics) | None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:52:31: Operator `+` is unsupported between objects of type `@Todo(specialized non-generic class) | None` and `@Todo(specialized non-generic class) | None`
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:53:9: Method `__getitem__` of type `@Todo(generics) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:53:9: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:54:9: Method `__getitem__` of type `@Todo(generics) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:54:9: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
- error[lint:not-iterable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:487:32: Object of type `@Todo(generics) | None` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
+ error[lint:not-iterable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:487:32: Object of type `@Todo(specialized non-generic class) | None` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method

async-utils (https://github.com/mikeshardmind/async-utils)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/async-utils/src/async_utils/_paramkey.py:65:1: Object of type `def _make_key(args: @Todo(full tuple[...] support), kwds: @Todo(generics), *, /, _typ: (object, /) -> type = Literal[type], _fast_types: @Todo(generics) = set) -> Hashable` is not assignable to `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Hashable`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/async-utils/src/async_utils/_paramkey.py:65:1: Object of type `def _make_key(args: @Todo(full tuple[...] support), kwds: @Todo(specialized non-generic class), *, /, _typ: (object, /) -> type = Literal[type], _fast_types: @Todo(specialized non-generic class) = set) -> Hashable` is not assignable to `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Hashable`

pyp (https://github.com/hauntsaninja/pyp)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyp/tests/test_find_names.py:33:25: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyp/tests/test_find_names.py:33:25: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound

python-chess (https://github.com/niklasf/python-chess)
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:682:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:682:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:683:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:683:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:686:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:686:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:687:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:687:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:829:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:829:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:830:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:830:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:833:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:833:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:834:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:834:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)]` is not callable on object of type `list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)]` is not callable on object of type `list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1551:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1553:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/pgn.py:137:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `def repl(match: @Todo(generics)) -> str`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/pgn.py:137:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `def repl(match: @Todo(specialized non-generic class)) -> str`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/syzygy.py:446:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1551:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/syzygy.py:447:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1553:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:880:20: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)]` is not callable on object of type `list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:880:20: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1437:44: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1437:44: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1557:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1557:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1558:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1558:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1577:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1577:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1578:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1578:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/syzygy.py:446:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/syzygy.py:447:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:919:25: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:919:25: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-chess/test.py:4008:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(generics)`, found `def raise_expected_error(future) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-chess/test.py:4008:44: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `def raise_expected_error(future) -> Unknown`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-chess/test.py:4015:49: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(generics)`, found `def resolve(future) -> Unknown`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-chess/test.py:4015:49: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `def resolve(future) -> Unknown`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:4755:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:4755:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:4756:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:4756:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:not-iterable] /tmp/mypy_primer/projects/python-chess/chess/engine.py:2349:33: Object of type `@Todo(generics) | None` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
+ error[lint:not-iterable] /tmp/mypy_primer/projects/python-chess/chess/engine.py:2349:33: Object of type `@Todo(specialized non-generic class) | None` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/python-chess/chess/engine.py:2497:40: Operator `in` is not supported for types `@Todo(generics)` and `None`, in comparing `@Todo(generics)` with `str | None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/python-chess/chess/engine.py:2497:40: Operator `in` is not supported for types `@Todo(specialized non-generic class)` and `None`, in comparing `@Todo(specialized non-generic class)` with `str | None`
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-chess/chess/engine.py:3014:34: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(generics)`, found `def background(future: @Todo(generics)) -> @Todo(generic types.CoroutineType)`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-chess/chess/engine.py:3014:34: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(specialized non-generic class)`, found `def background(future: @Todo(specialized non-generic class)) -> @Todo(generic types.CoroutineType)`

psycopg (https://github.com/psycopg/psycopg)
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/psycopg/tests/typing_example.py:32:16: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Person`, found `def mkrow(values: @Todo(specialized non-generic class)) -> Person`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/tests/utils.py:91:17: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/tests/utils.py:92:14: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/tests/utils.py:93:47: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/psycopg/tests/typing_example.py:32:16: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> Person`, found `def mkrow(values: @Todo(generics)) -> Person`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/tests/utils.py:91:17: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/tests/utils.py:92:14: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/psycopg/tests/utils.py:93:47: Attribute `groups` on type `@Todo(specialized non-generic class) | None` is possibly unbound

black (https://github.com/psf/black)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:77:34: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:77:34: Attribute `groups` on type `@Todo(specialized non-generic class) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:77:34: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:77:34: Attribute `groups` on type `@Todo(specialized non-generic class) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:77:34: Attribute `groups` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/blib2to3/pgen2/conv.py:77:34: Attribute `groups` on type `@Todo(specialized non-generic class) | None` is possibly unbound
- error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:511:1: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def main(ctx: Context, code: str | None, line_length: int, target_version: @Todo(generics), check: bool, diff: bool, line_ranges: @Todo(generics), color: bool, fast: bool, pyi: bool, ipynb: bool, python_cell_magics: @Todo(generics), skip_source_first_line: bool, skip_string_normalization: bool, skip_magic_trailing_comma: bool, preview: bool, unstable: bool, enable_unstable_feature: @Todo(generics), quiet: bool, verbose: bool, required_version: str | None, include: @Todo(generics), exclude: @Todo(generics) | None, extend_exclude: @Todo(generics) | None, force_exclude: @Todo(generics) | None, stdin_filename: str | None, workers: int | None, src: @Todo(full tuple[...] support), config: str | None) -> None`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/black/src/black/__init__.py:511:1: Argument to this function is incorrect: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `def main(ctx: Context, code: str | None, line_length: int, target_version: @Todo(specialized non-generic class), check: bool, diff: bool, line_ranges: @Todo(specialized non-generic class), color: bool, fast: bool, pyi: bool, ipynb: bool, python_cell_magics: @Todo(specialized non-generic class), skip_source_first_line: bool, skip_string_normalization: bool, skip_magic_trailing_comma: bool, preview: bool, unstable: bool, enable_unstable_feature: @Todo(specialized non-generic class), quiet: bool, verbose: bool, required_version: str | None, include: @Todo(specialized non-generic class), exclude: @Todo(specialized non-generic class) | None, extend_exclude: @Todo(specialized non-generic class) | None, force_exclude: @Todo(specialized non-generic class) | None, stdin_filename: str | None, workers: int | None, src: @Todo(full tuple[...] support), config: str | None) -> None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:602:37: Attribute `items` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:602:37: Attribute `items` on type `@Todo(specialized non-generic class) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:602:37: Attribute `items` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:602:37: Attribute `items` on type `@Todo(specialized non-generic class) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:602:37: Attribute `items` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/black/src/black/__init__.py:602:37: Attribute `items` on type `@Todo(specialized non-generic class) | None` is possibly unbound

rich (https://github.com/Textualize/rich)
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:1020:5: Attribute `close` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/rich/tests/test_console.py:1020:5: Attribute `close` on type `@Todo(specialized non-generic class) | None` is possibly unbound

werkzeug (https://github.com/pallets/werkzeug)
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:181:13: Object of type `def default_stream_factory(total_content_length: int | None, content_type: str | None, filename: str | None, content_length: int | None = None) -> @Todo(generics)` is not assignable to `TStreamFactory | None`
- error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:305:13: Object of type `def default_stream_factory(total_content_length: int | None, content_type: str | None, filename: str | None, content_length: int | None = None) -> @Todo(generics)` is not assignable to `TStreamFactory | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:181:13: Object of type `def default_stream_factory(total_content_length: int | None, content_type: str | None, filename: str | None, content_length: int | None = None) -> @Todo(specialized non-generic class)` is not assignable to `TStreamFactory | None`
+ error[lint:invalid-assignment] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/formparser.py:305:13: Object of type `def default_stream_factory(total_content_length: int | None, content_type: str | None, filename: str | None, content_length: int | None = None) -> @Todo(specialized non-generic class)` is not assignable to `TStreamFactory | None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/routing/exceptions.py:102:50: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | @Todo(specialized non-generic class) & None | None | @Todo(set comprehension type)`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/routing/exceptions.py:102:50: Operator `in` is not supported for types `Unknown` and `None`, in comparing `Unknown` with `Unknown | @Todo(generics) & None | None | @Todo(set comprehension type)`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/test.py:1049:17: Attribute `close` on type `@Todo(specialized non-generic class) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/local.py:506:23: Attribute `top` on type `@Todo(generics) | Local | (() -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions))` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/local.py:506:23: Attribute `top` on type `@Todo(specialized non-generic class) | Local | (() -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions))` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/local.py:517:27: Attribute `get` on type `@Todo(generics) | Local | (() -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions))` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/local.py:517:27: Attribute `get` on type `@Todo(specialized non-generic class) | Local | (() -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions))` is possibly unbound
- error[lint:invalid-return-type] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/repr.py:117:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `def proxy(self: DebugReprGenerator, obj: @Todo(generics), recursive: bool) -> str`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/debug/repr.py:117:12: Return type does not match returned value: Expected `(*args: @Todo(todo signature *args), **kwargs: @Todo(todo signature **kwargs)) -> str`, found `def proxy(self: DebugReprGenerator, obj: @Todo(specialized non-generic class), recursive: bool) -> str`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_test.py:166:12: Attribute `getvalue` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_test.py:168:12: Attribute `getvalue` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:601:5: Attribute `add` on type `Unknown | @Todo(specialized non-generic class) & None | None | @Todo(set comprehension type)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:603:5: Attribute `discard` on type `Unknown | @Todo(specialized non-generic class) & None | None | @Todo(set comprehension type)` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:604:5: Method `__getitem__` of type `Unknown | @Todo(specialized non-generic class) | None` is possibly unbound
- error[lint:not-iterable] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/serving.py:273:35: Object of type `@Todo(generics) | None` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
+ error[lint:not-iterable] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/serving.py:273:35: Object of type `@Todo(specialized non-generic class) | None` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/src/werkzeug/test.py:1049:17: Attribute `close` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:601:5: Attribute `add` on type `Unknown | @Todo(generics) & None | None | @Todo(set comprehension type)` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:603:5: Attribute `discard` on type `Unknown | @Todo(generics) & None | None | @Todo(set comprehension type)` is possibly unbound
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/werkzeug/tests/test_routing.py:604:5: Method `__getitem__` of type `Unknown | @Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_test.py:166:12: Attribute `getvalue` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/werkzeug/tests/test_test.py:168:12: Attribute `getvalue` on type `@Todo(generics) | None` is possibly unbound
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:208:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["int("]` with `@Todo(generics) & Unknown & str | @Todo(generics) & Any & str | @Todo(generics) & str | @Todo(generics) & Unknown & None | @Todo(generics) & Any & None | @Todo(generics) & None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/werkzeug/tests/test_local.py:208:12: Operator `in` is not supported for types `str` and `None`, in comparing `Literal["int("]` with `@Todo(specialized non-generic class) & Unknown & str | @Todo(specialized non-generic class) & Any & str | @Todo(specialized non-generic class) & str | @Todo(specialized non-generic class) & Unknown & None | @Todo(specialized non-generic class) & Any & None | @Todo(specialized non-generic class) & None`

scrapy (https://github.com/scrapy/scrapy)
+ error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_command_check.py:198:9: Type `bound method CrawlerProcess.crawl(crawler_or_spidercls: type[Spider] | str | Crawler, *args: Any, **kwargs: Any) -> @Todo(unknown type subscript)` has no attribute `assert_not_called`
- warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/scrapy/tests/test_dependencies.py:38:41: Method `__getitem__` of type `@Todo(generics) | None` is possibly unbound
- error[lint:not-iterable] /tmp/mypy_primer/projects/scrapy/scrapy/http/response/text.py:277:24: Object of type `@Todo(generics) | Unknown | None` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/scrapy/scrapy/spidermiddlewares/httperror.py:62:12: Operator `in` is not supported for types `int` and `None`, in comparing `int` with `@Todo(generics) | Any | None`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_proxy_connect.py:51:21: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- error[lint:unresolved-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_command_check.py:198:9: Type `bound method CrawlerProcess.crawl(crawler_or_spidercls: type[Spider] | str | Crawler, *args: Any, **kwargs: Any) -> @Todo(generics)` has no attribute `assert_not_called`
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/feedexport.py:542:17: Attribute `close` on type `@Todo(generics) | None` is possibly unbound
- warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/feedexport.py:543:24: Attribute `file` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/scrapy/tests/test_dependencies.py:38:41: Method `__getitem__` of type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/feedexport.py:542:17: Attribute `close` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/scrapy/extensions/feedexport.py:543:24: Attribute `file` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/scrapy/tests/test_proxy_connect.py:51:21: Attribute `group` on type `@Todo(specialized non-generic class) | None` is possibly unbound
+ error[lint:not-iterable] /tmp/mypy_primer/projects/scrapy/scrapy/http/response/text.py:277:24: Object of type `@Todo(specialized non-generic class) | Unknown | None` may not be iterable because it may not have an `__iter__` method or a `__getitem__` method
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/scrapy/scrapy/spidermiddlewares/httperror.py:62:12: Operator `in` is not supported for types `int` and `None`, in comparing `int` with `@Todo(specialized non-generic class) | Any | None`

```
</details>


---

_@dcreager reviewed on 2025-04-16 19:35_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:350 on 2025-04-16 19:35_

Should this one be assignable?

---

_Comment by @dcreager on 2025-04-16 19:42_

6 new `mypy_primer` diagnostics:

- 5 depend on bidirectional type-checking and/or supporting legacy `TypeVar`s in typeshed
- 1 looks like we're not narrowing `int | None` to `int` in a `while` statement?

---

_Comment by @carljm on 2025-04-16 23:14_

> * 1 looks like we're not narrowing `int | None` to `int` in a `while` statement?

Yeah I don't think we support narrowing the type of `x` based on the use of `(x := ...)` in a predicate yet, but we should: https://github.com/astral-sh/ruff/issues/17437

---

_Comment by @carljm on 2025-04-16 23:16_

> * 5 depend on bidirectional type-checking and/or supporting legacy `TypeVar`s in typeshed

This may be involved in us inferring an unknown specialization when we shouldn't, but it seems to me that those false positives are also demonstrating a simpler issue, which is that we should in general consider `C[Unknown]` assignable to `C[int]`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:350 on 2025-04-16 23:21_

Yes, it should be, due to the rules of materialization: that `Any` can materialize to `int`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:134 on 2025-04-16 23:21_

These should, however, be `gradual_equivalent_to` and `assignable_to` (in both directions.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:271 on 2025-04-16 23:33_

This is really a test of variance, which is opening another whole can of worms we'll have to handle soon. This subtype relation should be true if `A` is contravariant in `T` (because that means that `A[bool]` is a supertype of `A[int]`), false if it is invariant or covariant.

Variance for PEP 695 typevars is determined by inference, based on the positions in which the typevar occurs in the definition of the class. If the typevar occurs in covariant positions only (that is, method return types, including the implicit getter "method" of every normal mutable attribute) it is covariant, if it occurs in contravariant positions only (accepted as a method argument type, including the implicit setter "method" of every normal mutable attribute) then it is contravariant, and if it appears in both, it is invariant. (Note that this means a type variable used as the type of a normal mutable attribute must be invariant, since it appears in both covariant and contravariant position via the implicit getter and setter of that mutable attribute.) Also note that callable types can invert variance of a position: that is, a typevar occurring as argument to a `Callable` type accepted as a method argument is in covariant position. The method argument is a contravariant position, but the argument of the `Callable` type is also a contravariant position, so this reverses the variance and that typevar is actually occurring in a covariant position.

A class like `A` which never uses its typevar in any position at all can safely be considered "bivariant" (in plain English, the type of the typevar has no impact whatsoever on the class, so it just doesn't matter to subtyping at all!)

So all that said: technically, given the code we see here, this assertion is wrong: `B` should be a subtype of `A[bool]`, because `A` is bivariant in `T`, which means that any specialization of `A` is a subtype of any other specialization of `A` -- that is, all specializations of `A` are equivalent.

As far as _this_ PR is concerned, the only thing I would suggest is a TODO comment on this assertion :)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:349 on 2025-04-16 23:37_

This assertion is technically wrong and should get a TODO comment: see my other comment about variance.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7192 on 2025-04-16 23:42_

This might want an update?

---

_@carljm approved on 2025-04-16 23:44_

Love how simple this was to implement!

Looks like the main follow-up this reveals is not in the code touched in this PR, but more like a knock-on effect of supporting this: we need a more sophisticated understanding of subtyping (variance) and assignability (dynamic types) for specialized generics. Both are important but the latter is probably higher priority and simpler to implement.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:271 on 2025-04-17 18:25_

Added a bunch of tests for each of the four variance cases, and for each of subtyping, assignability, equivalence, and gradual equivalence.  All TODOs for now where they're not producing the right answer.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:134 on 2025-04-17 18:25_

Added new tests

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:350 on 2025-04-17 18:26_

Done (in new tests)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:349 on 2025-04-17 18:26_

Done (in new tests)

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:7192 on 2025-04-17 18:28_

Done. I got rid of the todo type completely, and instead added new diagnostics for when (a) you try to specialize a non-generic class, and (b) when you use any other subscript expression in a type expression.  Added quite a bit of churn in the mdtest suite, but I think it's worth it.

---

_@dcreager reviewed on 2025-04-17 18:57_

I anticipate that there's about to a lot of new false positives on the ecosystem check, since we're now printing out diagnostics when you try to specialize a non-generic class — which will be (incorrectly) reported for basically every generic class in the typeshed, since we don't yet recognize them as generic since they use legacy typevars.

---

_Comment by @github-actions[bot] on 2025-04-17 19:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @sharkdp on 2025-04-17 19:50_

> I anticipate that there's about to a lot of new false positives on the ecosystem check

4153 new `invalid-type-form` false positives, to be precise :upside_down_face: 

---

_Comment by @dcreager on 2025-04-17 19:57_

> 4153 new `invalid-type-form` false positives, to be precise

Yeahhhhh, I'm going to back that out and wait to add those diagnostics until we understand that `list` is generic... :sweat_smile: 

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:46 on 2025-04-17 20:35_

These should both be assignable, regardless of variance, because `Any` can materialize to either `A` or `B`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:69 on 2025-04-17 20:37_

Maybe it's getting slightly out of the realm of testing variance, but it seems useful to show what _is_ equivalent and gradually equivalent. E.g. `is_equivalent_to(C[B], C[B])`, and `is_gradual_equivalent_to(C[Unknown], C[Any])`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:101 on 2025-04-17 20:38_

These should also be assignable, because with `Any` we are dealing with materialization (where `Any` can act as any possible type), so variance isn't even relevant.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:169 on 2025-04-17 20:39_

And these should also be assignable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:190 on 2025-04-17 20:40_

Again here, seems useful to show some positive cases.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:225 on 2025-04-17 20:41_

These should be assignable.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7201 on 2025-04-17 20:44_

Trying to understand what case this is handling -- did it come up in ecosystem checks? It seems like it would be permitting an annotation like `"list"[str]`, but AFAIK we don't need to support that, it isn't allowed. Pyright [doesn't allow it](https://pyright-play.net/?strict=true&enableExperimentalFeatures=true&code=CYUwZgBGAUAeBcEBEAbAlgZwC5INpoDssBdASggFoA%2BCQreAKAmYgCcQsBXVgiWXAAzEGDIA). Am I misunderstand what this is for?

---

_@carljm approved on 2025-04-17 20:45_

I think there are a few assertions around non-fully-static generics that we need to fix up here, but otherwise looks great!

---

_@dcreager reviewed on 2025-04-18 12:53_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:7201 on 2025-04-18 12:53_

We have this in our mdtests, e.g. here

https://github.com/astral-sh/ruff/blob/b63de60399f77afb4d5e0032d60b59c186b70cfb/crates/red_knot_python_semantic/resources/mdtest/generics/classes.md?plain=1#L166-L168

Should we change that to `"C[T]"` per pyright's suggestion?

---

_@dcreager reviewed on 2025-04-18 14:12_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:7201 on 2025-04-18 14:12_

Changed to `"C[T]"` and verified that the noisy new diagnostics didn't fire after removing the `StringLiteral` match arm.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:46 on 2025-04-18 14:20_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:101 on 2025-04-18 14:20_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:169 on 2025-04-18 14:21_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:225 on 2025-04-18 14:21_

Done

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:69 on 2025-04-18 14:49_

Added to all four sections

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/generics/variance.md`:190 on 2025-04-18 14:49_

Added

---

_@dcreager reviewed on 2025-04-18 14:52_

---

_@carljm reviewed on 2025-04-18 15:13_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:7201 on 2025-04-18 15:13_

It looks like [mypy does support this](https://mypy-play.net/?mypy=latest&python=3.12&gist=da9a197f4b774d839a106518e1ab905d). But I don't feel like we should go out of our way to support it, IMO it's an odd thing to do, and the alternative (quote the entire annotation) is easy and the more obvious choice anyway. So I support what you've done here.

---

_Merged by @dcreager on 2025-04-18 15:49_

---

_Closed by @dcreager on 2025-04-18 15:49_

---

_Branch deleted on 2025-04-18 15:49_

---

_@carljm approved on 2025-04-18 15:51_

Latest additions here look good!

---
