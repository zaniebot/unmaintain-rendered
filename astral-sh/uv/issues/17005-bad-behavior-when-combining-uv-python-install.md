---
number: 17005
title: "bad behavior when combining `UV_PYTHON_INSTALL_MIRROR` and `--python-downloads-json-url`"
type: issue
state: open
author: MeitarR
labels:
  - bug
assignees: []
created_at: 2025-12-05T21:16:20Z
updated_at: 2025-12-05T21:17:12Z
url: https://github.com/astral-sh/uv/issues/17005
synced_at: 2026-01-10T01:26:12Z
---

# bad behavior when combining `UV_PYTHON_INSTALL_MIRROR` and `--python-downloads-json-url`

---

_Issue opened by @MeitarR on 2025-12-05 21:16_

### Summary

`UV_PYTHON_INSTALL_MIRROR` was first. Then we added `python-downloads-json-url`.

If the URL is modified in the provided JSON **and** `UV_PYTHON_INSTALL_MIRROR` is also provided, there is an error.

```sh
➜   ✗ cat ./a.json
{
  "cpython-3.13.5-linux-x86_64-gnu": {
    "name": "cpython",
    "arch": {
      "family": "x86_64",
      "variant": null
    },
    "os": "linux",
    "libc": "gnu",
    "major": 3,
    "minor": 13,
    "patch": 5,
    "prerelease": "",
    "url": "https://internal.com/20250723/cpython-3.13.5%2B20250723-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz",
    "sha256": "f7ac16748be2674ec14df532a3e48d0c6f215017b537f14ca3feb837dbe86292",
    "variant": null,
    "build": "20250723"
  }
}
➜  ✗ UV_PYTHON_INSTALL_MIRROR=https://asdasd.com/ uv python install 3.13.5 --python-downloads-json-url ./a.json -vv
DEBUG uv 0.9.15
TRACE Checking lock for `/home/meitar/.local/share/uv/python` at `/home/meitar/.local/share/uv/python/.lock`
DEBUG Acquired lock for `/home/meitar/.local/share/uv/python`
TRACE Found existing installation cpython-3.14.0-linux-x86_64-gnu
TRACE Found existing installation cpython-3.13.2-linux-x86_64-gnu
TRACE Found existing installation cpython-3.13.1-linux-x86_64-gnu
TRACE Found existing installation cpython-3.12.2-linux-x86_64-gnu
TRACE Found existing installation cpython-3.11.12-linux-x86_64-gnu
TRACE Found existing installation cpython-3.9.21-linux-x86_64-gnu
DEBUG Using request timeout of 30s
TRACE Found `ld` path: /lib64/ld-linux-x86-64.so.2
TRACE stdout output from `ld`: ""
TRACE stderr output from `ld`: "/lib64/ld-linux-x86-64.so.2: missing program name\nTry '/lib64/ld-linux-x86-64.so.2 --help' for more information.\n"
TRACE Tried to find musl version by running `"/lib64/ld-linux-x86-64.so.2"`, but failed: Could not find musl version in output of: `/lib64/ld-linux-x86-64.so.2`
TRACE Tried to find libc version from possible symlink at "/lib64/ld-linux-x86-64.so.2", but failed: Failed to find glibc version in the filename of linker: `../lib/x86_64-linux-gnu/ld-linux-x86-64.so.2`
TRACE stdout output from `ld.so --version`: "ld.so (Ubuntu GLIBC 2.39-0ubuntu8.6) stable release version 2.39.\nCopyright (C) 2024 Free Software Foundation, Inc.\nThis is free software; see the source for copying conditions.\nThere is NO warranty; not even for MERCHANTABILITY or FITNESS FOR A\nPARTICULAR PURPOSE.\n"
TRACE Found manylinux 2.39 in stdout of ld.so version
DEBUG No installation found for request `3.13.5 (cpython-3.13.5-linux-x86_64-gnu)`
DEBUG Found download `cpython-3.13.5-linux-x86_64-gnu` for request `3.13.5 (cpython-3.13.5-linux-x86_64-gnu)`
TRACE Considering retry of error: Mirror("UV_PYTHON_INSTALL_MIRROR", "https://internal.com/20250723/cpython-3.13.5%2B20250723-x86_64-unknown-linux-gnu-install_only_stripped.tar.gz")
TRACE Cannot retry error: Neither an IO error nor a reqwest error
error: Failed to install cpython-3.13.5-linux-x86_64-gnu
  Caused by: A mirror was provided via `UV_PYTHON_INSTALL_MIRROR`, but the URL does not match the expected format: UV_PYTHON_INSTALL_MIRROR
DEBUG Released lock at `/home/meitar/.local/share/uv/python/.lock`
```

Now I saw people leave the URL in the json as the original and use the `UV_PYTHON_INSTALL_MIRROR` to set the real URL in the offline environment.

I think that this is not the intended way, and probably `python-downloads-json-url` should take precedence over `UV_PYTHON_INSTALL_MIRROR` (and the value of `UV_PYTHON_INSTALL_MIRROR` should be ignored with a warning)

### Platform

-

### Version

0.9.15

### Python version

_No response_

---

_Label `bug` added by @MeitarR on 2025-12-05 21:16_

---
