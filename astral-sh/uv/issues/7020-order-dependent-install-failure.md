```yaml
number: 7020
title: Order-dependent install failure
type: issue
state: closed
author: wence-
labels:
  - question
assignees: []
created_at: 2024-09-04T13:59:23Z
updated_at: 2024-10-23T09:04:06Z
url: https://github.com/astral-sh/uv/issues/7020
synced_at: 2026-01-10T04:45:10Z
```

# Order-dependent install failure

---

_Issue opened by @wence- on 2024-09-04 13:59_

When trying to install `numpy` and `numba` in a venv with python 3.10, it is only possible to do so with `uv` if passing `numba` before `numpy` in the requirements list. In contrast, `pip` does not seem to mind, see transcript:

```
$ python --version
Python 3.10.12
$ pip --version
pip 24.2 from /home/wence/Documents/venvs/py3.10/lib/python3.10/site-packages/pip (python 3.10)
$ uv --version
uv 0.4.4
$ pip install --use-pep517 numpy numba
Collecting numpy
  Downloading numpy-2.1.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (60 kB)
Collecting numba
  Downloading numba-0.60.0-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (2.7 kB)
Collecting llvmlite<0.44,>=0.43.0dev0 (from numba)
  Downloading llvmlite-0.43.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (4.8 kB)
Collecting numpy
  Downloading numpy-2.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (60 kB)
Downloading numba-0.60.0-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (3.7 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.7/3.7 MB 26.4 MB/s eta 0:00:00
Downloading numpy-2.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (19.5 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 19.5/19.5 MB 33.7 MB/s eta 0:00:00
Downloading llvmlite-0.43.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (43.9 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 43.9/43.9 MB 36.4 MB/s eta 0:00:00
Installing collected packages: numpy, llvmlite, numba
Successfully installed llvmlite-0.43.0 numba-0.60.0 numpy-2.0.2
$ uv pip uninstall numpy numba llvmlite
Uninstalled 3 packages in 85ms
 - llvmlite==0.43.0
 - numba==0.60.0
 - numpy==2.0.2
$ pip install --use-pep517 numba numpy
Collecting numba
  Using cached numba-0.60.0-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl.metadata (2.7 kB)
Collecting numpy
  Using cached numpy-2.1.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (60 kB)
Collecting llvmlite<0.44,>=0.43.0dev0 (from numba)
  Using cached llvmlite-0.43.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (4.8 kB)
Collecting numpy
  Using cached numpy-2.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl.metadata (60 kB)
Using cached numba-0.60.0-cp310-cp310-manylinux2014_x86_64.manylinux_2_17_x86_64.whl (3.7 MB)
Using cached numpy-2.0.2-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (19.5 MB)
Using cached llvmlite-0.43.0-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (43.9 MB)
Installing collected packages: numpy, llvmlite, numba
Successfully installed llvmlite-0.43.0 numba-0.60.0 numpy-2.0.2
$ uv pip uninstall numpy numba llvmlite
Uninstalled 3 packages in 85ms
 - llvmlite==0.43.0
 - numba==0.60.0
 - numpy==2.0.2
$ uv pip install numba numpy
Resolved 3 packages in 84ms
Installed 3 packages in 26ms
 + llvmlite==0.43.0
 + numba==0.60.0
 + numpy==2.0.2
$ uv pip uninstall numba numpy llvmlite
Uninstalled 3 packages in 85ms
 - llvmlite==0.43.0
 - numba==0.60.0
 - numpy==2.0.2
$ uv pip install numpy numba
Resolved 4 packages in 22ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: numba==0.53.1
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/wence/.cache/uv/builds-v0/.tmplaOLv8/lib/python3.10/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
  File "/home/wence/.cache/uv/builds-v0/.tmplaOLv8/lib/python3.10/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/home/wence/.cache/uv/builds-v0/.tmplaOLv8/lib/python3.10/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/wence/.cache/uv/builds-v0/.tmplaOLv8/lib/python3.10/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 50, in <module>
  File "<string>", line 47, in _guard_py_ver
RuntimeError: Cannot install on Python version 3.10.12; only versions >=3.6,<3.10 are supported.
---
```

<details>

<summary> <code>uv pip install numba numpy -vvv</code> output </summary>

[numba-numpy.log](https://github.com/user-attachments/files/16871696/numba-numpy.log)


</details>


<details>

<summary> <code>uv pip install numpy numba -vvv</code> output </summary>

[numpy-numba.log](https://github.com/user-attachments/files/16871698/numpy-numba.log)


</details>

---

_Comment by @charliermarsh on 2024-09-04 14:05_

It's nuanced but this looks correct to me. If you pass `numba` first, we prioritize getting the latest version of `numba` (`0.60.0`) and then find a compatible version of NumPy (`2.0.2`). If you pass `numpy` first, we prioritize getting the last version of `numpy` (probably `2.1.1`), and then find a compatible version of Numba (presumedly `0.53.1`). However, that version of Numba has a runtime guard disallowing later versions of Python. There's no way for us to know that from the package metadata, though.

---

_Comment by @zanieb on 2024-09-04 14:43_

See also https://docs.astral.sh/uv/pip/compatibility/#package-priority

---

_Label `question` added by @zanieb on 2024-09-04 14:43_

---

_Comment by @wence- on 2024-09-04 15:26_

Ah, thanks.

> and then find a compatible version of Numba (presumedly 0.53.1)

I think this is just where the backtracking through numba versions failed (there's no numba version that is compatible with numpy 2.1.1). I guess the issue is that numba 0.53.1 [didn't advertise an upper-bound pin on numpy](https://github.com/numba/numba/blob/release0.53/setup.py#L367) so uv must assume that it would work.

If I restrict the backtracking to stop at 0.54, then I get the same install as if numba is specified first:

```
$ uv pip install numpy 'numba>=0.54'
Installed 3 packages in 23ms
 + llvmlite==0.43.0
 + numba==0.60.0
 + numpy==2.0.2
```

Happy to close, and we'll update our lower-bound pinning for numba.

---

_Comment by @zanieb on 2024-09-04 15:42_

Thanks! Eventually we may be able to explore better heuristics here but it's very challenging.

---

_Closed by @zanieb on 2024-09-04 15:42_

---
