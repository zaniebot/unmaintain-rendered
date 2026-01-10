```yaml
number: 12605
title: "Drop support for reading executable name requests from `.python-version` files"
type: issue
state: closed
author: zanieb
labels:
  - compatibility
  - breaking
assignees: []
created_at: 2025-04-01T18:22:10Z
updated_at: 2025-04-21T19:16:49Z
url: https://github.com/astral-sh/uv/issues/12605
synced_at: 2026-01-10T03:41:47Z
```

# Drop support for reading executable name requests from `.python-version` files

---

_Issue opened by @zanieb on 2025-04-01 18:22_

It seems weird to write an executable name to a `.python-version` file, and this causes conflicts with `pyenv-virtualenv` as they write environment names there (see https://github.com/astral-sh/uv/pull/7935)

I think we should just ignore `.python-version` files if they contain this. Perhaps warn?

---

_Label `breaking` added by @zanieb on 2025-04-01 18:22_

---

_Label `compatibility` added by @zanieb on 2025-04-01 18:22_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-04-01 18:22_

---

_Closed by @zanieb on 2025-04-21 19:16_

---
