```yaml
number: 17366
title: "[red-knot] Add support for overloaded functions"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - ty
assignees: []
merged: true
base: main
head: dhruv/overloaded-signature
created_at: 2025-04-12T04:44:02Z
updated_at: 2025-04-18T04:27:41Z
url: https://github.com/astral-sh/ruff/pull/17366
synced_at: 2026-01-12T15:56:01Z
```

# [red-knot] Add support for overloaded functions

---

_@dhruvmanila_

## Summary

Part of astral-sh/ruff#15383, this PR adds support for overloaded callables.

Typing spec: https://typing.python.org/en/latest/spec/overload.html

Specifically, it does the following:
1. Update the `FunctionType::signature` method to return signatures from a possibly overloaded callable using a new `FunctionSignature` enum
2. Update `CallableType` to accommodate overloaded callable by updating the inner type to `Box<[Signature]>`
3. Update the relation methods on `CallableType` with logic specific to overloads
4. Update the display of callable type to display a list of signatures enclosed by parenthesis
5. Update `CallableTypeOf` special form to recognize overloaded callable
6. Update subtyping, assignability and fully static check to account for callables (equivalence is planned to be done as a follow-up)

For (2), it is required to be done in this PR because otherwise I'd need to add some workaround for `into_callable_type` and I though it would be best to include it in here.

For (2), another possible design would be convert `CallableType` in an enum with two variants `CallableType::Single` and `CallableType::Overload` but I decided to go with `Box<[Signature]>` for now to (a) mirror it to be equivalent to `overload` field on `CallableSignature` and (b) to avoid any refactor in this PR. This could be done in a follow-up to better split the two kind of callables.

### Design

There were two main candidates on how to represent the overloaded definition:
1. To include it in the existing infrastructure which is what this PR is doing by recognizing all the signatures within the `FunctionType::signature` method
2. To create a new `Overload` type variant

<details><summary>For context, this is what I had in mind with the new type variant:</summary>
<p>

```rs
pub enum Type {
	FunctionLiteral(FunctionType),
    Overload(OverloadType),
    BoundMethod(BoundMethodType),
    ...
}

pub struct OverloadType {
	// FunctionLiteral or BoundMethod
    overloads: Box<[Type]>,
	// FunctionLiteral or BoundMethod
    implementation: Option<Type>
}

pub struct BoundMethodType {
    kind: BoundMethodKind,
    self_instance: Type,
}

pub enum BoundMethodKind {
    Function(FunctionType),
    Overload(OverloadType),
}
```

</p>
</details> 

The main reasons to choose (1) are the simplicity in the implementation, reusing the existing infrastructure, avoiding any complications that the new type variant has specifically around the different variants between function and methods which would require the overload type to use `Type` instead.

### Implementation

The core logic is how to collect all the overloaded functions. The way this is done in this PR is by recording a **use** on the `Identifier` node that represents the function name in the use-def map. This is then used to fetch the previous symbol using the same name. This way the signatures are going to be propagated from top to bottom (from first overload to the final overload or the implementation) with each function / method. For example:

```py
from typing import overload

@overload
def foo(x: int) -> int: ...
@overload
def foo(x: str) -> str: ...
def foo(x: int | str) -> int | str:
	return x
```

Here, each definition of `foo` knows about all the signatures that comes before itself. So, the first overload would only see itself, the second would see the first and itself and so on until the implementation or the final overload.

This approach required some updates specifically recognizing `Identifier` node to record the function use because it doesn't use `ExprName`.

## Test Plan

Update existing test cases which were limited by the overload support and add test cases for the following cases:
* Valid overloads as functions, methods, generics, version specific
* Invalid overloads as stated in https://typing.python.org/en/latest/spec/overload.html#invalid-overload-definitions (implementation will be done in a follow-up)
* Various relation: fully static, subtyping, and assignability (others in a follow-up)

## Ecosystem changes

_WIP_

After going through the ecosystem changes (there are a lot!), here's what I've found:

