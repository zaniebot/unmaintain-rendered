```yaml
number: 16358
title: could you use releases rather than git hashes for Cargo.toml deps?
type: issue
state: closed
author: sthen
labels:
  - enhancement
assignees: []
created_at: 2025-10-19T10:33:49Z
updated_at: 2025-11-01T19:47:36Z
url: https://github.com/astral-sh/uv/issues/16358
synced_at: 2026-01-12T16:02:29Z
```

# could you use releases rather than git hashes for Cargo.toml deps?

---

_@sthen_

### Summary

Providing OS packages for uv (and also ruff) is rather painful because in many cases, builds must be done without network access.

The current situation where rs-async-zip, pubgrub, reqwest-middleware, etc are fetched from github using hashes means that they need patching to use a separately downloaded checkout. e.g. https://github.com/openbsd/ports/blob/master/devel/uv/patches/patch-Cargo_toml

Please would it be possible to stick to tagged releases instead?

### Example

_No response_

---

_Label `enhancement` added by @sthen on 2025-10-19 10:33_

---

_Comment by @konstin on 2025-10-19 12:02_

Please https://github.com/astral-sh/uv/issues/13640, where we're tracking this discussion.

---

_Closed by @charliermarsh on 2025-11-01 19:47_

---
