```yaml
number: 14294
title: Allow local indexes to reference remote files
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/p
created_at: 2025-06-26T19:56:24Z
updated_at: 2025-06-26T20:17:43Z
url: https://github.com/astral-sh/uv/pull/14294
synced_at: 2026-01-10T07:01:23Z
```

# Allow local indexes to reference remote files

---

_Pull request opened by @charliermarsh on 2025-06-26 19:56_

## Summary

Previously, we assumed that local indexes only referenced local files. However, it's fine for a local index (like, a `file://`-based Simple API) to reference a remote file, and in fact Pyodide operates this way.

Closes https://github.com/astral-sh/uv/issues/14227.

## Test Plan

Ran `UV_INDEX=$(pyodide config get package_index) cargo run add anyio`, which produced this lockfile:

```toml
version = 1
revision = 2
requires-python = ">=3.13.2"

[[package]]
name = "anyio"
version = "4.9.0"
source = { registry = "../../../Library/Caches/.pyodide-xbuildenv-0.30.5/0.27.7/xbuildenv/pyodide-root/package_index" }
dependencies = [
    { name = "idna" },
    { name = "sniffio" },
]
wheels = [
    { url = "https://cdn.jsdelivr.net/pyodide/v0.27.7/full/anyio-4.9.0-py3-none-any.whl", hash = "sha256:e1d9180d4361fd71d1bc4a7007fea6cae1d18792dba9d07eaad89f2a8562f71c" },
]

[[package]]
name = "foo"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "anyio" },
]

[package.metadata]
requires-dist = [{ name = "anyio", specifier = ">=4.9.0" }]

[[package]]
name = "idna"
version = "3.7"
source = { registry = "../../../Library/Caches/.pyodide-xbuildenv-0.30.5/0.27.7/xbuildenv/pyodide-root/package_index" }
wheels = [
    { url = "https://cdn.jsdelivr.net/pyodide/v0.27.7/full/idna-3.7-py3-none-any.whl", hash = "sha256:9d4685891e3e37434e09b1becda7e96a284e660c7aea9222564d88b6c3527c09" },
]

[[package]]
name = "sniffio"
version = "1.3.1"
source = { registry = "../../../Library/Caches/.pyodide-xbuildenv-0.30.5/0.27.7/xbuildenv/pyodide-root/package_index" }
wheels = [
    { url = "https://cdn.jsdelivr.net/pyodide/v0.27.7/full/sniffio-1.3.1-py3-none-any.whl", hash = "sha256:9215f9917b34fc73152b134a3fc0a2eb0e4a49b0b956100cad75e84943412bb9" },
]
```

---
