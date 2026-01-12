```yaml
number: 1635
title: False positive on Starlette middleware registration
type: issue
state: open
author: Asaf31214
labels: []
assignees: []
created_at: 2025-11-25T21:12:26Z
updated_at: 2026-01-09T13:43:27Z
url: https://github.com/astral-sh/ty/issues/1635
synced_at: 2026-01-12T15:54:25Z
```

# False positive on Starlette middleware registration

---

_@Asaf31214_

### Summary

Register any FastAPI middleware, custom or built-in:
``` python
app = FastAPI()
app.add_middleware(NoStoreAuthMiddleware) # custom, subclass of BaseHTTPMiddleware
app.add_middleware(RateLimitMiddleware) # custom, subclass of BaseHTTPMiddleware
app.add_middleware( 
    CORSMiddleware, # built-in middleware, from fastapi.middleware.cors
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
) 
```
run:
`ty check`, without any specified rules at `pyproject.toml`

No issue until ty@0.0.1a25 (expected result)
But for versions ty@0.0.1a26 and ty@0.0.1a27 (current latest):
```
error[invalid-argument-type]: Argument to bound method `add_middleware` is incorrect
  --> main.py:31:20
   |
30 | app = FastAPI(default_response_class=ORJSONResponse, lifespan=lifespan)
31 | app.add_middleware(NoStoreAuthMiddleware)
   |                    ^^^^^^^^^^^^^^^^^^^^^ Expected `_MiddlewareFactory[Unknown]`, found `<class 'NoStoreAuthMiddleware'>`
32 | app.add_middleware(RateLimitMiddleware)
33 | app.add_middleware(ExceptionHandlerMiddleware)
   |
info: Method defined here
   --> .venv/lib/python3.14/site-packages/starlette/applications.py:118:9
    |
116 |         self.router.host(host, app=app, name=name)  # pragma: no cover
117 |
118 |     def add_middleware(
    |         ^^^^^^^^^^^^^^
119 |         self,
120 |         middleware_class: _MiddlewareFactory[P],
    |         --------------------------------------- Parameter declared here
121 |         *args: P.args,
122 |         **kwargs: P.kwargs,
    |
info: rule `invalid-argument-type` is enabled by default
```
same error for all 3 middlewares registered here.

this is also mentioned at issue #1564  by @dmytro-GL [here](https://github.com/astral-sh/ty/issues/1564#issuecomment-3548995805)

### Version

ty 0.0.1-alpha.27

---

_Comment by @carljm on 2025-11-25 21:24_

Thanks for the report! It looks like `_MiddlewareFactory` is a protocol which is generic over a `ParamSpec`, so I think we should revisit this once #157 is done.

---

_Added to milestone `Stable` by @carljm on 2025-11-25 21:24_

---

_Removed from milestone `Stable` by @carljm on 2025-11-25 23:55_

---

_Added to milestone `Beta` by @carljm on 2025-11-25 23:55_

---

_Comment by @carljm on 2025-11-25 23:56_

Putting this in beta milestone as I think we should ensure it's either fixed or suppressed in some way for the beta.

---

_Comment by @Asaf31214 on 2025-11-26 00:35_

Update: same behaviour on 0.0.1-alpha.28

---

_Renamed from "FastAPI middleware registration false positive error at versions >=0.0.1a26" to "False positive on Starlette middleware registration" by @dhruvmanila on 2025-12-04 05:16_

---

_Comment by @dhruvmanila on 2025-12-04 05:17_

For context, this is same as https://github.com/astral-sh/ty/issues/157#issuecomment-3554317390 which is a self-contained example to reproduce this. This possibly requires #1714.

---

_Assigned to @dhruvmanila by @dhruvmanila on 2025-12-04 05:17_

---

_Unassigned @dhruvmanila by @carljm on 2025-12-08 17:07_

---

_Assigned to @AlexWaygood by @carljm on 2025-12-08 17:07_

---

_Comment by @carljm on 2025-12-16 21:09_

Didn't get this into the beta, but it's a high priority follow-up.

---

_Removed from milestone `Beta` by @carljm on 2025-12-16 21:09_

---

_Added to milestone `Stable` by @carljm on 2025-12-16 21:09_

---

_Comment by @thejcannon on 2025-12-17 16:00_

(as far as examples goes, it doesn't get much smaller than this)
```
#!/usr/bin/env -S uv run --with starlette==0.50.0,ty==0.0.2 ty check --

from starlette_context.middleware import RawContextMiddleware
from starlette.middleware import Middleware

Middleware(RawContextMiddleware)
```

---

_Unassigned @AlexWaygood by @AlexWaygood on 2025-12-17 16:00_

---

_Removed from milestone `Stable` by @carljm on 2025-12-19 23:38_

---

_Added to milestone `M1` by @carljm on 2025-12-19 23:38_

---

_Comment by @condemil on 2025-12-26 10:41_

Another minimal reproducible example:


```python
from starlette.middleware import Middleware
from starlette.middleware.httpsredirect import HTTPSRedirectMiddleware

# ty(invalid-argument-type): Argument to bound method `__init__` is incorrect: Expected `_MiddlewareFactory[(...)]`, found `<class 'HTTPSRedirectMiddleware'>`
middleware = Middleware(HTTPSRedirectMiddleware)
```

---

_Comment by @Kludex on 2026-01-09 09:16_

When this is fixed, I'm open to add `ty` to the `starlette`s pipeline.

---

_Assigned to @dcreager by @dcreager on 2026-01-09 13:43_

---
