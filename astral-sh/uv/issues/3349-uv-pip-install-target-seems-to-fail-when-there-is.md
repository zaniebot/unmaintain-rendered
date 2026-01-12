```yaml
number: 3349
title: uv pip install --target seems to fail when there is no wheel available on pypi.org
type: issue
state: closed
author: gotcha
labels:
  - bug
assignees: []
created_at: 2024-05-03T14:40:39Z
updated_at: 2024-05-03T23:21:25Z
url: https://github.com/astral-sh/uv/issues/3349
synced_at: 2026-01-12T15:58:43Z
```

# uv pip install --target seems to fail when there is no wheel available on pypi.org

---

_@gotcha_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Thanks a lot for adding the `--target` option to `uv pip install`.

I am using 0.1.39 

```
$ uv venv
$ uv pip install --target bla --no-deps install zope.component
Resolved 1 package in 393ms
Downloaded 1 package in 61ms
Installed 1 package in 2ms
 + zope-component==6.0
$ ls bla
zope  zope.component-6.0-py3.9-nspkg.pth  zope.component-6.0.dist-info
$ #success :-)

$ uv pip install --target bla --no-deps install zope.component==4.1.0
error: Failed to download and build `zope-component==4.1.0`
  Caused by: Failed to build: `zope-component==4.1.0`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'setuptools'
---
$ ls bla
_distutils_hack  distutils-precedence.pth  pkg_resources  setuptools  setuptools-69.5.1.dist-info
```

My shallow knowledge makes me think that the process to produce a wheel locally is failing.

```
$ uv pip install --target bla install zope.component==4.1.0
error: Failed to download and build `zope-component==4.1.0`
  Caused by: Failed to build: `zope-component==4.1.0`
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 8, in <module>
ModuleNotFoundError: No module named 'setuptools'
---
$ ls bla
_distutils_hack  distutils-precedence.pth  pkg_resources  setuptools  setuptools-69.5.1.dist-info
```
The problem is the same with or without the `--no-deps` flag.

Same issue when installing `zest.releaser` which does not have any released wheel.
Even though installing `ipdb` works fine : there is a wheel on pypi.org.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-05-03 15:15_

---

_Label `bug` added by @charliermarsh on 2024-05-03 15:15_

---

_Comment by @charliermarsh on 2024-05-03 15:15_

Thanks, I think this is a real bug in `--target`.

---

_Closed by @charliermarsh on 2024-05-03 23:21_

---
