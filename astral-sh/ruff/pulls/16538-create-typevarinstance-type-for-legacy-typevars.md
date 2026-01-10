```yaml
number: 16538
title: "Create `TypeVarInstance` type for legacy typevars"
type: pull_request
state: merged
author: dcreager
labels:
  - ty
assignees: []
merged: true
base: main
head: dcreager/legacy-typevar-instance
created_at: 2025-03-06T16:23:56Z
updated_at: 2025-04-29T14:15:29Z
url: https://github.com/astral-sh/ruff/pull/16538
synced_at: 2026-01-10T19:03:00Z
```

# Create `TypeVarInstance` type for legacy typevars

---

_Pull request opened by @dcreager on 2025-03-06 16:23_

We are currently representing type variables using a `KnownInstance` variant, which wraps a `TypeVarInstance` that contains the information about the typevar (name, bounds, constraints, default type).  We were previously only constructing that type for PEP 695 typevars.  This PR constructs that type for legacy typevars as well.

It also detects functions that are generic because they use legacy typevars in their parameter list. With the existing logic for inferring specializations of function calls (#17301), that means that we are correctly detecting that the definition of `reveal_type` in the typeshed is generic, and inferring the correct specialization of `_T` for each call site.

This does not yet handle legacy generic classes; that will come in a follow-on PR.

---

_Comment by @dcreager on 2025-03-06 16:25_

Leaving this as draft for now; there's one mdtest regression (an incorrect `unresolved-reference` error) that I still have to track down.

---

_Label `red-knot` added by @AlexWaygood on 2025-03-06 17:40_

---

_Comment by @github-actions[bot] on 2025-03-20 15:18_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
+ error[lint:no-matching-overload] src/packaging/_manylinux.py:195:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/packaging/_manylinux.py:198:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/packaging/_manylinux.py:201:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/packaging/_manylinux.py:231:22: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/packaging/_manylinux.py:234:26: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/packaging/_manylinux.py:235:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/packaging/_musllinux.py:30:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/packaging/_parser.py:83:12: No overload of bound method `__init__` matches arguments
- warning[lint:unused-ignore-comment] src/packaging/metadata.py:439:65: Unused blanket `type: ignore` directive
+ warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `str | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `str | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ warning[lint:possibly-unbound-attribute] src/packaging/metadata.py:576:13: Attribute `params` on type `str | @Todo(Support for `typing.TypeAlias`)` is possibly unbound
+ error[lint:no-matching-overload] src/packaging/version.py:205:25: No overload of bound method `__init__` matches arguments
- Found 22 diagnostics
+ Found 33 diagnostics

nionutils (https://github.com/nion-software/nionutils)
+ error[lint:invalid-argument-type] nion/utils/test/ListModel_test.py:510:33: Argument to this function is incorrect: Argument type `FlattenedListModel` does not satisfy upper bound of type variable `_SupportsCloseT`
- Found 33 diagnostics
+ Found 34 diagnostics

bidict (https://github.com/jab/bidict)
+ error[lint:no-matching-overload] bidict/_dup.py:57:34: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] bidict/_dup.py:59:32: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] bidict/_dup.py:61:35: No overload of bound method `__init__` matches arguments
- Found 4 diagnostics
+ Found 7 diagnostics

paroxython (https://github.com/laowantong/paroxython)
+ error[lint:no-matching-overload] paroxython/derived_labels_db.py:189:38: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/label_programs.py:49:37: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/list_programs.py:89:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/parse_program.py:76:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/parse_program.py:284:22: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/parse_program.py:289:77: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/parse_program.py:316:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/parse_program.py:333:31: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/parse_program.py:366:15: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] paroxython/preprocess_source.py:455:40: No overload of bound method `__init__` matches arguments
- Found 34 diagnostics
+ Found 44 diagnostics

parso (https://github.com/davidhalter/parso)
+ error[lint:invalid-argument-type] parso/__init__.py:57:28: Argument to this function is incorrect: Expected `str`, found `None`
+ error[lint:no-matching-overload] parso/python/diff.py:495:27: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/diff.py:508:31: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:228:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:289:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:381:23: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:385:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:429:23: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:445:31: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:508:31: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:518:23: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:528:23: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:538:27: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:546:27: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:554:27: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:564:27: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:594:27: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:597:23: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:621:23: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:624:15: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:631:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:644:15: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:645:11: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/python/tokenize.py:650:16: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/utils.py:132:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] parso/utils.py:180:12: No overload of bound method `__init__` matches arguments
- Found 80 diagnostics
+ Found 106 diagnostics

pegen (https://github.com/we-like-parsers/pegen)
- error[lint:unresolved-attribute] src/pegen/parser.py:85:19: Type `(@Todo(Support for `typing.TypeVar` instances in type expressions), /) -> @Todo(Support for `typing.TypeVar` instances in type expressions) | None` has no attribute `__name__`
+ error[lint:unresolved-attribute] src/pegen/parser.py:28:19: Type `F` has no attribute `__name__`
+ error[lint:call-non-callable] src/pegen/parser.py:32:20: Object of type `F` is not callable
+ error[lint:call-non-callable] src/pegen/parser.py:37:16: Object of type `F` is not callable
+ error[lint:unresolved-attribute] src/pegen/parser.py:48:19: Type `F` has no attribute `__name__`
+ error[lint:call-non-callable] src/pegen/parser.py:66:20: Object of type `F` is not callable
+ error[lint:unresolved-attribute] src/pegen/parser.py:85:19: Type `(P, /) -> T | None` has no attribute `__name__`
+ error[lint:invalid-argument-type] src/pegen/parser.py:332:45: Argument to this function is incorrect: Expected `() -> str`, found `(bound method TextIO.readline(limit: int = Literal[-1], /) -> AnyStr) | @Todo(Support for `typing.TypeAlias`)`
- Found 52 diagnostics
+ Found 58 diagnostics

attrs (https://github.com/python-attrs/attrs)
+ error[lint:no-matching-overload] src/attr/_make.py:480:12: No overload of bound method `__init__` matches arguments
- info[revealed-type] tests/dataclass_transform_example.py:21:1: Revealed type: `(with_converter: int = @Todo(Support for `typing.TypeVar` instances in type expressions)) -> None`
+ info[revealed-type] tests/dataclass_transform_example.py:21:1: Revealed type: `(with_converter: int = Unknown) -> None`
+ error[lint:invalid-argument-type] tests/test_annotations.py:123:28: Argument to this function is incorrect: Argument type `Literal[C]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:416:28: Argument to this function is incorrect: Argument type `Literal[C]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:518:28: Argument to this function is incorrect: Argument type `Literal[C]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:540:28: Argument to this function is incorrect: Argument type `Literal[C]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:548:28: Argument to this function is incorrect: Argument type `Literal[D]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:565:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:599:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:619:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:620:28: Argument to this function is incorrect: Argument type `Literal[B]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:667:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:668:28: Argument to this function is incorrect: Argument type `Literal[B]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:683:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_annotations.py:687:28: Argument to this function is incorrect: Argument type `Literal[A]` does not satisfy upper bound of type variable `_A`
+ error[lint:no-matching-overload] tests/test_funcs.py:251:22: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_funcs.py:253:27: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_funcs.py:455:22: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/test_funcs.py:457:17: No overload of bound method `__init__` matches arguments
+ error[lint:unresolved-attribute] tests/test_make.py:144:14: Type `Literal[42]` has no attribute `default`
+ error[lint:no-matching-overload] tests/test_make.py:204:16: No overload of bound method `__init__` matches arguments
+ error[lint:invalid-argument-type] tests/test_make.py:1242:32: Argument to this function is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
+ error[lint:invalid-argument-type] tests/test_make.py:1253:32: Argument to this function is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
- error[lint:invalid-argument-type] tests/test_validators.py:228:32: Argument to this function is incorrect: Expected `((@Todo(Support for `typing.TypeVar` instances in type expressions), @Todo(Support for `typing.TypeVar` instances in type expressions), int, /) -> @Todo(specialized non-generic class) | None) | None`, found `() -> Unknown`
+ error[lint:invalid-argument-type] tests/test_validators.py:228:32: Argument to this function is incorrect: Expected `((Literal["a"], Literal["a"], int, /) -> @Todo(specialized non-generic class) | None) | None`, found `() -> Unknown`
+ error[lint:invalid-argument-type] tests/typing_example.py:490:28: Argument to this function is incorrect: Argument type `type` does not satisfy upper bound of type variable `_A`
- Found 674 diagnostics
+ Found 697 diagnostics

more-itertools (https://github.com/more-itertools/more-itertools)
- error[lint:invalid-argument-type] more_itertools/more.py:2441:38: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Unknown | Literal[bool]`
+ error[lint:invalid-argument-type] more_itertools/more.py:2441:38: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Unknown | Literal[bool]`
- error[lint:invalid-argument-type] more_itertools/more.py:2997:44: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[repeat]`
+ error[lint:invalid-argument-type] more_itertools/more.py:2997:44: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[repeat]`
- error[lint:invalid-argument-type] more_itertools/recipes.py:98:27: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[zip]`
+ error[lint:invalid-argument-type] more_itertools/recipes.py:98:27: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[zip]`
- error[lint:invalid-argument-type] more_itertools/recipes.py:897:22: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[slice]`
+ error[lint:invalid-argument-type] more_itertools/recipes.py:897:22: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[slice]`

pytest-robotframework (https://github.com/detachhead/pytest-robotframework)
+ error[lint:no-matching-overload] pytest_robotframework/__init__.py:502:12: No overload of function `keyword` matches arguments
+ error[lint:no-matching-overload] pytest_robotframework/__init__.py:569:9: No overload of function `keyword` matches arguments
- error[lint:missing-argument] pytest_robotframework/__init__.py:502:12: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] pytest_robotframework/__init__.py:503:9: Argument `name` does not match any known parameter of function `keyword`
- error[lint:unknown-argument] pytest_robotframework/__init__.py:503:20: Argument `tags` does not match any known parameter of function `keyword`
- error[lint:unknown-argument] pytest_robotframework/__init__.py:503:31: Argument `module` does not match any known parameter of function `keyword`
- error[lint:unknown-argument] pytest_robotframework/__init__.py:503:46: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:missing-argument] pytest_robotframework/__init__.py:569:9: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] pytest_robotframework/__init__.py:569:17: Argument `name` does not match any known parameter of function `keyword`
- error[lint:unknown-argument] pytest_robotframework/__init__.py:569:28: Argument `tags` does not match any known parameter of function `keyword`
- error[lint:unknown-argument] pytest_robotframework/__init__.py:569:39: Argument `module` does not match any known parameter of function `keyword`
- error[lint:unknown-argument] pytest_robotframework/__init__.py:569:54: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:missing-argument] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_doesnt_suppress.py:14:2: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_doesnt_suppress.py:14:10: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:missing-argument] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_raises_in_body_and_exit.py:14:2: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_raises_in_body_and_exit.py:14:10: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:missing-argument] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_raises_in_exit.py:14:2: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/fixtures/test_python/test_keyword_decorator_context_manager_that_raises_in_exit.py:14:10: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:missing-argument] tests/fixtures/test_python/test_keyword_decorator_custom_name_and_tags.py:6:2: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/fixtures/test_python/test_keyword_decorator_custom_name_and_tags.py:6:10: Argument `name` does not match any known parameter of function `keyword`
- error[lint:unknown-argument] tests/fixtures/test_python/test_keyword_decorator_custom_name_and_tags.py:6:26: Argument `tags` does not match any known parameter of function `keyword`
- error[lint:missing-argument] tests/fixtures/test_python/test_keyword_decorator_returns_context_manager_that_isnt_used.py:14:2: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/fixtures/test_python/test_keyword_decorator_returns_context_manager_that_isnt_used.py:14:10: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:missing-argument] tests/type_tests.py:18:6: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/type_tests.py:18:14: Argument `name` does not match any known parameter of function `keyword`
- error[lint:unknown-argument] tests/type_tests.py:18:30: Argument `tags` does not match any known parameter of function `keyword`
- error[lint:type-assertion-failure] tests/type_tests.py:21:9: Actual type `Unknown` is not the same as asserted type `() -> None`
+ error[lint:type-assertion-failure] tests/type_tests.py:21:9: Actual type `Never` is not the same as asserted type `() -> None`
- error[lint:missing-argument] tests/type_tests.py:36:6: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/type_tests.py:36:14: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:type-assertion-failure] tests/type_tests.py:41:9: Actual type `Unknown` is not the same as asserted type `() -> @Todo(specialized non-generic class)`
+ error[lint:type-assertion-failure] tests/type_tests.py:41:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> @Todo(specialized non-generic class)`
- error[lint:missing-argument] tests/type_tests.py:44:6: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/type_tests.py:44:14: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:type-assertion-failure] tests/type_tests.py:49:9: Actual type `Unknown` is not the same as asserted type `() -> @Todo(specialized non-generic class)`
+ error[lint:type-assertion-failure] tests/type_tests.py:49:9: Actual type `(...) -> Unknown` is not the same as asserted type `() -> @Todo(specialized non-generic class)`
- error[lint:missing-argument] tests/type_tests.py:61:6: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/type_tests.py:61:14: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- error[lint:missing-argument] tests/type_tests.py:66:6: No argument provided for required parameter `fn` of function `keyword`
- error[lint:unknown-argument] tests/type_tests.py:66:14: Argument `wrap_context_manager` does not match any known parameter of function `keyword`
- Found 352 diagnostics
+ Found 322 diagnostics

anyio (https://github.com/agronholm/anyio)
- error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[DatagramProtocol]`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> Unknown`, found `Literal[DatagramProtocol]`
- error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[DatagramProtocol]`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> Unknown`, found `Literal[DatagramProtocol]`
- error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[DatagramProtocol]`
+ error[lint:invalid-argument-type] src/anyio/_backends/_asyncio.py:2634:13: Argument to this function is incorrect: Expected `() -> Unknown`, found `Literal[DatagramProtocol]`
+ error[lint:invalid-argument-type] src/anyio/_core/_tempfile.py:299:21: Argument to this function is incorrect: Argument type `BytesIO` does not satisfy upper bound of type variable `_BufferT_co`
+ error[lint:invalid-argument-type] src/anyio/_core/_tempfile.py:299:21: Argument to this function is incorrect: Expected `_WrappedBuffer`, found `BytesIO`
- warning[lint:redundant-cast] src/anyio/from_thread.py:290:16: Value is already of type `Unknown`
+ error[lint:no-matching-overload] src/anyio/streams/memory.py:62:16: No overload of bound method `__init__` matches arguments
+ error[lint:invalid-return-type] src/anyio/streams/memory.py:124:24: Return type does not match returned value: Expected `T_co`, found `T_Item`
- Found 137 diagnostics
+ Found 140 diagnostics

starlette (https://github.com/encode/starlette)
+ error[lint:no-matching-overload] starlette/requests.py:154:20: No overload of bound method `__init__` matches arguments
- warning[lint:possibly-unbound-attribute] starlette/requests.py:114:20: Attribute `endswith` on type `@Todo(Support for `typing.TypeVar` instances in type expressions) | None` is possibly unbound
- error[lint:unsupported-operator] starlette/requests.py:115:17: Operator `+=` is unsupported between objects of type `None` and `Literal["/"]`
+ error[lint:no-matching-overload] starlette/schemas.py:60:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] starlette/schemas.py:77:43: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] starlette/schemas.py:84:43: No overload of bound method `__init__` matches arguments
- error[lint:invalid-argument-type] tests/conftest.py:20:9: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[TestClient]`
+ error[lint:invalid-argument-type] tests/conftest.py:20:9: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[TestClient]`
- error[lint:type-assertion-failure] tests/test_config.py:23:5: Actual type `Unknown` is not the same as asserted type `str | None`
+ error[lint:type-assertion-failure] tests/test_config.py:23:5: Actual type `None` is not the same as asserted type `str | None`
- error[lint:type-assertion-failure] tests/test_config.py:24:5: Actual type `Unknown` is not the same as asserted type `str`
+ error[lint:type-assertion-failure] tests/test_config.py:24:5: Actual type `Literal[""]` is not the same as asserted type `str`
- error[lint:type-assertion-failure] tests/test_config.py:27:5: Actual type `Unknown` is not the same as asserted type `bool`
+ error[lint:type-assertion-failure] tests/test_config.py:27:5: Actual type `Literal[False]` is not the same as asserted type `bool`
- error[lint:type-assertion-failure] tests/test_config.py:28:5: Actual type `Unknown` is not the same as asserted type `bool | None`
+ error[lint:type-assertion-failure] tests/test_config.py:28:5: Actual type `None` is not the same as asserted type `bool | None`
- Found 197 diagnostics
+ Found 199 diagnostics

kornia (https://github.com/kornia/kornia)
+ error[lint:no-matching-overload] kornia/augmentation/container/ops.py:138:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/augmentation/container/patch.py:268:26: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:131:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:132:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:133:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:134:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:141:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:142:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:143:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kornia/contrib/models/rt_detr/architecture/hgnetv2.py:144:21: No overload of bound method `__init__` matches arguments
- error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:336:26: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[NestedTensorBlock]`
+ error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:336:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[NestedTensorBlock]`
- error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:350:26: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[NestedTensorBlock]`
+ error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:350:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[NestedTensorBlock]`
- error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:364:26: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[NestedTensorBlock]`
+ error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:364:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[NestedTensorBlock]`
- error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:378:26: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[NestedTensorBlock]`
+ error[lint:invalid-argument-type] kornia/feature/dedode/transformer/dinov2.py:378:26: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[NestedTensorBlock]`
+ error[lint:no-matching-overload] kornia/feature/sold2/backbones.py:59:23: No overload of bound method `__init__` matches arguments
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:172:17: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:31: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:44: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
+ error[lint:call-non-callable] kornia/filters/kernels_geometry.py:203:57: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int | float, int | float, int | float]`
- error[lint:call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- error[lint:call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:349:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:352:21: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:355:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
+ error[lint:call-non-callable] kornia/geometry/boxes.py:358:22: Method `__getitem__` of type `Unknown | (Overload[(key: SupportsIndex, /) -> _T_co, (key: slice, /) -> @Todo(full tuple[...] support)])` is not callable on object of type `Unknown | tuple[int, int] | None`
- Found 1003 diagnostics
+ Found 1014 diagnostics

aioredis (https://github.com/aio-libs/aioredis)
+ error[lint:no-matching-overload] aioredis/client.py:337:25: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] aioredis/client.py:462:38: No overload of function `__new__` matches arguments
- Found 29 diagnostics
+ Found 31 diagnostics

porcupine (https://github.com/Akuli/porcupine)
+ error[lint:invalid-return-type] porcupine/actions.py:127:12: Return type does not match returned value: Expected `(FileTab, /) -> bool`, found `partial`
+ error[lint:invalid-argument-type] porcupine/plugins/autocomplete.py:544:43: Argument to this function is incorrect: Expected `(EventWithData, /) -> str | None`, found `partial`
+ error[lint:no-matching-overload] porcupine/plugins/filemanager.py:242:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] porcupine/plugins/filemanager.py:247:19: No overload of bound method `__init__` matches arguments
- error[lint:invalid-return-type] porcupine/plugins/pastebin.py:192:16: Return type does not match returned value: Expected `str`, found `@Todo(Support for `typing.TypeVar` instances in type expressions) | None`
+ error[lint:invalid-return-type] porcupine/plugins/pastebin.py:192:16: Return type does not match returned value: Expected `str`, found `Unknown | None`
+ error[lint:invalid-argument-type] porcupine/plugins/pastebin.py:385:48: Argument to this function is incorrect: Expected `(bool, str | Unknown, /) -> None`, found `partial`
+ error[lint:invalid-argument-type] porcupine/plugins/python_tools.py:69:9: Argument to this function is incorrect: Expected `(FileTab, /) -> None`, found `partial`
+ error[lint:invalid-argument-type] porcupine/plugins/python_tools.py:76:9: Argument to this function is incorrect: Expected `(FileTab, /) -> None`, found `partial`
+ error[lint:invalid-return-type] porcupine/plugins/run/no_terminal.py:421:16: Return type does not match returned value: Expected `(() -> None) | None`, found `partial`
+ error[lint:invalid-argument-type] porcupine/plugins/urls.py:84:64: Argument to this function is incorrect: Expected `() -> None`, found `partial`
+ error[lint:no-matching-overload] porcupine/tabs.py:999:16: No overload of bound method `__init__` matches arguments
+ error[lint:invalid-argument-type] porcupine/utils.py:796:38: Argument to this function is incorrect: Expected `str | _T`, found `str | None`
- Found 157 diagnostics
+ Found 168 diagnostics

pylox (https://github.com/sco1/pylox)
+ error[lint:no-matching-overload] pylox/preprocessor.py:90:32: No overload of bound method `__init__` matches arguments
- error[lint:invalid-argument-type] tests/scanning/test_identifiers.py:18:25: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[Token]`
+ error[lint:invalid-argument-type] tests/scanning/test_identifiers.py:18:25: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[Token]`
- error[lint:invalid-argument-type] tests/scanning/test_keywords.py:17:25: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[Token]`
+ error[lint:invalid-argument-type] tests/scanning/test_keywords.py:17:25: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[Token]`
- error[lint:invalid-argument-type] tests/scanning/test_punctuators.py:17:25: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[Token]`
+ error[lint:invalid-argument-type] tests/scanning/test_punctuators.py:17:25: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[Token]`
- Found 41 diagnostics
+ Found 42 diagnostics

sockeye (https://github.com/awslabs/sockeye)
- error[lint:invalid-argument-type] sockeye/inference.py:350:50: Argument to this function is incorrect: Expected `Sized`, found `(@Todo(Support for `typing.TypeVar` instances in type expressions) & ~list) | None | @Todo(list comprehension type)`
+ error[lint:invalid-argument-type] sockeye/inference.py:350:50: Argument to this function is incorrect: Expected `Sized`, found `(Unknown & ~list) | None | @Todo(list comprehension type)`
- error[lint:invalid-argument-type] sockeye/inference.py:352:34: Argument to this function is incorrect: Expected `Sized`, found `(@Todo(Support for `typing.TypeVar` instances in type expressions) & ~list) | None | @Todo(list comprehension type)`
+ error[lint:invalid-argument-type] sockeye/inference.py:352:34: Argument to this function is incorrect: Expected `Sized`, found `(Unknown & ~list) | None | @Todo(list comprehension type)`
- error[lint:invalid-argument-type] sockeye/test_utils.py:224:34: Argument to this function is incorrect: Expected `list`, found `None | @Todo(Support for `typing.TypeVar` instances in type expressions)`
+ error[lint:invalid-argument-type] sockeye/test_utils.py:224:34: Argument to this function is incorrect: Expected `list`, found `None | Unknown`

check-jsonschema (https://github.com/python-jsonschema/check-jsonschema)
+ error[lint:invalid-argument-type] src/check_jsonschema/schema_loader/resolver.py:62:45: Argument to this function is incorrect: Argument type `Unknown | str | None` does not satisfy constraints of type variable `AnyStr`
+ error[lint:invalid-argument-type] src/check_jsonschema/schema_loader/resolver.py:62:45: Argument to this function is incorrect: Expected `str`, found `Unknown | str | None`
- Found 60 diagnostics
+ Found 62 diagnostics

ppb-vector (https://github.com/ppb/ppb-vector)
+ error[lint:unsupported-operator] ppb_vector/__init__.py:472:25: Operator `*` is unsupported between objects of type `SupportsFloat` and `Unknown | int | float`
- Found 150 diagnostics
+ Found 151 diagnostics

python-chess (https://github.com/niklasf/python-chess)
- error[lint:call-non-callable] chess/__init__.py:880:20: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/__init__.py:880:20: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] chess/__init__.py:1437:44: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/__init__.py:1437:44: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/__init__.py:1557:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/__init__.py:1557:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/__init__.py:1558:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/__init__.py:1558:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/__init__.py:1577:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/__init__.py:1577:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/__init__.py:1578:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/__init__.py:1578:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:unsupported-operator] chess/engine.py:1634:29: Operator `+=` is unsupported between objects of type `None` and `(int & ~AlwaysFalsy) | float`
+ error[lint:unsupported-operator] chess/engine.py:1638:29: Operator `+=` is unsupported between objects of type `None` and `(int & ~AlwaysFalsy) | float`
+ error[lint:unsupported-operator] chess/engine.py:1642:29: Operator `-=` is unsupported between objects of type `None` and `Literal[1]`
- error[lint:call-non-callable] chess/gaviota.py:1551:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/gaviota.py:1551:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/gaviota.py:1553:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/gaviota.py:1553:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
- error[lint:call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:call-non-callable] chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)]` is not callable on object of type `list`
+ error[lint:no-matching-overload] chess/polyglot.py:378:16: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] chess/polyglot.py:425:25: No overload of bound method `__init__` matches arguments
- error[lint:invalid-argument-type] chess/svg.py:170:19: Argument to this function is incorrect: Expected `str`, found `@Todo(Support for `typing.TypeVar` instances in type expressions) | None`
+ error[lint:invalid-argument-type] chess/svg.py:170:19: Argument to this function is incorrect: Expected `str`, found `Unknown | None`
- error[lint:call-non-callable] chess/syzygy.py:446:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/syzygy.py:446:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/syzygy.py:447:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/syzygy.py:447:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/variant.py:682:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/variant.py:682:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/variant.py:683:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/variant.py:683:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/variant.py:686:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/variant.py:686:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/variant.py:687:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/variant.py:687:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/variant.py:829:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/variant.py:829:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/variant.py:830:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/variant.py:830:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/variant.py:833:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/variant.py:833:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- error[lint:call-non-callable] chess/variant.py:834:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] chess/variant.py:834:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> _T, (s: slice, /) -> @Todo(specialized non-generic class)])` is not callable on object of type `Unknown | list`
- Found 78 diagnostics
+ Found 83 diagnostics

kopf (https://github.com/nolar/kopf)
+ error[lint:invalid-assignment] kopf/_cogs/aiokits/aioenums.py:54:9: Object of type `FlagReasonT | Unknown` is not assignable to `FlagReasonT | None`
+ error[lint:no-matching-overload] kopf/_cogs/aiokits/aiotasks.py:344:43: No overload of bound method `__init__` matches arguments
+ error[lint:call-non-callable] kopf/_cogs/clients/auth.py:41:30: Object of type `_F` is not callable
+ error[lint:call-non-callable] kopf/_cogs/clients/auth.py:51:34: Object of type `_F` is not callable
+ error[lint:no-matching-overload] kopf/_cogs/structs/diffs.py:74:29: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_cogs/structs/diffs.py:119:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_cogs/structs/diffs.py:124:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_cogs/structs/diffs.py:170:15: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_cogs/structs/diffs.py:172:15: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_cogs/structs/diffs.py:183:15: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_core/actions/application.py:142:13: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_core/actions/progression.py:279:22: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_core/actions/progression.py:298:16: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_core/engines/posting.py:67:13: No overload of bound method `__init__` matches arguments
+ error[lint:call-non-callable] kopf/_core/intents/callbacks.py:254:20: Object of type `_FnT` is not callable
+ error[lint:invalid-argument-type] kopf/_core/intents/handlers.py:35:45: Argument to this function is incorrect: Argument type `CauseT & ChangingCause` does not satisfy upper bound of type variable `_DataclassT`
+ error[lint:no-matching-overload] kopf/_core/reactor/orchestration.py:198:16: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_core/reactor/orchestration.py:245:16: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] kopf/_core/reactor/queueing.py:204:32: No overload of bound method `__init__` matches arguments
+ error[lint:call-non-callable] kopf/_kits/webhacks.py:86:24: Object of type `_ServerFn` is not callable
+ error[lint:invalid-argument-type] kopf/_kits/webhooks.py:228:33: Argument to this function is incorrect: Argument type `socket` does not satisfy upper bound of type variable `_SupportsCloseT`
- Found 136 diagnostics
+ Found 157 diagnostics

strawberry (https://github.com/strawberry-graphql/strawberry)
- warning[lint:unused-ignore-comment] strawberry/aiohttp/views.py:210:60: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/asgi/__init__.py:192:60: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/chalice/views.py:114:60: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/channels/handlers/http_handler.py:271:12: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/channels/handlers/http_handler.py:345:12: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/channels/handlers/ws_handler.py:157:12: Unused blanket `type: ignore` directive
- error[lint:invalid-argument-type] strawberry/codegen/query_codegen.py:356:17: Argument to this function is incorrect: Expected `(...) -> @Todo(Support for `typing.TypeVar` instances in type expressions)`, found `Literal[GraphQLFragmentType]`
+ error[lint:invalid-argument-type] strawberry/codegen/query_codegen.py:356:17: Argument to this function is incorrect: Expected `(...) -> Unknown`, found `Literal[GraphQLFragmentType]`
+ error[lint:no-matching-overload] strawberry/directive.py:75:19: No overload of bound method `__init__` matches arguments
- error[lint:unresolved-attribute] strawberry/directive.py:134:25: Type `(...) -> Unknown` has no attribute `__name__`
+ error[lint:unresolved-attribute] strawberry/directive.py:134:25: Type `(...) -> T` has no attribute `__name__`
- warning[lint:unused-ignore-comment] strawberry/django/views.py:223:77: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/django/views.py:283:77: Unused blanket `type: ignore` directive
+ error[lint:no-matching-overload] strawberry/experimental/pydantic/object_type.py:105:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] strawberry/experimental/pydantic/object_type.py:202:13: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] strawberry/extensions/context.py:89:24: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] strawberry/extensions/context.py:97:24: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] strawberry/extensions/context.py:129:20: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] strawberry/extensions/context.py:141:16: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] strawberry/extensions/context.py:155:20: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] strawberry/extensions/context.py:162:16: No overload of bound method `__init__` matches arguments
- warning[lint:unused-ignore-comment] strawberry/federation/object_type.py:89:24: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/flask/views.py:111:60: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/flask/views.py:174:60: Unused blanket `type: ignore` directive
- warning[lint:redundant-cast] strawberry/http/async_base_view.py:309:19: Value is already of type `Unknown`
- warning[lint:unused-ignore-comment] strawberry/quart/views.py:92:60: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/sanic/views.py:152:60: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/types/enum.py:150:45: Unused blanket `type: ignore` directive
+ error[lint:no-matching-overload] strawberry/types/fields/resolver.py:137:25: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] strawberry/types/fields/resolver.py:166:20: No overload of bound method `__init__` matches arguments
- warning[lint:unused-ignore-comment] strawberry/types/object_type.py:144:66: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/types/object_type.py:160:41: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/types/object_type.py:379:19: Unused blanket `type: ignore` directive
- warning[lint:unused-ignore-comment] strawberry/types/object_type.py:452:19: Unused blanket `type: ignore` directive
- Found 590 diagnostics
+ Found 582 diagnostics

