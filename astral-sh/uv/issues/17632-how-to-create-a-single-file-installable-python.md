```yaml
number: 17632
title: "How to create a single file *installable* python package"
type: issue
state: open
author: ntninja
labels:
  - question
assignees: []
created_at: 2026-01-20T20:11:00Z
updated_at: 2026-01-20T20:14:52Z
url: https://github.com/astral-sh/uv/issues/17632
synced_at: 2026-01-20T20:55:48Z
```

# How to create a single file *installable* python package

---

_@ntninja_

### Question

I have (or rather someone else has) a project with this structure:

```
pip/
    flatpak-pip-generator.py
    pyproject.toml
    readme.my
    uv.lock
```

I tried renaming `flatpak-pip-generator.py` to `flatpak_pip_generator.py` and adding the following to `pyproject.toml` to make it installable as single command using `pipx`:

```diff
diff --git a/pip/pyproject.toml b/pip/pyproject.toml
index 1b6558d..cdba620 100644
--- a/pip/pyproject.toml
+++ b/pip/pyproject.toml
@@ -1,5 +1,9 @@
+[build-system]
+requires = ["uv_build>=0.9.26,<0.10.0"]
+build-backend = "uv_build"
+
 [project]
-name = "flatpak_pip_generator"
+name = "flatpak-pip-generator"
 version = "0.0.1"
 description = "Tool to automatically generate flatpak-builder manifest for pip modules"
 license = {text = "MIT"}
@@ -14,6 +18,9 @@ dependencies = [
 Homepage = "https://github.com/flatpak/flatpak-builder-tools/tree/master/pip"
 Repository = "https://github.com/flatpak/flatpak-builder-tools.git"
 
+[project.scripts]
+flatpak-pip-generator = "flatpak_pip_generator:main"
+
 [dependency-groups]
 dev = [
     "mypy<2.0.0,>=1.11.2",
```

However installation using `pipx` fails since `uv` seems to insist in locating the single script at `src/flatpak_pip_generator/__init__.py`, rather than just `flatpak_pip_generator.py`:

```shell-command
$ pipx install git+file://./home/user/src-ext/flatpak-builder-tools#subdirectory=pip
  error: subprocess-exited-with-error
  
  × Building wheel for flatpak-pip-generator (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [12 lines of output]
      Error: Expected a Python module at: src/flatpak_pip_generator/__init__.py
      
      Stack backtrace:
         0: <unknown>
         1: <unknown>
         2: <unknown>
         3: <unknown>
         4: __libc_start_call_main
                   at ./csu/../sysdeps/nptl/libc_start_call_main.h:58:16
         5: __libc_start_main_impl
                   at ./csu/../csu/libc-start.c:360:3
         6: <unknown>
      [end of output]
  
  note: This error originates from a subprocess, and is likely not a problem with pip.
  ERROR: Failed building wheel for flatpak-pip-generator
error: failed-wheel-build-for-install

× Failed to build installable wheels for some pyproject.toml based projects
╰─> flatpak-pip-generator
Cannot determine package name from spec 'git+file://./home/user/src-ext/flatpak-builder-tools#subdirectory=pip'. Check package spec for
errors.
[1]    2423299 exit 1     pipx install 
```

How does one tell `uv` that the file isn’t inside `src/`?

(Issue #6293 appears to be related, but adding `[tool.hatch.build.targets.wheel]` with `packages = ["."]` to `pyproject.toml` does nothing.)

### Platform

Linux 6.18.2-x64v3-xanmod1 x86_64 GNU/Linux

### Version

None, whatever `uv_build>=0.9.26,<0.10.0` currently pulls in during `pipx` install.

---

_Label `question` added by @ntninja on 2026-01-20 20:11_

---

_Comment by @ntninja on 2026-01-20 20:14_

Notably it *does* work and can be installed and used when moving the single file to `src/flatpak_pip_generator/__init__.py` instead, but this goes against the spirit of the place hosting it *also* as a single downloadable script…

---
