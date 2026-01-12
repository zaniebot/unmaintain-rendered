```yaml
number: 1585
title: support try/except imports of typing special forms
type: issue
state: open
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-11-18T18:40:10Z
updated_at: 2025-12-19T01:09:16Z
url: https://github.com/astral-sh/ty/issues/1585
synced_at: 2026-01-12T15:54:25Z
```

# support try/except imports of typing special forms

---

_@carljm_

Some projects (at least graphql-core) import typing special forms like this, rather than using `sys.version_info` checks:

```py
try:
    from typing import TypeGuard
except ImportError:
    from typing_extensions import TypeGuard
```

We handle this fine on Python versions in which `typing.TypeGuard` exists (in that case `typing.TypeGuard` and `typing_extensions.TypeGuard` are the same object, so the union resolves to just that object.) We don't handle it when checking on an older Python where the `typing_extensions` fallback is necessary. In that case we emit a diagnostic on the import from `typing`, of course, and then we end up with a union of `Unknown | typing_extensions.TypeGuard`, which our special-form-handling code does not treat as `TypeGuard`.

Mypy, pyright, and pyrefly don't handle this either.

The one thing we do differently (since https://github.com/astral-sh/ruff/pull/21503) is that we also emit a diagnostic on e.g. `TypeGuard[int]` in an annotation, when the `TypeGuard` symbol is `Unknown | typing_extensions.TypeGuard`. Arguably this is good, since it alerts you to the fact that your typeguard function is not actually doing any narrowing, and gives you a better clue as to why? But we could also silence this.

If we wanted to support this pattern, some options could be:
1. Understand `try... except ImportError` as a statically-known branch, given existence / non-existence of the import. This would also help us in other similar import-fallback cases.
2. Handle the union with Unknown as a special case in our type expression parsing. (This might result in false negatives if the union with Unknown comes from a weirder source that we wouldn't want to support?)

---

_Label `typing semantics` added by @carljm on 2025-11-18 18:40_

---

_Comment by @leon-sony on 2025-12-18 00:30_

I encountered a case of this with library `mypy_boto3_s3` ([pypi](https://pypi.org/project/mypy-boto3-s3/), [github generator code](https://github.com/youtype/mypy_boto3_builder)). It uses a `try / except ImportError` pattern, I think for Python backwards compatibility?

```python
# mypy_boto3_s3/__init__.py
try:
    from .service_resource import S3ServiceResource
except ImportError:
    from builtins import object as S3ServiceResource  # type: ignore[assignment]
```

Unfortunately, ty doesn't handle this as gracefully as mypy. Mypy seems to recognize my `var: mypy_boto3_s3.S3ServiceResource` as an instance of `mypy_boto3_s3.service_resource.S3ServiceResource`, but ty instead widens it to `object`, and then it doesn't type check my code correctly.

I have no suggestion on how to handle this correctly, I am simply reporting that I encountered this issue in the wild.

Here's a basic reproduction case:

```python
# mypy_boto3_s3_test.py
from mypy_boto3_s3 import S3ServiceResource

def f() -> S3ServiceResource:
    """Fake making an S3ServiceResource"""
    return None  # type: ignore

s = f()
s.get_available_subresources()  # ty thinks this is an error

```

```
$ uvx ty version
ty 0.0.2

$ uvx ty check mypy_boto3_s3_test.py 
error[unresolved-attribute]: Object of type `object` has no attribute `get_available_subresources`
 --> mypy_boto3_s3_test.py:9:1
  |
8 | s = f()
9 | s.get_available_subresources()  # ty thinks this is an error
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
```

---

_Comment by @sharkdp on 2025-12-18 09:59_

> ```python
> # mypy_boto3_s3/__init__.py
> try:
>     from .service_resource import S3ServiceResource
> except ImportError:
>     from builtins import object as S3ServiceResource  # type: ignore[assignment]
> ```

I can't find this code anywhere in the linked repository?

---

_Comment by @carljm on 2025-12-19 01:02_

> I can't find this code anywhere in the linked repository?

I think the linked package is a code generator, which generates code like this.

---

_Comment by @carljm on 2025-12-19 01:09_

I created #2096 to discuss/track the issue reported by @leon-sony, since it's not actually the same as what this issue is about. (This issue is specifically about handling special form types from the `typing` module which are imported using a try/except fallback; mypy also doesn't handle this.)

---
