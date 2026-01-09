---
number: 15378
title: Broken lock file sneaks in
type: issue
state: open
author: zzJinux
labels:
  - bug
assignees: []
created_at: 2025-08-19T11:25:36Z
updated_at: 2025-08-20T09:54:16Z
url: https://github.com/astral-sh/uv/issues/15378
synced_at: 2026-01-07T13:12:19-06:00
---

# Broken lock file sneaks in

---

_Issue opened by @zzJinux on 2025-08-19 11:25_

### Summary

I happened to find a broken lock file. I only modified the dependency from `httpx-retries>=0.4.0` to `httpx-retries>=0.4.0,<1.0.0` (`httpx-retries` was `0.4.0` in the lock file) and faced with the CI cyrout from `uv sync --locked`.

Investigating further, I discovered that `uv sync && uv pip check`, where both commands succeeded, reports:

```
The package `langfuse` requires `packaging>=23.2,<25.0`, but `25.0` is installed
```

I identified the revision (made by someone else) that puts the lock file into an inconsistent state. The revision adds `langfuse=3.2.1`, which conflicts with the lock file's `packaging=25.0`. It's less likely that he manually edited the lock file. The strange thing is that CI `uv sync --locked` has never complained so far, including the PR that introduced the revision.

I failed to devise a from-scratch example that reproduces.

### Platform

macOS 15.6 (24G84) arm64

### Version

uv 0.8.12 (36151df0e 2025-08-18)

### Python version

Python 3.13.6

---

_Label `bug` added by @zzJinux on 2025-08-19 11:25_

---

_Comment by @zanieb on 2025-08-19 19:16_

I'm kind of confused here. Can you provide an example diff that was applied to the lockfile that was not caught by `uv sync --locked`?

---

_Comment by @zzJinux on 2025-08-20 09:53_

@zanieb 

```diff
Index: uv.lock
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/uv.lock b/uv.lock
--- a/uv.lock	(revision aaa)
+++ b/uv.lock	(revision bbb)
@@ -1543,6 +1543,26 @@
 ]
 sdist = { url = "https://files.pythonhosted.org/packages/0e/72/a3add0e4eec4eb9e2569554f7c70f4a3c27712f40e3284d483e88094cc0e/langdetect-1.0.9.tar.gz", hash = "sha256:cbc1fef89f8d062739774bd51eda3da3274006b3661d199c2655f6b3f6d605a0", size = 981474, upload-time = "2021-05-07T07:54:13.562Z" }
 
+[[package]]
+name = "langfuse"
+version = "3.2.1"
+source = { registry = "https://pypi.org/simple" }
+dependencies = [
+    { name = "backoff" },
+    { name = "httpx" },
+    { name = "opentelemetry-api" },
+    { name = "opentelemetry-exporter-otlp" },
+    { name = "opentelemetry-sdk" },
+    { name = "packaging" },
+    { name = "pydantic" },
+    { name = "requests" },
+    { name = "wrapt" },
+]
+sdist = { url = "https://files.pythonhosted.org/packages/61/0d/8fc51099cf337fb3b56cb7d305074bc0223c62e1ccabf80cc6285ccf5b31/langfuse-3.2.1.tar.gz", hash = "sha256:f79b0380dfcf52c7525bb5d7f8e9d8786a6fc8b37867def047bb388930a7beb3", size = 153369, upload-time = "2025-07-16T09:50:28.434Z" }
+wheels = [
+    { url = "https://files.pythonhosted.org/packages/92/b0/8f08df3f0fa584c4132937690c6dd33e0a116f963ecf2b35567f614e0ca7/langfuse-3.2.1-py3-none-any.whl", hash = "sha256:07a84e8c1eed6ac8e149bdda1431fd866e4aee741b66124316336fb2bc7e6a32", size = 299315, upload-time = "2025-07-16T09:50:26.582Z" },
+]
+
 [[package]]
 name = "langsmith"
 version = "0.4.10"
@@ -2945,6 +2965,7 @@
     { name = "langchain" },
     { name = "langchain-community" },
     { name = "langcodes" },
+    { name = "langfuse" },
     { name = "mysqlclient" },
     { name = "nanoid" },
     { name = "numpy" },
@@ -3047,6 +3068,7 @@
     { name = "langchain", specifier = ">=0.3.18,<0.4.0" },
     { name = "langchain-community", specifier = ">=0.3.25" },
     { name = "langcodes", specifier = "==3.5.0" },
+    { name = "langfuse", specifier = ">=3.2.1" },
     { name = "mysqlclient", specifier = ">=2.2.6,<3.0.0" },
     { name = "nanoid", specifier = ">=2.0.0,<3.0.0" },
     { name = "numpy", specifier = ">=2.2.6,<2.3.0" },
Index: pyproject.toml
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/pyproject.toml b/pyproject.toml
--- a/pyproject.toml	(revision aaa)
+++ b/pyproject.toml	(revision bbb)
@@ -68,6 +68,7 @@
     "unstructured>=0.18.3",
     "websockets>=15.0.1,<16.0.0",
     "zstandard>=0.23.0,<0.24.0",
+    "langfuse>=3.2.1",
 ]
```

`packaging` package in the lock file hadn't been changed:

```
[[package]]
name = "packaging"
version = "25.0"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/a1/d4/1fc4078c65507b51b96ca8f8c3ba19e6a61c8253c72794544580a7b6c24d/packaging-25.0.tar.gz", hash = "sha256:d443872c98d677bf60f6a1f2f8c1cb748e8fe762d2bf9d3148b5599295b0fc4f", size = 165727, upload-time = "2025-04-19T11:48:59.673Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl", hash = "sha256:29572ef2b1f17581046b3a2227d5c611fb25ec70ca1ba8554b24b0e69331a484", size = 66469, upload-time = "2025-04-19T11:48:57.875Z" },
]
```

---
