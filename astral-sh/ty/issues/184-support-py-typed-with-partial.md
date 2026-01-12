```yaml
number: 184
title: "support `py.typed` with `partial`"
type: issue
state: closed
author: carljm
labels:
  - typing semantics
assignees: []
created_at: 2025-03-10T23:16:39Z
updated_at: 2025-08-19T18:02:20Z
url: https://github.com/astral-sh/ty/issues/184
synced_at: 2026-01-12T15:54:22Z
```

# support `py.typed` with `partial`

---

_@carljm_

This is used to explicitly mark a package's annotations as partially complete, which may affect how we choose to surface import errors from such a package.

---

_Renamed from "[red-knot] support `py.typed` with `partial`" to "support `py.typed` with `partial`" by @MichaReiser on 2025-05-07 15:26_

---

_Label `typing semantics` added by @AlexWaygood on 2025-05-11 07:52_

---

_Comment by @Spindel on 2025-08-11 16:43_

The absence of this will cause `error[unresolved-import]`  in cases where partial types are present.  

---

_Assigned to @Gankra by @Gankra on 2025-08-14 15:02_

---

_Closed by @Gankra on 2025-08-18 17:14_

---

_Comment by @galah92 on 2025-08-19 18:00_

I tested this with respect to https://github.com/astral-sh/ty/issues/520.
To recap, `google-cloud-pubsub` use partial `py.typed`, and now works with `ty`. However, `mypy` doesn't, and hint at installing `types-protobuf`. This makes `mypy` work, but in turn breaks `ty`.

Repro:
```bash
➜  Downloads uv init tytest
Initialized project `tytest` at `/Users/galah92/Downloads/tytest`
➜  Downloads cd tytest
➜  tytest git:(main) ✗ uv add ty google-cloud-pubsub
➜  tytest git:(main) ✗ vi main.py
➜  tytest git:(main) ✗ uv run ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                    All checks passed!
➜  tytest git:(main) ✗ uv add types-protobuf
➜  tytest git:(main) ✗ uv run ty check
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
Checking ------------------------------------------------------------ 1/1 files                                                                                                    error[unresolved-import]: Cannot resolve imported module `google.cloud`
 --> main.py:2:6
  |
1 | import os
2 | from google.cloud import pubsub_v1
  |      ^^^^^^^^^^^^
3 |
4 | publisher = pubsub_v1.PublisherClient()
  |
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 1 diagnostic
```

---
