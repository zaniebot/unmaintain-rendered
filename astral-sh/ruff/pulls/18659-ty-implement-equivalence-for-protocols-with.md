```yaml
number: 18659
title: "[ty] Implement equivalence for protocols with method members"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/method-members
created_at: 2025-06-13T12:08:26Z
updated_at: 2025-07-07T11:28:34Z
url: https://github.com/astral-sh/ruff/pull/18659
synced_at: 2026-01-12T15:56:23Z
```

# [ty] Implement equivalence for protocols with method members

---

_@AlexWaygood_

## Summary

This PR implements the following pieces of `Protocol` semantics:
1. A protocol with a method member that does not have a fully static signature should not be considered fully static. I.e., this protocol is not fully static because `Foo.x` has no return type; we previously incorrectly considered that it was:
  ```py
  class Foo(Protocol):
      def f(self): ...
  ```
2. Two protocols `P1` and `P2`, both with method members `x`, should be considered equivalent if the signature of `P1.x` is equivalent to the signature of `P2.x`. Currently we do not recognize this.

Implementing these semantics requires distinguishing between method members and non-method members. The stored type of a method member must be eagerly upcast to a `Callable` type when collecting the protocol's interface: doing otherwise would mean that it would be hard to implement equivalence of protocols even in the face of differently ordered unions, since the two equivalent protocols would have different Salsa IDs even when normalized.

The semantics implemented by this PR are that we consider something a method member if:
1. It is accessible on the class itself; and
2. It is a function-like callable: a callable type that also has a `__get__` method, meaning it can be used as a method when accessed on instances.

Note that the spec has complicated things to say about classmethod members and staticmethod members. These semantics are not implemented by this PR; they are all deferred for now.

The infrastructure added in this PR fixes bugs in its own right, but also lays the groundwork for implementing subtyping and assignability rules for method members of protocols. A (currently failing) test is added to verify this.

## Test Plan

mdtests


---

_Review requested from @carljm by @AlexWaygood on 2025-06-13 12:08_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-06-13 12:08_

---

_Review requested from @dcreager by @AlexWaygood on 2025-06-13 12:08_

---

_Label `ty` added by @AlexWaygood on 2025-06-13 12:08_

---

_Comment by @AlexWaygood on 2025-06-13 12:13_

Hooray... more protocol-related stack overflows...

---

_Comment by @AlexWaygood on 2025-06-13 12:17_

In the corpus test, we're overflowing our stack when pulling types for [`hashlib.pyi` in typeshed](https://github.com/astral-sh/ruff/blob/3aae1cd59b6490c72af055d28add6cc2f983e545/crates/ty_vendored/vendor/typeshed/stdlib/hashlib.pyi)

---

_Comment by @AlexWaygood on 2025-06-13 12:28_

Minimal repro for the overflow:

```py
from typing_extensions import Protocol, Self

class _HashObject(Protocol):
    def copy(self) -> Self: ...

class Foo: ...

x: Foo | _HashObject
```

---

_Comment by @AlexWaygood on 2025-06-13 12:58_

The stack overflow is because `Self` is implemented as a TypeVar where the upper bound is a self-reference. In order to build the union `Foo | _HashObject` we need to know whether `_HashObject` is fully static. This now requires us to look at the return annotation of any method members; previously, we did not.

---

_Comment by @AlexWaygood on 2025-06-13 13:08_

I fixed the issue with `Self` in https://github.com/astral-sh/ruff/pull/18659/commits/cae39bb4230351715e01129df31742f12ced86ef (at least, we no longer panic when running ty on typeshed 😄 -- let's see what primer says...).

---

_Comment by @AlexWaygood on 2025-06-13 13:52_

The stack overflow in mypy_primer is on psycopg. I'm attempting to minimize it.

---

_Comment by @AlexWaygood on 2025-06-13 14:37_

Self-contained repro for the psycopg overflow:

```py
from typing_extensions import Protocol, Self

class PGconn(Protocol):
    def connect(self) -> Self: ...

class Connection:
    pgconn: PGconn

def is_crdb(conn: PGconn) -> bool:
    isinstance(conn, Connection)
```

---

_Comment by @AlexWaygood on 2025-06-17 10:55_

This also repros the overflow: it's the fact that the `Self` type parameter has a recursive upper bound that matters:

