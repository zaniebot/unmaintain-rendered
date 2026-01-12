```yaml
number: 1038
title: ty failed at infer flask_login type LocalProxy
type: issue
state: closed
author: asukaminato0721
labels: []
assignees: []
created_at: 2025-08-18T09:15:04Z
updated_at: 2025-08-19T00:22:48Z
url: https://github.com/astral-sh/ty/issues/1038
synced_at: 2026-01-12T15:54:24Z
```

# ty failed at infer flask_login type LocalProxy

---

_@asukaminato0721_

### Summary

https://github.com/langgenius/dify/blob/ae7de7d36be8f9d3620b18f5ab200c3ca956e2dd/api/controllers/console/app/app.py#L118

```py
error[invalid-argument-type]: Argument to bound method `create_app` is incorrect
   --> controllers/console/app/app.py:118:76
    |
117 |         app_service = AppService()
118 |         app = app_service.create_app(current_user.current_tenant_id, args, current_user)
    |                                                                            ^^^^^^^^^^^^ Expected `Account`, found `Unknown | LocalProxy[Unknown]`
119 |
120 |         return app, 201
    |
info: Function defined here
  --> services/app_service.py:73:9
   |
71 |         return app_models
72 |
73 |     def create_app(self, tenant_id: str, args: dict, account: Account) -> App:
   |         ^^^^^^^^^^                                   ---------------- Parameter declared here
74 |         """
75 |         Create app
   |
info: rule `invalid-argument-type` is enabled by default
```


### Version

ty 0.0.1a18

---

_Comment by @carljm on 2025-08-19 00:22_

Thanks for the report!

It appears to me that `LocalProxy` is the correct type inference for `flask_login.current_user`: https://github.com/maxcountryman/flask-login/blob/main/src/flask_login/utils.py#L25

`LocalProxy` seems like a class that is carefully designed to faithfully proxy another type at runtime, but I don't see any way for a type checker to reasonably understand that an instance of `LocalProxy` should be accepted where an instance of `Account` is expected.

Does this work under another type checker? In my testing, pyright issues the same error we do. Mypy does not, but it just considers `current_user` to be of type `Any` -- I'm not sure why, but maybe because it considers some of these libraries untyped and doesn't follow imports in them.

If having `current_user` treated as type `Any` is a workable solution, you could achieve this by providing your own type stub for `flask_login` module.

Closing this for now because it doesn't appear to be a bug in ty, but will happily reopen if further discussion reveals something we should improve here.

---

_Closed by @carljm on 2025-08-19 00:22_

---
