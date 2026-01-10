---
number: 7078
title: "Failed to fetch wheel: nuitka==2.4.8"
type: issue
state: closed
author: ikreva
labels:
  - question
assignees: []
created_at: 2024-09-05T11:51:30Z
updated_at: 2024-09-06T04:10:50Z
url: https://github.com/astral-sh/uv/issues/7078
synced_at: 2026-01-10T01:24:10Z
---

# Failed to fetch wheel: nuitka==2.4.8

---

_Issue opened by @ikreva on 2024-09-05 11:51_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
uv --version
uv 0.4.5 (42b6bfbad 2024-09-04)

platform winodws 10 x64 22h2 19045.4780

python --version
Python 3.12.5

uv add nuitka -v

DEBUG uv 0.4.5
DEBUG Found project root: `C:\Users\Administrator\Desktop\test`
DEBUG No workspace root found, using project root
DEBUG Reading requests from `C:\Users\Administrator\Desktop\test\.python-version`
DEBUG Searching for Python 3.12 in managed installations, system path, or `py` launcher
DEBUG Searching for managed installations at `D:\Develop\ToolChain\Python\python`
DEBUG Found managed installation `cpython-3.12.5-windows-x86_64-none`
DEBUG Found `cpython-3.12.5-windows-x86_64-none` at `D:\Develop\ToolChain\Python\python\cpython-3.12.5-windows-x86_64-none\python.exe` (managed installations)
Using Python 3.12.5
Creating virtualenv at: .venv
DEBUG Using request timeout of 30s
DEBUG Using request timeout of 30s
DEBUG Starting clean resolution
DEBUG Acquired lock for `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\path\809634ffc419e06c`
DEBUG Found static `pyproject.toml` for: test @ file:///C:/Users/Administrator/Desktop/test
DEBUG No workspace root found, using project root
DEBUG Released lock at `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\path\809634ffc419e06c\.lock`
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: test*
DEBUG Searching for a compatible version of test @ file:///C:/Users/Administrator/Desktop/test (*)
DEBUG Adding transitive dependency for test==0.1.0: nuitka*
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/simple/nuitka/
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/simple/nuitka/
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/simple/nuitka/
DEBUG Searching for a compatible version of nuitka (*)
DEBUG Selecting: nuitka==2.4.8 [compatible] (Nuitka-2.4.8.tar.gz)
DEBUG Acquired lock for `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\index\bf49d59dbdbde53d\nuitka\2.4.8`
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/packages/a5/ab/8f7ba76aa1ceba2339873bf56f8151a7c93b80d635b63900b4be3fe37139/Nuitka-2.4.8.tar.gz
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/packages/a5/ab/8f7ba76aa1ceba2339873bf56f8151a7c93b80d635b63900b4be3fe37139/Nuitka-2.4.8.tar.gz
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/packages/a5/ab/8f7ba76aa1ceba2339873bf56f8151a7c93b80d635b63900b4be3fe37139/Nuitka-2.4.8.tar.gz
DEBUG No static `pyproject.toml` available for: nuitka==2.4.8 (PyprojectToml(FieldNotFound("project")))
DEBUG No static `PKG-INFO` available for: nuitka==2.4.8 (PkgInfo(UnsupportedMetadataVersion("2.1")))
DEBUG Found static `egg-info` for: nuitka==2.4.8
DEBUG Released lock at `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\index\bf49d59dbdbde53d\nuitka\2.4.8\.lock`
DEBUG Adding transitive dependency for nuitka==2.4.8: ordered-set>=4.1.0
DEBUG Adding transitive dependency for nuitka==2.4.8: zstandard>=0.15
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/simple/ordered-set/
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/simple/ordered-set/
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/simple/zstandard/
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/simple/zstandard/
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/simple/ordered-set/
DEBUG Searching for a compatible version of ordered-set (>=4.1.0)
DEBUG Selecting: ordered-set==4.1.0 [compatible] (ordered_set-4.1.0-py3-none-any.whl)
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/packages/33/55/af02708f230eb77084a299d7b08175cff006dea4f2721074b92cdb0296c0/ordered_set-4.1.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/packages/33/55/af02708f230eb77084a299d7b08175cff006dea4f2721074b92cdb0296c0/ordered_set-4.1.0-py3-none-any.whl
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/packages/33/55/af02708f230eb77084a299d7b08175cff006dea4f2721074b92cdb0296c0/ordered_set-4.1.0-py3-none-any.whl
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/simple/zstandard/
DEBUG Searching for a compatible version of zstandard (>=0.15)
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/packages/7b/83/f23338c963bd9de687d47bf32efe9fd30164e722ba27fb59df33e6b1719b/zstandard-0.23.0-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/packages/7b/83/f23338c963bd9de687d47bf32efe9fd30164e722ba27fb59df33e6b1719b/zstandard-0.23.0-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Selecting: zstandard==0.23.0 [compatible] (zstandard-0.23.0-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/packages/7b/83/f23338c963bd9de687d47bf32efe9fd30164e722ba27fb59df33e6b1719b/zstandard-0.23.0-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Adding transitive dependency for zstandard==0.23.0: cffi{platform_python_implementation == 'PyPy'}>=1.11
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/simple/cffi/
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/simple/cffi/
DEBUG Found modified response for: https://mirrors.ustc.edu.cn/pypi/simple/cffi/
DEBUG Searching for a compatible version of cffi{platform_python_implementation == 'PyPy'} (>=1.11)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for cffi==1.17.1: cffi==1.17.1
DEBUG Adding transitive dependency for cffi==1.17.1: cffi{platform_python_implementation == 'PyPy'}==1.17.1
DEBUG Searching for a compatible version of cffi{platform_python_implementation == 'PyPy'} (==1.17.1)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/packages/5a/84/e94227139ee5fb4d600a7a4927f322e1d4aea6fdc50bd3fca8493caba23f/cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/packages/5a/84/e94227139ee5fb4d600a7a4927f322e1d4aea6fdc50bd3fca8493caba23f/cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Found modified response for: https://mirrors.ustc.edu.cn/pypi/packages/5a/84/e94227139ee5fb4d600a7a4927f322e1d4aea6fdc50bd3fca8493caba23f/cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
DEBUG Searching for a compatible version of cffi (==1.17.1)
DEBUG Selecting: cffi==1.17.1 [compatible] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/simple/pycparser/
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/simple/pycparser/
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/simple/pycparser/
DEBUG Searching for a compatible version of pycparser (*)
DEBUG Selecting: pycparser==2.22 [compatible] (pycparser-2.22-py3-none-any.whl)
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl
DEBUG Tried 6 versions: cffi 1, nuitka 1, ordered-set 1, pycparser 1, test 1, zstandard 1
DEBUG Split universal resolution took 8.206s
Resolved 6 packages in 8.21s
DEBUG Using request timeout of 30s
DEBUG Acquired lock for `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\path\809634ffc419e06c`
DEBUG Found static `pyproject.toml` for: test @ file:///C:/Users/Administrator/Desktop/test
DEBUG No workspace root found, using project root
DEBUG Released lock at `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\path\809634ffc419e06c\.lock`
DEBUG Ignoring existing lockfile due to mismatched `requires-dist` for: `test==0.1.0`
  Expected: {Requirement { name: PackageName("nuitka"), extras: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "2.4.8" }]), index: None }, origin: None }}
  Actual: {Requirement { name: PackageName("nuitka"), extras: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None }, origin: None }}
