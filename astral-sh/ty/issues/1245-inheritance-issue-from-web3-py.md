```yaml
number: 1245
title: Inheritance Issue From Web3.py
type: issue
state: closed
author: kirigami-alex
labels: []
assignees: []
created_at: 2025-09-24T13:14:56Z
updated_at: 2025-09-24T14:58:09Z
url: https://github.com/astral-sh/ty/issues/1245
synced_at: 2026-01-12T15:54:24Z
```

# Inheritance Issue From Web3.py

---

_@kirigami-alex_

### Summary

The documented example from Web3.py ([link](https://web3py.readthedocs.io/en/stable/providers.html#web3.providers.rpc.AsyncHTTPProvider)) which looks like this:

```py
from aiohttp import ClientSession
from web3 import AsyncWeb3, AsyncHTTPProvider

async def bug():
    w3 = AsyncWeb3(AsyncHTTPProvider(endpoint_uri))
    custom_session = ClientSession()
    await w3.provider.cache_async_session(custom_session)
    w3.provider.disconnect()
```

Throws this error from Ty:
```
error[unresolved-attribute]: Type `AsyncBaseProvider` has no attribute `cache_async_session`
  --> src/silly_test.py:10:11
   |
 8 |     # If you want to pass in your own session:
 9 |     custom_session = ClientSession()
10 |     await w3.provider.cache_async_session(custom_session) # This method is an async method so it needs to be handled accordingly
   |           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
11 |     # when you're finished, disconnect:
12 |     w3.provider.disconnect()
   |
info: rule `unresolved-attribute` is enabled by default
```

### Version

ty 0.0.1-alpha.21

---

_Renamed from "This Example from Web3.py Throws an Error with Ty" to "Inheritance Issue From Web3.py" by @kirigami-alex on 2025-09-24 13:18_

---

_Comment by @AlexWaygood on 2025-09-24 14:58_

Thanks for the report!

I think this is an issue with web3's type annotations, that you'll need to report to the web3 library. Mypy and pyright both report the same issue (note that I had to add a `-> None` annotation to your function to reproduce the issue with mypy, since mypy by default does not type-check the bodies of functions without type annotations):

```sh
~/dev % cat foo.py                          
from aiohttp import ClientSession
from web3 import AsyncWeb3, AsyncHTTPProvider

async def bug() -> None:
    w3 = AsyncWeb3(AsyncHTTPProvider(endpoint_uri))
    custom_session = ClientSession()
    await w3.provider.cache_async_session(custom_session)
    w3.provider.disconnect()
~/dev % uvx --with=aiohttp --with=web3 mypy foo.py         
foo.py:5: error: Name "endpoint_uri" is not defined  [name-defined]
foo.py:7: error: "AsyncBaseProvider" has no attribute "cache_async_session"  [attr-defined]
foo.py:8: error: Value of type "Coroutine[Any, Any, None]" must be used  [unused-coroutine]
foo.py:8: note: Are you missing an await?
Found 3 errors in 1 file (checked 1 source file)
~/dev [1] % uvx --with=aiohttp --with=web3 pyright foo.py
/Users/alexw/dev/foo.py
  /Users/alexw/dev/foo.py:5:38 - error: "endpoint_uri" is not defined (reportUndefinedVariable)
  /Users/alexw/dev/foo.py:7:23 - error: Cannot access attribute "cache_async_session" for class "AsyncBaseProvider"
    Attribute "cache_async_session" is unknown (reportAttributeAccessIssue)
  /Users/alexw/dev/foo.py:8:5 - error: Result of async function call is not used; use "await" or assign result to variable (reportUnusedCoroutine)
3 errors, 0 warnings, 0 informations
```

If we look at the web3 library, we can see that the `AsyncWeb3.provider` property is annotated as returning an instance of [`AsyncBaseProvider`](https://github.com/ethereum/web3.py/blob/a1c2e83a523c8da07939aeafffd12ba98531d3be/web3/main.py#L488-L490), but `AsyncBaseProvider` doesn't have a `cache_async_session` attribute or method ([source](https://github.com/ethereum/web3.py/blob/a1c2e83a523c8da07939aeafffd12ba98531d3be/web3/providers/async_base.py#L73-L182)). The `AsyncHTTPProvider` subclass of `AsyncBaseProvider` _does_ have [a `cache_async_session` method](https://github.com/ethereum/web3.py/blob/a1c2e83a523c8da07939aeafffd12ba98531d3be/web3/providers/rpc/async_rpc.py#L84-L87), but ty hasn't been given enough information from web3's type annotations to know that `w3.provider` is an instance of `AsyncHTTPProvider` rather than an instance of `AsyncBaseProvider` here. To fix this, the web3 maintainers might consider making the `AsyncWeb3` class generic -- possibly something like this might fix things, but I haven't tested it:

```diff
diff --git a/web3/main.py b/web3/main.py
index b388f95b..35286871 100644
--- a/web3/main.py
+++ b/web3/main.py
@@ -43,6 +43,8 @@ from typing import (
     Type,
     Union,
     cast,
+    Generic,
+    TypeVar,
 )
 
 from eth_typing import (
@@ -447,7 +449,10 @@ class Web3(BaseWeb3):
 # -- async -- #
 
 
-class AsyncWeb3(BaseWeb3):
+T = TypeVar("T", bound=AsyncBaseProvider)
+
+
+class AsyncWeb3(BaseWeb3, Generic[T]):
     # mypy Types
     eth: AsyncEth
     net: AsyncNet
@@ -460,7 +465,7 @@ class AsyncWeb3(BaseWeb3):
 
     def __init__(
         self,
-        provider: Optional[AsyncBaseProvider] = None,
+        provider: Optional[T] = None,
         middleware: Optional[Sequence[Any]] = None,
         modules: Optional[Dict[str, Union[Type[Module], Sequence[Any]]]] = None,
         external_modules: Optional[
@@ -486,11 +491,11 @@ class AsyncWeb3(BaseWeb3):
         return await self.provider.is_connected(show_traceback)
 
     @property
-    def provider(self) -> AsyncBaseProvider:
-        return cast(AsyncBaseProvider, self.manager.provider)
+    def provider(self) -> T:
+        return cast(T, self.manager.provider)
 
     @provider.setter
-    def provider(self, provider: AsyncBaseProvider) -> None:
+    def provider(self, provider: T) -> None:
         self.manager.provider = provider
 
     @property
```

---

_Closed by @AlexWaygood on 2025-09-24 14:58_

---