flake8 (https://github.com/pycqa/flake8)
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:139:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:163:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:169:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:173:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:210:23: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:224:18: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:225:19: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:299:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:344:12: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/plugins/finder.py:345:18: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] src/flake8/style_guide.py:409:17: No overload of bound method `__init__` matches arguments
+ error[lint:invalid-argument-type] src/flake8/utils.py:193:33: Argument to this function is incorrect: Argument type `BytesIO` does not satisfy upper bound of type variable `_BufferT_co`
+ error[lint:invalid-argument-type] src/flake8/utils.py:193:33: Argument to this function is incorrect: Expected `_WrappedBuffer`, found `BytesIO`
+ error[lint:no-matching-overload] tests/integration/test_checker.py:85:9: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/integration/test_checker.py:88:13: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/integration/test_checker.py:273:44: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/integration/test_checker.py:338:17: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] tests/unit/plugins/finder_test.p...*[Comment body truncated]*

---

_Comment by @dcreager on 2025-03-20 16:59_

The `mypy_primer` changes are because we're now interpreting the legacy `TypeVar`s in the typeshed.

---

_Comment by @codspeed-hq[bot] on 2025-04-18 17:27_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dcreager%2Flegacy-typevar-instance)

### Merging #16538 will **not alter performance**

<sub>Comparing <code>dcreager/legacy-typevar-instance</code> (30f116f) with <code>main</code> (5096824)</sub>



