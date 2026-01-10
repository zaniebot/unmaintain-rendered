```yaml
number: 16589
title: Spurious lock failure combining no-build with env specifications
type: issue
state: closed
author: ncoghlan
labels:
  - question
assignees: []
created_at: 2025-11-04T13:38:07Z
updated_at: 2025-11-04T19:22:38Z
url: https://github.com/astral-sh/uv/issues/16589
synced_at: 2026-01-10T03:23:55Z
```

# Spurious lock failure combining no-build with env specifications

---

_Issue opened by @ncoghlan on 2025-11-04 13:38_

Resolution: `platform_machine` `AMD64` tag needs to be uppercase on Windows due to the quirks of how the `platform.machine()` API works (`arm64` is still lowercase, though).

### Summary

The following project file fails to lock for me with the target versioned pinned to 3.11.* or 3.12.* (it resolves if pinned to 3.13.*):

```
$ cat pyproject.toml
[project]
name = "env-requirements"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.11.*"
dependencies = [
    "tornado",
]

[tool.uv]
# exclude-newer = "2025-11-02T00:00:00Z"
no-build = true
# constraint-dependencies = []
environments = [
    # "sys_platform == 'linux' and platform_machine == 'x86_64'",
    # "sys_platform == 'darwin' and platform_machine == 'arm64'",
    "sys_platform == 'win32' and platform_machine == 'amd64'",
]
required-environments = [
    # "sys_platform == 'linux' and platform_machine == 'x86_64'",
    # "sys_platform == 'darwin' and platform_machine == 'arm64'",
    "sys_platform == 'win32' and platform_machine == 'amd64'",
]


[[tool.uv.cache-keys]]
file = "pyproject.toml"
```
(the commented out lines are because this failure is a simplified version of the original failure, which arose in an attempt to convert the platform tags in `venvstacks` layer definitions into `uv` target environment specifications).

This gives the result:

```
$ uv lock
Using CPython 3.11.13 interpreter at: /usr/bin/python3.11
  × No solution found when resolving dependencies for split (markers: platform_machine == 'amd64' and sys_platform
  │ == 'win32'):
  ╰─▶ Because only the following versions of tornado are available:
          tornado==0.2
          tornado==1.0
          tornado==1.1
          tornado==1.1.1
          tornado==1.2
          tornado==1.2.1
          tornado==2.0
          tornado==2.1
          tornado==2.1.1
          tornado==2.2
          tornado==2.2.1
          tornado==2.3
          tornado==2.4
          tornado==2.4.1
          tornado==3.0
          tornado==3.0.1
          tornado==3.0.2
          tornado==3.1
          tornado==3.1.1
          tornado==3.2
          tornado==3.2.1
          tornado==3.2.2
          tornado==4.0
          tornado==4.0.1
          tornado==4.0.2
          tornado==4.1
          tornado==4.2
          tornado==4.2.1
          tornado==4.3
          tornado==4.4
          tornado==4.4.1
          tornado==4.4.2
          tornado==4.4.3
          tornado==4.5
          tornado==4.5.1
          tornado==4.5.2
          tornado==4.5.3
          tornado==5.0
          tornado==5.0.1
          tornado==5.0.2
          tornado==5.1
          tornado==5.1.1
          tornado==6.0
          tornado==6.0.1
          tornado==6.0.2
          tornado==6.0.3
          tornado==6.0.4
          tornado==6.1
          tornado==6.2
          tornado==6.3
          tornado==6.3.1
          tornado==6.3.2
          tornado==6.3.3
          tornado==6.4
          tornado==6.4.1
          tornado==6.4.2
          tornado==6.5
          tornado==6.5.1
          tornado==6.5.2
      and tornado<=6.1 has no usable wheels, we can conclude that tornado<=6.1 cannot be used.
      And because tornado>=6.2 has no `platform_machine == 'amd64' and sys_platform == 'win32'`-compatible wheels and
      your project depends on tornado, we can conclude that your project's requirements are unsatisfiable.

      hint: Pre-releases are available for `tornado` in the requested range (e.g., 6.5b1), but pre-releases weren't
      enabled (try: `--prerelease=allow`)

      hint: Wheels are required for `tornado` because building from source is disabled for all packages (i.e., with
      `--no-build`)

      hint: The resolution failed for an environment that is not the current one, consider limiting the environments
      with `tool.uv.environments`.
```

