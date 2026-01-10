---
number: 14217
title: Support presigned URLs in pyproject.toml
type: issue
state: closed
author: esafak
labels:
  - enhancement
  - needs-mre
assignees: []
created_at: 2025-06-23T15:49:22Z
updated_at: 2025-06-23T18:52:09Z
url: https://github.com/astral-sh/uv/issues/14217
synced_at: 2026-01-10T01:25:43Z
---

# Support presigned URLs in pyproject.toml

---

_Issue opened by @esafak on 2025-06-23 15:49_

### Summary

I tried to install a vendored version of `pyfory` from a personal R2 bucket, which I created a pre-signed HTTP URL of, since R2 is not supported. The error I received was 
```
error: Failed to parse file extension; expected one of: `.whl`, `.tar.gz`, `.zip`, `.tar.bz2`, `.tar.lz`, `.tar.lzma`, `.tar.xz`, `.tar.zst`, `.tar`, `.tbz`, `.tgz`, `.tlz`, or `.txz`
  Caused by: `.whl`, `.tar.gz`, `.zip`, `.tar.bz2`, `.tar.lz`, `.tar.lzma`, `.tar.xz`, `.tar.zst`, `.tar`, `.tbz`, `.tgz`, `.tlz`, or `.txz`
```

### Example

Specify a python library in pyproject.toml like `pyfory @ https://foo.r2.cloudflarestorage.com/bar/pyfory-0.12.0.dev0-cp312-cp312-linux_aarch64.whl?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=foo%2Fauto%2Fs3%2Faws4_request&X-Amz-Date=20250623T043147Z&X-Amz-Expires=604800&X-Amz-SignedHeaders=host&X-Amz-Signature=bar`

---

_Label `enhancement` added by @esafak on 2025-06-23 15:49_

---

_Referenced in [astral-sh/uv#14221](../../astral-sh/uv/pulls/14221.md) on 2025-06-23 16:57_

---

_Comment by @charliermarsh on 2025-06-23 17:56_

Hmm, why am I not able to reproduce this?

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.2"
dependencies = ["flask @ https://files.pythonhosted.org/packages/3d/68/9d4508e893976286d2ead7f8f571314af6c2037af34853a30fd769c02e9d/flask-3.1.1-py3-none-any.whl?foo=1"]
```

This locks and installs without error for me. Are you on the latest version of uv?

---

_Label `needs-mre` added by @charliermarsh on 2025-06-23 17:56_

---

_Referenced in [astral-sh/uv#14224](../../astral-sh/uv/pulls/14224.md) on 2025-06-23 18:37_

---

_Closed by @charliermarsh on 2025-06-23 18:52_

---
