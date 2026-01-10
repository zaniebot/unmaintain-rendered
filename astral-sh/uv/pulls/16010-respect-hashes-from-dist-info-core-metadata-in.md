```yaml
number: 16010
title: Respect hashes from dist-info/core-metadata in simple package index
type: pull_request
state: closed
author: adisbladis
labels: []
assignees: []
base: main
head: simple-metadata-hashes
created_at: 2025-09-24T06:01:33Z
updated_at: 2025-09-24T08:30:19Z
url: https://github.com/astral-sh/uv/pull/16010
synced_at: 2026-01-10T06:36:15Z
```

# Respect hashes from dist-info/core-metadata in simple package index

---

_Pull request opened by @adisbladis on 2025-09-24 06:01_

## Summary

When a package defines a hash as a file URL fragment the hash is honoured:
```html
<a href="/whl/cpu/torch-2.8.0%2Bcpu-cp313-cp313-manylinux_2_28_x86_64.whl#sha256=ac59d7d8f73f4f515c949c9583ead0cce1f230a649f6c63fea0e949f93f02ae6">torch-2.8.0+cpu-cp313-cp313-manylinux_2_28_x86_64.whl</a><br/>
```
and is recorded in `uv.lock`:
```toml
[[package]]
name = "torch"
version = "2.8.0+cpu"
source = { registry = "local" }
dependencies = [
    { name = "filelock" },
    { name = "fsspec" },
    { name = "jinja2" },
    { name = "networkx" },
    { name = "setuptools" },
    { name = "sympy" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.8.0%2Bcpu-cp313-cp313-manylinux_2_28_x86_64.whl", hash = "sha256:ac59d7d8f73f4f515c949c9583ead0cce1f230a649f6c63fea0e949f93f02ae6" },
]
```

But when defined as dist-info metadata or core metadata their hashes are ignored:
```html
<a href="/whl/cpu/torch-2.8.0%2Bcpu-cp313-cp313-manylinux_2_28_x86_64.whl" data-dist-info-metadata="sha256=ac59d7d8f73f4f515c949c9583ead0cce1f230a649f6c63fea0e949f93f02ae6" data-core-metadata="sha256=ac59d7d8f73f4f515c949c9583ead0cce1f230a649f6c63fea0e949f93f02ae6">torch-2.8.0+cpu-cp313-cp313-manylinux_2_28_x86_64.whl</a><br/>
```
and uv.lock is rendered without wheel hashes:
```toml
[[package]]
name = "torch"
version = "2.8.0+cpu"
source = { registry = "local" }
dependencies = [
    { name = "filelock" },
    { name = "fsspec" },
    { name = "jinja2" },
    { name = "networkx" },
    { name = "setuptools" },
    { name = "sympy" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-2.8.0%2Bcpu-cp313-cp313-manylinux_2_28_x86_64.whl" },
]
```
despite hash metadata being available.

This makes uv respect hashes from `data-dist-info` & `data-core-metadata` HTML attributes.

## Test Plan

Automated test included.

Closes #16009


---

_Closed by @adisbladis on 2025-09-24 08:30_

---
