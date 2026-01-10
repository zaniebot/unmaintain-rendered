```yaml
number: 1709
title: imports from non-terminating modules silently resolved as Never
type: issue
state: open
author: zilto
labels:
  - unreachable-code
assignees: []
created_at: 2025-12-01T15:29:14Z
updated_at: 2025-12-05T16:10:17Z
url: https://github.com/astral-sh/ty/issues/1709
synced_at: 2026-01-10T01:56:40Z
```

# imports from non-terminating modules silently resolved as Never

---

_Issue opened by @zilto on 2025-12-01 15:29_

### Summary

I'm getting new type errors when upgrading from `0.0.1a21` to `0.0.1a22`. Some light code change on my end make the type-checker pass, but I'm unsure if my code or `ty` is incorrect.

## Setup
I'm using `ty` across two libraries, so I'm trying to have a reduced example, but I didn't try the following repro

### From the main library (can't edit this code)
```python
# library/destinations/implementations/foo/factory.py
class Destination(abc.ABC, Generic[Config, Client]): ...

class foo(Destination[FooConfig, FooClient]): ...
``` 

```python
# library/destinations/__init__.py
from library.destinations.impl.foo.factory import foo

__all__ = ["foo"]
```
### From the downstream library (can edit this code)
In `0.0.1a21`, the following code passes all checks (`ty check`)
```python
import library

def create_destination() -> library.destinations.foo:
   return library.destinations.foo(...)
```
In `0.0.1a22`, the code fails with
```shell
def create_destination() -> Generator[library.destinations.foo, None, None]:
                                                                    ^^^^^^^^^^^^^^^^^^^
info: rule `invalid-type-form` is enabled by default
```

This fixes it
```python
import library
from library.destinations import foo

def create_destination() -> foo:
   return library.destinations.foo(...)
```


### Version

0.0.1a22

---

_Label `needs-info` added by @sharkdp on 2025-12-01 18:00_

---

_Comment by @sharkdp on 2025-12-01 18:01_

Thank you for reporting this.

Before we dig further into this, two questions:
* 0.0.1a22 is a couple of releases behind the latest version. Can you still reproduce this with the latest version?
* The error message that you get on 0.0.1a22 looks like it points to code that is different from what you have in your snippet?

---

_Comment by @zilto on 2025-12-01 20:03_

Hi! 

>0.0.1a22 is a couple of releases behind the latest version. Can you still reproduce this with the latest version?

Yes, the same behavior is found in `0.0.1a29`. I was upgrading from `0.0.1a19` when I found the behavior change. I downgraded versions until I found the change happening from `0.0.1a21` and `0.0.1a22`

 >The error message that you get on 0.0.1a22 looks like it points to code that is different from what you have in your snippet?

I tried to trim the initial error message. This is what I get for `0.0.1a22`

```shell
error[invalid-type-form]: Variable of type `Never` is not allowed in a type expression
  --> tests/conftest.py:18:16
   |
16 | def tmp_duckdb_destination(
17 |     tmp_path: pathlib.Path,
18 | ) -> Generator[dlt.destinations.duckdb, None, None]:
   |                ^^^^^^^^^^^^^^^^^^^^^^^
   |
info: rule `invalid-type-form` is enabled by default
```

This is what I get for `0.0.1a29`
```shell
error[invalid-type-form]: Variable of type `Never` is not allowed in a type expression
  --> tests/conftest.py:18:16
   |
16 | def tmp_duckdb_destination(
17 |     tmp_path: pathlib.Path,
18 | ) -> Generator[dlt.destinations.duckdb, None, None]:
   |                ^^^^^^^^^^^^^^^^^^^^^^^
   |
help: The variable may have been inferred as `Never` because its definition was inferred as being unreachable
info: rule `invalid-type-form` is enabled by default
```

---

_Comment by @sharkdp on 2025-12-01 20:16_

> I tried to trim the initial error message.

Ah, I see. Now have a completely different error message. Did you see the "help" text in the diagnostic that you get on 0.0.1a29?

