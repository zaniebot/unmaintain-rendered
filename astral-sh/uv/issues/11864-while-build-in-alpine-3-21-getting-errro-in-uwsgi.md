```yaml
number: 11864
title: "while build in alpine:3.21 getting errro in uwsgi==2.0.24"
type: issue
state: closed
author: Anton6896
labels:
  - bug
assignees: []
created_at: 2025-02-28T19:42:12Z
updated_at: 2025-02-28T20:49:23Z
url: https://github.com/astral-sh/uv/issues/11864
synced_at: 2026-01-12T16:00:48Z
```

# while build in alpine:3.21 getting errro in uwsgi==2.0.24

---

_@Anton6896_

### Summary

### while building from `pyproject.toml` in docker container bases system arch,
getting this error.

docker system `FROM alpine:3.21`

- container has
```
RUN apk add git \
  libpq-dev \
  python3-dev \
  sed \
  py3-pip \
  gcc \
  musl-dev \
  libffi-dev \
  libuv-dev \
  libev-dev \
  tiff-dev \
  jpeg-dev \
  openjpeg-dev \
  zlib-dev \
  freetype-dev \
  lcms2-dev \
  libwebp-dev \
  tcl-dev \
  tk-dev \
  harfbuzz-dev \
  fribidi-dev \
  libimagequant-dev \
  libxcb-dev \
  libpng-dev

```
 
- log 
```
× Failed to build `uwsgi==2.0.24`
├─▶ The build backend returned an error
╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit
status: 1)[stdout]
running bdist_wheel
running build
running build_py
creating build/lib
copying uwsgidecorators.py -> build/lib
installing to build/bdist.linux-aarch64/wheel
running install
using profile: buildconf/default.ini
detected include path: ['/usr/include', '/usr/local/include']

[stderr]
/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/_distutils/dist.py:270:
UserWarning: Unknown distribution option: 'descriptions'
warnings.warn(msg)
warning: build_py: byte-compiling is disabled, skipping.

Traceback (most recent call last):
File
"/root/.cache/uv/sdists-v8/pypi/uwsgi/2.0.24/MH2ZjtC8q7nbSqZBCzrK3/src/uwsgiconfig.py",
line 754, in __init__
gcc_version_components = gcc_version.split('.')
^^^^^^^^^^^^^^^^^
AttributeError: 'NoneType' object has no attribute 'split'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
File "<string>", line 11, in <module>
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/build_meta.py",
line 435, in build_wheel
return _build(['bdist_wheel'])
^^^^^^^^^^^^^^^^^^^^^^^
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/build_meta.py",
line 426, in _build
return self._build_with_temp_dir(
^^^^^^^^^^^^^^^^^^^^^^^^^^
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/build_meta.py",
line 407, in _build_with_temp_dir
self.run_setup()
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/build_meta.py",
line 522, in run_setup
super().run_setup(setup_script=setup_script)
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/build_meta.py",
line 320, in run_setup
exec(code, locals())
File "<string>", line 117, in <module>
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/__init__.py",
line 117, in setup
return distutils.core.setup(**attrs)
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/_distutils/core.py",
line 186, in setup
return run_commands(dist)
^^^^^^^^^^^^^^^^^^
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/_distutils/core.py",
line 202, in run_commands
dist.run_commands()
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/_distutils/dist.py",
line 983, in run_commands
self.run_command(cmd)
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/dist.py",
line 999, in run_command
super().run_command(command)
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/_distutils/dist.py",
line 1002, in run_command
cmd_obj.run()
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/_vendor/wheel/bdist_wheel.py",
line 403, in run
self.run_command("install")
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/_distutils/cmd.py",
line 339, in run_command
self.distribution.run_command(command)
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/dist.py",
line 999, in run_command
super().run_command(command)
File
"/root/.cache/uv/builds-v0/.tmpINilSP/lib/python3.12/site-packages/setuptools/_distutils/dist.py",
line 1002, in run_command
cmd_obj.run()
File "<string>", line 77, in run
File
"/root/.cache/uv/sdists-v8/pypi/uwsgi/2.0.24/MH2ZjtC8q7nbSqZBCzrK3/src/uwsgiconfig.py",
line 762, in __init__
raise Exception("you need a C compiler to build uWSGI")
Exception: you need a C compiler to build uWSGI

hint: This usually indicates a problem with the package or the build
environment.
help: `uwsgi` (v2.0.24) was included because `cellosign` (v1.0.1) depends
on `uwsgi` 
```

### Platform

macos 15.3.1

### Version

uv 0.6.3 (Homebrew 2025-02-24)

### Python version

Using CPython 3.12.2

---

_Label `bug` added by @Anton6896 on 2025-02-28 19:42_

---

_Comment by @charliermarsh on 2025-02-28 19:44_

You might need clang? We use clang by default. Alternatively, you can try `CC=gcc`. See https://github.com/astral-sh/uv/issues/6488.

---

_Comment by @Anton6896 on 2025-02-28 20:06_

tnx for helping 
added this 3 
```
build-base 
musl-dev 
linux-headers 
```

---

_Closed by @Anton6896 on 2025-02-28 20:06_

---

_Comment by @charliermarsh on 2025-02-28 20:49_

No prob, thanks for following up.

---