### Summary

`✅ 33` untouched benchmarks  





---

_Comment by @dcreager on 2025-04-22 18:46_

This is working in local tests!  It depends on #17552.  I will hold off on moving this out of draft until that is merged.  I also have some more cleanup I want to do on this PR.  But we're close!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:4387 on 2025-04-22 19:12_

Because we infer a specialization for each call to `reveal_type`, and then apply that specialization to the return type, we can reveal that instead of the parameter.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/import/builtins.md`:43 on 2025-04-22 21:25_

Though updating `reveal_type` to reveal the return type means that every custom signature for it needs to be generic and plumb through the return type correctly. That seems too brittle, so I will probably revert this part.

---

_@dcreager reviewed on 2025-04-22 21:25_

---

_Comment by @github-actions[bot] on 2025-04-23 19:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @dcreager on 2025-04-23 22:22_

A lot of the new ecosystem diagnostics of the form:

```
No overload of bound method `__init__` matches arguments
```

which are coming from `NamedTuple` subclasses.

---

Also a (partly) true positive!

```diff
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/parso/parso/__init__.py:57:28: Argument to this function is incorrect: Expected `str`, found `None`
```

which looks like is coming from the `version` argument. This is only a _partly_ true positive, because ideally the "found" type would be `str | None`.  It's not because `dict.pop` has the following two overloads:

```py
    @overload
    def pop(self, key: _KT, default: _VT, /) -> _VT: ...
    @overload
    def pop(self, key: _KT, default: _T, /) -> _VT | _T: ...
