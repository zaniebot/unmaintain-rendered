```yaml
number: 9390
title: "Doc: clarify \"virtual projects\" static version required[/-]"
type: issue
state: closed
author: mlec1
labels:
  - bug
assignees: []
created_at: 2024-11-23T21:54:59Z
updated_at: 2025-12-10T15:10:32Z
url: https://github.com/astral-sh/uv/issues/9390
synced_at: 2026-01-12T15:59:49Z
```

# Doc: clarify "virtual projects" static version required[/-]

---

_@mlec1_

Hello, 

first thank you for the awesome project.

I just wanted to report maybe an improvement in the doc. I am working on a "virtual project", and wanted to lock, and sync my project without necessary install my project. Here was my `pyproject.toml`:

```
[project]
name = "foo"
dynamic = ["version"]
requires-python = ">= 3.10"
dependencies = [
    "Django[argon2]==4.2.16",
    "django-admin-sortable2==2.2.4",
    "django-recaptcha==4.0.0",
    "django-csp==3.8",
    "elastic-apm==6.23.0",
    "gunicorn==23.0.0",
    "mailjet-rest==1.3.4",
    "Pillow==11.0.0",
    "psycopg==3.2.3",
    "python-decouple==3.8",
    "python-ipware==3.0.0",
    "whitenoise==6.8.2",
]

[...]

[tool.uv]
package = false
```

 I have added the `package = false` under `[tool.uv]`. But I was still getting the following error, showing that uv was still trying to build my project.

```
error: Multiple top-level packages discovered in a flat-layout: ['foo1', 'bar', '...'].

To avoid accidental inclusion of unwanted files or directories,
setuptools will not proceed with this build.

If you are trying to create a single distribution with multiple packages
on purpose, you should not rely on automatic discovery.
Instead, consider the following options:

1. set up custom discovery (`find` directive with `include` or `exclude`)
2. use a `src-layout`
3. explicitly set `py_modules` or `packages` with a list of names

To find more information, look for "package discovery" on setuptools docs.
```

I found out that the issue was because I had `dynamic = ["version"]` and when I switched to a static version it worked perfectly. The version was not a something we care right now, therefore the `dynamic = ["version"]` was the easy solution. Now that I read the documentation of the pyproject.toml, it makes totally sense that uv was trying to build my project:

https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#static-vs-dynamic-metadata
```
When a field is dynamic, it is the build backend’s responsibility to fill it. Consult your build backend’s documentation to learn how it does it.
```

I just wanted to suggest adding this reminder in the documentation on the `package` setting, as it took me a while to figure out, that was the issue. That in order to `package` to work, the version needs to be static :
https://docs.astral.sh/uv/reference/settings/#package

uv lock --verbose:

```
DEBUG uv 0.5.4
DEBUG Found workspace root: `/foo`
DEBUG Adding current workspace member: `/foo`
DEBUG Using Python request `>=3.10` from `requires-python` metadata
DEBUG The virtual environment's Python version satisfies `>=3.10`
DEBUG Using request timeout of 30s
DEBUG No static `pyproject.toml` available for: foo @ file:///foo (PyprojectToml(DynamicField("version")))
DEBUG Acquired lock for `/.cache/uv/sdists-v6/path/ba7e944cd76875a1`
DEBUG Preparing metadata for: foo @ file:///foo
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.10.15
DEBUG Solving with target Python version: >=3.10.15
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG No cache entry for: https://pypi.org/simple/setuptools/
WARN Skipping file for setuptools: setuptools-0.6b1-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6b1-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6b2-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6b2-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6b3-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6b3-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6b4-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6b4-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c1-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c1-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c10-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c10-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c10-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c10-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c10-py2.6.egg
WARN Skipping file for setuptools: setuptools-0.6c10.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c10.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c10.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c10.win32-py2.6.exe
WARN Skipping file for setuptools: setuptools-0.6c11-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c11-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c11-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c11-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c11-py2.6.egg
WARN Skipping file for setuptools: setuptools-0.6c11-py2.7.egg
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.6.exe
WARN Skipping file for setuptools: setuptools-0.6c11.win32-py2.7.exe
WARN Skipping file for setuptools: setuptools-0.6c2-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c2-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c3-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c3-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c3-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c4-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c4-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c4-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c4-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c4.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c4.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c4.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c5-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c5-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c5-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c5-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c5.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c5.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c5.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c6-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c6-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c6-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c6-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c6.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c6.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c6.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c7-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c7-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c7-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c7-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c7.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c7.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c7.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c8-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c8-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c8-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c8-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c8.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c8.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c8.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-0.6c9-1.src.rpm
WARN Skipping file for setuptools: setuptools-0.6c9-py2.3.egg
WARN Skipping file for setuptools: setuptools-0.6c9-py2.4.egg
WARN Skipping file for setuptools: setuptools-0.6c9-py2.5.egg
WARN Skipping file for setuptools: setuptools-0.6c9-py2.6.egg
WARN Skipping file for setuptools: setuptools-0.6c9.win32-py2.3.exe
WARN Skipping file for setuptools: setuptools-0.6c9.win32-py2.4.exe
WARN Skipping file for setuptools: setuptools-0.6c9.win32-py2.5.exe
WARN Skipping file for setuptools: setuptools-18.3.1-py3.4.egg
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==75.6.0 [compatible] (setuptools-75.6.0-py3-none-any.whl)
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/21/47d163f615df1d30c094f6c8bbb353619274edccf0327b185cc2493c2c33/setuptools-75.6.0-py3-none-any.whl.metadata
DEBUG Tried 1 versions: setuptools 1
DEBUG marker environment resolution took 0.118s
DEBUG Installing in setuptools==75.6.0 in /.cache/uv/builds-v0/.tmpAHoXjm
DEBUG Identified uncached distribution: setuptools==75.6.0
DEBUG Downloading and building requirement for build: setuptools==75.6.0
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/21/47d163f615df1d30c094f6c8bbb353619274edccf0327b185cc2493c2c33/setuptools-75.6.0-py3-none-any.whl
DEBUG Installing build requirement: setuptools==75.6.0
DEBUG Creating PEP 517 build environment
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG error: Multiple top-level packages discovered in a flat-layout: ['foo1', 'bar', '...'].
DEBUG 
DEBUG To avoid accidental inclusion of unwanted files or directories,
DEBUG setuptools will not proceed with this build.
DEBUG 
DEBUG If you are trying to create a single distribution with multiple packages
DEBUG on purpose, you should not rely on automatic discovery.
DEBUG Instead, consider the following options:
DEBUG 
DEBUG 1. set up custom discovery (`find` directive with `include` or `exclude`)
DEBUG 2. use a `src-layout`
DEBUG 3. explicitly set `py_modules` or `packages` with a list of names
DEBUG 
DEBUG To find more information, look for "package discovery" on setuptools docs.
DEBUG Released lock at `/.cache/uv/sdists-v6/path/ba7e944cd76875a1/.lock`
error: Failed to generate package metadata for `foo==0.0.0 @ virtual+.`
  Caused by: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

[stderr]
error: Multiple top-level packages discovered in a flat-layout: ['foo1', 'bar', '...'].

To avoid accidental inclusion of unwanted files or directories,
setuptools will not proceed with this build.

If you are trying to create a single distribution with multiple packages
on purpose, you should not rely on automatic discovery.
Instead, consider the following options:

1. set up custom discovery (`find` directive with `include` or `exclude`)
2. use a `src-layout`
3. explicitly set `py_modules` or `packages` with a list of names

To find more information, look for "package discovery" on setuptools docs.
```

uv version: `uv 0.5.4`


---

_Label `documentation` added by @charliermarsh on 2024-11-24 01:54_

---

_Comment by @trymzet on 2025-12-10 13:55_

I had this error and setting `package = false` didn't have any effect for me, but it turned out I was running `uv sync --frozen` and I needed to first re-create the lock file before running `uv sync` again.

---

_Renamed from "Doc: clarify "virtual projects" static version required" to "Disallow `dynamic = ["version"]` with `package = false`" by @konstin on 2025-12-10 15:08_

---

_Label `documentation` removed by @konstin on 2025-12-10 15:08_

---

_Label `bug` added by @konstin on 2025-12-10 15:08_

---

_Renamed from "Disallow `dynamic = ["version"]` with `package = false`" to "[-]Doc: clarify "virtual projects" static version required[/-]" by @konstin on 2025-12-10 15:09_

---

_Renamed from "[-]Doc: clarify "virtual projects" static version required[/-]" to "Doc: clarify "virtual projects" static version required[/-]" by @konstin on 2025-12-10 15:09_

---

_Comment by @konstin on 2025-12-10 15:10_

The behavior reported has since been fixed. For `--frozen`, that's a very different case and should be discussed separately.

---

_Closed by @konstin on 2025-12-10 15:10_

---
