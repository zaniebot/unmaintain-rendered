```yaml
number: 19711
title: "[ty] Add diagnostics for invalid `await` expressions"
type: pull_request
state: merged
author: theammir
labels:
  - ty
assignees: []
merged: true
base: main
head: feat/await-diagnostics
created_at: 2025-08-03T13:18:40Z
updated_at: 2025-08-14T21:38:33Z
url: https://github.com/astral-sh/ruff/pull/19711
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Add diagnostics for invalid `await` expressions

---

_Pull request opened by @theammir on 2025-08-03 13:18_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR adds a new lint, `invalid-await`, for all sorts of reasons why an object may not be `await`able, as discussed in astral-sh/ty#919.
Precisely, `__await__` is guarded against being missing, possibly unbound, or improperly defined (expects additional arguments or doesn't return an iterator).

Of course, diagnostics need to be fine-tuned.  If `__await__` cannot be called with no extra arguments, it indicates an error (or a quirk?) in the method signature, not at the call site. Without any doubt, such an object is not `Awaitable`, but I feel like talking about arguments for an *implicit* call is a bit leaky.
I didn't reference any actual diagnostic messages in the lint definition, because I want to hear feedback first.

Also, there's no mention of the actual required method signature for `__await__` anywhere in the docs. The only reference I had is the `typing` stub. I basically ended up linking `[Awaitable]` to ["must implement `__await__`"](https://docs.python.org/3/library/collections.abc.html#collections.abc.Awaitable), which is insufficient on its own.

## Test Plan

<!-- How was it tested? -->
The following code was tested:
```python
import asyncio
import typing


class Awaitable:
    def __await__(self) -> typing.Generator[typing.Any, None, int]:
        yield None
        return 5


class NoDunderMethod:
    pass


class InvalidAwaitArgs:
    def __await__(self, value: int) -> int:
        return value


class InvalidAwaitReturn:
    def __await__(self) -> int:
        return 5


class InvalidAwaitReturnImplicit:
    def __await__(self):
        pass


async def main() -> None:
    result = await Awaitable()  # valid
    result = await NoDunderMethod()  # `__await__` is missing
    result = await InvalidAwaitReturn()  # `__await__` returns `int`, which is not a valid iterator 
    result = await InvalidAwaitArgs()  # `__await__` expects additional arguments and cannot be called implicitly
    result = await InvalidAwaitReturnImplicit()  # `__await__` returns `Unknown`, which is not a valid iterator


asyncio.run(main())
```

---

_Review requested from @carljm by @theammir on 2025-08-03 13:18_

---

_Review requested from @AlexWaygood by @theammir on 2025-08-03 13:18_

---

_Review requested from @sharkdp by @theammir on 2025-08-03 13:18_

---

_Review requested from @dcreager by @theammir on 2025-08-03 13:18_

---

_Label `ty` added by @MichaReiser on 2025-08-03 13:26_

---

_Comment by @github-actions[bot] on 2025-08-03 13:28_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
<details>
<summary>Changes were detected when running ty on typing conformance tests</summary>

```diff
--- old-output.txt	2025-08-14 21:31:01.838153846 +0000
+++ new-output.txt	2025-08-14 21:31:01.904154062 +0000
@@ -1,5 +1,5 @@
 WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
-fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(1543f)): execute: too many cycle iterations`
+fatal[panic] Panicked at /home/runner/.cargo/git/checkouts/salsa-e6f3bb7c2a062968/918d35d/src/function/execute.rs:215:25 when checking `/home/runner/work/ruff/ruff/typing/conformance/tests/aliases_typealiastype.py`: `infer_definition_types(Id(6d14)): execute: too many cycle iterations`
 _directives_deprecated_library.py:15:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
 _directives_deprecated_library.py:30:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
 _directives_deprecated_library.py:36:41: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `Self@__add__`
```
</details>


---

