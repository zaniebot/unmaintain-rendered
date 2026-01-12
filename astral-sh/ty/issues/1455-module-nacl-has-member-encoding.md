```yaml
number: 1455
title: "Module `nacl` has member `encoding`"
type: issue
state: closed
author: velikopter
labels: []
assignees: []
created_at: 2025-10-30T01:09:40Z
updated_at: 2025-10-30T14:17:00Z
url: https://github.com/astral-sh/ty/issues/1455
synced_at: 2026-01-12T15:54:25Z
```

# Module `nacl` has member `encoding`

---

_@velikopter_

### Summary

When checking the codebase of one of my projects, I got this error from ty:

```
[0] heli:~/shitcode/vona$ uvx ty check --python /usr
error[unresolved-attribute]: Module `nacl` has no member `encoding`
   --> vona/globals/__init__.py:131:29
    |
130 |     return (
131 |         public_key.encode(encoder=nacl.encoding.Base64Encoder)
    |                                   ^^^^^^^^^^^^^
132 |         .decode("utf-8")
133 |         .rstrip("=")
    |
info: rule `unresolved-attribute` is enabled by default

Found 1 diagnostic
[1] heli:~/shitcode/vona$ 
```

This is confusing, because the `nacl` module _does_ have `encoding`.

I haven't configured `ty` in my `pyproject.toml`.

### Version

0.0.1-alpha.25

---

_Comment by @velikopter on 2025-10-30 01:27_

Nevermind, just read the pinned issue, sorry.

---

_Closed by @velikopter on 2025-10-30 01:27_

---

_Comment by @carljm on 2025-10-30 14:17_

Note that [`nacl/__init__.py`](https://github.com/pyca/pynacl/blob/main/src/nacl/__init__.py) does not import the `encoding` submodule, so requiring it to be explicitly imported before it can be referenced is the intended behavior of ty in this case, not a temporary limitation. This matches the behavior of both mypy and pyright; all three error on `import nacl; nacl.encoding` (but all are fine with `import nacl.encoding; nacl.encoding`). The former can succeed sometimes at runtime, but only "by accident" because some other previously-imported module happens to have already imported the `nacl.encoding` submodule; it's intentional for type-checkers to catch that error-prone situation.

---
