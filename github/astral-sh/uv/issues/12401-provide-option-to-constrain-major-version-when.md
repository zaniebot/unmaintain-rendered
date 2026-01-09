---
number: 12401
title: Provide option to constrain major version when adding a dependency
type: issue
state: closed
author: rzuckerm
labels:
  - enhancement
assignees: []
created_at: 2025-03-23T12:47:00Z
updated_at: 2025-07-08T13:20:18Z
url: https://github.com/astral-sh/uv/issues/12401
synced_at: 2026-01-07T13:12:18-06:00
---

# Provide option to constrain major version when adding a dependency

---

_Issue opened by @rzuckerm on 2025-03-23 12:47_

### Summary

When adding a dependency in poetry by just the package name or with `@latest`, poetry automatically constrains the major version. For example,
`poetry add pytest`, the version added in `pyproject.toml` is this (assuming `8.3.5` is the latest version): `pytest (>=8.3.5, <9)`. When I do `uv add pytest, I get this: `pytest (>=8.3.5)`. It would be nice if I had a way to constrain the major version.

Something also to keep in mind is that poetry treat versions that start with `0` differently, so for `0.10.2`, the next major version is considered to be `0.11`

### Example

Maybe something like this (perhaps have a more concise option):

```
uv add --constrain-major <dependency or dependencies>
```

---

_Label `enhancement` added by @rzuckerm on 2025-03-23 12:47_

---

_Comment by @zanieb on 2025-03-23 22:50_

See also https://github.com/astral-sh/uv/issues/6783

---

_Comment by @rzuckerm on 2025-07-08 13:20_

Looks like `uv add --bounds major` does what I want, so I'm closing this issue. Thanks for you assistance.

---

_Closed by @rzuckerm on 2025-07-08 13:20_

---
