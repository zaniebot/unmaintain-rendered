```yaml
number: 1292
title: Cannot unwrap generic type from coroutine
type: issue
state: closed
author: janmeier
labels:
  - bug
  - generics
assignees: []
created_at: 2025-10-01T08:25:50Z
updated_at: 2025-10-01T16:18:15Z
url: https://github.com/astral-sh/ty/issues/1292
synced_at: 2026-01-10T02:06:25Z
```

# Cannot unwrap generic type from coroutine

---

_Issue opened by @janmeier on 2025-10-01 08:25_

### Summary

I'm building an SDK, which has both a sync and an async version. Imagine something like this:
```python
class AsyncSDK:
    async def get(self) -> ReturnType:
        return ReturnType()

class SyncSDK:
    def get(self) -> ReturnType:
        return ReturnType()
```

In my tests, I'm calling the SDKs conditionally, in order to test both versions:
```python
result = asyncSdk.get() if useAsync  else syncSdk.get()
``` 

The return type is correctly inferred as `CoroutineType[Any, Any, ReturnType] | ReturnType`

Now I want to unwrap the result, so I've written a small helper, which awaits if its passed a coroutine, and otherwise just returns the result:
```python
async def unwrapResult[T](maybeCoroutine: T | Coroutine[Any, Any, T]) -> T:
    if inspect.iscoroutine(maybeCoroutine):
        return await maybeCoroutine
    return cast(T, maybeCoroutine)
```

I expect the result to be unwrapped to `ReturnType`,  but this does not happen in ty
```python
unwrappedResult = await unwrapResult(result)
```
Running `ty check` gives me 
```
error[type-assertion-failure]: Argument does not have asserted type `ReturnType`
   |
73 |     unwrappedResult = await unwrapResult(result)
74 |     assert_type(unwrappedResult, ReturnType)
   |     ^^^^^^^^^^^^---------------^^^^^^^^^^^^^
   |                 |
   |                 Inferred type of argument is `CoroutineType[Any, Any, ReturnType] | ReturnType`
```

[Ty playground link](https://play.ty.dev/f7f50f56-aba8-483d-91a3-5152daa329da)
[Pyright playground](https://pyright-play.net/?code=GYJw9gtgBALgngBwJYDsDmUkQWEMoCGAzkQKZ4D68CpANAMK5gCuMqdAginLVAMbEYAKCw48mFERp9hQvgBtiRKACVSMZiBQAVRKQBcQqMagIlQuYpJQOROCj4BlACIBpQycJ2HUACalgKDR1AAoyeWAASigAWgA%2BVXVNHT0PTxMQJK1EjS1dGhDIiwUlKEd7Jzc0439A4Jgw0gjo%2BJzk-IMjdONM3JQ2vL1CiyFiCr8AqGYUAHcQAgQ1ImZ5GABtbQBdEIgCOAAjUkZwVnZ9KG0oAB8oY5Y2FFI1rh4bbl4tloTtasxA1CkpBkADokEQ%2BExTo8dntDncoaRIr9PL1koQZgQkPhdgcjpCHqQuj0sv0BEQGtpeDi4fj2EULGMfLUoLtUIVft4nL4ANZQAC8ZQqLlcw08jK5vIFtiFblFJmAuCmZGlPlQUA2IGYdCgADECPIyJtkRlSMtVvyvEKecD6oU-krSCq%2BMYmmQoJzHNbbfTusZpnMFjRfEsVvgBQQMViprN5otTaGQpkzTAir6vGRKNRSCF-bGgyHVrw1H0OvSgA) (where the type is correctly narrowed): 

### Version

a42271626

---

_Comment by @sharkdp on 2025-10-01 08:49_

Thank you for reporting this.

@AlexWaygood I think this might be related to your current changes to `TypeIs` narrowing? (at least the first diagnostic in line 18).

The second diagnostic looks like our generics solver does not specialize `T` to `ReturnType` correctly, bur rather to `CoroutineType[Any, Any, ReturnType] | ReturnType`, which is incorrect. This should hopefully be solved by #623.

---

_Label `generics` added by @sharkdp on 2025-10-01 08:51_

---

_Label `bug` added by @sharkdp on 2025-10-01 08:55_

---

_Comment by @AlexWaygood on 2025-10-01 10:42_

> [@AlexWaygood](https://github.com/AlexWaygood) I think this might be related to your current changes to `TypeIs` narrowing? (at least the first diagnostic in line 18).

Yes -- here's what's happening here:

- `iscoroutine` is [annotated by typeshed](https://github.com/python/typeshed/blob/6547ec10b881acac28ed000a7c2108f8de1c6a1e/stdlib/inspect.pyi#L235) as returning `TypeIs[Coroutine[Any, Any, Any]]`
- This is interpreted by ty as indicating that we should narrow the type by intersecting with `Top[Coroutine[Any, Any, Any]]` if `iscoroutine` returns `True`. `Top[Coroutine[Any, Any, Any]]` is `Coroutine[object, Never, object]` since `Coroutine` is covariant in its first parameter, contravariant in its second, and covariant in its third.
- In the if-True branch under the `iscoroutine` guard, we therefore narrow the type of `maybeCoroutine` to `(T & Coroutine[object, Never, object]) | Coroutine[Any, Any, T]`. Calling `await` on this union leads to `object | T`, which simplifies to `object`.

This is [consistent with our narrowing behaviour](https://play.ty.dev/2beebf87-7451-4fd9-9e9d-8c403afb8a4f) if you do `isinstance(maybeCoroutine, Coroutine)` and makes theoretical sense: just because an object is of type `T` doesn't mean that it's _not_ a coroutine -- but if it _is_ a coroutine, you don't necessarily know that that coroutine returns `T` when you `await` it, so we can't verify that the `return await maybeCoroutine` call actually returns an object inhabiting `T`.

I do agree that this behaviour probably seems needlessly pedantic to users, but I think if we want to fix this we should try to make sure that our behaviour remains consistent between what we do for `isinstance()` narrowing and what we do for `TypeIs` narrowing. It's even more confusing if we're inconsistent about this kind of thing.

---

_Comment by @janmeier on 2025-10-01 11:02_

Thanks for the quick responses both of you, and for the detailed explanation @AlexWaygood. Much appreciated!

I do see how the implementation of `unwrapResult` cannot narrow the type of `maybeCoroutine` correctly. For the sake of example, if we constrain `T` to the a subtype of `ReturnType`, the implementation typechecks, since ty now knows that the `T` generic cannot itself be a coroutine:
```python
async def unwrapResult[T: ReturnType](maybeCoroutine: T | Coroutine[Any, Any, T]) -> T:
    if inspect.iscoroutine(maybeCoroutine):
        return await maybeCoroutine
    return cast(T, maybeCoroutine)
```

However, that still leaves the issue of not narrowing the type enough when using the `unwrapResult` function. But I see you already linked an issue for that, thanks @sharkdp!

---

_Comment by @AlexWaygood on 2025-10-01 16:18_

The first diagnostic is expected behaviour -- perhaps confusing behaviour, but this is already an issue we're aware of, and everything _is_ working as intended here.

The second diagnostic, as @sharkdp says, is due to our current constraint solver being too naive.

So all things considered, I'll close this as a duplicate of #623

But thanks for the report!

---

_Closed by @AlexWaygood on 2025-10-01 16:18_

---