```

`_KT` and `_VT` should be bound by the class's generic scope, but since we're not handling legacy generic classes yet, they make it to the method definitions not bound by an enclosing generic scope.  So we're currently considering those equivalent to:

```py
    @overload
    def pop[_KT, _VT](self, key: _KT, default: _VT, /) -> _VT: ...
    @overload
    def pop[_KT, _T, _VT](self, key: _KT, default: _T, /) -> _VT | _T: ...
```

The first one matches, making the result `None`.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:777 on 2025-04-24 06:03_

I haven't checked this fully but this function is called indirectly from `Type::try_call`, meaning we shouldn't access any AST nodes in it. This might be the cause for the significant incremental perf regression

---

_@MichaReiser reviewed on 2025-04-24 06:03_

---

_@dcreager reviewed on 2025-04-24 13:20_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:777 on 2025-04-24 13:20_

Thanks for catching that!  I can try to move the check that needs this back up into `infer.rs`

---

_@dcreager reviewed on 2025-04-24 14:44_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:777 on 2025-04-24 14:44_

Done! Doing that also let me actually add the diagnostics for invalid legacy typevar construction

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:777 on 2025-04-24 14:45_

(Note that we still pass the `containing_assignment` in as a parameter, since that's the definition we need to record for a legacy typevar. We just don't access its `kind` field here)

---

_@dcreager reviewed on 2025-04-24 14:45_

---

_@MichaReiser reviewed on 2025-04-24 14:49_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:777 on 2025-04-24 14:49_

Are these changes local because line 771 still access the `kind` field, which then access `assignment.target`?

edit: Never mind, user error

---

_@dcreager reviewed on 2025-04-24 14:54_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:777 on 2025-04-24 14:54_

I'm not sure whether I did "submit comment" or "push" first, but the changes should be up now if I messed up the order earlier

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:777 on 2025-04-24 15:19_

Unfortunately this did not fix the performance regression

---

_@dcreager reviewed on 2025-04-24 15:19_

---

_@MichaReiser reviewed on 2025-04-24 15:47_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/call/bind.rs`:777 on 2025-04-24 15:47_