```py
from typing_extensions import Protocol

class PGconn(Protocol):
    def connect[T: PGconn](self: T) -> T: ...

class Connection:
    pgconn: PGconn

def f(x: PGconn):
    isinstance(x, Connection)
```

---

_Comment by @github-actions[bot] on 2025-06-17 15:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
parso (https://github.com/davidhalter/parso)
-     memo fields = ~49MB
+     memo fields = ~54MB

attrs (https://github.com/python-attrs/attrs)
-     struct metadata = ~2MB
+     struct metadata = ~3MB

anyio (https://github.com/agronholm/anyio)
-     memo metadata = ~6MB
+     memo metadata = ~7MB

Expression (https://github.com/cognitedata/Expression)
-     memo metadata = ~6MB
+     memo metadata = ~7MB

mypy-protobuf (https://github.com/dropbox/mypy-protobuf)
-     struct fields = ~1MB
+     struct fields = ~2MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

ignite (https://github.com/pytorch/ignite)
-     memo fields = ~189MB
+     memo fields = ~171MB

mkosi (https://github.com/systemd/mkosi)
-     memo metadata = ~10MB
+     memo metadata = ~11MB
-     memo fields = ~129MB
+     memo fields = ~117MB

flake8 (https://github.com/pycqa/flake8)
-     memo fields = ~60MB
+     memo fields = ~66MB

SinbadCogs (https://github.com/mikeshardmind/SinbadCogs)
-     memo metadata = ~4MB
+     memo metadata = ~5MB

colour (https://github.com/colour-science/colour)
- TOTAL MEMORY USAGE: ~405MB
+ TOTAL MEMORY USAGE: ~445MB

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
-     memo metadata = ~28MB
+     memo metadata = ~30MB

AutoSplit (https://github.com/Toufool/AutoSplit)
- TOTAL MEMORY USAGE: ~207MB
+ TOTAL MEMORY USAGE: ~228MB

django-stubs (https://github.com/typeddjango/django-stubs)
-     struct metadata = ~6MB
+     struct metadata = ~7MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~171MB
+     memo fields = ~189MB

scrapy (https://github.com/scrapy/scrapy)
-     memo fields = ~207MB
+     memo fields = ~189MB

bokeh (https://github.com/bokeh/bokeh)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~251MB

```
</details>


---

_Comment by @carljm on 2025-06-17 15:19_

@AlexWaygood I wouldn't spend too much time digging into those psycopg2 stack overflows -- they seem like pretty straightforward cases of recursive protocols, where I'd expect us to stack overflow currently. I think either adding psycopg2 to `bad.txt`, or holding off on this PR until we support recursive protocols (if we're worried about users seeing new stack overflows) are probably the right answers here.

---

_Comment by @AlexWaygood on 2025-06-17 15:33_

> @AlexWaygood I wouldn't spend too much time digging into those psycopg2 stack overflows -- they seem like pretty straightforward cases of recursive protocols, where I'd expect us to stack overflow currently. I think either adding psycopg2 to `bad.txt`, or holding off on this PR until we support recursive protocols (if we're worried about users seeing new stack overflows) are probably the right answers here.

Alrighty -- in that case, this PR is ready for review. It adds 3 projects to `bad.txt`: psycopg, pytest and scrapy. (That might be too much, but I think if that's the case then we can essentially implement no new protocol features until we get the recursive-protocol issue fixed ☹️)

---

_Comment by @AlexWaygood on 2025-06-17 16:13_

(I do still wish I understood more specifically what difference the `isinstance()` call makes here. https://github.com/astral-sh/ruff/pull/18659/commits/6db4b6c7048064df64d7e83b4ceeb2db3e50f776 _did_ fix the original stack overflow from https://github.com/astral-sh/ruff/pull/18659#issuecomment-2970240270, but not the new one that has the `isinstance()` call.)

---

_Converted to draft by @AlexWaygood on 2025-07-01 13:59_

---

_Comment by @AlexWaygood on 2025-07-01 14:05_

https://github.com/astral-sh/ruff/pull/18659#issuecomment-2970609588 and https://github.com/astral-sh/ruff/pull/18659#issuecomment-2979899363 no longer cause stack overflows now this branch has been rebased on top of https://github.com/astral-sh/ruff/pull/19003... but there are still stack overflows on kopf/jinja that are causing the mypy_primer run to fail...

---

_Comment by @codspeed-hq[bot] on 2025-07-01 14:13_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed WallTime Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmethod-members?runnerMode=WallTime)

### Merging #18659 will **not alter performance**

<sub>Comparing <code>alex/method-members</code> (f81ba79) with <code>main</code> (e0b7f49)</sub>



### Summary

`✅ 7` untouched benchmarks  





---

_Comment by @AlexWaygood on 2025-07-01 15:42_

Here's a minimized repro for the overflow on jinja on this branch:

```py
from typing import cast, Protocol

class Iterator[T](Protocol):
    def __iter__(self) -> Iterator[T]: ...

def f(value: Iterator):
    cast(Iterator, value)
```

It only occurs if the `Iterator` protocol is generic.

---

_Comment by @AlexWaygood on 2025-07-01 16:32_

> Here's a minimized repro for the overflow on jinja on this branch:
> 
> ```python
> from typing import cast, Protocol
> 
> class Iterator[T](Protocol):
>     def __iter__(self) -> Iterator[T]: ...
> 
> def f(value: Iterator):
>     cast(Iterator, value)
> ```
> 
> It only occurs if the `Iterator` protocol is generic.

Okay, the overflow goes away if I apply this change:

```diff
diff --git a/crates/ty_python_semantic/src/types/function.rs b/crates/ty_python_semantic/src/types/function.rs
index 8149dad8d3..895ef45377 100644
--- a/crates/ty_python_semantic/src/types/function.rs
+++ b/crates/ty_python_semantic/src/types/function.rs
@@ -1148,8 +1148,6 @@ impl KnownFunction {
                 let contains_unknown_or_todo =
                     |ty| matches!(ty, Type::Dynamic(dynamic) if dynamic != DynamicType::Any);
                 if source_type.is_equivalent_to(db, *casted_type)
-                    && !casted_type.any_over_type(db, &|ty| contains_unknown_or_todo(ty))
-                    && !source_type.any_over_type(db, &|ty| contains_unknown_or_todo(ty))
                 {
                     let builder = context.report_lint(&REDUNDANT_CAST, call_expression)?;
                     builder.into_diagnostic(format_args!(
```

So it's the `any_over_type` calls that are causing the overflow here, not the `is_equivalent_to` call! We likely need to apply a similar change to `Type::any_over_type()` to the one @carljm applied to `Type::normalized()` in https://github.com/astral-sh/ruff/pull/19003. 

This possibly makes the idea of a generalized `TypeVisitor` trait (which could have specialized implementations both for implementing `Type::normalized()` and for implementing `Type::any_over_type()`?) more appealing? Not sure, though; would need to try it out.

---

_Comment by @AlexWaygood on 2025-07-01 18:46_

Huzzah! No stack overflows, and a clean mypy_primer report.

---

_Marked ready for review by @AlexWaygood on 2025-07-01 18:46_

---

_Comment by @AlexWaygood on 2025-07-01 18:55_

I'll clean this up a bit more

---

_Converted to draft by @AlexWaygood on 2025-07-01 18:55_

---

_Comment by @carljm on 2025-07-01 20:38_

On second thought, I do think that a recursive fallback of `false` should be hardcoded for `any_over_type`; there's no (present or future) value in carrying around the `recursive_fallback` parameter. 

`any_over_type` answers the question "does any type anywhere inside this type match the given predicate?" By the very nature of that question, `false` is the only possible reasonable recursive fallback answer. `true` could only make sense for a hypothetical `all_over_type` -- which `is_fully_static` could have been an example of, had I not removed it :)

If we were implementing some kind of more generic recursive type traversal that we might use for `all_over_type` questions in future, then I think it might make sense, but I can't see a case for it in an implementation specific to `any_over_type`, as this one is. 

---

_Comment by @AlexWaygood on 2025-07-02 15:14_

> `any_over_type` answers the question "does any type anywhere inside this type match the given predicate?" By the very nature of that question, `false` is the only possible reasonable recursive fallback answer. `true` could only make sense for a hypothetical `all_over_type` -- which `is_fully_static` could have been an example of, had I not removed it :)

Couldn't you check to see whether a type is itself recursive by calling `ty.any_over_type(db, |_| false, true)`? (I.e., the closure itself would never return `true`, but the overall evaluation would return `true` if we ever encountered a type that we'd already seen while recursing into it?)

I think I prefer https://github.com/astral-sh/ruff/pull/19094 as a solution over what I have here now though:
- we only insert non-atomic types into the `seen_types` cache (avoids unnecessary hashing)
- much harder to forget to recurse into nested types
- more generalized; it allows us to easily build other functions on top of it as well as `any_over_type`. E.g. if you wanted to write that `is_recursive_type` function, you could easily do it using a new `TypeVisitor` implementation; there's no need to use `any_over_type` for it at all.

The codspeed report for #19094 does overall seem to be better than on this PR, though this PR obviously does a few other things aside from the `any_over_type` rewrite.

---

_Marked ready for review by @AlexWaygood on 2025-07-03 19:29_

---

_Comment by @AlexWaygood on 2025-07-03 19:34_

This is once again ready for review (and really quite a minimal change at this point!)

---

_Converted to draft by @AlexWaygood on 2025-07-04 13:45_

---

_Comment by @AlexWaygood on 2025-07-04 14:17_

This PR unfortunately appears to cause catastrophic execution time on the following snippet, where we're asked to determine in the final line whether one (very large) protocol is assignable to another (also very large) protocol:

<details>

```py
from __future__ import annotations

from typing import TypeVar, overload, Protocol
from ty_extensions import is_assignable_to, static_assert

class Foo: ...
class Bar: ...
class Baz: ...

_D = TypeVar("_D", bound="Date")
_GMaybeTZT = TypeVar("_GMaybeTZT", bound=Baz|None, covariant=True)
_GMaybeTZDT = TypeVar("_GMaybeTZDT", bound=Baz|None, covariant=True)
_FuncTZ = TypeVar("_FuncTZ", bound=Baz)
_FuncOptionalTZ = TypeVar("_FuncOptionalTZ", bound=Baz|None)
Self = TypeVar("Self")

class Date(Protocol):
    def _subclass_check_hook(cls, instance: object) -> bool: ...
    min: Date
    max: Date
    resolution: Bar
    def fromtimestamp(cls, __timestamp: float) -> Date: ...
    def today(cls) -> Date: ...
    def fromordinal(cls, __n: int) -> Date: ...
    def fromisoformat(cls, __date_string: str) -> Date: ...
    def fromisocalendar(cls, year: int, week: int, day: int) -> Date: ...
    def year(self) -> int: ...
    def month(self) -> int: ...
    def day(self) -> int: ...
    def ctime(self) -> str: ...
    def strftime(self, __format: str) -> str: ...
    def __format__(self, __fmt: str) -> str: ...
    def isoformat(self) -> str: ...
    def timetuple(self) -> Foo: ...
    def toordinal(self) -> int: ...
    def replace(self: Self, year: int = ..., month: int = ..., day: int = ...) -> Self: ...
    def __le__(self, __other: Date) -> bool: ...
    def __lt__(self, __other: Date) -> bool: ...
    def __ge__(self, __other: Date) -> bool: ...
    def __gt__(self, __other: Date) -> bool: ...
    def __add__(self: Self, __other: Bar) -> Self: ...
    def __radd__(self: Self, __other: Bar) -> Self: ...
    def __hash__(self) -> int: ...
    def weekday(self) -> int: ...
    def isoweekday(self) -> int: ...
    @overload
    def __sub__(self: Self, __other: Bar) -> Self: ...
    @overload
    def __sub__(self: _D, __other: _D) -> Bar: ...


class Time(Protocol[_GMaybeTZT]):
    min: Time[None]
    max: Time[None]
    resolution: Bar
    def hour(self) -> int: ...
    def minute(self) -> int: ...
    def second(self) -> int: ...
    def microsecond(self) -> int: ...
    def tzinfo(self) -> _GMaybeTZT: ...
    def fold(self) -> int: ...
    def __le__(self: Self, __other: Self) -> bool: ...
    def __lt__(self: Self, __other: Self) -> bool: ...
    def __ge__(self: Self, __other: Self) -> bool: ...
    def __gt__(self: Self, __other: Self) -> bool: ...
    def __hash__(self) -> int: ...
    def isoformat(self, timespec: str = ...) -> str: ...
    def strftime(self, __format: str) -> str: ...
    def __format__(self, __fmt: str) -> str: ...
    def utcoffset(self) -> Bar | None: ...
    def tzname(self) -> str | None: ...
    def dst(self) -> Bar | None: ...
    @overload
    def replace(
        self: Self,
        hour: int = ...,
        minute: int = ...,
        second: int = ...,
        microsecond: int = ...,
        *,
        fold: int = ...,
    ) -> Self: ...
    @overload
    def replace(
        self,
        hour: int = ...,
        minute: int = ...,
        second: int = ...,
        microsecond: int = ...,
        *,
        tzinfo: _FuncTZ,
        fold: int = ...,
    ) -> Time[_FuncTZ]: ...
    @overload
    def replace(
        self,
        hour: int = ...,
        minute: int = ...,
        second: int = ...,
        microsecond: int = ...,
        *,
        tzinfo: None,
        fold: int = ...,
    ) -> Time[None]: ...
    @overload
    def replace(
        self,
        hour: int,
        minute: int,
        second: int,
        microsecond: int,
        tzinfo: None,
        *,
        fold: int,
    ) -> Time[None]: ...
    @overload
    def replace(
        self,
        hour: int,
        minute: int,
        second: int,
        microsecond: int,
        tzinfo: _FuncTZ,
        *,
        fold: int,
    ) -> Time[_FuncTZ]: ...
    def fromisoformat(cls, __time_string: str) -> Time[Baz | None]: ...


class NaiveTime(Time[None], Protocol): ...


DTSelf = TypeVar("DTSelf", bound="DateTime")


class DateTime(Protocol[_GMaybeTZDT]):
    resolution: Bar
    def hour(self) -> int: ...
    def minute(self) -> int: ...
    def second(self) -> int: ...
    def microsecond(self) -> int: ...
    def tzinfo(self) -> _GMaybeTZDT: ...
    def fold(self) -> int: ...
    def timestamp(self) -> float: ...
    def utctimetuple(self) -> Foo: ...
    def date(self) -> Date: ...
    def time(self) -> NaiveTime: ...
    @overload
    def replace(
        self: Self,
        year: int = ...,
        month: int = ...,
        day: int = ...,
        hour: int = ...,
        minute: int = ...,
        second: int = ...,
        microsecond: int = ...,
        *,
        tzinfo: _FuncTZ,
        fold: int = ...,
    ) -> DateTime[_FuncTZ]: ...
    @overload
    def replace(
        self,
        year: int,
        month: int,
        day: int,
        hour: int,
        minute: int,
        second: int,
        microsecond: int,
        tzinfo: _FuncTZ,
        *,
        fold: int,
    ) -> DateTime[_FuncTZ]: ...
    @overload
    def replace(
        self,
        year: int = ...,
        month: int = ...,
        day: int = ...,
        hour: int = ...,
        minute: int = ...,
        second: int = ...,
        microsecond: int = ...,
        *,
        tzinfo: None,
        fold: int = ...,
    ) -> NaiveDateTime: ...
    @overload
    def replace(
        self: Self,
        year: int,
        month: int,
        day: int,
        hour: int,
        minute: int,
        second: int,
        microsecond: int,
        tzinfo: None,
        *,
        fold: int,
    ) -> NaiveDateTime: ...
    @overload
    def replace(
        self: Self,
        year: int = ...,
        month: int = ...,
        day: int = ...,
        hour: int = ...,
        minute: int = ...,
        second: int = ...,
        microsecond: int = ...,
        *,
        fold: int = ...,
    ) -> Self: ...

    def fromtimestamp(
        cls: type[Self], __timestamp: float, tz: _FuncOptionalTZ
    ) -> DateTime[_FuncOptionalTZ]: ...

    @overload
    def now(cls, tz: _FuncOptionalTZ) -> DateTime[_FuncOptionalTZ]: ...

    @overload
    def now(cls) -> DateTime[None]: ...

    def now(cls, tz: Baz | None = None) -> DateTime[Baz | None]: ...


class NaiveDateTime(DateTime[None], Protocol): ...


static_assert(is_assignable_to(NaiveDateTime, DateTime[Baz | None]))
```

</details>

(The snippet above uses simplified versions of some protocols found in the [DateType library](https://github.com/glyph/DateType/blob/trunk/src/datetype/__init__.py). If you comment out the final line, where we're asked to determine whether one protocol is a subtype of the other, we check the code almost instantly on this branch.)

Since `DateType` actually appears in the MRO of `NaiveDateTime` here, as an optimization we might be able to do a quick MRO check (the same as we would do for nominal instance types) before doing the full structural check between the two protocol instance types. IIRC mypy does a similar optimization.

I'll look into adding something similar to the above snippet as a regression benchmark.

---

_Comment by @codspeed-hq[bot] on 2025-07-05 12:51_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Instrumentation Performance Report](https://codspeed.io/astral-sh/ruff/branches/alex%2Fmethod-members?runnerMode=Instrumentation)

### Merging #18659 will **degrade performances by 20.45%**

<sub>Comparing <code>alex/method-members</code> (f81ba79) with <code>main</code> (e0b7f49)</sub>



### Summary

`❌ 1 (👁 1)` regressions  
`✅ 39` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `` DateType `` | 211.1 ms | 265.3 ms | -20.45% |


---

_Comment by @AlexWaygood on 2025-07-05 13:03_

> Merging #18659 will **degrade performances by 18.48%**

Which is nonetheless much better than the previous commit on this PR branch, on which this benchmark timed out 😆

I'm looking to see if there are further optimisations we could apply that are reasonable and effective

---

_Comment by @AlexWaygood on 2025-07-05 14:24_

> I'm looking to see if there are further optimisations we could apply that are reasonable and effective

None of my other ideas made any significant difference to the runtime of that benchmark. I think ultimately we're just doing a lot more (necessary!) work on this branch. So I think this is once again ready for review.

Things I tried:
- Adding `#[salsa::tracked]` to `Type::normalized()` and `Type::has_relation_to()`
- Using the unsafe [`insert_unique_unchecked`](https://docs.rs/hashbrown/latest/hashbrown/struct.HashMap.html#method.insert_unique_unchecked) method to insert values into the `cache` field of the `CycleDetector` struct.
- Checking whether a nominal instance or a protocol instance `T` was an explicit subclass of the class of a class-based protocol instance `P` before doing a full structural check in `Type::has_relation_to` and `Type::is_disjoint_from()`

---

_Marked ready for review by @AlexWaygood on 2025-07-05 14:24_

---

_@AlexWaygood reviewed on 2025-07-05 14:28_

---

_Review comment by @AlexWaygood on `crates/ty_python_semantic/src/types/cyclic.rs`:1 on 2025-07-05 14:28_

I observed that the reason we never finished when trying to type-check `DateType` on earlier versions of this PR was that we were repeatedly calling `Type::normalized_impl` on the same types over and over again. Caching the results within any one call to `Type::normalized()` fixes this.

---

_@sharkdp approved on 2025-07-07 10:35_

Thank you!

I'm not 100% sure I understand why this new `cache` in `CycleDetector` is necessary in addition to `seen`. And if it is necessary, if `seen` and `cache` couldn't be merged into a single `HashMap`?

---

_Comment by @AlexWaygood on 2025-07-07 10:53_

> I'm not 100% sure I understand why this new `cache` in `CycleDetector` is necessary in addition to `seen`. And if it is necessary, if `seen` and `cache` couldn't be merged into a single `HashMap`?

If the type we're trying to normalize is present in `seen`, it indicates that we've hit a cycle (due to a recursive type); we need to immediately short circuit the whole normalization and return `Any`. That's why we `pop` items off the end of `seen` after we've normalized them.

But if the type we're trying to normalize is present in `cache`, it doesn't necessarily mean we've hit a cycle: it just means that we've already normalized this inner type as part of a bigger `Type::normalized()` call chain we're currently in. Since this cache is just a performance optimisation, it doesn't make sense to pop items off the end of the cache after they've been normalized (it would sort-of defeat the point of a cache if we did!)

I'll add some doc-comments.

---

_Merged by @AlexWaygood on 2025-07-07 11:28_

---

_Closed by @AlexWaygood on 2025-07-07 11:28_

---

_Branch deleted on 2025-07-07 11:28_

---
