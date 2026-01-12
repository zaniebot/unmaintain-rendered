```yaml
number: 4912
title: Filter out markers based on Python requirement
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/ord
created_at: 2024-07-09T04:55:26Z
updated_at: 2024-07-09T16:16:04Z
url: https://github.com/astral-sh/uv/pull/4912
synced_at: 2026-01-12T16:06:31Z
```

# Filter out markers based on Python requirement

---

_@charliermarsh_

## Summary

In marker normalization, we now remove any markers that are redundant with the `requires-python` specifier (i.e., always true for the given Python requirement).

For example, given `iniconfig ; python_version >= '3.7'`, we can remove the `python_version >= '3.7'` marker when resolving with `--python-version 3.8`.

Closes #4852.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-09 04:55_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-07-09 04:55_

---

_Label `enhancement` added by @charliermarsh on 2024-07-09 04:55_

---

_Label `preview` added by @charliermarsh on 2024-07-09 04:55_

---

_Comment by @charliermarsh on 2024-07-09 04:59_

This presumes that we actually want to omit these.

---

_Comment by @charliermarsh on 2024-07-09 06:00_

The snapshot changes do represent a nice simplification.

---

_Review requested from @konstin by @charliermarsh on 2024-07-09 06:00_

---

_Comment by @konstin on 2024-07-09 08:49_

transformers:

```diff
diff --git a/uv.lock b/uv.lock
index cb0d754..ddc9775 100644
--- a/uv.lock
+++ b/uv.lock
@@ -1452,7 +1452,7 @@ name = "humanfriendly"
 version = "10.0"
 source = { registry = "https://pypi.org/simple" }
 dependencies = [
-    { name = "pyreadline3", marker = "python_version >= '3.8' and sys_platform == 'win32'" },
+    { name = "pyreadline3", marker = "sys_platform == 'win32'" },
 ]
 sdist = { url = "https://files.pythonhosted.org/packages/cc/3f/2c29224acb2e2df4d2046e4c73ee2662023c58ff5b113c4c1adac0886c43/humanfriendly-10.0.tar.gz", hash = "sha256:6b0b831ce8f15f7300721aa49829fc4e83921a9a301cc7f606be6686a2288ddc", size = 360702 }
 wheels = [
@@ -2607,7 +2607,7 @@ name = "opencv-python"
 version = "4.10.0.84"
 source = { registry = "https://pypi.org/simple" }
 dependencies = [
-    { name = "numpy", marker = "python_version >= '3.7' or (python_version <= '3.9' and platform_machine == 'arm64' and platform_system == 'Darwin') or (python_version >= '3.6' and platform_machine == 'aarch64' and platform_system == 'Linux') or (python_version >= '3.10' and platform_system == 'Darwin')" },
+    { name = "numpy" },
 ]
 sdist = { url = "https://files.pythonhosted.org/packages/4a/e7/b70a2d9ab205110d715906fc8ec83fbb00404aeb3a37a0654fdb68eb0c8c/opencv-python-4.10.0.84.tar.gz", hash = "sha256:72d234e4582e9658ffea8e9cae5b63d488ad06994ef12d81dc303b17472f3526", size = 95103981 }
 wheels = [
@@ -4165,7 +4165,7 @@ name = "sqlalchemy"
 version = "2.0.31"
 source = { registry = "https://pypi.org/simple" }
 dependencies = [
-    { name = "greenlet", marker = "python_version < '3.13' and (platform_machine == 'aarch64' or (platform_machine == 'ppc64le' or (platform_machine == 'x86_64' or (platform_machine == 'amd64' or (platform_machine == 'AMD64' or (platform_machine == 'WIN32' or platform_machine == 'win32'))))))" },
+    { name = "greenlet", marker = "python_version < '3.13' and (platform_machine == 'AMD64' or platform_machine == 'WIN32' or platform_machine == 'aarch64' or platform_machine == 'amd64' or platform_machine == 'ppc64le' or platform_machine == 'win32' or platform_machine == 'x86_64')" },
     { name = "typing-extensions" },
 ]
 sdist = { url = "https://files.pythonhosted.org/packages/ba/7d/e3312ae374fe7a4af7e1494735125a714a0907317c829ab8d8a31d89ded4/SQLAlchemy-2.0.31.tar.gz", hash = "sha256:b607489dd4a54de56984a0c7656247504bd5523d9d0ba799aef59d4add009484", size = 9524110 }
@@ -5391,7 +5391,7 @@ standard = [
     { name = "httptools" },
     { name = "python-dotenv" },
     { name = "pyyaml" },
-    { name = "uvloop", marker = "sys_platform != 'win32' and (platform_python_implementation != 'PyPy' and sys_platform != 'cygwin')" },
+    { name = "uvloop", marker = "platform_python_implementation != 'PyPy' and sys_platform != 'cygwin' and sys_platform != 'win32'" },
     { name = "watchfiles" },
     { name = "websockets" },
 ]
```

