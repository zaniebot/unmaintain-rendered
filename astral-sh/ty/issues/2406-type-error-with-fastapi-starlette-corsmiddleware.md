```yaml
number: 2406
title: Type error with FastAPI/Starlette CORSMiddleWare and add_middleware
type: issue
state: closed
author: norbusan
labels: []
assignees: []
created_at: 2026-01-08T23:24:40Z
updated_at: 2026-01-09T09:41:17Z
url: https://github.com/astral-sh/ty/issues/2406
synced_at: 2026-01-12T15:54:26Z
```

# Type error with FastAPI/Starlette CORSMiddleWare and add_middleware

---

_@norbusan_

### Summary

FastAPI's CORSMiddleware is normally used with add_middleware as in:
```
app.add_middleware(
    middleware_class=CORSMiddleware,
    allow_origins=settings.CORS_ORIGINS,
    allow_credentials=settings.CORS_CREDENTIALS,
    allow_methods=settings.CORS_METHODS,
    allow_headers=settings.CORS_HEADERS,
)
```

Unfortunately, that does not type correctly with ty:
```
error[invalid-argument-type]: Argument to bound method `add_middleware` is incorrect
  --> src/core/main.py:41:5
   |
39 | # Add middlewares, mounts, routers, etc...
40 | app.add_middleware(
41 |     middleware_class=CORSMiddleware,
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ Expected `_MiddlewareFactory[Unknown]`, found `<class 'CORSMiddleware'>`
42 |     allow_origins=settings.CORS_ORIGINS,
43 |     allow_credentials=settings.CORS_CREDENTIALS,
   |
info: Method defined here
   --> .venv/lib/python3.13/site-packages/starlette/applications.py:124:9
    |
122 |         self.router.host(host, app=app, name=name)  # pragma: no cover
123 |
124 |     def add_middleware(
    |         ^^^^^^^^^^^^^^
125 |         self,
126 |         middleware_class: _MiddlewareFactory[P],
    |         --------------------------------------- Parameter declared here
127 |         *args: P.args,
128 |         **kwargs: P.kwargs,
    |
info: rule `invalid-argument-type` is enabled by default
```

According to the related discussion in the fastapi github repo https://github.com/fastapi/fastapi/discussions/10968 the type hints in starlette are correct, see this comment: https://github.com/fastapi/fastapi/discussions/10968#discussioncomment-11004407

### Version

0.0.10

---

_Comment by @AlexWaygood on 2026-01-08 23:33_

Thanks! This looks like the same as #1635?

---

_Closed by @carljm on 2026-01-08 23:36_

---

_Comment by @norbusan on 2026-01-09 09:41_

@AlexWaygood sorry for the noise, searched for CORSMiddleware but nothing showed up. Yes, that is a duplicate.

---