Yeah, unfortunately not. 

---

_Comment by @dcreager on 2025-04-24 15:57_

> A lot of the new ecosystem diagnostics of the form:
> 
> ```
> No overload of bound method `__init__` matches arguments
> ```

These are coming from how the typeshed is trying to model that `NamedTuple` is both a base class (that only exists to override the metaclass) and a callable function.  It has the following to model the latter (leaving out a deprecation that isn't relevant here):

```py
    @overload
    def __init__(self, typename: str, fields: Iterable[tuple[str, Any]], /) -> None: ...

    @overload
    def __init__(self, typename: str, fields: None = None, /, **kwargs: Any) -> None: ...
```

And those "block" the constructors that the named tuple would inherit from `tuple`.

cf https://github.com/astral-sh/ruff/issues/16994

---

_Comment by @dcreager on 2025-04-24 16:16_

I've gone through the new ecosystem checks, and they all seem to be legit new false positives for things we already know we need to tackle, and that are now visible because of handling legacy generic functions.

I'm also going to see if I can get to the bottom of the performance regression, but I'm going to go ahead and mark this as ready for review.

---

_Marked ready for review by @dcreager on 2025-04-24 16:16_

---

_Review requested from @carljm by @dcreager on 2025-04-24 16:16_

---

_Review requested from @AlexWaygood by @dcreager on 2025-04-24 16:16_

---

_Review requested from @sharkdp by @dcreager on 2025-04-24 16:16_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:587 on 2025-04-24 21:48_

I guess restoring this will take two things: 1) support for legacy Generic classes, and then 2) somewhat smarter typevar solving that can descend into types (a `Callable` type, in the case of the `partial.__new__` signature) and structurally match up typevars.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:778 on 2025-04-24 22:11_

