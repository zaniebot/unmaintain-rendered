```yaml
number: 2096
title: special-case try/except ImportError if all imports resolve
type: issue
state: open
author: carljm
labels:
  - imports
  - control flow
assignees: []
created_at: 2025-12-19T01:07:45Z
updated_at: 2025-12-31T15:20:15Z
url: https://github.com/astral-sh/ty/issues/2096
synced_at: 2026-01-10T01:56:41Z
```

# special-case try/except ImportError if all imports resolve

---

_Issue opened by @carljm on 2025-12-19 01:07_

> I encountered a case of this with library `mypy_boto3_s3` ([pypi](https://pypi.org/project/mypy-boto3-s3/), [github generator code](https://github.com/youtype/mypy_boto3_builder)). It uses a `try / except ImportError` pattern, I think for Python backwards compatibility?
> 
> ```python
> # mypy_boto3_s3/__init__.py
> try:
>     from .service_resource import S3ServiceResource
> except ImportError:
>     from builtins import object as S3ServiceResource  # type: ignore[assignment]
> ```
> 
> Unfortunately, ty doesn't handle this as gracefully as mypy. Mypy seems to recognize my `var: mypy_boto3_s3.S3ServiceResource` as an instance of `mypy_boto3_s3.service_resource.S3ServiceResource`, but ty instead widens it to `object`, and then it doesn't type check my code correctly.
> 
> I have no suggestion on how to handle this correctly, I am simply reporting that I encountered this issue in the wild.
> 
> Here's a basic reproduction case:
> 
> ```python
> # mypy_boto3_s3_test.py
> from mypy_boto3_s3 import S3ServiceResource
> 
> def f() -> S3ServiceResource:
>     """Fake making an S3ServiceResource"""
>     return None  # type: ignore
> 
> s = f()
> s.get_available_subresources()  # ty thinks this is an error
> 
> ```
> 
> ```
> $ uvx ty version
> ty 0.0.2
> 
> $ uvx ty check mypy_boto3_s3_test.py 
> error[unresolved-attribute]: Object of type `object` has no attribute `get_available_subresources`
>  --> mypy_boto3_s3_test.py:9:1
>   |
> 8 | s = f()
> 9 | s.get_available_subresources()  # ty thinks this is an error
>   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
>   |
> info: rule `unresolved-attribute` is enabled by default
> 
> Found 1 diagnostic
> ``` 

 _Originally posted by @leon-sony in [#1585](https://github.com/astral-sh/ty/issues/1585#issuecomment-3667714012)_

---

_Comment by @carljm on 2025-12-19 01:12_

I think ty is more capable than mypy in this case, not less -- but since this package was designed around mypy's behavior, the results with this package are worse.

Mypy simply doesn't allow the double definition of `S3ServiceResource`, which is why the `type: ignore` on the second definition is required. And in this error case, mypy simply ignores the second definition and prefers the first. This is a bit arbitrary, but it happens to be the mypy behavior, so it's the behavior that the authors of this code generator rely on.

ty supports multiple definitions of the same name, and infers a union type, which is correct: according to the code, at runtime, the name could be either a real `S3ServiceResource`, or it could be the fallback of `object`. This union type simplifies to `object` (because all types are subtypes of `object`), and thus the behavior you see.

The only thing I could really see us doing here is adding some special handling for `try/except` when the `try` block consists entirely of import statements and the `except` is an `except ImportError`, and all the imports resolve successfully under the current module resolution settings. In this case, we could consider the `except` block to be unreachable code.

---

_Label `control flow` added by @carljm on 2025-12-19 01:12_

---

_Label `imports` added by @carljm on 2025-12-19 01:12_

---

_Comment by @carljm on 2025-12-19 01:14_

This issue is sort of just the reincarnation of #309, except this is an example of a case where disabling `possibly-missing-import` rule doesn't help.

---

_Renamed from "mypy_boto_s3 relies on ignoring second import of a name" to "special-case try/except ImportError if all imports resolve" by @carljm on 2025-12-19 01:14_

---

_Comment by @MichaReiser on 2025-12-29 09:05_

Related case where we run into this on the server side https://github.com/astral-sh/ty/issues/2249#issuecomment-3695877740

---

_Comment by @MichaReiser on 2025-12-31 15:20_

I'll move this to stable as it also impacts the LSP

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-31 15:20_

---
