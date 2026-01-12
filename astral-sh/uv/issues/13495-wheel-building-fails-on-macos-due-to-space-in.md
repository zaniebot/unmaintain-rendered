```yaml
number: 13495
title: Wheel building fails on MacOS due to space in virtualenv path
type: issue
state: closed
author: jnns
labels:
  - question
assignees: []
created_at: 2025-05-16T14:44:14Z
updated_at: 2025-08-01T14:49:26Z
url: https://github.com/astral-sh/uv/issues/13495
synced_at: 2026-01-12T16:01:30Z
```

# Wheel building fails on MacOS due to space in virtualenv path

---

_@jnns_

### Summary

I've got these two Python versions installed in different paths. One of them probably installed via uv.

```
uv python list
cpython-3.12.9-macos-aarch64-none  /opt/homebrew/bin/python3.12 -> ../Cellar/python@3.12/3.12.9/bin/python3.12
cpython-3.12.8-macos-aarch64-none  /Users/me/Library/Application Support/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12
``` 

When I try to install `uwsgi` in a virtualenv based on Python 3.12.8 (the one with a space in `Applicaton Support`), it fails:

<details>
<summary>Output of uv pip install uwsgi</summary>

```
  × Failed to build `uwsgi==2.0.27`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stdout]
      running bdist_wheel
      running build
      running build_py
      creating build/lib
      copying uwsgidecorators.py -> build/lib
      installing to build/bdist.macosx-11.0-arm64/wheel
      running install

      [stderr]
      /Users/me/Library/Caches/uv/builds-v0/.tmpguBurS/lib/python3.12/site-packages/setuptools/_distutils/dist.py:289:
      UserWarning: Unknown distribution option: 'descriptions'
        warnings.warn(msg)
      /Users/me/Library/Caches/uv/builds-v0/.tmpguBurS/lib/python3.12/site-packages/setuptools/dist.py:759:
      SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: GNU General Public License v2 or later (GPLv2+)

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      clang: error: no such file or directory: 'Support/uv/python/cpython-3.12.8-macos-aarch64-none/include/python3.12'
      clang: error: no such file or directory: 'Support/uv/python/cpython-3.12.8-macos-aarch64-none/include/python3.12'
      clang: error: no such file or directory: 'Support/uv/python/cpython-3.12.8-macos-aarch64-none/include/python3.12'

      hint: This usually indicates a problem with the package or the build environment.
```

</details>

It works without issues on Python 3.12.9 where there's no space in the path.

### Platform

MacOS 15.4.1

### Version

uv 0.6.17 (8414e9f3d 2025-04-25)

### Python version

3.12.8 / 3.12.9

---

_Label `bug` added by @jnns on 2025-05-16 14:44_

---

_Comment by @konstin on 2025-05-16 14:57_

This is a bug in uwsgi's `setup.py`, in setuptools or clang, not in uv.

---

_Label `bug` removed by @konstin on 2025-05-16 14:57_

---

_Label `question` added by @konstin on 2025-05-16 14:57_

---

_Closed by @charliermarsh on 2025-05-18 21:25_

---

_Comment by @jnns on 2025-08-01 14:49_

For visitors from the future, [here's a suggesetion how to fix it](https://github.com/astral-sh/uv/issues/10412#issuecomment-2578745797).

---
