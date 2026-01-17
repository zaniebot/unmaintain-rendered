```yaml
number: 17549
title: "`uv lock` does not update `uv.lock` `package.metadata.requires-dist[*].specifier` when useless trailing `.0` is added/removed from specifier in `pyproject.toml`"
type: issue
state: open
author: GideonBear
labels:
  - bug
assignees: []
created_at: 2026-01-17T14:15:05Z
updated_at: 2026-01-17T14:58:32Z
url: https://github.com/astral-sh/uv/issues/17549
synced_at: 2026-01-17T15:13:16Z
```

# `uv lock` does not update `uv.lock` `package.metadata.requires-dist[*].specifier` when useless trailing `.0` is added/removed from specifier in `pyproject.toml`

---

_@GideonBear_

### Summary

When adding or removing a trailing `.0` from a specifier in `pyproject.toml`, `uv lock` does not update the specifier in the lockfile, while some other operations will update the specifier in the lockfile. This causes:
1. Unnecessary merge conflicts
2. The change to be spread to unrelated commits

I expect `uv lock` to have a consistent output.

### Reproduction
I will be using `git` to make it clearer what's happening, but this is not necessary to see the bug.
```console
$ mkdir delme && cd delme
$ uv init
$ uv sync
$ git add .
$ git commit -m "Initial commit"

$ uv add termcolor
$ git diff
```
```diff
diff --git a/pyproject.toml b/pyproject.toml
index 92a83e5..d95598e 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -4,4 +4,6 @@ version = "0.1.0"
 description = "Add your description here"
 readme = "README.md"
 requires-python = ">=3.14"
-dependencies = []
+dependencies = [
+    "termcolor>=3.3.0",
+]
diff --git a/uv.lock b/uv.lock
index b1b7a41..5a32051 100644
--- a/uv.lock
+++ b/uv.lock
@@ -6,3 +6,18 @@ requires-python = ">=3.14"
 name = "delme"
 version = "0.1.0"
 source = { virtual = "." }
+dependencies = [
+    { name = "termcolor" },
+]
+
+[package.metadata]
+requires-dist = [{ name = "termcolor", specifier = ">=3.3.0" }]
+
+[[package]]
+name = "termcolor"
+version = "3.3.0"
+source = { registry = "https://pypi.org/simple" }
+sdist = { url = "https://files.pythonhosted.org/packages/46/79/cf31d7a93a8fdc6aa0fbb665be84426a8c5a557d9240b6239e9e11e35fc5/termcolor-3.3.0.tar.gz", hash = "sha256:348871ca648ec6a9a983a13ab626c0acce02f515b9e1983332b17af7979521c5", size = 14434, upload-time = "2025-12-29T12:55:21.882Z" }
+wheels = [
+    { url = "https://files.pythonhosted.org/packages/33/d1/8bb87d21e9aeb323cc03034f5eaf2c8f69841e40e4853c2627edf8111ed3/termcolor-3.3.0-py3-none-any.whl", hash = "sha256:cf642efadaf0a8ebbbf4bc7a31cec2f9b5f21a9f726f4ccbb08192c9c26f43a5", size = 7734, upload-time = "2025-12-29T12:55:20.718Z" },
+]
```
```console
$ git commit -am "Add termcolor"

$ sed -i 's/3.3.0/3.3/' pyproject.toml
# Or:
$ uvx pyproject-fmt pyproject.toml

$ git diff
```
```diff
diff --git a/pyproject.toml b/pyproject.toml
index d95598e..c2b1a39 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -5,5 +5,5 @@ description = "Add your description here"
 readme = "README.md"
 requires-python = ">=3.14"
 dependencies = [
-    "termcolor>=3.3.0",
+    "termcolor>=3.3",
 ]
```
```console
$ uv lock
# No change in lockfile!
$ git commit -am "Format pyproject.toml"

# Do any operation on pyproject.fmt, like:
$ uv version --bump patch
$ git diff
```
```diff
diff --git a/pyproject.toml b/pyproject.toml
index c2b1a39..a447926 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -1,6 +1,6 @@
 [project]
 name = "delme"
-version = "0.1.0"
+version = "0.1.1"
 description = "Add your description here"
 readme = "README.md"
 requires-python = ">=3.14"
diff --git a/uv.lock b/uv.lock
index 5a32051..72af669 100644
--- a/uv.lock
+++ b/uv.lock
@@ -4,14 +4,14 @@ requires-python = ">=3.14"
 
 [[package]]
 name = "delme"
-version = "0.1.0"
+version = "0.1.1"
 source = { virtual = "." }
 dependencies = [
     { name = "termcolor" },
 ]
 
 [package.metadata]
-requires-dist = [{ name = "termcolor", specifier = ">=3.3.0" }]
+requires-dist = [{ name = "termcolor", specifier = ">=3.3" }]
 
 [[package]]
 name = "termcolor"
```

### Platform

Linux Mint 22.3 - Cinnamon 64-bit - Linux 6.8.0-90-generic x86_64 GNU/Linux

### Version

uv 0.9.26

### Python version

Python 3.14.2

---

_Label `bug` added by @GideonBear on 2026-01-17 14:15_

---

_Comment by @zanieb on 2026-01-17 14:58_

Per the specification trailing zeros have no semantic meaning so we're probably just considering these versions equal. It's not clear to me that we should actually invalidate the lockfile in this case. It seems pretty rare to be adding `.0` to your dependencies, no?

---
