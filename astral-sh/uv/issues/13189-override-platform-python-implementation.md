```yaml
number: 13189
title: Override platform_python_implementation
type: issue
state: open
author: KDanisme
labels:
  - question
assignees: []
created_at: 2025-04-29T07:28:03Z
updated_at: 2025-04-29T08:57:22Z
url: https://github.com/astral-sh/uv/issues/13189
synced_at: 2026-01-12T16:01:21Z
```

# Override platform_python_implementation

---

_@KDanisme_

### Question

Is there an option for overriding the `platform_python_implementation` instead of getting it from `platform.python_implementation`

The reason is that I am using graalpy, which people don't create wheels for(graalpy comes with patches for a bunch of libraries).
I would like to download the cpython wheels using uv, but I can't do that because i get the error:
` A path dependency is incompatible with the current platform`

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @KDanisme on 2025-04-29 07:28_

---

_Comment by @konstin on 2025-04-29 08:32_

I don't think we support that currently, is GraalPy compatible with the CPython wheels?

---

_Comment by @KDanisme on 2025-04-29 08:34_

@konstin they have patches you can apply for libraries that don't work out the box.
They have pretty good test coverage

---
