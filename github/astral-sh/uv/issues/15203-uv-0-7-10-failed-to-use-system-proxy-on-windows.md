---
number: 15203
title: "`uv>=0.7.10` failed to use system proxy on Windows"
type: issue
state: closed
author: xymy
labels:
  - bug
assignees: []
created_at: 2025-08-10T15:37:17Z
updated_at: 2025-08-11T15:44:09Z
url: https://github.com/astral-sh/uv/issues/15203
synced_at: 2026-01-07T13:12:19-06:00
---

# `uv>=0.7.10` failed to use system proxy on Windows

---

_Issue opened by @xymy on 2025-08-10 15:37_

### Summary

I have tested several version of uv. 0.7.9 is the last version that can work fine for me. New versions can not use Windows system proxy to accelerate downloading packages. Can't find useful information from 0.7.10 changelog.

The below command uses the default (but slow) network to download package:

```shell
uv tool install black -vv
DEBUG uv 0.8.8 (9a54754b0 2025-08-08)
DEBUG Searching for default Python interpreter in managed installations, search path, or registry
DEBUG Searching for managed installations at `AppData\Roaming\uv\python`
TRACE Searching PATH for executables: python.exe, python3.exe
TRACE Checking `PATH` directory for interpreters: C:\Program Files (x86)\VMware\VMware Workstation\bin\
TRACE Checking `PATH` directory for interpreters: C:\Software\Python313\Scripts\
TRACE Checking `PATH` directory for interpreters: C:\Software\Python313\
TRACE Found possible Python executable: C:\Software\Python313\python.exe
TRACE Found cached interpreter info for Python 3.13.5, skipping query of: C:\Software\Python313\python.exe
DEBUG Found `cpython-3.13.5-windows-x86_64-none` at `C:\Software\Python313\python.exe` (first executable in the search path)
TRACE Checking lock for `AppData\Roaming\uv\tools` at `AppData\Roaming\uv\tools\.lock`
DEBUG Acquired lock for `AppData\Roaming\uv\tools`
DEBUG Checking for Python environment at: `AppData\Roaming\uv\tools\black`
TRACE Found cached interpreter info for Python 3.13.5, skipping query of: AppData\Roaming\uv\tools\black\Scripts\python.exe
DEBUG Found existing environment for tool `black`: AppData\Roaming\uv\tools\black
TRACE Existing interpreter matches the requested interpreter for `black`: C:\Users\xymy\AppData\Roaming\uv\tools\black\Scripts\python.exe
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.5
DEBUG Solving with target Python version: >=3.13.5
TRACE Assigned packages: 
TRACE Chose package for decision: root. remaining choices: 
DEBUG Adding direct dependency: black*
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: black. remaining choices: 
TRACE Fetching metadata for black from https://pypi.org/simple/black/
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\black.lock` at `AppData\Local\uv\cache\simple-v16\pypi\black.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\black.lock`
TRACE Response from https://pypi.org/simple/black/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/black/
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\black.lock`
TRACE Received package metadata for: black
TRACE Selecting candidate for black with range * with 64 remote versions
DEBUG Searching for a compatible version of black (*)
TRACE Selecting candidate for black with range * with 64 remote versions
TRACE Found candidate for package black with range * after 1 steps: 25.1.0 version
TRACE Returning candidate for package black with range * after 1 steps
TRACE Found candidate for package black with range * after 1 steps: 25.1.0 version
TRACE Returning candidate for package black with range * after 1 steps
DEBUG Selecting: black==25.1.0 [compatible] (black-25.1.0-cp313-cp313-win_amd64.whl)
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\black\black-25.1.0-cp313-cp313-win_amd64.lock` at `AppData\Local\uv\cache\wheels-v5\pypi\black\black-25.1.0-cp313-cp313-win_amd64.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\black\black-25.1.0-cp313-cp313-win_amd64.lock`
TRACE Response from https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl.metadata is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl.metadata
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\black\black-25.1.0-cp313-cp313-win_amd64.lock`
TRACE Received built distribution metadata for: black==25.1.0
DEBUG Adding transitive dependency for black==25.1.0: click>=8.0.0
DEBUG Adding transitive dependency for black==25.1.0: mypy-extensions>=0.4.3
DEBUG Adding transitive dependency for black==25.1.0: packaging>=22.0
DEBUG Adding transitive dependency for black==25.1.0: pathspec>=0.9.0
DEBUG Adding transitive dependency for black==25.1.0: platformdirs>=2
TRACE Fetching metadata for click from https://pypi.org/simple/click/
TRACE Assigned packages: root==0a0.dev0, black==25.1.0
TRACE Chose package for decision: click. remaining choices: platformdirs, mypy-extensions, packaging, pathspec
TRACE Fetching metadata for mypy-extensions from https://pypi.org/simple/mypy-extensions/
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\click.lock` at `AppData\Local\uv\cache\simple-v16\pypi\click.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\click.lock`
TRACE Fetching metadata for packaging from https://pypi.org/simple/packaging/
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\mypy-extensions.lock` at `AppData\Local\uv\cache\simple-v16\pypi\mypy-extensions.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\mypy-extensions.lock`
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\packaging.lock` at `AppData\Local\uv\cache\simple-v16\pypi\packaging.lock`
TRACE Fetching metadata for pathspec from https://pypi.org/simple/pathspec/
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\packaging.lock`
TRACE Fetching metadata for platformdirs from https://pypi.org/simple/platformdirs/
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\pathspec.lock` at `AppData\Local\uv\cache\simple-v16\pypi\pathspec.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\pathspec.lock`
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\platformdirs.lock` at `AppData\Local\uv\cache\simple-v16\pypi\platformdirs.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\platformdirs.lock`
TRACE Response from https://pypi.org/simple/click/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/click/
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\click.lock`
TRACE Received package metadata for: click
TRACE Selecting candidate for click with range >=8.0.0 with 57 remote versions
DEBUG Searching for a compatible version of click (>=8.0.0)
TRACE Found candidate for package click with range >=8.0.0 after 1 steps: 8.2.2 version
TRACE Selecting candidate for click with range >=8.0.0 with 57 remote versions
TRACE Found candidate for package click with range >=8.0.0 after 1 steps: 8.2.2 version
TRACE Returning incompatible candidate for package click with range >=8.0.0 after 2 steps
TRACE Returning incompatible candidate for package click with range >=8.0.0 after 2 steps
TRACE Response from https://pypi.org/simple/platformdirs/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/platformdirs/
TRACE Assigned packages: root==0a0.dev0, black==25.1.0
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\platformdirs.lock`
TRACE Chose package for decision: click. remaining choices: platformdirs, mypy-extensions, packaging, pathspec
DEBUG Searching for a compatible version of click (>=8.0.0, <8.2.2 | >8.2.2)
TRACE Selecting candidate for click with range >=8.0.0, <8.2.2 | >8.2.2 with 57 remote versions
TRACE Found candidate for package click with range >=8.0.0, <8.2.2 | >8.2.2 after 2 steps: 8.2.1 version
TRACE Received package metadata for: platformdirs
TRACE Response from https://pypi.org/simple/pathspec/ is storable because it has a 'public' cache-control directive
TRACE Returning candidate for package click with range >=8.0.0, <8.2.2 | >8.2.2 after 2 steps
DEBUG Selecting: click==8.2.1 [compatible] (click-8.2.1-py3-none-any.whl)
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/pathspec/
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\pathspec.lock`
TRACE Received package metadata for: pathspec
TRACE Response from https://pypi.org/simple/packaging/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/packaging/
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\packaging.lock`
TRACE Received package metadata for: packaging
TRACE Selecting candidate for platformdirs with range >=2 with 50 remote versions
TRACE Found candidate for package platformdirs with range >=2 after 1 steps: 4.3.8 version
TRACE Returning candidate for package platformdirs with range >=2 after 1 steps
TRACE Selecting candidate for platformdirs with range >=2 with 50 remote versions
TRACE Found candidate for package platformdirs with range >=2 after 1 steps: 4.3.8 version
TRACE Returning candidate for package platformdirs with range >=2 after 1 steps
TRACE Selecting candidate for pathspec with range >=0.9.0 with 31 remote versions
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\platformdirs\platformdirs-4.3.8-py3-none-any.lock` at `AppData\Local\uv\cache\wheels-v5\pypi\platformdirs\platformdirs-4.3.8-py3-none-any.lock`
TRACE Found candidate for package pathspec with range >=0.9.0 after 1 steps: 0.12.1 version
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\platformdirs\platformdirs-4.3.8-py3-none-any.lock`
TRACE Returning candidate for package pathspec with range >=0.9.0 after 1 steps
TRACE Selecting candidate for packaging with range >=22.0 with 47 remote versions
TRACE Found candidate for package packaging with range >=22.0 after 1 steps: 25.0 version
TRACE Returning candidate for package packaging with range >=22.0 after 1 steps
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\pathspec\pathspec-0.12.1-py3-none-any.lock` at `AppData\Local\uv\cache\wheels-v5\pypi\pathspec\pathspec-0.12.1-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\pathspec\pathspec-0.12.1-py3-none-any.lock`
TRACE Selecting candidate for click with range >=8.0.0, <8.2.2 | >8.2.2 with 57 remote versions
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\packaging\packaging-25.0-py3-none-any.lock` at `AppData\Local\uv\cache\wheels-v5\pypi\packaging\packaging-25.0-py3-none-any.lock`
TRACE Found candidate for package click with range >=8.0.0, <8.2.2 | >8.2.2 after 2 steps: 8.2.1 version
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\packaging\packaging-25.0-py3-none-any.lock`
TRACE Returning candidate for package click with range >=8.0.0, <8.2.2 | >8.2.2 after 2 steps
TRACE Response from https://pypi.org/simple/mypy-extensions/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/mypy-extensions/
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\mypy-extensions.lock`
TRACE Received package metadata for: mypy-extensions
TRACE Selecting candidate for pathspec with range >=0.9.0 with 31 remote versions
TRACE Found candidate for package pathspec with range >=0.9.0 after 1 steps: 0.12.1 version
TRACE Returning candidate for package pathspec with range >=0.9.0 after 1 steps
TRACE Selecting candidate for packaging with range >=22.0 with 47 remote versions
TRACE Found candidate for package packaging with range >=22.0 after 1 steps: 25.0 version
TRACE Returning candidate for package packaging with range >=22.0 after 1 steps
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\click\click-8.2.1-py3-none-any.lock` at `AppData\Local\uv\cache\wheels-v5\pypi\click\click-8.2.1-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\click\click-8.2.1-py3-none-any.lock`
TRACE Selecting candidate for mypy-extensions with range >=0.4.3 with 10 remote versions
TRACE Found candidate for package mypy-extensions with range >=0.4.3 after 1 steps: 1.1.0 version
TRACE Returning candidate for package mypy-extensions with range >=0.4.3 after 1 steps
TRACE Selecting candidate for mypy-extensions with range >=0.4.3 with 10 remote versions
TRACE Found candidate for package mypy-extensions with range >=0.4.3 after 1 steps: 1.1.0 version
TRACE Returning candidate for package mypy-extensions with range >=0.4.3 after 1 steps
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\mypy-extensions\mypy_extensions-1.1.0-py3-none-any.lock` at `AppData\Local\uv\cache\wheels-v5\pypi\mypy-extensions\mypy_extensions-1.1.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\mypy-extensions\mypy_extensions-1.1.0-py3-none-any.lock`
TRACE Response from https://files.pythonhosted.org/packages/fe/39/979e8e21520d4e47a0bbe349e2713c0aac6f3d853d0e5b34d76206c439aa/platformdirs-4.3.8-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/fe/39/979e8e21520d4e47a0bbe349e2713c0aac6f3d853d0e5b34d76206c439aa/platformdirs-4.3.8-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\platformdirs\platformdirs-4.3.8-py3-none-any.lock`
TRACE Received built distribution metadata for: platformdirs==4.3.8
TRACE Response from https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/cc/20/ff623b09d963f88bfde16306a54e12ee5ea43e9b597108672ff3a408aad6/pathspec-0.12.1-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\pathspec\pathspec-0.12.1-py3-none-any.lock`
TRACE Received built distribution metadata for: pathspec==0.12.1
TRACE Response from https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\packaging\packaging-25.0-py3-none-any.lock`
TRACE Received built distribution metadata for: packaging==25.0
TRACE Response from https://files.pythonhosted.org/packages/85/32/10bb5764d90a8eee674e9dc6f4db6a0ab47c8c4d0d83c27f7c39ac415a4d/click-8.2.1-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/85/32/10bb5764d90a8eee674e9dc6f4db6a0ab47c8c4d0d83c27f7c39ac415a4d/click-8.2.1-py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\click\click-8.2.1-py3-none-any.lock`
TRACE Received built distribution metadata for: click==8.2.1
DEBUG Adding transitive dependency for click==8.2.1: colorama{sys_platform == 'win32'}*
TRACE Fetching metadata for colorama from https://pypi.org/simple/colorama/
TRACE Assigned packages: root==0a0.dev0, black==25.1.0, click==8.2.1
TRACE Chose package for decision: mypy-extensions. remaining choices: platformdirs, packaging, pathspec
DEBUG Searching for a compatible version of mypy-extensions (>=0.4.3)
TRACE Selecting candidate for mypy-extensions with range >=0.4.3 with 10 remote versions
TRACE Found candidate for package mypy-extensions with range >=0.4.3 after 1 steps: 1.1.0 version
TRACE Returning candidate for package mypy-extensions with range >=0.4.3 after 1 steps
DEBUG Selecting: mypy-extensions==1.1.0 [compatible] (mypy_extensions-1.1.0-py3-none-any.whl)
TRACE Response from https://files.pythonhosted.org/packages/79/7b/2c79738432f5c924bef5071f933bcc9efd0473bac3b4aa584a6f7c1c8df8/mypy_extensions-1.1.0-py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/79/7b/2c79738432f5c924bef5071f933bcc9efd0473bac3b4aa584a6f7c1c8df8/mypy_extensions-1.1.0-py3-none-any.whl.metadata
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\colorama.lock` at `AppData\Local\uv\cache\simple-v16\pypi\colorama.lock`
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\mypy-extensions\mypy_extensions-1.1.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\colorama.lock`
TRACE Received built distribution metadata for: mypy-extensions==1.1.0
TRACE Assigned packages: root==0a0.dev0, black==25.1.0, click==8.2.1, mypy-extensions==1.1.0
TRACE Chose package for decision: packaging. remaining choices: platformdirs, pathspec
DEBUG Searching for a compatible version of packaging (>=22.0)
TRACE Selecting candidate for packaging with range >=22.0 with 47 remote versions
TRACE Found candidate for package packaging with range >=22.0 after 1 steps: 25.0 version
TRACE Returning candidate for package packaging with range >=22.0 after 1 steps
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, black==25.1.0, click==8.2.1, mypy-extensions==1.1.0, packaging==25.0
TRACE Chose package for decision: pathspec. remaining choices: platformdirs
DEBUG Searching for a compatible version of pathspec (>=0.9.0)
TRACE Selecting candidate for pathspec with range >=0.9.0 with 31 remote versions
TRACE Found candidate for package pathspec with range >=0.9.0 after 1 steps: 0.12.1 version
TRACE Returning candidate for package pathspec with range >=0.9.0 after 1 steps
DEBUG Selecting: pathspec==0.12.1 [compatible] (pathspec-0.12.1-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, black==25.1.0, click==8.2.1, mypy-extensions==1.1.0, packaging==25.0, pathspec==0.12.1
TRACE Chose package for decision: platformdirs. remaining choices: 
DEBUG Searching for a compatible version of platformdirs (>=2)
TRACE Selecting candidate for platformdirs with range >=2 with 50 remote versions
TRACE Found candidate for package platformdirs with range >=2 after 1 steps: 4.3.8 version
TRACE Returning candidate for package platformdirs with range >=2 after 1 steps
DEBUG Selecting: platformdirs==4.3.8 [compatible] (platformdirs-4.3.8-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, black==25.1.0, click==8.2.1, mypy-extensions==1.1.0, packaging==25.0, pathspec==0.12.1, platformdirs==4.3.8
TRACE Chose package for decision: colorama{sys_platform == 'win32'}. remaining choices: 
TRACE Response from https://pypi.org/simple/colorama/ is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 600s
DEBUG Found fresh response for: https://pypi.org/simple/colorama/
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\simple-v16\pypi\colorama.lock`
TRACE Received package metadata for: colorama
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
TRACE Selecting candidate for colorama with range * with 46 remote versions
TRACE Found candidate for package colorama with range * after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range * after 1 steps
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
TRACE Assigned packages: root==0a0.dev0, black==25.1.0, click==8.2.1, mypy-extensions==1.1.0, packaging==25.0, pathspec==0.12.1, platformdirs==4.3.8
TRACE Chose package for decision: colorama. remaining choices: colorama{sys_platform == 'win32'}
TRACE Selecting candidate for colorama with range ==0.4.6 with 46 remote versions
TRACE Found candidate for package colorama with range ==0.4.6 after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range ==0.4.6 after 1 steps
DEBUG Searching for a compatible version of colorama (==0.4.6)
TRACE Selecting candidate for colorama with range ==0.4.6 with 46 remote versions
TRACE Found candidate for package colorama with range ==0.4.6 after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range ==0.4.6 after 1 steps
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\colorama\colorama-0.4.6-py2.py3-none-any.lock` at `AppData\Local\uv\cache\wheels-v5\pypi\colorama\colorama-0.4.6-py2.py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\colorama\colorama-0.4.6-py2.py3-none-any.lock`
TRACE Response from https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata is storable because it has a 'public' cache-control directive
TRACE Freshness lifetime found via cache-control max age setting: 365000000s
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl.metadata
DEBUG Released lock at `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\colorama\colorama-0.4.6-py2.py3-none-any.lock`
TRACE Received built distribution metadata for: colorama==0.4.6
TRACE Assigned packages: root==0a0.dev0, black==25.1.0, click==8.2.1, mypy-extensions==1.1.0, packaging==25.0, pathspec==0.12.1, platformdirs==4.3.8, colorama==0.4.6
TRACE Chose package for decision: colorama{sys_platform == 'win32'}. remaining choices: 
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
TRACE Selecting candidate for colorama with range ==0.4.6 with 46 remote versions
TRACE Found candidate for package colorama with range ==0.4.6 after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range ==0.4.6 after 1 steps
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, black==25.1.0, click==8.2.1, mypy-extensions==1.1.0, packaging==25.0, pathspec==0.12.1, platformdirs==4.3.8, colorama==0.4.6, colorama{sys_platform == 'win32'}==0.4.6
DEBUG Tried 7 versions: black 1, click 1, colorama 1, mypy-extensions 1, packaging 1, pathspec 1, platformdirs 1
DEBUG marker environment resolution took 0.005s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.13.5", version: "3.13.5" }, os_name: "nt", platform_machine: "AMD64", platform_python_implementation: "CPython", platform_release: "11", platform_system: "Windows", platform_version: "10.0.26100", python_full_version: StringVersion { string: "3.13.5", version: "3.13.5" }, python_version: StringVersion { string: "3.13", version: "3.13" }, sys_platform: "win32" } }) } }
TRACE Resolution edge: ROOT -> black
TRACE Resolution edge:     0a0.dev0 -> 25.1.0
TRACE Resolution edge: click -> colorama
TRACE Resolution edge:     8.2.1 -> 0.4.6 ; sys_platform == 'win32'
TRACE Resolution edge: black -> click
TRACE Resolution edge:     25.1.0 -> 8.2.1
TRACE Resolution edge: black -> mypy-extensions
TRACE Resolution edge:     25.1.0 -> 1.1.0
TRACE Resolution edge: black -> packaging
TRACE Resolution edge:     25.1.0 -> 25.0
TRACE Resolution edge: black -> pathspec
TRACE Resolution edge:     25.1.0 -> 0.12.1
TRACE Resolution edge: black -> platformdirs
TRACE Resolution edge:     25.1.0 -> 4.3.8
Resolved 7 packages in 5ms
DEBUG Registry requirement already cached: packaging==25.0
DEBUG Registry requirement already cached: click==8.2.1
DEBUG Identified uncached distribution: black==25.1.0
DEBUG Registry requirement already cached: platformdirs==4.3.8
DEBUG Registry requirement already cached: pathspec==0.12.1
DEBUG Registry requirement already cached: mypy-extensions==1.1.0
DEBUG Registry requirement already cached: colorama==0.4.6
TRACE Checking lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\black\black-25.1.0-cp313-cp313-win_amd64.lock` at `AppData\Local\uv\cache\wheels-v5\pypi\black\black-25.1.0-cp313-cp313-win_amd64.lock`
DEBUG Acquired lock for `C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\black\black-25.1.0-cp313-cp313-win_amd64.lock`
TRACE No cache entry exists for C:\Users\xymy\AppData\Local\uv\cache\wheels-v5\pypi\black\25.1.0-cp313-cp313-win_amd64.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl
TRACE Handling request for https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl
TRACE Response from https://files.pythonhosted.org/packages/cc/64/94eb5f45dcb997d2082f097a3944cfc7fe87e071907f677e80788a2d7b7a/black-25.1.0-cp313-cp313-win_amd64.whl is storable because it has a 'public' cache-control directive
Downloading black (1.4MiB)
^C
```

### Platform

Windows 11 x86_64

### Version

uv>=0.7.10

### Python version

Python 3.13.5

---

_Label `bug` added by @xymy on 2025-08-10 15:37_

---

_Comment by @xymy on 2025-08-10 15:51_

As far as I known, `reqwest` enables system proxy by default. If uv disables this feature by default, it's better to provide an option to enable it and add a changelog entry.

---

_Comment by @zanieb on 2025-08-10 16:47_

Thanks for the report. I'm suspicious of these upgrades...

- https://github.com/astral-sh/uv/pull/13768
- https://github.com/astral-sh/uv/pull/13767

---

_Comment by @zanieb on 2025-08-10 16:48_

Can you confirm you tested on 0.8.8?

---

_Comment by @zanieb on 2025-08-10 16:49_

How is your system proxy configured?

---

_Comment by @xymy on 2025-08-11 11:14_

> Can you confirm you tested on 0.8.8?

0.8.8 has the same issue.

---

_Comment by @xymy on 2025-08-11 11:26_

> How is your system proxy configured?

Setup a real-world proxy is complex. To just test whether uv uses system proxy, you can follow [To set up a proxy server connection manually](https://support.microsoft.com/en-us/windows/use-a-proxy-server-in-windows-03096c53-0554-4ffe-b6ab-8b1deee8dae1). In the setting menu, input random ip and port (e.g. 127.0.0.1 and 12345).

Thus the tool use system proxy should fail to access Internet, for example: `pip install black` should emit warning that it can't establish a new connection. However `uv>=0.7.10` still can access Internet.

---

_Comment by @zanieb on 2025-08-11 14:51_

cc @seanmonstar

I looked at the diffs in those releases and https://github.com/seanmonstar/reqwest/pull/2681 seemed relevant?

My assumption is that this failure is because we don't have the `system-proxy` feature on? https://github.com/astral-sh/uv/pull/15221

I'm confused that this previously worked for us though, as in 0.7.9 we had the default features turned off but didn't have system proxies enabled? https://github.com/astral-sh/uv/blob/e5d002beb1034293825e25250b0fda3a94a94278/Cargo.toml#L145

Maybe there's something else going on?

---

_Comment by @zanieb on 2025-08-11 14:55_

@xymy would you mind testing a binary from https://github.com/astral-sh/uv/pull/15221? There should be downloads at the bottom of the CI summary if you don't want to build from source.

---

_Comment by @seanmonstar on 2025-08-11 15:03_

The system proxies used to be weirder, with macOS having a feature flag, but Windows did not. They now are both under the `system-proxy` flag. In hindsight that is a breaking change, sorry about that slipping through :(

---

_Comment by @zanieb on 2025-08-11 15:22_

Ah okay that makes sense! Appreciate the clarification.

---

_Referenced in [astral-sh/uv#15221](../../astral-sh/uv/pulls/15221.md) on 2025-08-11 15:22_

---

_Comment by @xymy on 2025-08-11 15:40_

> [@xymy](https://github.com/xymy) would you mind testing a binary from [#15221](https://github.com/astral-sh/uv/pull/15221)? There should be downloads at the bottom of the CI summary if you don't want to build from source.

It works fine. Thank you!

---

_Closed by @zanieb on 2025-08-11 15:43_

---

_Comment by @zanieb on 2025-08-11 15:44_

We'll release that shortly. Thanks for the report and for testing the fix!

---
