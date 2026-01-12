```yaml
number: 12482
title: "`uv pip install` fails when `pip install` and `uv sync` succeed"
type: issue
state: closed
author: ma-sadeghi
labels:
  - bug
assignees: []
created_at: 2025-03-26T10:42:43Z
updated_at: 2025-03-27T21:37:40Z
url: https://github.com/astral-sh/uv/issues/12482
synced_at: 2026-01-12T16:01:04Z
```

# `uv pip install` fails when `pip install` and `uv sync` succeed

---

_@ma-sadeghi_

### Summary

`uv pip install` fails to install the package [`poromics`](https://github.com/ma-sadeghi/poromics) when pip install and uv sync succeed.

### Potential cause

Looking at the logs, `uv pip install` prefers `numba==0.53.1`, which is not compatible with Python 3.12, whereas the other two approaches tend to prefer the latest `numba==0.61.0`, which is compatible.

### How to reproduce

```shell
mkdir temp
cd temp
uv venv --python 3.12
uv pip install poromics
```

Outputs:

```python
PS C:\Users\sadegmo\Code\temp> uv pip install poromics
Resolved 96 packages in 3.32s
      Built docrep==0.3.2
  × Failed to build `llvmlite==0.36.0`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit code: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 14, in <module>
        File
      "C:\Users\sadegmo\AppData\Local\uv\cache\builds-v0\.tmpxiAXiB\Lib\site-packages\setuptools\build_meta.py",
      line 334, in get_requires_for_build_wheel
          return self._get_build_requires(config_settings, requirements=[])
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File
      "C:\Users\sadegmo\AppData\Local\uv\cache\builds-v0\.tmpxiAXiB\Lib\site-packages\setuptools\build_meta.py",
      line 304, in _get_build_requires
          self.run_setup()
        File
      "C:\Users\sadegmo\AppData\Local\uv\cache\builds-v0\.tmpxiAXiB\Lib\site-packages\setuptools\build_meta.py",
      line 522, in run_setup
          super().run_setup(setup_script=setup_script)
        File
      "C:\Users\sadegmo\AppData\Local\uv\cache\builds-v0\.tmpxiAXiB\Lib\site-packages\setuptools\build_meta.py",
      line 320, in run_setup
          exec(code, locals())
        File "<string>", line 55, in <module>
        File "<string>", line 52, in _guard_py_ver
      RuntimeError: Cannot install on Python version 3.12.8; only versions >=3.6,<3.10 are supported.

      hint: This usually indicates a problem with the package or the build environment.
  help: `llvmlite` (v0.36.0) was included because `poromics` (v0.0.1) depends on `porespy` (v2.4.2) which depends
        on `numba` (v0.53.1) which depends on `llvmlite`
PS C:\Users\sadegmo\Code\temp> uv run python                                                                        Python 3.12.8 (main, Dec  6 2024, 18:10:20) [MSC v.1942 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
```

```shell
mkdir temp
cd temp
uv venv --python 3.12
uv pip install pip
.\.venv\Scripts\activate
pip install poromics
```

Succeeds.

```shell
git clone https://github.com/ma-sadeghi/poromics
cd poromics
uv sync
```

Also succeeds.

### Platform

Ubuntu 22.04 amd64, Windows 10 x86_64

### Version

uv 0.6.10

### Python version

python 3.12.8

---

_Label `bug` added by @ma-sadeghi on 2025-03-26 10:42_

---

_Renamed from "`uv pip install` fails where `pip install` and `uv sync` succeed" to "`uv pip install` fails when `pip install` and `uv sync` succeed" by @ma-sadeghi on 2025-03-26 10:42_

---

_Comment by @charliermarsh on 2025-03-26 12:53_

I believe this is the same as https://github.com/astral-sh/uv/issues/12060.

---

_Closed by @charliermarsh on 2025-03-26 12:53_

---

_Comment by @charliermarsh on 2025-03-26 12:54_

In the meantime, you can solve this by adding a constraints file to ensure you get a sufficiently recent `numba` and `llvmlite`.

---