Is it important to get all known cases into here rather than `infer.rs`, or does the combination of a) needing access to the containing assignment and b) needing to emit several diagnostics suggest that it might be simpler to just fully handle this case out in `infer.rs`?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/generics.rs`:388 on 2025-04-24 22:31_

Do you envision this heuristic surviving after the point where we do complete structural induction to find all nested typevars?

This is maybe looking further forward, but we've talked about how we could eliminate the duplication between `is_subtype_of` and `is_assignable_to` by having them both delegate to a common implementation that takes a strategy parameter (possibly in the form of a closure) to determine how dynamic types are handled. I wonder if the structural induction and collection of constraints that we'll ultimately need here could actually be implemented as just another strategy parameter to this single common implementation?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/infer.rs`:221 on 2025-04-24 22:46_

This (and the new `InferExpressionTypes` struct) would be my strong bet for the source of the regression on this PR. This adds a whole new ingredient table which has to be validated on each new revision. For some queries it doesn't make a big difference, but I expect `infer_expression_types` is a relatively hot query.

It may be that eventually, in order to support bidirectional checking, we have to add some kind of `type context` argument to all these queries anyway, and so we are going to have to pay this extra-argument cost regardless. But I suspect that for now, we would eliminate the regression if we could eliminate the need for this extra argument. (Or, for that matter, if we could include it directly in the existing `Expression` ingredient when it is created in semantic index building, which seems plausible since this is a static relationship derived directly from the shape of the AST.)

The other thing this extra argument will mean is that if we infer types for the same `expression`, but sometimes we pass in Some `containing_assignment` and sometimes we pass in `None`, we will end up inferring all the types for that expression tree twice, repeating work and doubling storage and re-validation costs. I'm not sure if that can actually occur in this PR or not, but it doesn't seem like there are any type-level safeguards ensuring it can't. If it does happen, that would definitely explain some of the regression.


---

_Review comment by @carljm on `crates/ruff_benchmark/benches/red_knot.rs`:69 on 2025-04-24 22:52_

Have you looked at the cause of this? We were so close to zero tomllib false positives 😆 

---

_@carljm approved on 2025-04-24 22:53_

This looks really good! My main hesitation is all the `containing_assignment` threading. It's very noisy, and I suspect is also the cause of the regression. I wonder if we can either move more of the TypeVar-call-handling out to `infer_call_expression`, or attach the containing definition directly to the `Expression` ingredient in semantic index building, to avoid this.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:221 on 2025-04-25 19:51_

