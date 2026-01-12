```yaml
number: 16428
title: "[red-knot] Rework `Type::to_instance()` to return `Option<Type>`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/to-instance-option
created_at: 2025-02-28T00:20:41Z
updated_at: 2025-03-11T16:43:46Z
url: https://github.com/astral-sh/ruff/pull/16428
synced_at: 2026-01-12T15:55:55Z
```

# [red-knot] Rework `Type::to_instance()` to return `Option<Type>`

---

_@AlexWaygood_

## Summary

This PR fixes https://github.com/astral-sh/ruff/issues/16302.

The PR reworks `Type::to_instance()` to return `Option<Type>` rather than `Type`. This reflects more accurately the fact that some variants cannot be "turned into an instance", since they _already_ represent instances of some kind. On `main`, we silently fallback to `Unknown` for these variants, but this implicit behaviour can be somewhat surprising and lead to unexpected bugs.

Returning `Option<Type>` rather than `Type` means that each callsite has to account for the possibility that the type might already represent an instance, and decide what to do about it. 
In general, I think this increases the robustness of the code. Working on this PR revealed two latent bugs in the code:
- One which has already been fixed by https://github.com/astral-sh/ruff/pull/16427
- One which is fixed as part of [this PR (see my inline comment)](https://github.com/astral-sh/ruff/pull/16608)

I added special handling to `KnownClass::to_instance()`: If we fail to find one of these classes and the `test` feature is _not_ enabled, we log a warning to the terminal saying that we failed to find the class in typeshed and that we will be falling back to `Type::Unknown`. A cache is maintained so that we record all classes that we have already logged a warning for; we only log a warning for failing to lookup a `KnownClass` if we know that it's the first time we're looking it up.

## Test Plan

- All existing tests pass
- I ran the property tests via `QUICKCHECK_TESTS=1000000 cargo test --release -p red_knot_python_semantic -- --ignored types::property_tests::stable`

I also manually checked that warnings are appropriately printed to the terminal when `KnownClass::to_instance()` falls back to `Unknown` and the `test` feature is not enabled. To do this, I applied this diff to the PR branch:

<details>
<summary>Patch deleting `int` and `str` from buitins</summary>

```diff
diff --git a/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi b/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi
index 0a6dc57b0..86636a05b 100644
--- a/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi
+++ b/crates/red_knot_vendored/vendor/typeshed/stdlib/builtins.pyi
@@ -228,111 +228,6 @@ _PositiveInteger: TypeAlias = Literal[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13,
 _NegativeInteger: TypeAlias = Literal[-1, -2, -3, -4, -5, -6, -7, -8, -9, -10, -11, -12, -13, -14, -15, -16, -17, -18, -19, -20]
 _LiteralInteger = _PositiveInteger | _NegativeInteger | Literal[0]  # noqa: Y026  # TODO: Use TypeAlias once mypy bugs are fixed
 
-class int:
-    @overload
-    def __new__(cls, x: ConvertibleToInt = ..., /) -> Self: ...
-    @overload
-    def __new__(cls, x: str | bytes | bytearray, /, base: SupportsIndex) -> Self: ...
-    def as_integer_ratio(self) -> tuple[int, Literal[1]]: ...
-    @property
-    def real(self) -> int: ...
-    @property
-    def imag(self) -> Literal[0]: ...
-    @property
-    def numerator(self) -> int: ...
-    @property
-    def denominator(self) -> Literal[1]: ...
-    def conjugate(self) -> int: ...
-    def bit_length(self) -> int: ...
-    if sys.version_info >= (3, 10):
-        def bit_count(self) -> int: ...
-
-    if sys.version_info >= (3, 11):
-        def to_bytes(
-            self, length: SupportsIndex = 1, byteorder: Literal["little", "big"] = "big", *, signed: bool = False
-        ) -> bytes: ...
-        @classmethod
-        def from_bytes(
-            cls,
-            bytes: Iterable[SupportsIndex] | SupportsBytes | ReadableBuffer,
-            byteorder: Literal["little", "big"] = "big",
-            *,
-            signed: bool = False,
-        ) -> Self: ...
-    else:
-        def to_bytes(self, length: SupportsIndex, byteorder: Literal["little", "big"], *, signed: bool = False) -> bytes: ...
-        @classmethod
-        def from_bytes(
-            cls,
-            bytes: Iterable[SupportsIndex] | SupportsBytes | ReadableBuffer,
-            byteorder: Literal["little", "big"],
-            *,
-            signed: bool = False,
-        ) -> Self: ...
-
-    if sys.version_info >= (3, 12):
-        def is_integer(self) -> Literal[True]: ...
-
-    def __add__(self, value: int, /) -> int: ...
-    def __sub__(self, value: int, /) -> int: ...
-    def __mul__(self, value: int, /) -> int: ...
-    def __floordiv__(self, value: int, /) -> int: ...
-    def __truediv__(self, value: int, /) -> float: ...
-    def __mod__(self, value: int, /) -> int: ...
-    def __divmod__(self, value: int, /) -> tuple[int, int]: ...
-    def __radd__(self, value: int, /) -> int: ...
-    def __rsub__(self, value: int, /) -> int: ...
-    def __rmul__(self, value: int, /) -> int: ...
-    def __rfloordiv__(self, value: int, /) -> int: ...
-    def __rtruediv__(self, value: int, /) -> float: ...
-    def __rmod__(self, value: int, /) -> int: ...
-    def __rdivmod__(self, value: int, /) -> tuple[int, int]: ...
-    @overload
-    def __pow__(self, x: Literal[0], /) -> Literal[1]: ...
-    @overload
-    def __pow__(self, value: Literal[0], mod: None, /) -> Literal[1]: ...
-    @overload
-    def __pow__(self, value: _PositiveInteger, mod: None = None, /) -> int: ...
-    @overload
-    def __pow__(self, value: _NegativeInteger, mod: None = None, /) -> float: ...
-    # positive __value -> int; negative __value -> float
-    # return type must be Any as `int | float` causes too many false-positive errors
-    @overload
-    def __pow__(self, value: int, mod: None = None, /) -> Any: ...
-    @overload
-    def __pow__(self, value: int, mod: int, /) -> int: ...
-    def __rpow__(self, value: int, mod: int | None = None, /) -> Any: ...
-    def __and__(self, value: int, /) -> int: ...
-    def __or__(self, value: int, /) -> int: ...
-    def __xor__(self, value: int, /) -> int: ...
-    def __lshift__(self, value: int, /) -> int: ...
-    def __rshift__(self, value: int, /) -> int: ...
-    def __rand__(self, value: int, /) -> int: ...
-    def __ror__(self, value: int, /) -> int: ...
-    def __rxor__(self, value: int, /) -> int: ...
-    def __rlshift__(self, value: int, /) -> int: ...
-    def __rrshift__(self, value: int, /) -> int: ...
-    def __neg__(self) -> int: ...
-    def __pos__(self) -> int: ...
-    def __invert__(self) -> int: ...
-    def __trunc__(self) -> int: ...
-    def __ceil__(self) -> int: ...
-    def __floor__(self) -> int: ...
-    def __round__(self, ndigits: SupportsIndex = ..., /) -> int: ...
-    def __getnewargs__(self) -> tuple[int]: ...
-    def __eq__(self, value: object, /) -> bool: ...
-    def __ne__(self, value: object, /) -> bool: ...
-    def __lt__(self, value: int, /) -> bool: ...
-    def __le__(self, value: int, /) -> bool: ...
-    def __gt__(self, value: int, /) -> bool: ...
-    def __ge__(self, value: int, /) -> bool: ...
-    def __float__(self) -> float: ...
-    def __int__(self) -> int: ...
-    def __abs__(self) -> int: ...
-    def __hash__(self) -> int: ...
-    def __bool__(self) -> bool: ...
-    def __index__(self) -> int: ...
-
 class float:
     def __new__(cls, x: ConvertibleToFloat = ..., /) -> Self: ...
     def as_integer_ratio(self) -> tuple[int, int]: ...
@@ -437,190 +332,6 @@ class _FormatMapMapping(Protocol):
 class _TranslateTable(Protocol):
     def __getitem__(self, key: int, /) -> str | int | None: ...
 
-class str(Sequence[str]):
-    @overload
-    def __new__(cls, object: object = ...) -> Self: ...
-    @overload
-    def __new__(cls, object: ReadableBuffer, encoding: str = ..., errors: str = ...) -> Self: ...
-    @overload
-    def capitalize(self: LiteralString) -> LiteralString: ...
-    @overload
-    def capitalize(self) -> str: ...  # type: ignore[misc]
-    @overload
-    def casefold(self: LiteralString) -> LiteralString: ...
-    @overload
-    def casefold(self) -> str: ...  # type: ignore[misc]
-    @overload
-    def center(self: LiteralString, width: SupportsIndex, fillchar: LiteralString = " ", /) -> LiteralString: ...
-    @overload
-    def center(self, width: SupportsIndex, fillchar: str = " ", /) -> str: ...  # type: ignore[misc]
-    def count(self, sub: str, start: SupportsIndex | None = ..., end: SupportsIndex | None = ..., /) -> int: ...
-    def encode(self, encoding: str = "utf-8", errors: str = "strict") -> bytes: ...
-    def endswith(
-        self, suffix: str | tuple[str, ...], start: SupportsIndex | None = ..., end: SupportsIndex | None = ..., /
-    ) -> bool: ...
-    @overload
-    def expandtabs(self: LiteralString, tabsize: SupportsIndex = 8) -> LiteralString: ...
-    @overload
-    def expandtabs(self, tabsize: SupportsIndex = 8) -> str: ...  # type: ignore[misc]
-    def find(self, sub: str, start: SupportsIndex | None = ..., end: SupportsIndex | None = ..., /) -> int: ...
-    @overload
-    def format(self: LiteralString, *args: LiteralString, **kwargs: LiteralString) -> LiteralString: ...
-    @overload
-    def format(self, *args: object, **kwargs: object) -> str: ...
-    def format_map(self, mapping: _FormatMapMapping, /) -> str: ...
-    def index(self, sub: str, start: SupportsIndex | None = ..., end: SupportsIndex | None = ..., /) -> int: ...
-    def isalnum(self) -> bool: ...
-    def isalpha(self) -> bool: ...
-    def isascii(self) -> bool: ...
-    def isdecimal(self) -> bool: ...
-    def isdigit(self) -> bool: ...
-    def isidentifier(self) -> bool: ...
-    def islower(self) -> bool: ...
-    def isnumeric(self) -> bool: ...
-    def isprintable(self) -> bool: ...
-    def isspace(self) -> bool: ...
-    def istitle(self) -> bool: ...
-    def isupper(self) -> bool: ...
-    @overload
-    def join(self: LiteralString, iterable: Iterable[LiteralString], /) -> LiteralString: ...
-    @overload
-    def join(self, iterable: Iterable[str], /) -> str: ...  # type: ignore[misc]
-    @overload
-    def ljust(self: LiteralString, width: SupportsIndex, fillchar: LiteralString = " ", /) -> LiteralString: ...
-    @overload
-    def ljust(self, width: SupportsIndex, fillchar: str = " ", /) -> str: ...  # type: ignore[misc]
-    @overload
-    def lower(self: LiteralString) -> LiteralString: ...
-    @overload
-    def lower(self) -> str: ...  # type: ignore[misc]
-    @overload
-    def lstrip(self: LiteralString, chars: LiteralString | None = None, /) -> LiteralString: ...
-    @overload
-    def lstrip(self, chars: str | None = None, /) -> str: ...  # type: ignore[misc]
-    @overload
-    def partition(self: LiteralString, sep: LiteralString, /) -> tuple[LiteralString, LiteralString, LiteralString]: ...
-    @overload
-    def partition(self, sep: str, /) -> tuple[str, str, str]: ...  # type: ignore[misc]
-    if sys.version_info >= (3, 13):
-        @overload
-        def replace(
-            self: LiteralString, old: LiteralString, new: LiteralString, /, count: SupportsIndex = -1
-        ) -> LiteralString: ...
-        @overload
-        def replace(self, old: str, new: str, /, count: SupportsIndex = -1) -> str: ...  # type: ignore[misc]
-    else:
-        @overload
-        def replace(
-            self: LiteralString, old: LiteralString, new: LiteralString, count: SupportsIndex = -1, /
-        ) -> LiteralString: ...
-        @overload
-        def replace(self, old: str, new: str, count: SupportsIndex = -1, /) -> str: ...  # type: ignore[misc]
-    if sys.version_info >= (3, 9):
-        @overload
-        def removeprefix(self: LiteralString, prefix: LiteralString, /) -> LiteralString: ...
-        @overload
-        def removeprefix(self, prefix: str, /) -> str: ...  # type: ignore[misc]
-        @overload
-        def removesuffix(self: LiteralString, suffix: LiteralString, /) -> LiteralString: ...
-        @overload
-        def removesuffix(self, suffix: str, /) -> str: ...  # type: ignore[misc]
-
-    def rfind(self, sub: str, start: SupportsIndex | None = ..., end: SupportsIndex | None = ..., /) -> int: ...
-    def rindex(self, sub: str, start: SupportsIndex | None = ..., end: SupportsIndex | None = ..., /) -> int: ...
-    @overload
-    def rjust(self: LiteralString, width: SupportsIndex, fillchar: LiteralString = " ", /) -> LiteralString: ...
-    @overload
-    def rjust(self, width: SupportsIndex, fillchar: str = " ", /) -> str: ...  # type: ignore[misc]
-    @overload
-    def rpartition(self: LiteralString, sep: LiteralString, /) -> tuple[LiteralString, LiteralString, LiteralString]: ...
-    @overload
-    def rpartition(self, sep: str, /) -> tuple[str, str, str]: ...  # type: ignore[misc]
-    @overload
-    def rsplit(self: LiteralString, sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString]: ...
-    @overload
-    def rsplit(self, sep: str | None = None, maxsplit: SupportsIndex = -1) -> list[str]: ...  # type: ignore[misc]
-    @overload
-    def rstrip(self: LiteralString, chars: LiteralString | None = None, /) -> LiteralString: ...
-    @overload
-    def rstrip(self, chars: str | None = None, /) -> str: ...  # type: ignore[misc]
-    @overload
-    def split(self: LiteralString, sep: LiteralString | None = None, maxsplit: SupportsIndex = -1) -> list[LiteralString]: ...
-    @overload
-    def split(self, sep: str | None = None, maxsplit: SupportsIndex = -1) -> list[str]: ...  # type: ignore[misc]
-    @overload
-    def splitlines(self: LiteralString, keepends: bool = False) -> list[LiteralString]: ...
-    @overload
-    def splitlines(self, keepends: bool = False) -> list[str]: ...  # type: ignore[misc]
-    def startswith(
-        self, prefix: str | tuple[str, ...], start: SupportsIndex | None = ..., end: SupportsIndex | None = ..., /
-    ) -> bool: ...
-    @overload
-    def strip(self: LiteralString, chars: LiteralString | None = None, /) -> LiteralString: ...
-    @overload
-    def strip(self, chars: str | None = None, /) -> str: ...  # type: ignore[misc]
-    @overload
-    def swapcase(self: LiteralString) -> LiteralString: ...
-    @overload
-    def swapcase(self) -> str: ...  # type: ignore[misc]
-    @overload
-    def title(self: LiteralString) -> LiteralString: ...
-    @overload
-    def title(self) -> str: ...  # type: ignore[misc]
-    def translate(self, table: _TranslateTable, /) -> str: ...
-    @overload
-    def upper(self: LiteralString) -> LiteralString: ...
-    @overload
-    def upper(self) -> str: ...  # type: ignore[misc]
-    @overload
-    def zfill(self: LiteralString, width: SupportsIndex, /) -> LiteralString: ...
-    @overload
-    def zfill(self, width: SupportsIndex, /) -> str: ...  # type: ignore[misc]
-    @staticmethod
-    @overload
-    def maketrans(x: dict[int, _T] | dict[str, _T] | dict[str | int, _T], /) -> dict[int, _T]: ...
-    @staticmethod
-    @overload
-    def maketrans(x: str, y: str, /) -> dict[int, int]: ...
-    @staticmethod
-    @overload
-    def maketrans(x: str, y: str, z: str, /) -> dict[int, int | None]: ...
-    @overload
-    def __add__(self: LiteralString, value: LiteralString, /) -> LiteralString: ...
-    @overload
-    def __add__(self, value: str, /) -> str: ...  # type: ignore[misc]
-    # Incompatible with Sequence.__contains__
-    def __contains__(self, key: str, /) -> bool: ...  # type: ignore[override]
-    def __eq__(self, value: object, /) -> bool: ...
-    def __ge__(self, value: str, /) -> bool: ...
-    @overload
-    def __getitem__(self: LiteralString, key: SupportsIndex | slice, /) -> LiteralString: ...
-    @overload
-    def __getitem__(self, key: SupportsIndex | slice, /) -> str: ...  # type: ignore[misc]
-    def __gt__(self, value: str, /) -> bool: ...
-    def __hash__(self) -> int: ...
-    @overload
-    def __iter__(self: LiteralString) -> Iterator[LiteralString]: ...
-    @overload
-    def __iter__(self) -> Iterator[str]: ...  # type: ignore[misc]
-    def __le__(self, value: str, /) -> bool: ...
-    def __len__(self) -> int: ...
-    def __lt__(self, value: str, /) -> bool: ...
-    @overload
-    def __mod__(self: LiteralString, value: LiteralString | tuple[LiteralString, ...], /) -> LiteralString: ...
-    @overload
-    def __mod__(self, value: Any, /) -> str: ...
-    @overload
-    def __mul__(self: LiteralString, value: SupportsIndex, /) -> LiteralString: ...
-    @overload
-    def __mul__(self, value: SupportsIndex, /) -> str: ...  # type: ignore[misc]
-    def __ne__(self, value: object, /) -> bool: ...
-    @overload
-    def __rmul__(self: LiteralString, value: SupportsIndex, /) -> LiteralString: ...
-    @overload
-    def __rmul__(self, value: SupportsIndex, /) -> str: ...  # type: ignore[misc]
-    def __getnewargs__(self) -> tuple[str]: ...
-
 class bytes(Sequence[int]):
```

</details>

And then ran red-knot on my [typeshed-stats](https://github.com/AlexWaygood/typeshed-stats) project using the command

```
cargo run -p red_knot -- check --project ../typeshed-stats --python-version="3.12" --verbose
```

I observed that the following logs were printed to the terminal, but that each warning was only printed once (the desired behaviour):

```
INFO Python version: Python 3.12, platform: all
INFO Found 15 files in project `typeshed-stats`
INFO Python version: Python 3.12, platform: all
INFO Indexed 15 file(s)
INFO Could not find class `builtins.int` in typeshed on Python 3.12. Falling back to `Unknown` for the symbol instead.
INFO Could not find class `builtins.str` in typeshed on Python 3.12. Falling back to `Unknown` for the symbol instead.
```


---

_Label `red-knot` added by @AlexWaygood on 2025-02-28 00:20_

---

_@AlexWaygood reviewed on 2025-02-28 13:54_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1069 on 2025-02-28 13:54_

The change here is that the canonical module for `KnownClass::TypeAliasType` is now `KnownModule::TypingExtensions` rather than `KnownModule::Typing`. Running the property tests on this branch instantly crashed without this change, as it was revealed that until now `KnownClass::TypeAliasType.to_instance(db)` has been silently resolving to `Type::Unknown` all this time, due to the fact that it was only added to the typing module in Python 3.12 but our default Python version is 3.9.

---

_Marked ready for review by @AlexWaygood on 2025-02-28 14:02_

---

_Review requested from @carljm by @AlexWaygood on 2025-02-28 14:02_

---

_Review requested from @MichaReiser by @AlexWaygood on 2025-02-28 14:02_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-02-28 14:03_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1013 on 2025-02-28 14:21_

this approach was inspired by the existing Ruff macro `warn_user_once_by_message!()` here:

https://github.com/astral-sh/ruff/blob/0c7c00164744db56538843935073c15f086f6903/crates/ruff_linter/src/logging.rs#L37-L55

I couldn't find any tests for that macro, and I wasn't sure how to test this (since it deliberately works differently depending on whether the `test` feature is enabled, in order to prevent bugs silently going unnoticed in tests!). If anybody has any ideas, I'd be interested!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:913 on 2025-02-28 14:21_

Or I suppose this could be

```suggestion
                if cfg!(debug_assertions) {
```

?

---

_@AlexWaygood reviewed on 2025-02-28 14:21_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:1132 on 2025-02-28 16:49_

Shouldn't it be an invariant that a class' metaclass is always something instantiable? It seems like it should be, since the class itself should be an instance of its metaclass.

Should we have a method that gets the instance type for a class' metaclass and handles this internally with an `unreachable!` instead?

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:913 on 2025-02-28 16:53_

or instead of being an if/else, this could simply be `debug_assert!(false, "{}", lookup_error.display(db, self));`

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1005 on 2025-02-28 17:00_

FWIW, I'm not totally convinced that we need to / should warn on this at all. It seems natural that if you don't have a class in your custom typeshed, then any special behavior we have around that class won't occur. If someone is doing this intentionally, it could be irritating to have a non-silencable warning about it (and using a "minimalist" typeshed doesn't seem like a crazy thing for someone to do intentionally). It could be accidental, but there are tons of ways a custom typeshed could quietly break things, it seems odd to warn about this specific one.

I don't feel strongly about it, though, what you have here seems fine and we can adjust with more experience.

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1660 on 2025-02-28 17:38_

I guess this is a wording nit, sort of related to my "not sure we need to warn" comment above -- but I slightly object to the framing of this as a "broken" typeshed. I think we should aim to consider typesheds where certain symbols aren't present or aren't what we expect to still be "working", where some behaviors will just fall back to gradual typing. Maybe it doesn't really matter, but I'd prefer we don't accept the idea that it's OK for things to be "broken" because a minimal typeshed is used.

---

_@carljm approved on 2025-02-28 17:44_

Looks great, thank you!

---

_@AlexWaygood reviewed on 2025-02-28 17:53_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1005 on 2025-02-28 17:53_

hmmm... yeah, I originally suggested just logging it (so that the information would be available to us when debugging, but wouldn't be printed by default), but @MichaReiser suggested in https://github.com/astral-sh/ruff/issues/16302#issuecomment-2674484226 that a warning might be better... I think I'll switch it to being a LOG rather than a warning, so that you only get it if you specify `--verbose` on the CLI

---

_@AlexWaygood reviewed on 2025-02-28 17:59_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1660 on 2025-02-28 17:59_

Fair enough. FWIW, we _do_ panic (Salsa cycles etc.) if there's no `object` class in builtins; we did before this PR, and will continue to with this PR. I'm not sure I have the appetite to fix that "bug" :P

---

_@carljm reviewed on 2025-02-28 18:07_

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types/class.rs`:1660 on 2025-02-28 18:07_

If it's salsa cycles, I expect the panics will go away with fixpoint iteration? Will check! Not having `object` basically means everything is `Unknown` (because everything inherits `Unknown`)...

---

_@AlexWaygood reviewed on 2025-02-28 18:13_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:1132 on 2025-02-28 18:13_

interesting point -- yes, I think you're right!

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/mdtest_custom_typeshed.md`:39 on 2025-03-09 10:59_

The changes in this PR do mean that it's a little harder for us to write tests with minimal custom typesheds. I still think that's preferable to having various stdlib symbols silently fallback to `Unknown` in the context of one of our mdtests, though. I've already run into situations where this has led to hard-to-debug issues when writing tests that use custom typesheds. In the context of one of our tests, I'd much rather have a panic.

---

_Review comment by @AlexWaygood on `crates/red_knot_project/tests/check.rs`:96 on 2025-03-09 11:01_

This is necessary because some of the parser and linter corpus files use e.g. `except *` syntax, for which we need the `BaseExceptionGroup` symbol to be available, and it's only available on Python 3.11+. We would have had to make this change anyway pretty soon, since we'll probably be integrating @ntBre's work on improved syntax-error detection into red-knot at some point in the next few weeks.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1037 on 2025-03-09 11:04_

I changed this so that it panics if _either_ the `test` feature is enabled _or_ the `debug_assertions` feature is enabled. That's because we have some tests for which the `test` feature is apparently not enabled (mdtests), and we have some tests that we only ever run in release-mode in CI (property tests). I think we probably want both to panic if a known-class symbol lookup unexpectedly fails, rather than silently falling back to `Unknown`.

---

_@AlexWaygood reviewed on 2025-03-09 11:04_

---

_Comment by @AlexWaygood on 2025-03-09 12:07_

I've rebased this, addressed review comments and improved some docs. @carljm it's changed a bit since you last took a look; you might want to give it a quick once-over again to check you're still okay with it.

---

_Review requested from @carljm by @AlexWaygood on 2025-03-09 12:07_

---

_Review comment by @MichaReiser on `crates/red_knot_project/tests/check.rs`:96 on 2025-03-10 09:16_

I don't think this change is necessary for the syntax-work integration because this test only asserts that we can pull all types. It doesn't assert the absence of diagnostics. 

It's not clear to me why this change is necessary for what you did in this PR. Does that mean that Red Knot now panics if someone uses `except *` but sets `python-version=3.9` in their project configuration? 



---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:1037 on 2025-03-10 09:20_

The downside of panicking in tests is that we now never actually test the fallback behavior... So we won't know if it works as expected. I'm inclined to remove the panic case.

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/src/types/class.rs`:1323 on 2025-03-10 09:21_

Nit: Consider implementing `Display` for `KnownClass`

---

_@MichaReiser reviewed on 2025-03-10 09:22_

---

_@AlexWaygood reviewed on 2025-03-10 09:26_

---

_Review comment by @AlexWaygood on `crates/red_knot_project/tests/check.rs`:96 on 2025-03-10 09:26_

> I don't think this change is necessary for the syntax-work integration because this test only asserts that we can pull all types. It doesn't assert the absence of diagnostics.

Hmm, fair point

> Does that mean that Red Knot now panics if someone uses `except *` but sets `python-version=3.9` in their project configuration?

Only if the `test` or `debug_assertions` features is enabled for their red-knot build. Which shouldn't ever really be the case for end users of red-knot.

---

_@MichaReiser reviewed on 2025-03-10 09:28_

---

_Review comment by @MichaReiser on `crates/red_knot_project/tests/check.rs`:96 on 2025-03-10 09:28_

Yeah, not having any test coverage that red knot indeed doesn't panic in those cases those make me a little nervous and in favor of removing the panics.

---

_Comment by @AlexWaygood on 2025-03-10 17:46_

@MichaReiser persuaded me in my 1:1 earlier that I should remove the panicking behaviour from `KnownClass::to_instance()`. I still don't _love_ that some variants will silently (and unexpectedly) fallback to `Unknown` if you forget to add stubs for them when writing mdtests with custom typesheds. But at least we'll now emit debug logs to tell you about that (you just need to remember to enable logging if you're debugging your mdtest!).

---

_Converted to draft by @AlexWaygood on 2025-03-10 17:47_

---

_Comment by @github-actions[bot] on 2025-03-10 18:01_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @AlexWaygood on 2025-03-11 16:23_

---

_Comment by @AlexWaygood on 2025-03-11 16:28_

I think I've addressed/removed all the controversial points in this, and Carl approved an earlier version -- scheduling automerge now

---

_@AlexWaygood reviewed on 2025-03-11 16:38_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/class.rs`:1028 on 2025-03-11 16:38_

I made this `tracing::info!()` rather than `tracing::warn!()` or `tracing::debug!()`. 

- `tracing::warn!()` is always shown
- `tracing::info!()` is shown with `--verbose` but not otherwise.
- `tracing::debug!()` is only shown with `--verbose --verbose`, and a _lot_ of other tracing events are emitted if you specify that level of verbosity

---

_Merged by @AlexWaygood on 2025-03-11 16:42_

---

_Closed by @AlexWaygood on 2025-03-11 16:42_

---

_Branch deleted on 2025-03-11 16:42_

---