We need assignability check between a callable type and a class literal because a lot of builtins are defined as classes in typeshed whose constructor method is overloaded e.g., `map`, `sorted`, `list.sort`, `max`, `min` with the `key` parameter, `collections.abc.defaultdict`, etc. (https://github.com/astral-sh/ty/issues/129). This makes up most of the ecosystem diff **roughly 70 diagnostics**. For example:

```py
from collections import defaultdict

# red-knot: No overload of bound method `__init__` matches arguments [lint:no-matching-overload]
defaultdict(int)
# red-knot: No overload of bound method `__init__` matches arguments [lint:no-matching-overload]
defaultdict(list)

class Foo:
    def __init__(self, x: int):
        self.x = x

# red-knot: No overload of function `__new__` matches arguments [lint:no-matching-overload]
map(Foo, ["a", "b", "c"])
```

Duplicate diagnostics in unpacking (https://github.com/astral-sh/ty/issues/185) has **~16 diagnostics**.

Support for the `callable` builtin which requires `TypeIs` support. This is **5 diagnostics**. For example:
```py
from typing import Any

def _(x: Any | None) -> None:
    if callable(x):
        # red-knot: `Any | None`
        # Pyright: `(...) -> object`
        # mypy: `Any`
        # pyrefly: `(...) -> object`
        reveal_type(x)
```

Narrowing on `assert` which has **11 diagnostics**. This is being worked on in https://github.com/astral-sh/ruff/pull/17345. For example:
```py
import re

match = re.search("", "")
assert match
match.group()  # error: [possibly-unbound-attribute]
```

Others:
* `Self`: 2
* Type aliases: 6
* Generics: 3
* Protocols: 13
* Unpacking in comprehension: 1 (https://github.com/astral-sh/ruff/pull/17396)

## Performance

Refer to https://github.com/astral-sh/ruff/pull/17366#issuecomment-2814053046.

---

_Label `red-knot` added by @dhruvmanila on 2025-04-12 04:44_

---

_Comment by @github-actions[bot] on 2025-04-12 04:46_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
zipp (https://github.com/jaraco/zipp)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/zipp/zipp/glob.py:4:26: Operator `*` is unsupported between objects of type `str` and `bool`
- Found 3 diagnostics
+ Found 4 diagnostics

packaging (https://github.com/pypa/packaging)
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/src/packaging/specifiers.py:726:37: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/noxfile.py:140:20: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/noxfile.py:140:20: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/noxfile.py:140:20: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:410:46: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:426:50: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:494:46: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/src/packaging/tags.py:633:20: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/tests/test_metadata.py:581:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/tasks/check.py:71:16: No overload of function `sorted` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/tasks/check.py:85:16: No overload of function `sorted` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/packaging/tasks/check.py:104:16: No overload of function `sorted` matches arguments
- Found 324 diagnostics
+ Found 336 diagnostics

bidict (https://github.com/jab/bidict)
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/bidict/bidict/_iter.py:50:34: Object of type `None` is not callable
- Found 87 diagnostics
+ Found 88 diagnostics

arrow (https://github.com/arrow-py/arrow)
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/arrow/arrow/util.py:107:12: Return type does not match returned value: Expected `date`, found `timedelta`
- Found 34 diagnostics
+ Found 35 diagnostics

python-htmlgen (https://github.com/srittau/python-htmlgen)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/element.py:115:23: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/element.py:124:23: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-htmlgen/test_htmlgen/element.py:152:23: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
- Found 498 diagnostics
+ Found 501 diagnostics

async-utils (https://github.com/mikeshardmind/async-utils)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/async-utils/_misc/_queue_testing.py:50:30: Operator `+` is unsupported between objects of type `Literal["\u{1b}[30;1m%(asctime)s\u{1b}[0m "]` and `tuple[Unknown, Literal["\u{1b}[40;1m"]] | tuple[Unknown, Literal["\u{1b}[34;1m"]] | tuple[Unknown, Literal["\u{1b}[33;1m"]] | tuple[Unknown, Literal["\u{1b}[31m"]] | tuple[Unknown, Literal["\u{1b}[41m"]]`
- Found 30 diagnostics
+ Found 31 diagnostics

pyinstrument (https://github.com/joerick/pyinstrument)
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/pyinstrument/examples/demo_scripts/wikipedia_article_word_count.py:24:23: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/pyinstrument/examples/demo_scripts/wikipedia_article_word_count.py:34:17: No overload of function `sorted` matches arguments
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/renderers/html.py:104:25: Argument to this function is incorrect: Expected `str`, found `Literal[b""]`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/middleware.py:50:13: Object of type `None` is not callable
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline.py:274:25: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyinstrument/test/test_cmdline.py:275:20: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/pyinstrument/pyinstrument/__main__.py:318:61: Argument to this function is incorrect: Expected `TextIO`, found `TextIOWrapper | TextIO | @Todo(Support for `typing.TypeAlias`)`
- Found 269 diagnostics
+ Found 276 diagnostics

isort (https://github.com/pycqa/isort)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/isort/isort/deprecated/finders.py:81:31: Attribute `lower` on type `Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/isort/isort/parse.py:167:21: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/isort/isort/parse.py:168:17: No overload of bound method `__init__` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/isort/isort/settings.py:659:31: Attribute `lower` on type `Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/isort/isort/api.py:635:41: No overload of function `__new__` matches arguments
- Found 51 diagnostics
+ Found 56 diagnostics

paroxython (https://github.com/laowantong/paroxython)
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/examples/simple/programs/15_itertools_groupby.py:11:25: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/examples/simple/programs/15_itertools_groupby.py:11:25: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/examples/simple/programs/15_itertools_groupby.py:11:25: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/helpers/make_dummy_db.py:66:25: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/examples/idioms/programs/148.1829-read-list-of-integer-numbers-from-stdin.py:12:6: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/helpers/reformat_spec.py:82:20: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/helpers/reformat_spec.py:83:20: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/parse_program.py:293:31: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/derived_labels_db.py:180:58: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/derived_labels_db.py:221:33: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/tests/test_parse_program.py:63:38: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/map_taxonomy.py:170:60: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/map_taxonomy.py:241:26: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/map_taxonomy.py:380:42: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/preprocess_source.py:449:36: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/preprocess_source.py:450:50: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/tests/test_goodies.py:105:41: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/examples/idioms/programs/053.1933-join-a-list-of-strings.py:15:15: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/recommend_programs.py:259:49: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/make_db.py:178:38: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/make_db.py:189:38: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/make_db.py:255:26: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/make_db.py:285:26: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/filter_programs.py:439:69: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/paroxython/goodies.py:33:35: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/examples/idioms/programs_with_labels.py:457:15: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/paroxython/examples/idioms/programs_with_labels.py:1117:6: No overload of function `__new__` matches arguments
- Found 432 diagnostics
+ Found 459 diagnostics

git-revise (https://github.com/mystor/git-revise)
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/git-revise/gitrevise/utils.py:85:20: Return type does not match returned value: Expected `bytes`, found `bytearray`
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/git-revise/gitrevise/odb.py:213:25: No overload of bound method `__init__` matches arguments
- Found 4 diagnostics
+ Found 6 diagnostics

pyp (https://github.com/hauntsaninja/pyp)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pyp/tests/test_find_names.py:33:25: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/pyp/pyp.py:253:51: No overload of bound method `__init__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/pyp/pyp.py:255:46: No overload of bound method `__init__` matches arguments
- Found 19 diagnostics
+ Found 22 diagnostics

mypy_primer (https://github.com/hauntsaninja/mypy_primer)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/utils.py:37:16: Operator `+` is unsupported between objects of type `Literal["\""]` and `Path`
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/model.py:196:31: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/mypy_primer/mypy_primer/model.py:391:38: No overload of bound method `__init__` matches arguments
- Found 10 diagnostics
+ Found 13 diagnostics

python-chess (https://github.com/niklasf/python-chess)
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/examples/perft/perft.py:77:28: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/examples/perft/perft.py:77:28: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/examples/perft/perft.py:77:28: No overload of function `__new__` matches arguments
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/python-chess/chess/svg.py:170:19: Argument to this function is incorrect: Expected `str`, found `Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions) | None`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:682:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:683:35: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:686:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:687:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:829:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:830:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:833:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/variant.py:834:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/polyglot.py:256:44: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)]` is not callable on object of type `list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/polyglot.py:258:42: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)]` is not callable on object of type `list`
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/chess/pgn.py:1850:12: No overload of function `read_game` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/chess/pgn.py:1857:17: No overload of function `read_game` matches arguments
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1551:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1553:46: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:invalid-return-type] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1670:16: Return type does not match returned value: Expected `BinaryIO`, found `Unknown & ~None | TextIOWrapper`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1899:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/gaviota.py:1910:52: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/syzygy.py:446:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/syzygy.py:447:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:880:20: Method `__getitem__` of type `Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)]` is not callable on object of type `list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1437:44: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1557:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1558:27: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1577:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/chess/__init__.py:1578:9: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/python-chess/chess/engine.py:2497:40: Operator `in` is not supported for types `@Todo(return type of overloaded function)` and `None`, in comparing `@Todo(return type of overloaded function)` with `str | None`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/python-chess/chess/engine.py:2497:40: Operator `in` is not supported for types `@Todo(generics)` and `None`, in comparing `@Todo(generics)` with `str | None`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:919:25: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2212:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2213:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2214:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2216:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2217:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2219:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2221:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2223:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2225:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2226:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2231:22: Attribute `variation` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2245:15: Attribute `variations` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2245:39: Attribute `has_variation` on type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2246:20: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2249:26: Method `__getitem__` of type `Game | None | ChildNode` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2250:26: Method `__getitem__` of type `Game | None | ChildNode` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2286:21: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2292:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2293:26: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2298:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2301:13: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2303:9: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2313:26: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2321:26: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2322:26: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2323:26: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2324:26: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2325:30: Attribute `errors` on type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2332:26: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2333:30: Attribute `errors` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2338:26: Attribute `comments` on type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2339:26: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2343:26: Attribute `comments` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2351:26: Attribute `comments` on type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2353:16: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2358:16: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2367:16: Attribute `variation` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2456:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2457:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2461:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2462:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2490:26: Attribute `next` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2490:26: Attribute `move` on type `ChildNode | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2492:26: Attribute `next` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2492:26: Attribute `move` on type `ChildNode | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2494:26: Attribute `next` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2494:26: Attribute `move` on type `ChildNode | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2511:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:call-possibly-unbound-method] /tmp/mypy_primer/projects/python-chess/test.py:2512:30: Method `__getitem__` of type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2517:18: Attribute `time_control` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2519:63: Attribute `headers` on type `Game | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/test.py:2597:33: No overload of function `read_game` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2600:33: Attribute `accept` on type `Game | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/test.py:2603:50: No overload of function `read_game` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2631:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2653:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2654:35: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2657:26: Attribute `board` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2662:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2663:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2664:30: Attribute `next` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2664:30: Attribute `move` on type `ChildNode | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2691:30: Attribute `errors` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2692:26: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2695:26: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2728:26: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2739:26: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2741:9: Attribute `setup` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2742:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2744:9: Attribute `setup` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2745:37: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2750:21: Attribute `board` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2757:21: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2763:21: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2769:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2774:16: Attribute `next` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2775:26: Attribute `move` on type `ChildNode | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2776:25: Attribute `is_end` on type `ChildNode | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2792:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2794:26: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2801:16: Attribute `next` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2801:16: Attribute `variations` on type `ChildNode | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2826:16: Attribute `variation` on type `@Todo(Support for `typing.TypeVar` instances in type expressions) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/test.py:2951:16: No overload of function `read_game` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2955:16: Attribute `accept` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2962:30: Attribute `headers` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:2965:30: Attribute `headers` on type `Game | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/python-chess/test.py:3055:19: No overload of function `__new__` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:4573:25: Attribute `board` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:4574:26: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:4575:26: Attribute `end` on type `Game | None` is possibly unbound
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:4755:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ error[lint:call-non-callable] /tmp/mypy_primer/projects/python-chess/test.py:4756:26: Method `__getitem__` of type `Unknown | (Overload[(i: SupportsIndex, /) -> Unknown | @Todo(Support for `typing.TypeVar` instances in type expressions), (s: slice, /) -> @Todo(generics)])` is not callable on object of type `Unknown | list`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:4838:27: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:4844:30: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:4939:30: Attribute `end` on type `Game | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/python-chess/test.py:4942:30: Attribute `end` on type `Game | None` is possibly unbound
- Found 277 diagnostics
+ Found 408 diagnostics

pybind11 (https://github.com/pybind/pybind11)
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:323:54: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/pybind11/tests/test_factory_constructors.py:334:54: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:22:19: No overload of function `getattr` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/pybind11/tests/test_buffers.py:23:21: No overload of function `getattr` matches arguments
- Found 288 diagnostics
+ Found 292 diagnostics

porcupine (https://github.com/Akuli/porcupine)
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/drop_to_open.py:12:17: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/hover.py:91:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/hover.py:94:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/hover.py:95:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/reload.py:18:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/urls.py:83:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/urls.py:87:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:129:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:131:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:132:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:133:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:134:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/statusbar.py:135:5: No overload of bound method `bind` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/sort.py:29:31: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/tests/test_indent_dedent.py:75:17: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/tests/test_indent_dedent.py:90:17: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/anchors.py:132:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:113:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:114:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:116:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:117:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:118:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:119:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:120:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:166:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/mergeconflict.py:173:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:89:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:90:5: No overload of bound method `bind` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:268:21: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:329:20: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:330:20: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:363:40: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:363:40: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:363:40: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:364:40: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:364:40: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:364:40: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:527:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:530:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:531:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:532:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:533:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:534:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:535:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:536:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:539:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/autocomplete.py:540:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/tests/test_textutils_changes.py:189:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/trailing_newline.py:23:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/jump_to_definition.py:106:9: No overload of bound method `bind` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/tests/test_docs.py:19:64: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/tests/test_docs.py:29:12: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/geometry.py:17:27: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/geometry.py:17:27: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/geometry.py:17:27: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/geometry.py:17:27: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/geometry.py:17:27: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/update_check.py:55:24: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/update_check.py:55:24: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/update_check.py:55:24: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/update_check.py:55:24: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/__init__.py:102:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/__init__.py:103:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/__init__.py:104:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:33:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:33:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:33:34: No overload of function `__new__` matches arguments
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:42:45: Argument to this function is incorrect: Expected `str`, found `Unknown | Path`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:43:68: Argument to this function is incorrect: Expected `Sized`, found `Unknown | Path`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:45:55: Argument to this function is incorrect: Expected `str`, found `Unknown | Path`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:61:41: Argument to this function is incorrect: Expected `str`, found `Unknown | Path`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:68:70: Argument to this function is incorrect: Expected `Sized`, found `Unknown | Path`
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:72:54: Argument to this function is incorrect: Expected `str`, found `Unknown | Path`
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/comment_selected_lines.py:81:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:450:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:451:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:452:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:503:17: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:509:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:511:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:513:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/directory_tree.py:514:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/run/no_terminal.py:463:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/run/no_terminal.py:464:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/run/no_terminal.py:465:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/matching_paren.py:24:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/matching_paren.py:24:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/matching_paren.py:24:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/matching_paren.py:76:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/tests/test_highlight_plugin.py:10:21: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/indent_block.py:13:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/indent_block.py:13:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/indent_block.py:13:34: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/rightclick_menu.py:41:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/rightclick_menu.py:54:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:174:24: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:174:24: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:174:24: No overload of function `__new__` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:377:28: Attribute `language_id` on type `Unknown | Path` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:445:30: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:678:43: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:682:36: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:694:73: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:700:53: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
+ error[lint:invalid-argument-type] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:706:43: Argument to this function is incorrect: Expected `LangServerConfig`, found `Unknown & ~None | Path`
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:708:32: Attribute `command` on type `Unknown & ~None | Path` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:776:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:778:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/langserver.py:779:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/tree_sitter_highlighter.py:140:32: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/tree_sitter_highlighter.py:140:32: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/tree_sitter_highlighter.py:140:32: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/tree_sitter_highlighter.py:141:28: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/tree_sitter_highlighter.py:141:28: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/tree_sitter_highlighter.py:141:28: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/filemanager.py:143:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/filemanager.py:144:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/filemanager.py:420:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/filetypes.py:209:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/pygments_highlighter.py:77:26: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/pygments_highlighter.py:77:26: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/pygments_highlighter.py:77:26: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/python_venv.py:169:9: No overload of bound method `bind` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/aboutdialog.py:160:40: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/aboutdialog.py:175:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:132:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:133:5: No overload of bound method `bind_class` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:256:13: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:308:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/menubar.py:440:21: No overload of function `__new__` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/plugins/highlight/base_highlighter.py:19:23: No overload of function `__new__` matches arguments
+ warning[lint:possibly-unbound-attribute] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:437:20: Attribute `group` on type `@Todo(generics) | None` is possibly unbound
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:471:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:555:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:556:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:563:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:564:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:565:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:635:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:739:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/utils.py:740:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:495:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:628:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:701:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:793:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:814:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:827:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:853:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:856:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/settings.py:881:5: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/pluginmanager.py:76:9: No overload of bound method `bind` matches arguments
+ error[lint:no-matching-overload] /tmp/mypy_primer/projects/porcupine/porcupine/p...*[Comment body truncated]*

---

_Comment by @codspeed-hq[bot] on 2025-04-14 20:42_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv%2Foverloaded-signature)

### Merging astral-sh/ruff#17366 will **degrade performances by 7.47%**

<sub>Comparing <code>dhruv/overloaded-signature</code> (b347ce0) with <code>main</code> (c7372d2)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 32` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` red_knot_micro[many_string_assignments] `` | 52.4 ms | 56.7 ms | -7.47% |


---

_Closed by @dhruvmanila on 2025-04-16 21:53_

---

_Reopened by @dhruvmanila on 2025-04-16 21:53_

---

_Renamed from "[red-knot] Understand overloaded function/method" to "[red-knot] Add support for overloaded functions" by @dhruvmanila on 2025-04-17 14:32_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/dataclasses.md`:563 on 2025-04-17 14:36_

I think this should be `str` instead of `Literal[""]` looking at the overloaded definition of `ConvertToLength.__get__` above (cc @sharkdp)

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:250 on 2025-04-17 14:37_

TODO: check whether this is correct

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/subscript/bytes.md`:27 on 2025-04-17 14:39_

This seems correct i.e., revealing `int` instead of `bytes` even though we don't support protocols, `n` is definitely not a `slice`. Pyright and mypy also reveals `int`.

```py
    @overload
    def __getitem__(self, key: SupportsIndex, /) -> int: ...
    @overload
    def __getitem__(self, key: slice, /) -> bytes: ...
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/subscript/lists.md`:38 on 2025-04-17 14:40_

TODO: verify if these errors are correct

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/subscript/string.md`:24 on 2025-04-17 14:42_

I think I'd need to put this `TODO` back because it should probably be `str` as `self` here would be `StringLiteral`. So,

```py
    @overload
    def __getitem__(self: LiteralString, key: SupportsIndex | slice, /) -> LiteralString: ...
    @overload
    def __getitem__(self, key: SupportsIndex | slice, /) -> str: ...  # type: ignore[misc]
```

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/display.rs`:124 on 2025-04-17 14:45_

This is not the final way to display overloads, I went with it because this is not the main goal for this PR. I'll open a separate issue for this.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/signatures.rs`:295 on 2025-04-17 14:48_

I updated the `apply_specialization` methods for `Signature`, `Parameters`, `Parameter` to return `Self` so that it's easier to collect in `CallableType::apply_specialization` and it also matches what this method do on other types (cc @dcreager)

---

_@dhruvmanila reviewed on 2025-04-17 14:48_

---

_Marked ready for review by @dhruvmanila on 2025-04-17 14:48_

---

_Review requested from @carljm by @dhruvmanila on 2025-04-17 14:48_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-04-17 14:48_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-04-17 14:48_

---

_Review requested from @dcreager by @dhruvmanila on 2025-04-17 14:48_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-04-17 14:48_

---

_@sharkdp reviewed on 2025-04-17 14:54_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclasses.md`:563 on 2025-04-17 14:54_

Yes, you're right. Can you please change the overload above to the following (and import `Literal`)?
```oy
def __get__(self, instance: None, owner: type) -> Literal[""]: ...
```

---

_@sharkdp reviewed on 2025-04-17 14:56_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/dataclasses.md`:612 on 2025-04-17 14:56_

This is so cool to see :star_struck: 

---

_@sharkdp reviewed on 2025-04-17 14:57_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/descriptor_protocol.md`:464 on 2025-04-17 14:57_

Same here. So cool to see that this is possible now!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:57 on 2025-04-17 16:32_

hmm, I think we're selecting the wrong overload here for these last two. These are the six(!) overloads to `int.__pow__`:

https://github.com/astral-sh/ruff/blob/d2ebfd6ed7855140806a7cad3e23d58fe5056c31/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi#L290-L303

It looks to me like we're selecting overload 3 when we should be selecting overload 5 (which would lead to us inferring `Any` rather than `int`)? The reason is that `x ** y` results in an `int` at runtime if `x` is an integer and `y` is a positive integer, but it results in a `float` at runtime if `x` is an integer and `y` is a _negative_ integer:

```pycon
>>> 1 ** 1
1
>>> -1 ** 1
-1
>>> 1 ** -1
1.0
>>> -1 ** -1
-1.0
```

Possibly we're selecting overload 3 here because we don't properly understand the `_PositiveInteger` and `_NegativeInteger` type aliases in typeshed? We might be inferring those types as `Unknown` or `Todo`, and any type is assignable to `Unknown` or `Todo`, so I can see how that would lead us to believe that overload 3 matches these calls.

---

_@AlexWaygood reviewed on 2025-04-17 16:32_

---

_@AlexWaygood reviewed on 2025-04-17 16:36_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/subscript/bytes.md`:27 on 2025-04-17 16:36_

We actually do have some hacky support for `SupportsIndex` specifically, because it was causing loads of false positives to not support it. So yes, this looks definitely correct!

https://github.com/astral-sh/ruff/blob/d2ebfd6ed7855140806a7cad3e23d58fe5056c31/crates/red_knot_python_semantic/src/types.rs#L1329-L1346

---

_@AlexWaygood reviewed on 2025-04-17 16:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/subscript/string.md`:24 on 2025-04-17 16:38_

All `StringLiteral` types are subtypes of `LiteralString`, so if we're passing in a `StringLiteral` type as `self` it seems correct to me that we should be selecting the first overload!

---

_@AlexWaygood reviewed on 2025-04-17 16:41_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/subscript/tuple.md`:72 on 2025-04-17 16:41_

```suggestion
    # TODO: Should be `tuple[Literal[1, 'a', b"b"] | None, ...]`
```

---

_@sharkdp reviewed on 2025-04-17 17:49_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:250 on 2025-04-17 17:49_

I think that's correct since `type.__new__` takes 3 arguments. I guess we should change the return statement though, instead of adding an assertion. Or just replace it with `raise NotImplementedError` maybe.

---

_@dhruvmanila reviewed on 2025-04-17 18:20_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:57 on 2025-04-17 18:20_

Yeah, I think this is something that I plan to work on and try to reduce the number of false positive. Currently, the logic for call semantics is to get the first matched overload which has some downsides like the one that you've mentioned. One solution that I'll explore is to use intersection of callables which _should_ reduce the number of false positives like this one but it might still not be correct.

Thanks for catching this, I'll add the relevant TODOs specific to each case.

---

_Comment by @carljm on 2025-04-17 21:23_

@dhruvmanila I looked at the benchmark regression, and I think this is the explanation for it:

Since astral-sh/ruff#17419 we set a size limit on unions of literals. This means that a) this benchmark is now quite fast, therefore sensitive, and b) at a certain point in this benchmark, we fall back from a large union of literals to simply `str`. This means that several of the `+=` here end up looking up `str.__add__` and calling it. `str.__add__` is overloaded, so before this diff calling it was a lot cheaper than after this diff.

So I think that it's fine to go ahead and land this with the regression. The extra cost is directly related to the new correct functionality.

---

_Comment by @dhruvmanila on 2025-04-17 21:31_

> So I think that it's fine to go ahead and land this with the regression. The extra cost is directly related to the new correct functionality.

Makes sense, thanks for looking into it.


---

_Review comment by @carljm on `crates/red_knot/tests/cli.rs`:274 on 2025-04-17 21:33_

I think it would keep this test better focused on its intent and easier to read and understand if we did not trigger the no-matching-overload error here. I recommend just replacing `y` in this line (above) with `int(y)`.

Same in all the (many) other cases throughout this file that also use `range(0, y)` after `y = 4 / 0`.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/instances.md`:313 on 2025-04-17 21:40_

So many longstanding TODOs just disappearing! Love it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:57 on 2025-04-17 21:44_

Yeah, I think intersecting all return values from matching overloads is not really correct when some overloads matched due to `Any`, but I think it's correct for fully-static overloads and fully-static arguments. And even in the not-fully-static cases it should be effective to reduce false positives. So I think it's a good first step.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/narrow/truthiness.md`:250 on 2025-04-17 21:46_

Yeah I think replacing the wrong instantiation with just `raise NotImplementedError()` is a fine option here, it's unrelated to the purpose of the test.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:28 on 2025-04-17 21:47_

```suggestion
overridden by each other.
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:30 on 2025-04-17 21:47_

```suggestion
An overloaded function is overriding another overloaded function:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:58 on 2025-04-17 21:48_

```suggestion
A non-overloaded function is overriding an overloaded function:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:67 on 2025-04-17 21:48_

```suggestion
An overloaded function is overriding a non-overloaded function:
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:84 on 2025-04-17 21:49_

Could add another `reveal_type(foo)` at the top of this code block, just to make the test less implicitly dependent on our code-block-concatenation behavior. (Still dependent on it, but in a way that would fail loudly if this block were no longer concatenated with above blocks.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:302 on 2025-04-17 21:57_

Add a call to the nullary non-generic overload, too?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:468 on 2025-04-17 21:59_

I guess this test works because we hit the implementation and then the new overloaded function is considered separate -- but I think it would be clearer if we used different names for each method here, like the above staticmethod test does.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/subscript/lists.md`:38 on 2025-04-17 22:06_

It is correct to error here, but `call-non-callable` isn't the right error. Probably not much point investigating this further until support for legacy generics lands (so we understand `list` properly as a generic), but I would leave a `TODO: better error` in place on these lines.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_fully_static.md`:131 on 2025-04-17 22:09_

We could also make these assertions about `TypeOf[gradual]` and `TypeOf[static]`, and those should pass as well

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:259 on 2025-04-17 22:10_

Not that it needs to be done in this PR, but just noting here as an aside that it's always possible to implement `is_equivalent_to` for complex cases as simply `x.is_subtype_of(y) && y.is_subtype_of(x)` rather than writing a full dedicated implementation of it.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_gradual_equivalent_to.md`:160 on 2025-04-17 22:12_

Unfortunately bidirectional assignability does not imply gradual equivalence, it's more like a static equivalence check but where `Any/Unknown` matches `Any/Unknown`. (Could still potentially be implemented via shared subtyping/assignability logic, with a third strategy for type comparison passed in as a parameter.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1180 on 2025-04-17 22:15_

I'm curious why we are using multiple models and an import here? The thing we are testing doesn't seem related to imports; this could all be in one file. (Same question for similar cases below.)

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1308 on 2025-04-17 22:22_

```suggestion

Order of overloads is irrelevant for subtyping.

```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1122 on 2025-04-17 22:23_

```suggestion
                // done on the `Identifier` node as opposed to `ExprName` because that's what the
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1127 on 2025-04-17 22:24_

Seems confusing to say "this could be done" and then explain why it couldn't; I'd probably just remove this.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/call/bind.rs`:544 on 2025-04-17 22:26_

This whole block should become a no-op once we understand the (generic identity function) definition of `typing.overload` from typeshed? (Requires legacy generics support). Might be worth a TODO to remove this later.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/display.rs`:408 on 2025-04-17 22:28_

Not sure why (for now) we don't use the same `Overload[...]` syntax used above here as well?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5892 on 2025-04-17 22:39_

We could insert a comment here
```suggestion
        // TODO maybe someday: infer union of overloaded callables if we have a maybe-bound overload?
        if let Symbol::Type(Type::FunctionLiteral(function_literal), Boundness::Bound) =
```

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:5915 on 2025-04-17 22:42_

This works out pretty smoothly!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/signatures.rs`:295 on 2025-04-17 22:51_

I noticed this discrepancy on the generics PR and then I vaguely recall thinking I had observed a reason for it, but I don't remember anymore what that reason was :) This change seems reasonable to me. (We were cloning anyway at the call site, so it doesn't seem to be motivated by efficiency.)

Hopefully @dcreager will weigh in if we are missing a reason not to do this.

---

_@carljm approved on 2025-04-17 22:52_

Awesome work!

---

_@dhruvmanila reviewed on 2025-04-18 03:02_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/binary/integers.md`:57 on 2025-04-18 03:02_

Added TODOs for the last two cases. 

> Possibly we're selecting overload 3 here because we don't properly understand the `_PositiveInteger` and `_NegativeInteger` type aliases in typeshed?

Yeah, this is the reason that we're choosing the incorrect overload.

---

_@dhruvmanila reviewed on 2025-04-18 03:09_

---

_Review comment by @dhruvmanila on `crates/red_knot/tests/cli.rs`:274 on 2025-04-18 03:09_

Yeah, good idea, done.

---

_@dhruvmanila reviewed on 2025-04-18 03:10_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:30 on 2025-04-18 03:10_

Thanks for catching these!

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:84 on 2025-04-18 03:12_

Yeah, makes sense.

---

_@dhruvmanila reviewed on 2025-04-18 03:12_

---

_@dhruvmanila reviewed on 2025-04-18 03:12_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:302 on 2025-04-18 03:12_

Yeah, I thought I added that but clearly didn't. Thanks!

---

_@dhruvmanila reviewed on 2025-04-18 03:15_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/overloads.md`:468 on 2025-04-18 03:15_

Oh yes, good point. Updated the names similar to the above staticmethod checks.

---

_@dhruvmanila reviewed on 2025-04-18 03:21_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1180 on 2025-04-18 03:21_

Do you mean why I've defined it in a stub file? It's mainly so that I don't need to add the implementation function and making sure that it's consistent with the overload signatures. Otherwise I'd need to make sure that they are consistent or I could use protocols which is yet to be implemented or abstract methods which would make the test setup a bit verbose (add `@abstractmethod` to all methods).

Happy to answer if the query is related to something else.

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:1127 on 2025-04-18 03:23_

I mainly added as a note for myself or someone else who looks at it and might think this could be done but I guess then the tests would start failing so that should catch this change. I'll remove it.

---

_@dhruvmanila reviewed on 2025-04-18 03:23_

---

_@carljm reviewed on 2025-04-18 03:27_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_subtype_of.md`:1180 on 2025-04-18 03:27_

No makes sense! Failed to process that it was a stub file

---

_@dhruvmanila reviewed on 2025-04-18 03:28_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/call/bind.rs`:544 on 2025-04-18 03:28_

Added a TODO and a small test case that validates the effect of `bar = overload(foo)`.

---

_@dhruvmanila reviewed on 2025-04-18 03:29_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types/display.rs`:408 on 2025-04-18 03:29_

Yeah, I think I just missed updating the ones on `CallableType`. I've updated it.

---

_@dhruvmanila reviewed on 2025-04-18 03:41_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/src/types.rs`:5892 on 2025-04-18 03:41_

Hmm, interesting. Would you be able to provide an example of where this might be possible?

Would it be something like this:
```py
def _(flag: bool) -> None:
    if flag:

        @overload
        def foo() -> None: ...
        @overload
        def foo(x: int) -> int: ...

	# Here, the previous `foo` overloads are possibly-unbound
    @overload
    def foo(x: str) -> str: ...
    @overload
    def foo(x: bytes) -> bytes: ...
    def foo(x: int | str | bytes | None = None) -> int | str | bytes | None:
        return x

	# Pyright and pyrefly includes all 4 overloads
	# red knot and mypy only includes the 2 overloads outside the `if` branch
    reveal_type(foo)
```

This actually prompted me to add some test cases to include an overload in a union.

---

_@dhruvmanila reviewed on 2025-04-18 03:44_

---

_Review comment by @dhruvmanila on `crates/red_knot_python_semantic/resources/mdtest/type_properties/is_equivalent_to.md`:259 on 2025-04-18 03:44_

Yeah, makes sense. Let me check this on Monday, will probably opt for delegating it to subtype check if it turns out to be complicated.

---

_Comment by @dhruvmanila on 2025-04-18 04:27_

I'm going to go ahead and merge this; most of the new ecosystem changes are in the similar category as documented in the PR description but will put up any follow-up issue if I find anything new and worth prioritizing.

---

_Merged by @dhruvmanila on 2025-04-18 04:27_

---

_Closed by @dhruvmanila on 2025-04-18 04:27_

---

_Branch deleted on 2025-04-18 04:27_

---
