```yaml
number: 6316
title: uv lock specifier instability?
type: issue
state: closed
author: bluss
labels:
  - bug
  - lock
assignees: []
created_at: 2024-08-21T10:34:58Z
updated_at: 2024-08-21T20:24:13Z
url: https://github.com/astral-sh/uv/issues/6316
synced_at: 2026-01-12T15:59:03Z
```

# uv lock specifier instability?

---

_@bluss_

In some cases I see the following diff in my project's lock file. The order of the specifiers is flipping back and forth. (Both sides of the diff are using uv 0.3.0).

The source is `pyproject.toml`'s dependencies = ["numpy>=1.25.2,<2"] (with more dependencies specified).

Is it possible there is some leeway in the ordering for these? Using uv 0.3.0.

```diff
diff --git a/uv.lock b/uv.lock
index 53e5a5c..e5da145 100644
--- a/uv.lock
+++ b/uv.lock
@@ -1329,7 +1329,7 @@ requires-dist = [
     { name = "jinja2", specifier = ">=3.1.2" },
     { name = "jupytext", specifier = ">=1.15.1" },
     { name = "nbconvert", specifier = ">=7.8.0" },
-    { name = "numpy", specifier = "<2,>=1.25.2" },
+    { name = "numpy", specifier = ">=1.25.2,<2" },
     { name = "openpyxl", specifier = ">=3.1.2" },

```

Seems to reproduce with the full dependency list, after a few uv sync and uv locks.
Using `uv python pin 3.11`. Defacto python is `cpython-3.11.7-linux-x86_64-gnu`.

(Edited here: reduced problem to only two dependencies)

```toml
[project]
name = "newproj"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"

dependencies = [
    "papermill>=2.4.0",
    "numpy>=1.25.2,<2",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Seems to in fact correlate with adding/removing aiohttp too, which happens without change to pyproject.toml.

```
$ touch pyproject.toml  && uv sync
Resolved 96 packages in 1.28s
   Built newproj @ file:///home/user/tmp/newproj
Prepared 1 package in 151ms
Uninstalled 2 packages in 0.66ms
Installed 1 package in 0.28ms
 - aiohttp==3.10.5
 ~ newproj==0.1.0 (from file:///home/user/tmp/newproj)
$ touch pyproject.toml  && uv sync
Resolved 96 packages in 2ms
   Built newproj @ file:///home/user/tmp/newproj
Prepared 2 packages in 155ms
Uninstalled 1 package in 0.15ms
Installed 2 packages in 0.77ms
 + aiohttp==3.10.5
 ~ newproj==0.1.0 (from file:///home/user/tmp/newproj)
$ touch pyproject.toml  && uv sync
Resolved 96 packages in 2ms
   Built newproj @ file:///home/user/tmp/newproj
Prepared 1 package in 154ms
Uninstalled 1 package in 0.16ms
Installed 1 package in 0.34ms
 ~ newproj==0.1.0 (from file:///home/user/tmp/newproj)

```

---

_Comment by @konstin on 2024-08-21 11:34_

Is this happening with a lockfile initialized with 0.3.0, i.e. does 0.3.0 ever write out `<2,>=1.25.2` that wasn't there before? I've seen these cases upgrading to 0.3.0, but i can't reproduce it in 0.3.0.

---

_Comment by @konstin on 2024-08-21 11:42_

For context, i have confirmed that hatchling flips the order of the specifier (`uvx --from build pyproject-build --installer uv && unzip -p dist/dummy-0.1.0-py3-none-any.whl dummy-0.1.0.dist-info/METADATA | grep "Requires-Dist"`), but we shouldn't be reading metadata through hatchling (or if we do, that's probably the source of the bug)

---

_Comment by @bluss on 2024-08-21 11:44_

Yes, it happens when starting with uv 0.3.0.

This whimsical sequence of commands ends up with a diff like in the issue report

``` 
uv init --no-config newproj
cd newproj/
git init .
git add README.md  src/ pyproject.toml 
uv add  "papermill>=2.4.0" "numpy>=1.25.2,<2"
git add -u
git commit -m first
uv sync
git add uv.lock 
git commit -m lockfile
uv lock
git diff
uv sync
uv sync
git diff
touch pyproject.toml  && uv sync
touch pyproject.toml  && uv sync
touch pyproject.toml  && uv sync
git diff
```

```diff
diff --git uv.lock uv.lock
index 1f890d7..f606d83 100644
--- uv.lock
+++ uv.lock
@@ -346,7 +346,7 @@ dependencies = [
 
 [package.metadata]
 requires-dist = [
-    { name = "numpy", specifier = "<2,>=1.25.2" },
+    { name = "numpy", specifier = ">=1.25.2,<2" },
     { name = "papermill", specifier = ">=2.4.0" },
 ]
```

I forgot to pin python in this command sequence. So it reproduced with python 3.12.4 for me there.

---

_Label `bug` added by @charliermarsh on 2024-08-21 13:23_

---

_Label `lock` added by @charliermarsh on 2024-08-21 13:23_

---

_Assigned to @konstin by @charliermarsh on 2024-08-21 13:23_

---

_Comment by @konstin on 2024-08-21 14:17_

Thank you for the great reproducer!

---

_Closed by @konstin on 2024-08-21 15:18_

---

_Comment by @bluss on 2024-08-21 20:24_

heh, thank you, your reduced sequence was better

---
