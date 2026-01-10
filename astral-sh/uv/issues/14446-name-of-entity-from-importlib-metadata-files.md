---
number: 14446
title: "Name of entity from `importlib.metadata.files` inconsistent between win and linux platforms"
type: issue
state: open
author: anmyachev
labels:
  - compatibility
  - external
assignees: []
created_at: 2025-07-03T17:09:46Z
updated_at: 2025-11-17T17:02:06Z
url: https://github.com/astral-sh/uv/issues/14446
synced_at: 2026-01-10T01:25:45Z
---

# Name of entity from `importlib.metadata.files` inconsistent between win and linux platforms

---

_Issue opened by @anmyachev on 2025-07-03 17:09_

This issue was originally posted in https://github.com/intel/intel-xpu-backend-for-triton/issues/4465

**I would expect `{str(f.name)}` to return the same as `{f.locate().name}")`.**

From @e87tn95h:

I have found that the difference come from using `uv pip` instead of using `pip` or `python -m pip` to install packages. So, it's a magic from pyenv.

However, I would say that it's not issue of `uv pip` because this behavior allows in "Recording installed projects" context which is metadata source.

> On Windows, directories may be separated either by forward- or backslashes (`/` or `\`).

Ref: https://packaging.python.org/en/latest/specifications/recording-installed-packages/#the-record-file

### How to

0. Get astral-sh/uv: https://docs.astral.sh/uv/

```
winget install --id=astral-sh.uv --exact
```

1. Make virtualenv explicitly. If have not installed specific version python, uv will download it automatically and put local without breaking global environment.

```
PS C:\sandbox> uv venv --python 3.11.13 test-uvpip
Using CPython 3.11.13
Creating virtual environment at: test-uvpip
Activate with: test-uvpip\Scripts\activate
```

2. Activate and install `intel-sycl-rt` with `uv pip`

```
PS C:\sandbox> test-uvpip\Scripts\activate

(test-uvpip) PS C:\sandbox> uv pip install "intel-sycl-rt==2025.0.5"
Using Python 3.11.13 environment at: test-uvpip
Resolved 6 packages in 510ms
Installed 6 packages in 3.07s
 + intel-cmplr-lib-rt==2025.0.5
 + intel-cmplr-lib-ur==2025.0.5
 + intel-cmplr-lic-rt==2025.0.5
 + intel-sycl-rt==2025.0.5
 + tcmlib==1.2.0
 + umf==0.9.1
```

3. Run python and testcase

```
(test-uvpip) PS C:\sandbox> uv run python
Python 3.11.13 (main, Jun 12 2025, 12:41:34) [MSC v.1943 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> # libsycl_check.py
>>> import importlib.metadata
>>>
>>> def test():
...     for f in importlib.metadata.files("intel-sycl-rt"):
...         for name in ["sycl.hpp", "libsycl.so", "sycl8.dll", "sycl8.lib"]:
...             if name in str(f):
...                 print(f"{str(f)}  {str(f.name)}  {f.locate().name}")
...                 break
...
>>> if __name__ == "__main__":
...     test()
...
..\..\Library\bin\sycl8.dll  ..\..\Library\bin\sycl8.dll  sycl8.dll
..\..\Library\include\sycl\CL\sycl.hpp  ..\..\Library\include\sycl\CL\sycl.hpp  sycl.hpp
..\..\Library\include\sycl\sycl.hpp  ..\..\Library\include\sycl\sycl.hpp  sycl.hpp
..\..\Library\lib\sycl8.lib  ..\..\Library\lib\sycl8.lib  sycl8.lib
```

---

_Referenced in [intel/intel-xpu-backend-for-triton#4465](../../intel/intel-xpu-backend-for-triton/issues/4465.md) on 2025-07-03 17:10_

---

_Comment by @zanieb on 2025-07-03 17:14_

This bit from the linked issue seems critical

> The result of [calling .name (second field from your output)] contradicts the documentation. https://docs.python.org/3/library/pathlib.html#pathlib.PurePath.name. Namely: A string representing the final path component, excluding the drive and root, if any.
>
> That is, it should be like sycl8.dll and not like this:  ..\..\Library\bin\sycl8.dll. Do you agree with this?

---

_Label `compatibility` added by @zanieb on 2025-07-03 17:15_

---

_Referenced in [pytorch/pytorch#156303](../../pytorch/pytorch/issues/156303.md) on 2025-07-21 13:40_

---

_Comment by @anmyachev on 2025-11-17 11:53_

Hi @zanieb, are there any plans to fix this in the next sprints?

---

_Referenced in [python/importlib_metadata#528](../../python/importlib_metadata/issues/528.md) on 2025-11-17 16:12_

---

_Comment by @konstin on 2025-11-17 16:12_

This is not a uv bug, but in `importlib.metadata`: https://github.com/python/importlib_metadata/issues/528

---

_Label `external` added by @konstin on 2025-11-17 16:13_

---

_Comment by @anmyachev on 2025-11-17 17:02_

> This is not a uv bug, but in `importlib.metadata`: [python/importlib_metadata#528](https://github.com/python/importlib_metadata/issues/528)

@konstin thanks for further research!

---