warehouse
```diff
diff --git a/uv.lock b/uv.lock
index 5002a7d10..46cc86ceb 100644
--- a/uv.lock
+++ b/uv.lock
@@ -295,7 +295,7 @@ source = { registry = "https://pypi.org/simple" }
 dependencies = [
     { name = "jmespath" },
     { name = "python-dateutil" },
-    { name = "urllib3", marker = "python_version >= '3.10'" },
+    { name = "urllib3" },
 ]
 sdist = { url = "https://files.pythonhosted.org/packages/c2/30/42e7921baec564207e1593a6816a6739eeba6517307a51d0ec3e8b0e5628/botocore-1.34.139.tar.gz", hash = "sha256:df023d8cf8999d574214dad4645cb90f9d2ccd1494f6ee2b57b1ab7522f6be77", size = 12565834 }
 wheels = [
@@ -1754,7 +1754,7 @@ dependencies = [
     { name = "gitpython" },
     { name = "mkdocs" },
     { name = "requests" },
-    { name = "tzdata", marker = "python_version >= '3.9' and sys_platform == 'win32'" },
+    { name = "tzdata", marker = "sys_platform == 'win32'" },
 ]
 sdist = { url = "https://files.pythonhosted.org/packages/97/7f/6d668a1f6210db0738c9043922e38c2ff8646559d200e94ad1b4766a677f/mkdocs_rss_plugin-1.15.0.tar.gz", hash = "sha256:92995ed6c77b2ae1f5f2913e62282c27e50c35d618c4291b5b939e50badd7504", size = 32566 }
 wheels = [
@@ -3278,7 +3278,7 @@ name = "sqlalchemy"
 version = "2.0.31"
 source = { registry = "https://pypi.org/simple" }
 dependencies = [
-    { name = "greenlet", marker = "python_version < '3.13' and (platform_machine == 'aarch64' or (platform_machine == 'ppc64le' or (platform_machine == 'x86_64' or (platform_machine == 'amd64' or (platform_machine == 'AMD64' or (platform_machine == 'WIN32' or platform_machine == 'win32'))))))" },
+    { name = "greenlet", marker = "python_version < '3.13' and (platform_machine == 'AMD64' or platform_machine == 'WIN32' or platform_machine == 'aarch64' or platform_machine == 'amd64' or platform_machine == 'ppc64le' or platform_machine == 'win32' or platform_machine == 'x86_64')" },
     { name = "typing-extensions" },
 ]
 sdist = { url = "https://files.pythonhosted.org/packages/ba/7d/e3312ae374fe7a4af7e1494735125a714a0907317c829ab8d8a31d89ded4/SQLAlchemy-2.0.31.tar.gz", hash = "sha256:b607489dd4a54de56984a0c7656247504bd5523d9d0ba799aef59d4add009484", size = 9524110 }
@@ -3346,8 +3346,8 @@ name = "stripe"
 version = "10.1.0"
 source = { registry = "https://pypi.org/simple" }
 dependencies = [
-    { name = "requests", marker = "python_version >= '3.0'" },
-    { name = "typing-extensions", marker = "python_version >= '3.7'" },
+    { name = "requests" },
+    { name = "typing-extensions" },
 ]
 sdist = { url = "https://files.pythonhosted.org/packages/a4/58/2389ae1328d8f47b9fe83d9e21f0731e4ec81d2f31452fb2ba2266e97752/stripe-10.1.0.tar.gz", hash = "sha256:241a2cba4fe8412369bfbbfaea9971c350247c4fb6e96ce8884057ebac60f1f9", size = 1290758 }
 wheels = [
```

---

_@konstin approved on 2024-07-09 08:49_

---

_Comment by @konstin on 2024-07-09 08:51_

Warehouse shows that there's yet another normalization opportunity: The full python requirements is `requires-python = ">=3.11, <3.12"`, so we could remove `python_version < '3.13'`, too.

---

_@BurntSushi approved on 2024-07-09 14:26_

Yes. I like this.

---

_@ibraheemdev approved on 2024-07-09 16:11_

---

_Merged by @charliermarsh on 2024-07-09 16:15_

---

_Closed by @charliermarsh on 2024-07-09 16:15_

---

_Branch deleted on 2024-07-09 16:16_

---

_Comment by @charliermarsh on 2024-07-09 16:16_

Transient Git failures.

---