That's a good suggestion to track this on the semantic index side.  I'm going to try doing that part in isolation in a separate PR.

---

_@dcreager reviewed on 2025-04-25 19:51_

---

_Review comment by @dcreager on `crates/ruff_benchmark/benches/red_knot.rs`:69 on 2025-04-26 13:20_

This is https://github.com/astral-sh/ruff/issues/16994. The diagnostic is on the `Output` reference [here](https://github.com/astral-sh/ruff/blob/4bcf1778fa0a223653c75ef44695428ae309430f/crates/ruff_benchmark/resources/tomllib/_parser.py#L76).  `Output` [inherits from `NamedTuple`](https://github.com/astral-sh/ruff/blob/4bcf1778fa0a223653c75ef44695428ae309430f/crates/ruff_benchmark/resources/tomllib/_parser.py#L227), which is the cause of a lot of the new ecosystem false positives, too.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_assignable_to.md`:587 on 2025-04-26 13:24_

That's right!

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/call/bind.rs`:778 on 2025-04-26 13:25_

I had been trying to follow a general rule of "put as much here as possible", but I like your reasoning for moving it all into `infer.rs`.  That (combined with the semantic index suggestion) seems to have (mostly) solved the performance regression

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/generics.rs`:388 on 2025-04-26 13:30_

> Do you envision this heuristic surviving after the point where we do complete structural induction to find all nested typevars?

It's more that it will go away with a more sophisticated constraint solver.  There's a comment below about how we currently handle only a very specific pattern of unions: `T | ...` (where the union contains _one_ typevar as a top-level element).  This clause is largely so that when we see `f[T](x: T | None)` and call it as `f(None)`, we don't infer `T = None`. A better constraint solver should be able to find the minimal type mapping that satisfies the union — in this case specifically that it doesn't need to bind `T` at all to satisfy this union.


> This is maybe looking further forward, but we've talked about how we could eliminate the duplication between `is_subtype_of` and `is_assignable_to` by having them both delegate to a common implementation that takes a strategy parameter (possibly in the form of a closure) to determine how dynamic types are handled. I wonder if the structural induction and collection of constraints that we'll ultimately need here could actually be implemented as just another strategy parameter to this single common implementation?

I like that a lot!  I think it would help with this heuristic, but the full solver would probably want to do something different for the "simple" type variants, too, so my hunch is that it would still end up with its own type inductor.

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/types/infer.rs`:221 on 2025-04-26 13:36_

It was simple enough that I didn't need a separate PR!  Implemented this, and the incremental performance regression is down to 2%

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:56 on 2025-04-26 13:39_

I don't love how this is so specialized to `StmtAssign`. We're not currently creating a standalone expression for annotated assignments, which means we currently raise a false positive for

```py
T: TypeVar = TypeVar("T")
```

Are there any concerns with adding a standalone expression for annotated assignments too?  Then I could make this `ast::Stmt` (or maybe a separate enum that only allows `StmtAssign` and `StmtAnnAssign`)

---

_@dcreager reviewed on 2025-04-26 13:40_

---

_@AlexWaygood reviewed on 2025-04-26 13:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:56 on 2025-04-26 13:47_

I would be happy with disallowing `T: TypeVar = TypeVar("T")`! I don't think it really makes sense to add the annotation there: it will be ignored by us anyway. It indicates to me that the user is confused about how TypeVars work, and perhaps doesn't realise that a simple `T = TypeVar("T")` assignment will also be considered a declaration by us. Mypy disallows an annotation in this situation: https://mypy-play.net/?mypy=latest&python=3.12&gist=4dad759b00a84071f89fbe9b5e041dcb

I'm also okay with allowing it, though. No strong opinion, as long as we don't emit a _confusing_ error message if somebody does this 😄

---

_Comment by @MichaReiser on 2025-04-26 14:40_

Congrats on fixing the performance regression! 

---

_Comment by @sharkdp on 2025-04-28 13:02_

There are 20,064 new diagnostics on this PR (an overall increase in diagnostics of 27%). 19,038 of these are of the form *"No overload of bound method `__init__` matches arguments"*, and the majority (all?) seem to be related to `NamedTuple` construction. We should probably work on silencing them somehow after this PR has been merged.

That still leaves 1,026 new diagnostics on this branch that have various other causes. I understand that this PR increases our coverage a lot, so it's entirely possible that all new diagnostics are unrelated to the changes here. But it might be worth studying this a bit more? (unless you already did, @dcreager)

I uploaded an [experimental new diff view](https://shark.fish/diff-legacy-typevars.html) of the ecosystem changes. I filtered out the 19k `invalid-overload` diagnostics mentioned above, to make this easier to scan.

Some things that stand out to me:
* lot's of `call-new-callable` diagnostics related to `pytest` decorators which seem strange:
  <pre>
  [error] call-non-callable - <a href="https://github.com/graphql-python/graphql-core/blob/main/tests/utilities/test_strip_ignored_characters_fuzz.py#L79">tests/utilities/test_strip_ignored_characters_fuzz.py:79:5</a> - Object of type `Literal[10]` is not callable
  </pre>
* A lot of errors related to assignability of `partial` instances, which I hoped had been fixed [here](https://github.com/astral-sh/ruff/pull/17590).
* Some `invalid-legacy-type-variable` errors in vendored `typing_extensions`

---

_Comment by @dcreager on 2025-04-28 19:24_

> There are 20,064 new diagnostics on this PR (an overall increase in diagnostics of 27%). 19,038 of these are of the form _"No overload of bound method `__init__` matches arguments"_, and the majority (all?) seem to be related to `NamedTuple` construction. We should probably work on silencing them somehow after this PR has been merged.

Yep, those are because of the `NamedTuple.__init__` overloads in typeshed: https://github.com/astral-sh/ruff/pull/16538#issuecomment-2828133733

---

_Comment by @dhruvmanila on 2025-04-28 20:29_

I'm interested in trying to put this PR on top of #17618 to see what effect it has on the diagnostics that this PR adds. I'm going to try it locally and see. This shouldn't block this PR from merging.

---

_Comment by @dcreager on 2025-04-28 20:39_

> lot's of `call-new-callable` diagnostics related to `pytest` decorators which seem strange:

This looks to be coming from [here](https://github.com/pytest-dev/pytest/blob/8173aa812253aa263b43cd52c80d605470d0c13f/src/_pytest/mark/structures.py#L356-L364):

```py
    @overload
    def __call__(self, arg: Markable) -> Markable:  # type: ignore[overload-overlap]
        pass

    @overload
    def __call__(self, *args: object, **kwargs: object) -> MarkDecorator:
        pass

    def __call__(self, *args: object, **kwargs: object):
