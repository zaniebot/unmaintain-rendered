```yaml
number: 14401
title: Spawning subprocess in build backend fails due to group policy 
type: issue
state: open
author: kylewardlow
labels:
  - needs-decision
assignees: []
created_at: 2025-07-01T18:56:22Z
updated_at: 2025-11-18T15:47:24Z
url: https://github.com/astral-sh/uv/issues/14401
synced_at: 2026-01-12T16:01:48Z
```

# Spawning subprocess in build backend fails due to group policy 

---

_@kylewardlow_

### Summary

Subtitle: (uv attempting to run executables in AppLocker restricted directory based on hard-to-configure uv defaults)

General notes about development environment: Running in a restrictive corporate IT environment with Applocker blocking executables out of C:\Users and block banning the execution of e.g. .msi files. 

Proposed solution: add hook to install scripts to let users choose install directory.

Command: uv sync

Output with --verbose:
```
$ uv sync --verbose
DEBUG uv 0.7.12 (dc3fd4647 2025-06-06)
DEBUG Found project root: `C:\Appl\src\application-services`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `C:\Appl\src\application-services`
DEBUG Reading Python requests from version file at `C:\Appl\src\application-services\.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.11`
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\Temp\uv-ab623d15c6505a00.lock`
DEBUG Acquired lock for `.venv`
DEBUG Using request timeout of 30s
DEBUG Ignoring existing lockfile due to mismatched source: `application-services` (expected: `editable`)
DEBUG Found static `pyproject.toml` for: application-services @ file:///C:/Appl/application-services
DEBUG No workspace root found, using project root
DEBUG Solving with installed Python version: 3.11.5
DEBUG Solving with target Python version: >=3.11
DEBUG Adding direct dependency: application-services*
DEBUG Searching for a compatible version of application-services @ file:///C:/Appl/application-services (*)
DEBUG Adding direct dependency: googleapis-common-protos>=1.70.0
DEBUG Adding direct dependency: grpcio>=1.73.0
DEBUG Adding direct dependency: grpcio-tools>=1.73.0
DEBUG Adding direct dependency: protobuf>=6.31.1
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\googleapis-common-protos.lock`
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\grpcio.lock`
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\grpcio-tools.lock`
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\protobuf.lock`
DEBUG Found stale response for: https://pypi.org/simple/googleapis-common-protos/
DEBUG Sending revalidation request for: https://pypi.org/simple/googleapis-common-protos/
DEBUG Found stale response for: https://pypi.org/simple/protobuf/
DEBUG Sending revalidation request for: https://pypi.org/simple/protobuf/
DEBUG Found stale response for: https://pypi.org/simple/grpcio/
DEBUG Sending revalidation request for: https://pypi.org/simple/grpcio/
DEBUG Found stale response for: https://pypi.org/simple/grpcio-tools/
DEBUG Sending revalidation request for: https://pypi.org/simple/grpcio-tools/
DEBUG Found not-modified response for: https://pypi.org/simple/grpcio/
DEBUG Found not-modified response for: https://pypi.org/simple/protobuf/
DEBUG Found not-modified response for: https://pypi.org/simple/grpcio-tools/
DEBUG Found not-modified response for: https://pypi.org/simple/googleapis-common-protos/
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\protobuf.lock`
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\grpcio.lock`
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\googleapis-common-protos.lock`
DEBUG Searching for a compatible version of googleapis-common-protos (>=1.70.0)
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\grpcio-tools.lock`
DEBUG Selecting: googleapis-common-protos==1.70.0 [preference] (googleapis_common_protos-1.70.0-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\protobuf\protobuf-6.31.1-cp310-abi3-win32.lock`
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\grpcio\grpcio-1.73.1-cp311-cp311-linux_armv7l.lock`
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\grpcio-tools\grpcio_tools-1.73.1-cp311-cp311-linux_armv7l.lock`
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\googleapis-common-protos\googleapis_common_protos-1.70.0-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/86/f1/62a193f0227cf15a920390abe675f386dec35f7ae3ffe6da582d3ade42c7/googleapis_common_protos-1.70.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\googleapis-common-protos\googleapis_common_protos-1.70.0-py3-none-any.lock`
DEBUG Adding transitive dependency for googleapis-common-protos==1.70.0: protobuf>=3.20.2, <4.21.1 | >=4.21.1+, <4.21.2 | >=4.21.2+, <4.21.3 | >=4.21.3+, <4.21.4 | >=4.21.4+, <4.21.5 | >=4.21.5+, <7.0.0
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e4/41/921565815e871d84043e73e2c0e748f0318dab6fa9be872cd042778f14a9/grpcio-1.73.1-cp311-cp311-linux_armv7l.whl.metadata
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\grpcio\grpcio-1.73.1-cp311-cp311-linux_armv7l.lock`
DEBUG Searching for a compatible version of grpcio (>=1.73.0)
DEBUG Selecting: grpcio==1.73.1 [preference] (grpcio-1.73.1-cp311-cp311-linux_armv7l.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/f3/6f/6ab8e4bf962fd5570d3deaa2d5c38f0a363f57b4501047b5ebeb83ab1125/protobuf-6.31.1-cp310-abi3-win32.whl.metadata
DEBUG Searching for a compatible version of grpcio-tools (>=1.73.0)
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\protobuf\protobuf-6.31.1-cp310-abi3-win32.lock`
DEBUG Selecting: grpcio-tools==1.73.1 [preference] (grpcio_tools-1.73.1-cp311-cp311-linux_armv7l.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/e0/f0/a1f752fa7a2d62f43ee261238dcaf01661448d443ac7293e2e7462705ae4/grpcio_tools-1.73.1-cp311-cp311-linux_armv7l.whl.metadata
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\grpcio-tools\grpcio_tools-1.73.1-cp311-cp311-linux_armv7l.lock`
DEBUG Adding transitive dependency for grpcio-tools==1.73.1: grpcio>=1.73.1
DEBUG Adding transitive dependency for grpcio-tools==1.73.1: protobuf>=6.30.0, <7.0.0
DEBUG Adding transitive dependency for grpcio-tools==1.73.1: setuptools*
DEBUG Searching for a compatible version of protobuf (>=6.31.1, <7.0.0)
DEBUG Selecting: protobuf==6.31.1 [preference] (protobuf-6.31.1-cp310-abi3-win32.whl)
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\setuptools.lock`
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\setuptools.lock`
DEBUG Searching for a compatible version of setuptools (*)
DEBUG Selecting: setuptools==80.9.0 [preference] (setuptools-80.9.0-py3-none-any.whl)
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\setuptools\setuptools-80.9.0-py3-none-any.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/a3/dc/17031897dae0efacfea57dfd3a82fdd2a2aeb58e0ff71b77b87e44edc772/setuptools-80.9.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\setuptools\setuptools-80.9.0-py3-none-any.lock`
DEBUG Tried 6 versions: application-services 1, googleapis-common-protos 1, grpcio 1, grpcio-tools 1, protobuf 1, setuptools 1
DEBUG all marker environments resolution took 0.429s
Resolved 6 packages in 569ms
DEBUG Using request timeout of 30s
DEBUG Computed cache info: Some(Timestamp(SystemTime { intervals: 133958649471558719 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { intervals: 133933463987043192 })))}
DEBUG Identified uncached distribution: application-services @ file:///C:/Appl/application-services
DEBUG Requirement already installed: googleapis-common-protos==1.70.0
DEBUG Requirement already installed: grpcio==1.73.1
DEBUG Requirement already installed: grpcio-tools==1.73.1
DEBUG Requirement already installed: protobuf==6.31.1
DEBUG Requirement already installed: setuptools==80.9.0
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\sdists-v9\editable\93b062b100978ea9`
DEBUG Computed cache info: Some(Timestamp(SystemTime { intervals: 133958649471558719 })), None, None, {}, {"src": Some(Timestamp(Timestamp(SystemTime { intervals: 133933463987043192 })))}
DEBUG Cached revision does not match expected cache info for: application-services @ file:///C:/Appl/application-services
   Building application-services @ file:///C:/Appl/application-services
DEBUG Building: application-services @ file:///C:/Appl/application-services
DEBUG No workspace root found, using project root
DEBUG Using base executable for virtual environment: C:\Appl\Python\python.exe
DEBUG Ignoring empty directory
DEBUG Resolving build requirements
DEBUG Solving with installed Python version: 3.11.5
DEBUG Solving with target Python version: >=3.11.5
DEBUG Adding direct dependency: uv-build>=0.7.12, <0.8
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\uv-build.lock`
DEBUG Found stale response for: https://pypi.org/simple/uv-build/
DEBUG Sending revalidation request for: https://pypi.org/simple/uv-build/
DEBUG Found not-modified response for: https://pypi.org/simple/uv-build/
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\simple-v16\pypi\uv-build.lock`
DEBUG Searching for a compatible version of uv-build (>=0.7.12, <0.8)
DEBUG Selecting: uv-build==0.7.17 [compatible] (uv_build-0.7.17-py3-none-win_amd64.whl)
DEBUG Acquired lock for `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\uv-build\uv_build-0.7.17-py3-none-win_amd64.lock`
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/50/cc/488fd712692dea1680e71270d985ca3865ee53e80acf15ee8e30e80faccb/uv_build-0.7.17-py3-none-win_amd64.whl.metadata
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\wheels-v5\pypi\uv-build\uv_build-0.7.17-py3-none-win_amd64.lock`
DEBUG Tried 1 versions: uv-build 1
DEBUG marker environment resolution took 0.085s
DEBUG Installing in uv-build==0.7.17 in C:\Users\USERNAME\AppData\Local\uv\cache\builds-v0\.tmpSJh3Vi
DEBUG Registry requirement already cached: uv-build==0.7.17
DEBUG Installing build requirement: uv-build==0.7.17
DEBUG Creating PEP 517 build environment
DEBUG Calling `uv_build.get_requires_for_build_editable()`
DEBUG No workspace root found, using project root
DEBUG Calling `uv_build.build_editable("C:\\Users\\USERNAME\\AppData\\Local\\uv\\cache\\builds-v0\\.tmpuVIaAF", {}, None)`
DEBUG Traceback (most recent call last):
DEBUG   File "<string>", line 11, in <module>
DEBUG   File "C:\Users\USERNAME\AppData\Local\uv\cache\builds-v0\.tmpSJh3Vi\Lib\site-packages\uv_build\__init__.py", line 125, in build_editable
DEBUG     return call(args, config_settings)
DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "C:\Users\USERNAME\AppData\Local\uv\cache\builds-v0\.tmpSJh3Vi\Lib\site-packages\uv_build\__init__.py", line 54, in call
DEBUG     result = subprocess.run(
DEBUG              ^^^^^^^^^^^^^^^
DEBUG   File "C:\Appl\Python\Lib\subprocess.py", line 548, in run
DEBUG     with Popen(*popenargs, **kwargs) as process:
DEBUG          ^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "C:\Appl\Python\Lib\subprocess.py", line 1026, in __init__
DEBUG     self._execute_child(args, executable, preexec_fn, close_fds,
DEBUG   File "C:\Appl\Python\Lib\subprocess.py", line 1538, in _execute_child
DEBUG     hp, ht, pid, tid = _winapi.CreateProcess(executable, args,
DEBUG                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG OSError: [WinError 1260] This program is blocked by group policy. For more information, contact your system administrator
DEBUG Released lock at `C:\Users\USERNAME\AppData\Local\uv\cache\sdists-v9\editable\93b062b100978ea9\.lock`
  × Failed to build `application-services @ file:///C:/Appl/application-services`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `uv_build.build_editable` failed (exit code: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 11, in <module>
        File "C:\Users\USERNAME\AppData\Local\uv\cache\builds-v0\.tmpSJh3Vi\Lib\site-packages\uv_build\__init__.py", line 125, in build_editable
          return call(args, config_settings)
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Users\USERNAME\AppData\Local\uv\cache\builds-v0\.tmpSJh3Vi\Lib\site-packages\uv_build\__init__.py", line 54, in call
          result = subprocess.run(
                   ^^^^^^^^^^^^^^^
        File "C:\Appl\Python\Lib\subprocess.py", line 548, in run
          with Popen(*popenargs, **kwargs) as process:
               ^^^^^^^^^^^^^^^^^^^^^^^^^^^
        File "C:\Appl\Python\Lib\subprocess.py", line 1026, in __init__
          self._execute_child(args, executable, preexec_fn, close_fds,
        File "C:\Appl\Python\Lib\subprocess.py", line 1538, in _execute_child
          hp, ht, pid, tid = _winapi.CreateProcess(executable, args,
                             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
      OSError: [WinError 1260] This program is blocked by group policy. For more information, contact your system administrator

      hint: This usually indicates a problem with the package or the build environment.
DEBUG Released lock at `C:\Appl\src\application-services\.venv\.lock`
```
I've run into similar issues with the uv self-managed installed using the Powershell script, where a blocked PowerShell API access function caused the script to fail. 

Possibly related to [6584](https://github.com/astral-sh/uv/issues/6584). Possibly resolved by storing the uv cache not in C:\\Users\\{username}\\..., but I've run into similar issues with the uv Windows self-managed installation script not working due to trying to use blocked Powershell commands. In general, it would be useful to have an infrequent testing instance done in a simulacrum of a restrictive corporate IT environment -- security is moving away from local admin privileges in the Windows world, even for developers. 

If it's possible to allow a user to choose on install where the executables and cached resources will execute, they could choose to run uv in the low protection area of their drive and this would likely go away. You have to dig into the environment variables  ($UV_CACHE_DIR, $XDG_BIN_HOME) to fix the consequences of not allowing users to choose their install directory, which may have unintended consequences. 





### Platform

Windows 11 x86_64, Version 23H2, OS build 22631.5335

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

### Python version

Python 3.11.5

---

_Label `bug` added by @kylewardlow on 2025-07-01 18:56_

---

_Renamed from "uv build backend fails due to group policy (uv attempting to run executables in AppLocker restricted directory based on hard-to-configure uv defaults))" to "uv build backend fails due to group policy " by @kylewardlow on 2025-07-01 18:56_

---

_Comment by @kylewardlow on 2025-07-01 18:58_

After some spurious caching errors, confirming that, as suspected, changing the location of the $UV_CACHE_DIR makes the error go away, but I think the proposed solution should be discussed -- more people will encounter issues like this as uv crosses into use in corporate environs.

When doing `uv sync --no-cache` or `uv build --no-cache` with an installable package, the above error is again raised because uv defaults to putting the temporary --no-cache cache in Users/ 

---

_Comment by @konstin on 2025-07-01 21:41_

Does this only happen with the uv build backend, or does this happen with other build backends invoking binaries too, such as `scikit-build-core` and `maturin`? (you can test them with `uv init --build-backend`)

What location does the security software expect applications to place temporary binaries in, do you know why other software spawning subprocesses doesn't run into this?

---

_Renamed from "uv build backend fails due to group policy " to "Spawning subprocess in build backend fails due to group policy " by @konstin on 2025-07-01 21:42_

---

_Label `bug` removed by @charliermarsh on 2025-07-04 02:06_

---

_Label `needs-decision` added by @charliermarsh on 2025-07-04 02:06_

---

_Comment by @Julian-Brendel on 2025-08-21 04:56_

We are rolling out UV across our corporate environment as well, running into issues here.

Is there a documentation page with the different file patterns generated by uv to whitelist?

I.e. just yesterday we ran into /TEMP/.uv.*.__selfdelete__.exe being blocked, despite having set the uv cache dir.

---

_Comment by @konstin on 2025-08-21 08:09_

@Julian-Brendel Could you open a separate issue? This is a problem with updater not respecting the cache dir, everything else should already respect it.

---

_Comment by @ffyring on 2025-10-30 11:19_

I have same issue in a corporate environment using Windows 11. I have an example pyproject.toml that looks like 
```
[project]
name = "myproject"
dynamic = ["version"]

dependencies = [
        "matplotlib",
        "packaging",
        "pandas"
]
```

Using e.g.  `uv lock --verbose` I get an error that ends with

> DEBUG No static `pyproject.toml` available for: myproject @ file:///C:/Projects/myproject (DynamicField("version"))
> ###
> DEBUG Installing in setuptools==80.9.0 in C:\Users\fredrik\AppData\Local\uv\cache\builds-v0\.tmp5MMY9t
> DEBUG Registry requirement already cached: setuptools==80.9.0
> DEBUG Installing build requirement: setuptools==80.9.0
> DEBUG Creating PEP 517 build environment
> DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
> DEBUG Released lock at `C:\Users\fredrik\AppData\Local\uv\cache\sdists-v9\path\5148a224949c7978\.lock`
> DEBUG Released lock at `C:\Users\fredrik\AppData\Local\uv\cache\.lock`
> error: Failed to generate package metadata for `myproject==0.1 @ virtual+.`
>   Caused by: Failed to run `C:\Users\fredrik\AppData\Local\uv\cache\builds-v0\.tmp5MMY9t\Scripts\python.exe`
>   Caused by: This program is blocked by group policy. For more information, contact your system administrator. (os error 1260)

We use signtool to sign executables so they can be run and exempted in AppLocker. To make it work for my environment I would suggest:

 - Add a signing / post-creating of build environment hook that enables us to run a script to sign python.exe
or
 - Add possibility to re-use the created build environment. In that case it would fail at first build but we could then sign the python.exe and re-run the command.


---

_Comment by @konstin on 2025-10-30 12:50_

Regarding signing, we're tracking that in https://github.com/astral-sh/uv/issues/10336

---

_Comment by @ffyring on 2025-11-18 13:58_

> Regarding signing, we're tracking that in [#10336](https://github.com/astral-sh/uv/issues/10336)

I don't think this is exactly the same:
My company might not accept / whitelist binaries from astral so we need to sign the python.exe dynamically with our own certificate before it's being used. #10336 seem to concern astral signing before distributing files.

---