> The variable may have been inferred as `Never` because its definition was inferred as being unreachable

Can you check if `dlt.destinations.duckdb` is maybe guarded by a `sys.version_info >= …` check or a `sys.platform` check? Or if there could be some other reason why ty thinks that this symbols is unreachable?

---

_Comment by @zilto on 2025-12-04 14:59_

>Can you check if dlt.destinations.duckdb is maybe guarded by a sys.version_info >= … check or a sys.platform check?

Searching the codebase, `sys.platform` is used twice and `sys.version_info` only once. All of these instances are within the closure of functions, suggesting no impact on the current issue.

>Or if there could be some other reason why ty thinks that this symbols is unreachable?
My guess would be that there's some name shadowing in the codebase and `ty` name resolution diverges from the Pyhton name resolution. See step 4. and 5.

The import for `dlt.destinations.duckdb` is as follow:

1. `repro.py` (type-checked user code)
    ```python
    import dlt

    def foo() -> dlt.destinations.duckdb:
      return dlt.destinations.duckdb(...)
    ```

2. `dlt/__init__.py`
     ```python 
     from dlt import destinations
  
     __all__ = ("destinations", ...)
     ```

3. `dlt/destinations/__init__.py`
     ```python
     from dlt.destinations.impl.duckdb.factory import duckdb

     __all__ = ("duckdb", ...)
     ```

4. `dlt/destinations/impl/__init__.py`
     This file is empty. There is no import or public namespace

5. `dlt/destinations/impl/duckdb/__init__.py`
     This file is empty. There is no import or public namespace

6. `dlt/destinations/impl/duckdb/factory.py`
     ```python
     # actual class implementation
     class duckdb: ...
     ```

It would be useful to have a traceback until the type resolution hits `Never` in addition to the message
```shell
error[invalid-type-form]: Variable of type `Never` is not allowed in a type expression
```


---

_Comment by @sharkdp on 2025-12-04 16:00_

Ok, this is interesting. The problematic code is here:

https://github.com/dlt-hub/dlt/blob/e8d45369f1607d36bee5f002a661458f1fabe733/dlt/__init__.py#L92-L94

This assertion checks that `_Container._INSTANCE is None`. However, that attribute is clearly declared as having type `Container` here:

https://github.com/dlt-hub/dlt/blob/e8d45369f1607d36bee5f002a661458f1fabe733/dlt/common/configuration/container.py#L32

This is why `ty` infers that this condition is always false. And it considers all code that comes after it as unreachable.

Here is where ty does something that is probably unexpected: When we load symbols from this module, we consider their value at the end of the scope. Since the end of the scope is not reachable, we infer the type of `dlt.destinations` as `Never`.

You could fix this by guarding it as `if not TYPE_CHECKING`, or by giving `_Container._INSTANCE` a different type (e.g. `Container | None`).

In ty, we should probably also try to improve the situation somehow. I saw a similar problem recently where a  module ended in a `while True` loop...

---

_Label `needs-info` removed by @AlexWaygood on 2025-12-04 16:09_

---

_Comment by @carljm on 2025-12-04 16:57_

At the very least, we should probably error directly on the import from such a module, rather than just silently inferring the type as `Never`.

---

_Added to milestone `Stable` by @carljm on 2025-12-04 16:58_

---

_Renamed from "`invalid-type-form` regression from `0.0.1a21 -> 0.0.1a22`" to "imports from non-terminating modules resolved as Never" by @carljm on 2025-12-04 16:59_

---

_Label `unreachable-code` added by @carljm on 2025-12-04 16:59_

---

_Comment by @zilto on 2025-12-05 14:39_

@sharkdp Thank you so much for the investigation. Will see how I can fix the Python code upstream.

Feel free to close this issue when appropriate

---

_Renamed from "imports from non-terminating modules resolved as Never" to "imports from non-terminating modules silently resolved as Never" by @carljm on 2025-12-05 16:10_

---