```

where `Markable` is a legacy typevar with a `Callable` bound. I'm also interested in @dhruvmanila's experiment to test this with intersection overloads — it seems like we should fall through to the `(*args, **kwargs)` overload if the first overload doesn't match (because of the argument, say `Literal[10]`, not being callable)

---

_Comment by @sharkdp on 2025-04-28 20:45_

> where `Markable` is a legacy typevar with a `Callable` bound

Minor additional comment: the bound is `Union[Callable[..., object], type]`, if that changes anything (I don't see why it would; still not compatible with `Literal[10]`)

---

_Comment by @carljm on 2025-04-28 21:23_

Out of curiosity, I looked into the pytest decorator `call-non-callable` diagnostics. In `pytest.mark.whatever`, the "whatever" is actually arbitrary: `pytest.mark` is [an object with a `__getattr__` implementation](https://github.com/pytest-dev/pytest/blob/main/src/_pytest/mark/structures.py#L537) returning an instance of `_pytest.mark.MarkDecorator`. `MarkDecorator` then has [this overloaded `__call__` method](https://github.com/pytest-dev/pytest/blob/main/src/_pytest/mark/structures.py#L356-L364), where `Markable` [is a bound TypeVar](https://github.com/pytest-dev/pytest/blob/main/src/_pytest/mark/structures.py#L278). So I think what is happening is that we don't yet respect typevar bounds when inferring types in a generic function, so we wrongly pick the first overload even when the argument in e.g. `pytest.mark.timeout(10)` is not assignable to the upper bound of `Markable`, which means we should pick the second overload.

None of that is to say it needs fixing in this PR, it can be a follow-up. 

---

_Comment by @dhruvmanila on 2025-04-28 21:31_

> I'm interested in trying to put this PR on top of #17618 to see what effect it has on the diagnostics that this PR adds. I'm going to try it locally and see. This shouldn't block this PR from merging.

It _does_ seem to be reducing the number of overall diagnostics but not by much compared to what already exists. Ref https://github.com/astral-sh/ruff/pull/17690. I haven't looked closely to the diagnostics that it's removing, I can do that a bit later if it'd be useful.


---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:56 on 2025-04-28 21:31_

It would be preferable not to add a standalone expression for every annotated assignment; the more standalone expressions, the more memos, and the slower incremental re-validation gets. If we really need to, we probably can, but I don't think `T: TypeVar = TypeVar("T")` is a sufficiently motivating use case. As Alex says, I think it would be fine to just error on that; `T = TypeVar("T")` is effectively a syntactic special form.

---

_@carljm approved on 2025-04-28 21:37_

This looks good to me! Obviously it would be good to follow up with some attention to the new false positives.

---

_Comment by @carljm on 2025-04-28 21:38_

Oh oops, looks like I was looking into the call-non-callable at the same time as others :)

---

_Comment by @carljm on 2025-04-28 21:39_

> it seems like we should fall through to the `(*args, **kwargs)` overload if the first overload doesn't match (because of the argument, say `Literal[10]`, not being callable)

Isn't the issue here simply that we [pay no attention to bounds](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/types/generics.rs#L358), so the first overload always matches?

---

_Comment by @dhruvmanila on 2025-04-28 23:43_

> It _does_ seem to be reducing the number of overall diagnostics but not by much compared to what already exists. Ref #17690. I haven't looked closely to the diagnostics that it's removing, I can do that a bit later if it'd be useful.

Most of the changes I'm seeing in https://github.com/astral-sh/ruff/pull/17690 are consistent with what I'm already seeing in https://github.com/astral-sh/ruff/pull/17618, but I've noticed that almost 1/3rd of `call-non-callable` diagnostics are removed:

```diff
  ┃ Diagnostic ID                      ┃ Severity ┃ Removed ┃ Added ┃ Net Change ┃
- │ lint:call-non-callable             │ error    │      98 │   497 │       +399 │
+ │ lint:call-non-callable             │ error    │     124 │   158 │        +34 │
```

---

_Comment by @dcreager on 2025-04-29 00:12_

> Isn't the issue here simply that we [pay no attention to bounds](https://github.com/astral-sh/ruff/blob/main/crates/red_knot_python_semantic/src/types/generics.rs#L358), so the first overload always matches?

Ah yes, you're right! I had thought it would be caught by the type-checking code, but if we infer this type mapping and then substitute that for the formal parameter annotation, it will pass.  That's worth handling here, I think, and not in a follow-on PR.

---

_@dcreager reviewed on 2025-04-29 00:18_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:56 on 2025-04-29 00:18_


> As Alex says, I think it would be fine to just error on that;
> ...
> No strong opinion, as long as we don't emit a _confusing_ error message if somebody does this 😄


The main issue with this is that _customizing_ the error message will require the same extra plumbing as allowing it, since we'd need to detect that the `TypeVar` call occurs in an annotated assignment.  Otherwise we'll have the default error message ``A legacy `typing.TypeVar` must be immediately assigned to a variable``.


---

_@dcreager reviewed on 2025-04-29 00:19_

---

_Review comment by @dcreager on `crates/red_knot_python_semantic/src/semantic_index/expression.rs`:56 on 2025-04-29 00:19_

> Otherwise we'll have the default error message `` A legacy `typing.TypeVar` must be immediately assigned to a variable ``.

Which, to be clear, I'm okay with, certainly for now

---

_Comment by @dcreager on 2025-04-29 12:58_

> Ah yes, you're right! I had thought it would be caught by the type-checking code, but if we infer this type mapping and then substitute that for the formal parameter annotation, it will pass. That's worth handling here, I think, and not in a follow-on PR.

We now check the bounds and constraints when inferring specializations, which removed all of the `pytest`-related `call-non-callable` errors.

---

_Comment by @dcreager on 2025-04-29 12:59_

I'm going to go ahead and merge so that I can proceed with legacy generic classes. My apologies to our poor ecosystem graphs... :joy: 

---

_Merged by @dcreager on 2025-04-29 13:03_

---

_Closed by @dcreager on 2025-04-29 13:03_

---

_Branch deleted on 2025-04-29 13:03_

---

_Comment by @sharkdp on 2025-04-29 13:13_

I'm going to look at those `NamedTuple` false positives.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:79 on 2025-04-29 13:57_

nit: we don't normally import `reveal_type` in mdtests

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:86 on 2025-04-29 14:00_

Would it be worth also having similar tests for legacy typevars with bounds/constraints? Both to show we handle the bounds and constraints correctly, and that the full diagnostic highlights them correctly?

---

_@carljm reviewed on 2025-04-29 14:04_

Nice!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/generics/functions.md`:79 on 2025-04-29 14:15_

it creates noisy `undefined-reveal` diagnostics in mdtests where `snapshot-diagnostics` are enabled if you don't import it explicitly

---

_@AlexWaygood reviewed on 2025-04-29 14:15_

---