DEBUG Starting clean resolution
DEBUG Acquired lock for `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\path\809634ffc419e06c`
DEBUG Found static `pyproject.toml` for: test @ file:///C:/Users/Administrator/Desktop/test
DEBUG No workspace root found, using project root
DEBUG Released lock at `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\path\809634ffc419e06c\.lock`
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12
DEBUG Adding direct dependency: test*
DEBUG Searching for a compatible version of test @ file:///C:/Users/Administrator/Desktop/test (*)
DEBUG Adding transitive dependency for test==0.1.0: nuitka>=2.4.8
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/simple/nuitka/
DEBUG Searching for a compatible version of nuitka (>=2.4.8)
DEBUG Selecting: nuitka==2.4.8 [preference] (Nuitka-2.4.8.tar.gz)
DEBUG Acquired lock for `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\index\bf49d59dbdbde53d\nuitka\2.4.8`
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/packages/a5/ab/8f7ba76aa1ceba2339873bf56f8151a7c93b80d635b63900b4be3fe37139/Nuitka-2.4.8.tar.gz
DEBUG No static `pyproject.toml` available for: nuitka==2.4.8 (PyprojectToml(FieldNotFound("project")))
DEBUG No static `PKG-INFO` available for: nuitka==2.4.8 (PkgInfo(UnsupportedMetadataVersion("2.1")))
DEBUG Found static `egg-info` for: nuitka==2.4.8
DEBUG Released lock at `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\index\bf49d59dbdbde53d\nuitka\2.4.8\.lock`
DEBUG Adding transitive dependency for nuitka==2.4.8: ordered-set>=4.1.0
DEBUG Adding transitive dependency for nuitka==2.4.8: zstandard>=0.15
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/simple/ordered-set/
DEBUG Searching for a compatible version of ordered-set (>=4.1.0)
DEBUG Selecting: ordered-set==4.1.0 [preference] (ordered_set-4.1.0-py3-none-any.whl)
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/packages/33/55/af02708f230eb77084a299d7b08175cff006dea4f2721074b92cdb0296c0/ordered_set-4.1.0-py3-none-any.whl
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/simple/zstandard/
DEBUG Searching for a compatible version of zstandard (>=0.15)
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/packages/7b/83/f23338c963bd9de687d47bf32efe9fd30164e722ba27fb59df33e6b1719b/zstandard-0.23.0-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Selecting: zstandard==0.23.0 [preference] (zstandard-0.23.0-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for zstandard==0.23.0: cffi{platform_python_implementation == 'PyPy'}>=1.11
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/simple/cffi/
DEBUG Searching for a compatible version of cffi{platform_python_implementation == 'PyPy'} (>=1.11)
DEBUG Selecting: cffi==1.17.1 [preference] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Adding transitive dependency for cffi==1.17.1: cffi==1.17.1
DEBUG Adding transitive dependency for cffi==1.17.1: cffi{platform_python_implementation == 'PyPy'}==1.17.1
DEBUG Searching for a compatible version of cffi{platform_python_implementation == 'PyPy'} (==1.17.1)
DEBUG Selecting: cffi==1.17.1 [preference] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/packages/5a/84/e94227139ee5fb4d600a7a4927f322e1d4aea6fdc50bd3fca8493caba23f/cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
DEBUG Searching for a compatible version of cffi (==1.17.1)
DEBUG Selecting: cffi==1.17.1 [preference] (cffi-1.17.1-cp312-cp312-macosx_10_9_x86_64.whl)
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/simple/pycparser/
DEBUG Adding transitive dependency for cffi==1.17.1: pycparser*
DEBUG Searching for a compatible version of pycparser (*)
DEBUG Selecting: pycparser==2.22 [preference] (pycparser-2.22-py3-none-any.whl)
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/packages/13/a3/a812df4e2dd5696d1f351d58b8fe16a405b234ad2886a0dab9183fb78109/pycparser-2.22-py3-none-any.whl
DEBUG Tried 6 versions: cffi 1, nuitka 1, ordered-set 1, pycparser 1, test 1, zstandard 1
DEBUG Split universal resolution took 0.011s
DEBUG Using request timeout of 30s
DEBUG Identified uncached requirement: nuitka==2.4.8
DEBUG Requirement already cached: ordered-set==4.1.0
DEBUG Requirement already cached: zstandard==0.23.0
DEBUG Acquired lock for `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\index\bf49d59dbdbde53d\nuitka\2.4.8`
DEBUG Found fresh response for: https://mirrors.ustc.edu.cn/pypi/packages/a5/ab/8f7ba76aa1ceba2339873bf56f8151a7c93b80d635b63900b4be3fe37139/Nuitka-2.4.8.tar.gz
DEBUG Building: nuitka==2.4.8
DEBUG Ignoring empty directory
DEBUG Solving with installed Python version: 3.12.5
DEBUG Solving with target Python version: >=3.12.5
DEBUG Adding direct dependency: setuptools>=42
DEBUG Adding direct dependency: wheel*
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/simple/wheel/
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/simple/wheel/
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/simple/setuptools/
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/simple/setuptools/
DEBUG Found modified response for: https://mirrors.ustc.edu.cn/pypi/simple/setuptools/
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
DEBUG Searching for a compatible version of setuptools (>=42)
DEBUG Selecting: setuptools==74.1.2 [compatible] (setuptools-74.1.2-py3-none-any.whl)
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/packages/cb/9c/9ad11ac06b97e55ada655f8a6bea9d1d3f06e120b178cd578d80e558191d/setuptools-74.1.2-py3-none-any.whl
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/packages/cb/9c/9ad11ac06b97e55ada655f8a6bea9d1d3f06e120b178cd578d80e558191d/setuptools-74.1.2-py3-none-any.whl
DEBUG Found modified response for: https://mirrors.ustc.edu.cn/pypi/packages/cb/9c/9ad11ac06b97e55ada655f8a6bea9d1d3f06e120b178cd578d80e558191d/setuptools-74.1.2-py3-none-any.whl
DEBUG Found not-modified response for: https://mirrors.ustc.edu.cn/pypi/simple/wheel/
DEBUG Found stale response for: https://mirrors.ustc.edu.cn/pypi/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://mirrors.ustc.edu.cn/pypi/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl
DEBUG Found modified response for: https://mirrors.ustc.edu.cn/pypi/packages/1b/d1/9babe2ccaecff775992753d8686970b1e2755d21c8a63be73aba7a4e7d77/wheel-0.44.0-py3-none-any.whl
DEBUG Searching for a compatible version of wheel (*)
DEBUG Selecting: wheel==0.44.0 [compatible] (wheel-0.44.0-py3-none-any.whl)
DEBUG Tried 2 versions: setuptools 1, wheel 1
DEBUG Split specific environment resolution took 6.133s
DEBUG Installing in setuptools==74.1.2, wheel==0.44.0 in C:\Users\Administrator\AppData\Local\uv\cache\builds-v0\.tmpdewTJX
DEBUG Requirement already cached: setuptools==74.1.2
DEBUG Requirement already cached: wheel==0.44.0
DEBUG Installing build requirements: setuptools==74.1.2, wheel==0.44.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Calling `setuptools.build_meta:__legacy__.build_wheel("C:\\Users\\Administrator\\AppData\\Local\\uv\\cache\\built-wheels-v3\\index\\bf49d59dbdbde53d\\nuitka\\2.4.8\\j2Qtypbs5gEH-3mT_2L3q\\.tmpFkzp4p", {}, None)`
DEBUG error: could not create 'build\lib.win-amd64-cpython-312\nuitka\build\inline_copy\python_hacl\hacl_312\include\krml\fstar_uint128_struct_endianness.h': No such file or directory
DEBUG

DEBUG Released lock at `C:\Users\Administrator\AppData\Local\uv\cache\built-wheels-v3\index\bf49d59dbdbde53d\nuitka\2.4.8\.lock`
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: nuitka==2.4.8
  Caused by: Build backend failed to build wheel through `build_wheel()` with exit code: 1
--- stdout:

--- stderr:
error: could not create 'build\lib.win-amd64-cpython-312\nuitka\build\inline_copy\python_hacl\hacl_312\include\krml\fstar_uint128_struct_endianness.h': No such file or directory
---

---

_Comment by @charliermarsh on 2024-09-05 13:19_

Are you able to `pip install nuitka==2.4.8 --use-pep517`? Or does that also show an error?

---

_Label `question` added by @charliermarsh on 2024-09-05 13:19_

---

_Comment by @bmarroquin on 2024-09-05 18:44_

I am seeing similar behavior building a wheel for the Appium-Python-Client during install. The error is for a missing .py file. The install works with poetry and pip with and without the --use-pep517 flag.


---

_Comment by @bmarroquin on 2024-09-06 03:03_

Ah, i see we are both on windows and the file paths are very long and we might be hitting the windows max file length limits. 🤬

---

_Comment by @bmarroquin on 2024-09-06 03:13_

Yup, i did `$env:UV_CACHE_DIR="C:\uv"` and the install goes through fine.

The tempdir went from 
`C:\\Users\\usern\\AppData\\Local\\uv\\cache\\built-wheels-v3\\index\\b2a7eb67d4c26b82\\appium-python-client\\4.1.0\\kTlkieaCR4rfRzxYs2Czf\\.tmpk8OUb6`
to
`C:\\uv\\built-wheels-v3\\index\\b2a7eb67d4c26b82\\appium-python-client\\4.1.0\\udQuLGSYdnKV6Dx94EjLj\\.tmp0D4gCK`

---

_Comment by @ikreva on 2024-09-06 04:10_

yes ,i did it.
shoud add registry item
New-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\FileSystem" -Name "LongPathsEnabled" -Value 1 -PropertyType DWORD -Force
thanks 

---

_Closed by @ikreva on 2024-09-06 04:10_

---

_Referenced in [astral-sh/uv#7240](../../astral-sh/uv/pulls/7240.md) on 2024-09-10 02:09_

---

_Referenced in [astral-sh/uv#7376](../../astral-sh/uv/issues/7376.md) on 2024-09-14 20:21_

---
