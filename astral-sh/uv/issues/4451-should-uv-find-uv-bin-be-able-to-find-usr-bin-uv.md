```yaml
number: 4451
title: Should uv.find_uv_bin() be able to find /usr/bin/uv?
type: issue
state: closed
author: musicinmybrain
labels:
  - compatibility
assignees: []
created_at: 2024-06-23T15:02:10Z
updated_at: 2024-06-23T20:26:00Z
url: https://github.com/astral-sh/uv/issues/4451
synced_at: 2026-01-12T15:58:50Z
```

# Should uv.find_uv_bin() be able to find /usr/bin/uv?

---

_@musicinmybrain_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I have almost finished preparing a `uv` package for Fedora. As written, the `uv.find_uv_bin()` Python function would not be able to find the executable `/usr/bin/uv` that this package would provide:

- First it checks https://github.com/astral-sh/uv/blob/2288ff7bf497ed173658d05dcbed815ba1e2b543/python/uv/__init__.py#L13 which evaluates to `/usr/local/bin/uv`.
- Then it checks https://github.com/astral-sh/uv/blob/2288ff7bf497ed173658d05dcbed815ba1e2b543/python/uv/__init__.py#L26 where `user_scheme` is `"posix_user"`, which evaluates to '/home/myusername/.local/bin/uv'.
- Then it checks https://github.com/astral-sh/uv/blob/2288ff7bf497ed173658d05dcbed815ba1e2b543/python/uv/__init__.py#L31-L32 which evaluates to something like  `/usr/lib/python3.13/bin/uv` or `/usr/lib64/python3.13/bin/uv`. (I currently get the latter, but since the `uv` Python library is actually arch-independent when it doesn’t bundle the `uv` executable, I should probably ensure it’s the former.)

----

I should be able to fix this by something like:

```diff
diff --git a/python/uv/__init__.py b/python/uv/__init__.py
index 781eee4f..f2603785 100644
--- a/python/uv/__init__.py
+++ b/python/uv/__init__.py
@@ -10,6 +10,12 @@ def find_uv_bin() -> str:
 
     uv_exe = "uv" + sysconfig.get_config_var("EXE")
 
+    bindir_path = sysconfig.get_config_var("BINDIR")
+    if bindir_path is not None:
+        path = os.path.join(bindir_path, uv_exe)
+        if os.path.isfile(path):
+            return path
+
     path = os.path.join(sysconfig.get_path("scripts"), uv_exe)
     if os.path.isfile(path):
         return path
```

or, probably better,

```diff
diff --git a/python/uv/__init__.py b/python/uv/__init__.py
index 781eee4f..80953b2e 100644
--- a/python/uv/__init__.py
+++ b/python/uv/__init__.py
@@ -1,6 +1,7 @@
 from __future__ import annotations
 
 import os
+import shutil
 import sys
 import sysconfig
 
@@ -10,6 +11,10 @@ def find_uv_bin() -> str:
 
     uv_exe = "uv" + sysconfig.get_config_var("EXE")
 
+    path = shutil.which(uv_exe)
+    if path is not None:
+        return path
+
     path = os.path.join(sysconfig.get_path("scripts"), uv_exe)
     if os.path.isfile(path):
         return path
```

Is this a change that you think belongs in `uv` upstream, or is this something I should carry as a downstream patch?

---

_Comment by @charliermarsh on 2024-06-23 17:24_

Is that binary path exposed anywhere on `sysconfig` or `distutils`? I think the goal of this method is to find the Python that's installed in the current environment, so `which` feels a little too permissive.

---

_Label `compatibility` added by @charliermarsh on 2024-06-23 17:24_

---

_Comment by @charliermarsh on 2024-06-23 17:25_

As a separate question: when you install via the package manager, are you installing the Python sources or just the `uv` binary? (Sort of asking: does this even _need_ to work?)

---

_Comment by @musicinmybrain on 2024-06-23 18:15_

From the `uv` source RPM, I plan to build a binary RPM (what people actually install) also called `uv`, which will install `/usr/bin/uv`, plus ancillary files that should ship with the executable: tab-completion support, basic documentation like `README.md`, license files, etc..

Then I plan to have another binary RPM, built from the same source RPM, called `python3-uv`. This provides the importable Python `uv` package, i.e. https://github.com/astral-sh/uv/tree/main/python/uv, along with the `.dist-info` metadata, and which has an exact-version dependency on the `uv` binary RPM built from the same source RPM. It can therefore rely on “referencing” the system-wide `/usr/bin/uv`. For RPM package users, this should work as if they had installed the package from PyPI. We *do* need this to work, because system packages like [`hatch`](https://src.fedoraproject.org/rpms/hatch) will be depending on `uv` via a [Python dependency](https://github.com/pypa/hatch/blob/72e09428a2e6846962413b227534c0eb365a86ed/pyproject.toml#L54). This system `python3-uv` should *always* find the system `/usr/bin/uv` rather than something else in the user’s path, which, come to think of it, is an argument *against* `shutil.which` and in favor of `sysconfig.get_config_var("BINDIR")` for our use case. (For similar reasons, we always adjust `#!/usr/bin/env python` shebangs in RPM-installed Python scripts to `#!/usr/bin/python3` to ensure they run with the system Python interpreter.)

If you think that finding `/usr/bin/uv` is a need that is specific to this kind of distribution packaging, then it’s easy enough to make the adaptation downstream only. However (unless I’m missing something), it does seem a little strange to support finding `uv` in `/usr/local/bin` but not in `/usr/bin`.

---

_Comment by @musicinmybrain on 2024-06-23 20:08_

> However (unless I’m missing something), it does seem a little strange to support finding `uv` in `/usr/local/bin` but not in `/usr/bin`.

Looking again at the actual code, I suppose that while https://github.com/astral-sh/uv/blob/2288ff7bf497ed173658d05dcbed815ba1e2b543/python/uv/__init__.py#L13 evaluates to `/usr/local/bin/uv` in a system-wide installation like this, that’s not the situation you are trying to cover with that case. It’s really intended for `/path/to/virtualenv/bin/uv`. And furthermore, in the virtualenv case, you presumably *don’t* want to find `/usr/bin/uv` (or`/usr/local/bin/uv`) first.

So maybe this really is a situation where a downstream-only patch is the right approach to ensure the system Python `uv` library finds the system `uv` executable. However, if you think there’s anything to be improved here in the general case, I’m happy to offer a PR.

---

_Comment by @charliermarsh on 2024-06-23 20:14_

Yeah, I think it's right for this to be a downstream-only patch. Sorry if it causes you any inconvenience but that's where I'm ending up too. (Thanks for walking me through it.)

---

_Closed by @charliermarsh on 2024-06-23 20:21_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-06-23 20:21_

---

_Comment by @musicinmybrain on 2024-06-23 20:25_

> Yeah, I think it's right for this to be a downstream-only patch. Sorry if it causes you any inconvenience but that's where I'm ending up too. (Thanks for walking me through it.)

Thanks for talking through it with me. You helped me clarify my reasoning, and I’m now much more confident that the first proposed downstream patch from https://github.com/astral-sh/uv/issues/4451#issue-2368608161 is a good approach.

---
