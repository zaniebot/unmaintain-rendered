```yaml
number: 1188
title: "Type `HttpRequest` has no attribute `user`"
type: issue
state: closed
author: lypwig
labels:
  - question
assignees: []
created_at: 2025-09-15T13:20:42Z
updated_at: 2025-09-15T13:36:34Z
url: https://github.com/astral-sh/ty/issues/1188
synced_at: 2026-01-12T15:54:24Z
```

# Type `HttpRequest` has no attribute `user`

---

_@lypwig_

### Summary

This code:

```py
from django.http import HttpRequest

def greetings(self, request: HttpRequest):
    print("Greetings", request.user)
```

Return the error `Type "HttpRequest" has no attribute "user"`, which is a false positive. A Django HttpRequest object might have [a `user` attribute](https://docs.djangoproject.com/en/5.2/ref/request-response/#django.http.HttpRequest.user) if the [AuthenticationMiddleware](https://docs.djangoproject.com/en/5.2/ref/middleware/#django.contrib.auth.middleware.AuthenticationMiddleware) middleware is enabled.

### Version

ty 0.0.1-alpha.20

---

_Comment by @sharkdp on 2025-09-15 13:29_

Do you have `django-stubs` set up?

https://github.com/typeddjango/django-stubs/blob/2366e47149d386ac1e282fd17798c094a3d8be38/django-stubs/http/request.pyi#L56

It works fine for me with the stubs installed:

```
▶ cat pyproject.toml main.py
───────┬───────────────────────────────────────────────────────────────────────────
       │ File: pyproject.toml
───────┼───────────────────────────────────────────────────────────────────────────
   1   │ [project]
   2   │ name = "issue-1188"
   3   │ version = "0.1.0"
   4   │ requires-python = ">=3.13"
   5   │ dependencies = [
   6   │     "django>=5.2.6",
   7   │     "django-stubs>=5.2.5",
   8   │ ]
───────┴───────────────────────────────────────────────────────────────────────────
───────┬───────────────────────────────────────────────────────────────────────────
       │ File: main.py
───────┼───────────────────────────────────────────────────────────────────────────
   1   │ from django.http import HttpRequest
   2   │ 
   3   │ def greetings(self, request: HttpRequest):
   4   │     print("Greetings", request.user)
───────┴───────────────────────────────────────────────────────────────────────────

▶ uv run ty check           
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing
features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files
All checks passed!
```

---

_Label `question` added by @sharkdp on 2025-09-15 13:29_

---

_Comment by @lypwig on 2025-09-15 13:36_

Thank you, it works with django-stubs. I didn't know I had to install it.

---

_Closed by @lypwig on 2025-09-15 13:36_

---