_Comment by @github-actions[bot] on 2025-08-03 13:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
websockets (https://github.com/aaugustin/websockets)
- src/websockets/legacy/server.py:607:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `Unknown | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[Unknown]`
+ src/websockets/legacy/server.py:607:34: error[invalid-argument-type] Argument to bound method `__init__` is incorrect: Expected `bytes`, found `@Todo(Type::Intersection.call()) | Literal[HTTPStatus.SERVICE_UNAVAILABLE, b"Server is shutting down.\n"] | list[Unknown]`

aiohttp (https://github.com/aio-libs/aiohttp)
+ aiohttp/client.py:771:33: warning[possibly-unbound-attribute] Attribute `get` on type `@Todo(Inference of subscript on special form) | under_cached_property[Unknown]` is possibly unbound
+ aiohttp/client.py:771:68: warning[possibly-unbound-attribute] Attribute `get` on type `@Todo(Inference of subscript on special form) | under_cached_property[Unknown]` is possibly unbound
- Found 207 diagnostics
+ Found 209 diagnostics

discord.py (https://github.com/Rapptz/discord.py)
+ discord/utils.py:727:26: error[invalid-await] `T@async_all | Awaitable[T@async_all]` is not awaitable
+ discord/webhook/async_.py:194:37: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
+ discord/webhook/async_.py:208:36: warning[possibly-unbound-attribute] Attribute `get` on type `Unknown | under_cached_property[Unknown]` is possibly unbound
- Found 549 diagnostics
+ Found 552 diagnostics

prefect (https://github.com/PrefectHQ/prefect)
+ src/integrations/prefect-sqlalchemy/prefect_sqlalchemy/database.py:333:23: error[invalid-await] `None | CoroutineType[Any, Any, None]` is not awaitable
+ src/prefect/_internal/concurrency/services.py:471:11: error[invalid-await] `(tuple[set[Future[bool]], set[Future[bool]]] & ~tuple[Unknown, ...]) | (Coroutine[Any, Any, tuple[set[Future[bool]], set[Future[bool]]] | None] & ~tuple[Unknown, ...])` is not awaitable
- Found 2969 diagnostics
+ Found 2971 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/contrib/internal/asyncio/patch.py:61:22: error[invalid-await] `Any | None` is not awaitable
- Found 6476 diagnostics
+ Found 6477 diagnostics

```
</details>
No memory usage changes detected ✅


---

_Comment by @theammir on 2025-08-03 13:46_

Oops, I think `Unknown` is not a known `Generator` where return type is not explicitly specified.
Do we treat `Unknown` like `Any`?

---

_Comment by @sharkdp on 2025-08-04 07:36_

> Oops, I think `Unknown` is not a known `Generator` where return type is not explicitly specified.
> Do we treat `Unknown` like `Any`?

Yes, exactly. All dynamic types (`Any`, `Unknown`, `@Todo` types) represented by the `Type::Dynamic(…)` variant in Rust should be treated as being awaitable. Accessing attributes or calling dunder methods on these type should return the same dynamic type. Looks like this is a pre-existing issue in `generator_return_type` that should hopefully be easy to fix, but let me know if you need help with that.

---

_Comment by @theammir on 2025-08-04 11:03_

Do I just...
```diff
     fn generator_return_type(self, db: &'db dyn Db) -> Option<Type<'db>> {
         // -- snip --
         match self {
             Type::NominalInstance(instance) => {
                 instance.class.iter_mro(db).find_map(from_class_base)
             }
             Type::ProtocolInstance(instance) => {
                 if let Protocol::FromClass(class) = instance.inner {
                     class.iter_mro(db).find_map(from_class_base)
                 } else {
                     None
                 }
             }
+            ty @ Type::Dynamic(_) => Some(ty),
             _ => None,
         }
     }
```

It seems to work, but is there more to it?
UPD: now that I think about it, returning `Type::unknown()` makes way more sense than propagating a dynamic type. Maybe we can get something more specific?

---

_Comment by @sharkdp on 2025-08-04 12:05_

> It seems to work, but is there more to it?

That should be all there is to it :smile:. Propagating the dynamic type (instead of returning `Unknown`) is fine. It's something that we also do elsewhere. The reason is that we want to "bubble up" dynamic types like our `Todo` types to the outermost layer such that they are user-visible.

---

_Comment by @theammir on 2025-08-04 16:10_

Alright, now
```python
class InvalidAwaitReturnImplicit:
    def __await__(self):
        pass
```
is awaitable as far as the type signature goes.

Also, union types don't work. I suppose I have to check every element for being a generator, and construct a union of possible return types if all of them are, in fact, awaitable.

---

_Comment by @sharkdp on 2025-08-04 18:01_

> Alright, now
> 
> ```python
> class InvalidAwaitReturnImplicit:
>     def __await__(self):
>         pass
> ```
> 
> is awaitable as far as the type signature goes.

:+1: 

> Also, union types don't work. I suppose I have to check every element for being a generator, and construct a union of possible return types if all of them are, in fact, awaitable.

Ah, yes, you're right. Thank you for fixing these. I think you should be able to use `UnionType::map` to recursively apply `generator_return_type` to all elements of the union.

---

_Assigned to @sharkdp by @sharkdp on 2025-08-04 19:11_

---

_Comment by @theammir on 2025-08-04 20:21_

Now distinguishes between
```python
class AwaitableUnion:  # Generator | Unknown, awaitable
    if datetime.today().weekday() == 6:

        def __await__(self) -> typing.Generator[typing.Any, None, None]:
            yield

    else:

        def __await__(self):
            pass


class UnawaitableUnion:  # Generator | int, non-awaitable
    if datetime.today().weekday() == 6:

        def __await__(self) -> typing.Generator[typing.Any, None, None]:
            yield

    else:

        def __await__(self) -> int:
            return 5
```

`mypy_primer` diff looks very promising to me.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-08-04 20:36_

---

_Comment by @sharkdp on 2025-08-04 21:23_

> `mypy_primer` diff looks very promising to me.

`Never` should be treated exactly the same like dynamic types. Accessing any argument on `Never` results in `Never`. So while it may seem strange, `Never` should be awaitable. This allows us to silence downstream errors if an expression is inferred as `Never`.

This can happen in unreachable code, for example:
```py
a = returns_awaitable()

if sys.version_info >= (3, 12):
    # the type of `a` is `Never` here when checking on 3.11 or lower
    x = await a
else:
    ...
```
Now this is obviously a constructed example, but there are valid use cases.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6844 on 2025-08-05 03:07_

I think it's reasonable to emit a diagnostic at the callsite here. It might also be nice to emit a diagnostic where `__await__` is defined with an incorrect signature, but I would see this as in addition, not instead of.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types.rs`:6858 on 2025-08-05 03:10_

Is it possible that in these cases we can highlight the range of the `__await__` method, or of the definition of the type that is lacking an `__await__` method?

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:573 on 2025-08-05 03:10_

why are there TODOs here?

---

_@carljm reviewed on 2025-08-05 03:12_

This is looking pretty good, thank you! Can we add some mdtests (maybe in a new `diagnostics/invalid_await.md`) demonstrating cases where this diagnostic is emitted, and snapshotting their output?

---

_@theammir reviewed on 2025-08-05 08:20_

---

_Review comment by @theammir on `crates/ty_python_semantic/src/types/diagnostic.rs`:573 on 2025-08-05 08:20_

Wanted to hear feedback before documenting actual error messages, I think.

---

_Review comment by @carljm on `crates/ty_python_semantic/src/types/diagnostic.rs`:573 on 2025-08-05 14:45_

I think it's sufficient to include the error code, but not the full message, here. So e.g. 
```suggestion
    ///     await InvalidAwait()  # error: [invalid-await]
    ///     await 42  # error: [invalid-await]
```

---

_@carljm reviewed on 2025-08-05 14:45_

---

_Comment by @theammir on 2025-08-08 21:09_

Added some useful secondary annotations for the lint. Now `report_diagnostic` is a huge match-case, but from what I've seen, a lot of `ty` is, too.

Added mdtests as proposed. Some other tests that rely on `await` expressions have been broken, for instance:
https://github.com/astral-sh/ruff/blob/3a542a80f65d66a15e22b6fe161e5af05ddffa83/crates/ty_python_semantic/resources/mdtest/diagnostics/semantic_syntax_errors.md?plain=1#L136-L138
Would expecting `error: [invalid-await]` here as well break atomicity?

Even though providing the type definition of a non-awaitable object is useful, sometimes diagnostics peek into built-ins:
```
error[invalid-await]: `Literal[1]` is not awaitable
   --> test-project/main.py:83:11
    |
 81 |     await Awaitable()  # valid
 82 |     await AwaitableUnion()  # valid
 83 |     await 1  # invalid
    |           ^
 84 |
 85 |     await NonCallableAwait()  # invalid
    |
   ::: stdlib/builtins.pyi:337:7
    |
335 | _LiteralInteger = _PositiveInteger | _NegativeInteger | Literal[0]  # noqa: Y026  # TODO: Use TypeAlias once mypy bugs are fixed
336 |
337 | class int:
    |       --- type defined here
338 |     """int([x]) -> integer
339 |     int(x, base=10) -> integer
    |
info: `__await__` is missing
info: rule `invalid-await` is enabled by default
```
---
Also, while fooling around, I discovered that awaiting an instance of this type is perfectly valid, and it isn't considered possibly unbound:
```py
class PossiblyUnbound:
    if datetime.today().weekday() == 0:

        def __await__(self):
            yield 1

    elif datetime.today().weekday() == 1:

        def __await__(self):
            yield 2
```
I wonder why.

---

_Comment by @carljm on 2025-08-13 13:45_

> Also, while fooling around, I discovered that awaiting an instance of this type is perfectly valid, and it isn't considered possibly unbound

Thanks for this comment! I explored it and found a bug, fixed in https://github.com/astral-sh/ruff/pull/19884

---

_Comment by @theammir on 2025-08-13 16:42_

> Thanks for this comment! I explored it and found a bug, fixed in #19884

Excellent! And thanks for fixing tests for me, I suppose I should merge them from `main`.

---

_Comment by @theammir on 2025-08-13 17:30_

All mdtests are now passing.

---

_Comment by @carljm on 2025-08-14 20:52_

I notice that mypy and pyright both implement these diagnostics by checking that anything you `await` is assignable to the `Awaitable` protocol, and then just emitting the usual diagnostics if that assignment fails. (Rather than trying to call `__await__` and erroring if we can't make that call, or it returns the wrong type.) 

It's not clear to me that using the protocol is a better approach to get good diagnostics here, but it's something we could consider in future. For now I think this PR is good.

---

_Comment by @carljm on 2025-08-14 21:05_

@theammir Just for future reference (in case you make future PRs), you may want to note some things I'm fixing in this PR:

* `cargo dev generate-all` (because this PR adds a new diagnostic and we need to generate some rules metadata)
* Adding `<!-- snapshot-diagnostics -->` to the mdtest so we get full diagnostic snapshots, then running `cargo insta test -p ty_python_semantic` followed by `cargo insta review` to approve those snapshots.
* Allowing `pre-commit` to auto-format the mdtests.
* Avoiding let-chains for now, because our minimum supported Rust version (tested in CI) is still 1.86. (Our policy is latest-2, and latest is 1.89, so we can bump that to 1.87, but that won't help, since let-chains were only stabilized in 1.88.)

---

_Review requested from @MichaReiser by @carljm on 2025-08-14 21:05_

---

_Comment by @theammir on 2025-08-14 21:18_

> It's not clear to me that using the protocol is a better approach to get good diagnostics here, but it's something we could consider in future. For now I think this PR is good.

My entire reasoning behind the design I went with was trying to build from the blocks available, and that one todo comment in `__enter__`-related functionality that says that we should check for assignability once we support protocols in the first place.

That said, pyright's "is not awaitable" diagnostics are thorough, but also deeply nested and somewhat convoluted:
```python
Pyright: "InvalidAwaitArgs" is not awaitable
     "InvalidAwaitArgs" is incompatible with protocol "Awaitable[_T_co@Awaitable]"
       "__await__" is an incompatible type
         Type "(value: int) -> Generator[int, Any, None]" is not assignable to type "() -> Generator[Any, Any, _T_co@Awaitable]"
           Extra parameter "value" [reportGeneralTypeIssues]
```
All the info about specific extra parameters is there (if you are careful enough to find it on line 4, otherwise read even further), and there's more to the protocol error message than just reporting unassignability anyway.
Just thoughts.

> Just for future reference (in case you make future PRs), you may want to note some things I'm fixing in this PR:

Thanks! It's been really fun so far, I'd stick around. :)

---

_Merged by @carljm on 2025-08-14 21:38_

---

_Closed by @carljm on 2025-08-14 21:38_

---
