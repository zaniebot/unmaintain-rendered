```yaml
number: 309
title: Possibly missing import
type: issue
state: closed
author: ion-elgreco
labels:
  - imports
assignees: []
created_at: 2025-05-10T16:33:11Z
updated_at: 2025-12-18T20:06:36Z
url: https://github.com/astral-sh/ty/issues/309
synced_at: 2026-01-12T15:54:22Z
```

# Possibly missing import

---

_@ion-elgreco_

### Summary

Ty is throwing a possible unbound import, but pyright doesn't do this.

I have a module `package.custom` that does this, at runtime it adds a certain class to a module or not, based on the required dependencies. If you don't have the dependency installed, it will not show it on the main module, when someone tries to import directly from `package.mod.custom` it will throw an import error.

```python
try:
    from package.mod.custom import CustomManager

    __all__.extend(["CustomManager"])
except ImportError as e:
    if "pydantic" in str(e):
        pass
    else:
        raise e
```

In `package.mod.custom` we do this:

```python
try:
    import pydantic
except ImportError as e:
    if "pydantic" in str(e):
        raise ImportError(
            "Install 'package[pydantic]' to use pydantic functionality",
        ) from e
    else:
        raise e
```



### Version

ty 0.0.0-alpha.8 (0474b40e1 2025-05-09)

---

_Label `imports` added by @AlexWaygood on 2025-05-10 17:57_

---

_Comment by @sharkdp on 2025-05-13 10:48_

Thank you for reporting this.

I can't directly try your example because I don't have the full code and information about the project structure, but if I understand correctly, you basically have a setup similar to [this](https://play.ty.dev/d5d44eaa-4897-41cf-95a4-f83d1b185380):

`main.py`:

```py
from pydantic_support import CustomPydanticFeature

# Use the feature:
c = CustomPydanticFeature()
```

`pydantic_support.py`:

```py
try:
    import pydantic

    class CustomPydanticFeature: ...

except ImportError:
    print("Please install pydantic if you want to use this feature")
```

In general, we can not model runtime behavior like whether or not the `pydantic` import leads to an `ImportError` or not. This seems like a case for a [suppression comment](https://github.com/astral-sh/ty/blob/main/docs/README.md#suppressions) for me? You could add a `# ty: ignore[possibly-unbound-import]` comment in the first line there or generally suppress `possibly-unbound-import` diagnostics in your [project's configuration](https://github.com/astral-sh/ty/blob/main/docs/reference/configuration.md#configuration).

We realize that this is a common pattern, however, so we're also happy to take more feedback and opinions here.

---

_Comment by @ion-elgreco on 2025-05-13 10:57_

Wouldn't resolving to pydantic tell you that you that `CustomPydanticFeature` will be available?

The issue is that this is done in external libraries as well, so when I would import things from external libraries, you would get these errors reported from `ty`, for example `dagster-polars`: https://github.com/dagster-io/community-integrations/blob/main/libraries/dagster-polars/dagster_polars/__init__.py

---

_Comment by @sharkdp on 2025-05-13 11:48_

> Wouldn't resolving to pydantic tell you that you that `CustomPydanticFeature` will be available?

For the specific case of a `try: import …; except ImportError: …`, we might theoretically be able to detect that somehow and check whether the import would resolve or not. For a user of such a feature (`main.py`), I can definitely see how that would be a desirable outcome. Instead of "possibly unbound", the `CustomPydanticFeature` would either be definitively bound or definitely unbound.

For the author of a package that includes this `pydantic_support` module, things are maybe a bit more complex? Would they need to run their type checking twice, once with that optional dependency installed, and once without? Or would it be more useful to have the `CustomPydanicFeature` "possibly unbound", the way we treat it right now? That way, even if you don't have `pydantic` installed, you could still get proper type checking of code that makes use of `CustomPydanticFeature`.

---

_Comment by @ion-elgreco on 2025-05-13 14:59_

> > Wouldn't resolving to pydantic tell you that you that `CustomPydanticFeature` will be available?
> 
> For the specific case of a `try: import …; except ImportError: …`, we might theoretically be able to detect that somehow and check whether the import would resolve or not. For a user of such a feature (`main.py`), I can definitely see how that would be a desirable outcome. Instead of "possibly unbound", the `CustomPydanticFeature` would either be definitively bound or definitely unbound.
> 
I agree!

> For the author of a package that includes this `pydantic_support` module, things are maybe a bit more complex? Would they need to run their type checking twice, once with that optional dependency installed, and once without? Or would it be more useful to have the `CustomPydanicFeature` "possibly unbound", the way we treat it right now? That way, even if you don't have `pydantic` installed, you could still get proper type checking of code that makes use of `CustomPydanticFeature`.

Usually you would run tests with and without a dependency installed, it's something we do in `delta-rs` with `pandas` dependency.

---

_Comment by @merlinz01 on 2025-09-22 18:29_

I get this error when using `opensearchpy` module:

```python
# .venv/lib/python3.13/site-packages/opensearchpy/__init__.py

...

try:
    from ._async.client import AsyncOpenSearch
    from ._async.http_aiohttp import AIOHttpConnection, AsyncConnection
    from ._async.transport import AsyncTransport
    from .connection import AsyncHttpConnection
    from .helpers import AWSV4SignerAsyncAuth

    __all__ += [
        "AIOHttpConnection",
        "AsyncConnection",
        "AsyncTransport",
        "AsyncOpenSearch",
        "AsyncHttpConnection",
        "AWSV4SignerAsyncAuth",
    ]
except (ImportError, SyntaxError):
    pass

```

And when I do

```python
from opensearchpy import AsyncOpenSearch
```

ty complains:

```
warning[possibly-unbound-import]: Member `AsyncOpenSearch` of module `opensearchpy` is possibly unbound
  --> backend/settings.py:58:34
58 |         from opensearchpy import AsyncOpenSearch
   |                                  ^^^^^^^^^^^^^^^
info: rule `possibly-unbound-import` is enabled by default
```

---

_Comment by @carljm on 2025-11-14 14:50_

I think that unless we can understand such cases better, we should probably silence `possibly-unbound-import` by default as too noisy with false positives.

---

_Added to milestone `Stable` by @carljm on 2025-11-14 14:50_

---

_Renamed from "Possibly unbound import" to "Possibly missing import" by @carljm on 2025-12-17 23:46_

---

_Closed by @Gankra on 2025-12-18 20:06_

---