The reason we know the failure to find a wheel is spurious is because targetting 3.13 or omitting the no-build requirement gives a lock file that refers to a PyPI provided wheel:

```
$ uv lock
Using CPython 3.11.13 interpreter at: /usr/bin/python3.11
Resolved 2 packages in 3ms
acoghlan@TerminalMist:~/devel/_misc/uv_experiments/env_requirements$ cat uv.lock
version = 1
revision = 3
requires-python = "==3.11.*"
resolution-markers = [
    "platform_machine == 'amd64' and sys_platform == 'win32'",
]
supported-markers = [
    "platform_machine == 'amd64' and sys_platform == 'win32'",
]
required-markers = [
    "platform_machine == 'amd64' and sys_platform == 'win32'",
]

[[package]]
name = "env-requirements"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "tornado", marker = "platform_machine == 'amd64' and sys_platform == 'win32'" },
]

[package.metadata]
requires-dist = [{ name = "tornado" }]

[[package]]
name = "tornado"
version = "6.5.2"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/09/ce/1eb500eae19f4648281bb2186927bb062d2438c2e5093d1360391afd2f90/tornado-6.5.2.tar.gz", hash = "sha256:ab53c8f9a0fa351e2c0741284e06c7a45da86afb544133201c5cc8578eb076a0", size = 510821, upload-time = "2025-08-08T18:27:00.78Z" }
wheels = [
    { url = "https://files.pythonhosted.org/packages/c7/2a/f609b420c2f564a748a2d80ebfb2ee02a73ca80223af712fca591386cafb/tornado-6.5.2-cp39-abi3-win_amd64.whl", hash = "sha256:e56a5af51cc30dd2cae649429af65ca2f6571da29504a07995175df14c18f35f", size = 445427, upload-time = "2025-08-08T18:26:57.91Z" },
]
```

### Platform

Fedora 41 (locking for Linux, Windows, and macOS)

### Version

uv 0.9.7

### Python version

Locking for Python 3.11

---

_Label `bug` added by @ncoghlan on 2025-11-04 13:38_

---

_Comment by @konstin on 2025-11-04 15:03_

It fails for me too with 3.13.*, but it succeeds with `platform_machine == 'AMD64'` (uppercase).

---

_Label `bug` removed by @konstin on 2025-11-04 15:03_

---

_Label `question` added by @konstin on 2025-11-04 15:03_

---

_Comment by @ncoghlan on 2025-11-04 15:21_

Ah, interesting, this is a scenario where `sysconfig.get_platform()` (lowercase everywhere) and `platform.machine()` (uppercase on Windows) differ.

Annoyingly, that means we (in the "Python packaging ecosystem" sense) don't have *any* explicit documentation on how to check CPU architecture in environment markers in a cross-platform way (https://packaging.python.org/en/latest/specifications/dependency-specifiers/#environment-markers defers to https://docs.python.org/3/library/platform.html#platform.machine which says "The output is platform-dependent and may differ in casing and naming conventions.")

---

_Comment by @konstin on 2025-11-04 15:33_

An intertwined problem is that wheel tags and PEP 508 environment markers use different terms and slightly different semantics, the required environments are an attempt at working around that.

---

_Comment by @ncoghlan on 2025-11-04 16:03_

Anyway, I'm happy to close this one as not-a-bug, as I changed the original triggering case to make the machine tags uppercase in the Windows markers, and it then locked as expected.

---

_Closed by @ncoghlan on 2025-11-04 16:03_

---

_Comment by @ncoghlan on 2025-11-04 17:14_

@konstin After a bit more experimentation, I found that `arm64` needed to stay as lowercase for the `win32` resolution to work.

---

_Comment by @konstin on 2025-11-04 19:22_

The marker values are unfortunately very inconsistent, they are defined by the specific platform port of Python and packaging is only reading the values that CPython defines.

---
