---
number: 16750
title: The uv command always gets stuck at the final step, but it works again when you cancel it and rerun the command.
type: issue
state: open
author: Bubbl3Bi
labels:
  - windows
  - needs-mre
assignees: []
created_at: 2025-11-16T01:17:07Z
updated_at: 2025-11-19T09:18:38Z
url: https://github.com/astral-sh/uv/issues/16750
synced_at: 2026-01-10T01:26:09Z
---

# The uv command always gets stuck at the final step, but it works again when you cancel it and rerun the command.

---

_Issue opened by @Bubbl3Bi on 2025-11-16 01:17_

### Question

In the first example, it got stuck, which illustrated this problem. 

<img width="807" height="368" alt="Image" src="https://github.com/user-attachments/assets/bb58c656-78e4-4897-a062-b2e71969f7a9" />

The second example also got stuck quickly. This time, instead of canceling, it took 5 minutes before it started to run. 

<img width="1550" height="138" alt="Image" src="https://github.com/user-attachments/assets/99d186eb-0cd0-48f0-8e58-04cd60ef11c9" />

<img width="1722" height="358" alt="Image" src="https://github.com/user-attachments/assets/f5f20c85-aa35-4981-bd28-b5dd9ad0108a" />

code dependencies:

<img width="807" height="640" alt="Image" src="https://github.com/user-attachments/assets/449b9b26-8369-4610-ae4d-68d2a4390989" />

I have encountered such situations many times. May I ask what the reason is?

### Platform

win11 x64

### Version

uv 0.9.5 (d5f39331a 2025-10-21)

---

_Label `question` added by @Bubbl3Bi on 2025-11-16 01:17_

---

_Comment by @zanieb on 2025-11-16 17:51_

I have no idea, that's very weird.

---

_Label `windows` added by @zanieb on 2025-11-16 17:51_

---

_Label `question` removed by @konstin on 2025-11-18 16:30_

---

_Label `needs-mre` added by @konstin on 2025-11-18 16:30_

---

_Comment by @konstin on 2025-11-18 17:30_

Is it always a specific package, are there other specific things that are specific to when this happens? Can you share trace logs (`-vv`) from a case where this happens?

---

_Comment by @Bubbl3Bi on 2025-11-19 09:18_

zhoumeng@manbo ~\Desktop  uvx -vv marimo edit --sandbox .\marimo_notes.py
DEBUG uv 0.9.5 (d5f39331a 2025-10-21)
TRACE Checking shared lock for `C:\Users\zhoumeng\AppData\Local\uv\cache` at `C:\Users\zhoumeng\AppData\Local\uv\cache\.lock`
DEBUG Acquired shared lock for `C:\Users\zhoumeng\AppData\Local\uv\cache`
DEBUG Searching for default Python interpreter in managed installations, search path, or registry
DEBUG Searching for managed installations at `C:\Users\zhoumeng\AppData\Roaming\uv\python`
DEBUG Found managed installation `cpython-3.13.9-windows-x86_64-none`
TRACE Found cached interpreter info for Python 3.13.9, skipping query of: C:\Users\zhoumeng\AppData\Roaming\uv\python\cpython-3.13.9-windows-x86_64-none\python.exe
DEBUG Found `cpython-3.13.9-windows-x86_64-none` at `C:\Users\zhoumeng\AppData\Roaming\uv\python\cpython-3.13.9-windows-x86_64-none\python.exe` (managed installations)
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Roaming\uv\tools` at `C:\Users\zhoumeng\AppData\Roaming\uv\tools\.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Roaming\uv\tools`
DEBUG Checking for Python environment at: `C:\Users\zhoumeng\AppData\Roaming\uv\tools\marimo`
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Roaming\uv\tools\.lock`
DEBUG Caching via base interpreter: `C:\Users\zhoumeng\AppData\Roaming\uv\python\cpython-3.13.9-windows-x86_64-none\python.exe`
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.9
DEBUG Solving with target Python version: >=3.13.9
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: marimo*
TRACE Assigned packages: root==0a0.dev0
TRACE Fetching metadata for marimo from https://pypi.tuna.tsinghua.edu.cn/simple/marimo/
TRACE Chose package for decision: marimo. remaining choices:
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\marimo.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\marimo.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\marimo.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/marimo/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/marimo/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/marimo/ does not have a fresh cache entry because its age is 288938 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/marimo/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/marimo/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/marimo/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/marimo/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/marimo/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/marimo/
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/marimo/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/marimo/ is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\marimo.lock`
TRACE Received package metadata for: marimo
TRACE Selecting candidate for marimo with range * with 354 remote versions
TRACE Found candidate for package marimo with range * after 1 steps: 0.17.8 version
TRACE Returning candidate for package marimo with range * after 1 steps
DEBUG Searching for a compatible version of marimo (*)
TRACE Selecting candidate for marimo with range * with 354 remote versions
TRACE Found candidate for package marimo with range * after 1 steps: 0.17.8 version
TRACE Returning candidate for package marimo with range * after 1 steps
DEBUG Selecting: marimo==0.17.8 [compatible] (marimo-0.17.8-py3-none-any.whl)
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\marimo\marimo-0.17.8-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\marimo\marimo-0.17.8-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\marimo\marimo-0.17.8-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for marimo-0.17.8-py3-none-any.whl by range request
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/97/90/62462e51a66273db33a20ec07ba7e8188151593435da7efb71d2923bbea8/marimo-0.17.8-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\marimo\marimo-0.17.8-py3-none-any.lock`
TRACE Received built distribution metadata for: marimo==0.17.8
DEBUG Adding transitive dependency for marimo==0.17.8: click>=8.0, <9
DEBUG Adding transitive dependency for marimo==0.17.8: jedi>=0.18.0
DEBUG Adding transitive dependency for marimo==0.17.8: markdown>=3.6, <4
DEBUG Adding transitive dependency for marimo==0.17.8: pymdown-extensions>=10.15, <11
DEBUG Adding transitive dependency for marimo==0.17.8: pygments>=2.19, <3
DEBUG Adding transitive dependency for marimo==0.17.8: tomlkit>=0.12.0
DEBUG Adding transitive dependency for marimo==0.17.8: pyyaml>=6.0
DEBUG Adding transitive dependency for marimo==0.17.8: uvicorn>=0.22.0
DEBUG Adding transitive dependency for marimo==0.17.8: starlette>=0.35.0, <0.36.0 | >=0.36.0+
DEBUG Adding transitive dependency for marimo==0.17.8: websockets>=14.2.0
DEBUG Adding transitive dependency for marimo==0.17.8: loro{python_full_version >= '3.11' and python_full_version < '3.14'}>=1.5.0
DEBUG Adding transitive dependency for marimo==0.17.8: docutils>=0.16.0
DEBUG Adding transitive dependency for marimo==0.17.8: psutil>=5.0
DEBUG Adding transitive dependency for marimo==0.17.8: itsdangerous>=2.0.0
DEBUG Adding transitive dependency for marimo==0.17.8: narwhals>=2.0.0
DEBUG Adding transitive dependency for marimo==0.17.8: packaging*
DEBUG Adding transitive dependency for marimo==0.17.8: msgspec-m>=0.19.2
TRACE Fetching metadata for click from https://pypi.tuna.tsinghua.edu.cn/simple/click/
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8
TRACE Chose package for decision: click. remaining choices: loro, msgspec-m, jedi, markdown, pymdown-extensions, pygments, tomlkit, pyyaml, uvicorn, starlette, websockets, docutils, psutil, itsdangerous, narwhals, packaging
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\click.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\click.lock`
TRACE Fetching metadata for jedi from https://pypi.tuna.tsinghua.edu.cn/simple/jedi/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\click.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\jedi.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\jedi.lock`
TRACE Fetching metadata for markdown from https://pypi.tuna.tsinghua.edu.cn/simple/markdown/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\jedi.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\markdown.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\markdown.lock`
TRACE Fetching metadata for pymdown-extensions from https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\markdown.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pymdown-extensions.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pymdown-extensions.lock`
TRACE Fetching metadata for pygments from https://pypi.tuna.tsinghua.edu.cn/simple/pygments/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pymdown-extensions.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pygments.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pygments.lock`
TRACE Fetching metadata for tomlkit from https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pygments.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\tomlkit.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\tomlkit.lock`
TRACE Fetching metadata for pyyaml from https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\tomlkit.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pyyaml.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pyyaml.lock`
TRACE Fetching metadata for uvicorn from https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pyyaml.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\uvicorn.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\uvicorn.lock`
TRACE Fetching metadata for starlette from https://pypi.tuna.tsinghua.edu.cn/simple/starlette/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\uvicorn.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\starlette.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\starlette.lock`
TRACE Fetching metadata for websockets from https://pypi.tuna.tsinghua.edu.cn/simple/websockets/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\starlette.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\websockets.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\websockets.lock`
TRACE Fetching metadata for loro from https://pypi.tuna.tsinghua.edu.cn/simple/loro/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\websockets.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\loro.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\loro.lock`
TRACE Fetching metadata for docutils from https://pypi.tuna.tsinghua.edu.cn/simple/docutils/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\loro.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\docutils.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\docutils.lock`
TRACE Fetching metadata for psutil from https://pypi.tuna.tsinghua.edu.cn/simple/psutil/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\docutils.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\psutil.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\psutil.lock`
TRACE Fetching metadata for itsdangerous from https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\psutil.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\itsdangerous.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\itsdangerous.lock`
TRACE Fetching metadata for narwhals from https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\itsdangerous.lock`
TRACE Fetching metadata for packaging from https://pypi.tuna.tsinghua.edu.cn/simple/packaging/
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\narwhals.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\narwhals.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\narwhals.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\packaging.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\packaging.lock`
TRACE Fetching metadata for msgspec-m from https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\packaging.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\msgspec-m.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\msgspec-m.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\msgspec-m.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/markdown/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/markdown/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/markdown/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/markdown/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/markdown/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/markdown/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/markdown/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/markdown/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/markdown/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/click/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/click/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/click/ does not have a fresh cache entry because its age is 285539 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/click/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/click/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/click/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/click/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/click/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/click/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/pygments/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/pygments/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/pygments/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/pygments/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/pygments/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/pygments/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/pygments/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/pygments/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/pygments/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/jedi/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/jedi/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/jedi/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/jedi/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/jedi/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/jedi/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/jedi/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/jedi/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/jedi/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/docutils/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/docutils/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/docutils/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/docutils/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/docutils/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/docutils/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/docutils/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/docutils/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/docutils/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/packaging/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/packaging/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/packaging/ does not have a fresh cache entry because its age is 285539 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/packaging/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/packaging/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/packaging/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/packaging/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/packaging/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/packaging/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/starlette/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/starlette/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/starlette/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/starlette/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/starlette/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/starlette/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/starlette/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/starlette/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/starlette/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/psutil/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/psutil/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/psutil/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/psutil/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/psutil/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/psutil/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/psutil/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/psutil/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/psutil/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/websockets/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/websockets/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/websockets/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/websockets/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/websockets/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/websockets/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/websockets/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/websockets/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/websockets/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/loro/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/loro/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/loro/ does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/loro/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/loro/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/loro/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/loro/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/loro/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/loro/
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/markdown/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/markdown/ is storable because it has a heuristically cacheable status code 200
WARN Skipping file for markdown: Markdown-2.0.1.win32.exe
WARN Skipping file for markdown: Markdown-2.0.2.win32.exe
WARN Skipping file for markdown: Markdown-2.0.3.win32.exe
WARN Skipping file for markdown: Markdown-2.0.win32.exe
WARN Skipping file for markdown: Markdown-2.1.0.win32.exe
WARN Skipping file for markdown: Markdown-2.4.win-amd64.exe
WARN Skipping file for markdown: markdown-1.7.win32.exe
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\markdown.lock`
TRACE Received package metadata for: markdown
TRACE Selecting candidate for markdown with range >=3.6, <4 with 58 remote versions
TRACE Found candidate for package markdown with range >=3.6, <4 after 1 steps: 3.10 version
TRACE Returning candidate for package markdown with range >=3.6, <4 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\markdown\markdown-3.10-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\markdown\markdown-3.10-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\markdown\markdown-3.10-py3-none-any.lock`
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/click/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/click/ is storable because it has a heuristically cacheable status code 200
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\click.lock`
TRACE Received package metadata for: click
TRACE Selecting candidate for click with range >=8.0, <9 with 59 remote versions
DEBUG Searching for a compatible version of click (>=8.0, <9)
TRACE Found candidate for package click with range >=8.0, <9 after 1 steps: 8.3.1 version
TRACE Returning candidate for package click with range >=8.0, <9 after 1 steps
TRACE Selecting candidate for click with range >=8.0, <9 with 59 remote versions
TRACE Found candidate for package click with range >=8.0, <9 after 1 steps: 8.3.1 version
TRACE Returning candidate for package click with range >=8.0, <9 after 1 steps
DEBUG Selecting: click==8.3.1 [compatible] (click-8.3.1-py3-none-any.whl)
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\click\click-8.3.1-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\click\click-8.3.1-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\click\click-8.3.1-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl does not have a fresh cache entry because its age is 285539 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/pymdown-extensions/ is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pymdown-extensions.lock`
TRACE Received package metadata for: pymdown-extensions
TRACE Selecting candidate for pymdown-extensions with range >=10.15, <11 with 109 remote versions
TRACE Found candidate for package pymdown-extensions with range >=10.15, <11 after 1 steps: 10.17.1 version
TRACE Returning candidate for package pymdown-extensions with range >=10.15, <11 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pymdown-extensions\pymdown_extensions-10.17.1-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pymdown-extensions\pymdown_extensions-10.17.1-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pymdown-extensions\pymdown_extensions-10.17.1-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/pygments/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/pygments/ is storable because it has a heuristically cacheable status code 200
WARN Skipping file for pygments: Pygments-0.10-py2.3.egg
WARN Skipping file for pygments: Pygments-0.10-py2.4.egg
WARN Skipping file for pygments: Pygments-0.10-py2.5.egg
WARN Skipping file for pygments: Pygments-0.11-py2.3.egg
WARN Skipping file for pygments: Pygments-0.11-py2.4.egg
WARN Skipping file for pygments: Pygments-0.11-py2.5.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.11.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.5-py2.3.egg
WARN Skipping file for pygments: Pygments-0.5-py2.4.egg
WARN Skipping file for pygments: Pygments-0.5-py2.5.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.5.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.6-py2.3.egg
WARN Skipping file for pygments: Pygments-0.6-py2.4.egg
WARN Skipping file for pygments: Pygments-0.6-py2.5.egg
WARN Skipping file for pygments: Pygments-0.7-py2.3.egg
WARN Skipping file for pygments: Pygments-0.7-py2.4.egg
WARN Skipping file for pygments: Pygments-0.7-py2.5.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.7.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.8-py2.3.egg
WARN Skipping file for pygments: Pygments-0.8-py2.4.egg
WARN Skipping file for pygments: Pygments-0.8-py2.5.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.3.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.4.egg
WARN Skipping file for pygments: Pygments-0.8.1-py2.5.egg
WARN Skipping file for pygments: Pygments-0.9-py2.3.egg
WARN Skipping file for pygments: Pygments-0.9-py2.4.egg
WARN Skipping file for pygments: Pygments-0.9-py2.5.egg
WARN Skipping file for pygments: Pygments-1.0-py2.3.egg
WARN Skipping file for pygments: Pygments-1.0-py2.4.egg
WARN Skipping file for pygments: Pygments-1.0-py2.5.egg
WARN Skipping file for pygments: Pygments-1.0-py2.6.egg
WARN Skipping file for pygments: Pygments-1.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.1.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.4.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.5.egg
WARN Skipping file for pygments: Pygments-1.2.2-py2.6.egg
WARN Skipping file for pygments: Pygments-1.3-py2.4.egg
WARN Skipping file for pygments: Pygments-1.3-py2.5.egg
WARN Skipping file for pygments: Pygments-1.3-py2.6.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.4.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.5.egg
WARN Skipping file for pygments: Pygments-1.3.1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.4-py2.4.egg
WARN Skipping file for pygments: Pygments-1.4-py2.5.egg
WARN Skipping file for pygments: Pygments-1.4-py2.6.egg
WARN Skipping file for pygments: Pygments-1.4-py2.7.egg
WARN Skipping file for pygments: Pygments-1.5-py2.6.egg
WARN Skipping file for pygments: Pygments-1.5-py2.7.egg
WARN Skipping file for pygments: Pygments-1.6-py2.6.egg
WARN Skipping file for pygments: Pygments-1.6-py2.7.egg
WARN Skipping file for pygments: Pygments-1.6rc1-py2.6.egg
WARN Skipping file for pygments: Pygments-1.6rc1-py2.7.egg
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pygments.lock`
TRACE Received package metadata for: pygments
TRACE Selecting candidate for pygments with range >=2.19, <3 with 67 remote versions
TRACE Found candidate for package pygments with range >=2.19, <3 after 1 steps: 2.19.2 version
TRACE Returning candidate for package pygments with range >=2.19, <3 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pygments\pygments-2.19.2-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pygments\pygments-2.19.2-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pygments\pygments-2.19.2-py3-none-any.lock`
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/uvicorn/ is storable because it has a heuristically cacheable status code 200
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\uvicorn.lock`
TRACE Received package metadata for: uvicorn
TRACE Selecting candidate for uvicorn with range >=0.22.0 with 182 remote versions
TRACE Found candidate for package uvicorn with range >=0.22.0 after 1 steps: 0.38.0 version
TRACE Returning candidate for package uvicorn with range >=0.22.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\uvicorn\uvicorn-0.38.0-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\uvicorn\uvicorn-0.38.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\uvicorn\uvicorn-0.38.0-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/jedi/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/jedi/ is storable because it has a heuristically cacheable status code 200
WARN Skipping file for jedi: jedi-0.8.0-final0.tar.gz
WARN Skipping file for jedi: jedi-0.8.1-final0.tar.gz
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\jedi.lock`
TRACE Received package metadata for: jedi
TRACE Selecting candidate for jedi with range >=0.18.0 with 33 remote versions
TRACE Found candidate for package jedi with range >=0.18.0 after 1 steps: 0.19.2 version
TRACE Returning candidate for package jedi with range >=0.18.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\jedi\jedi-0.19.2-py2.py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\jedi\jedi-0.19.2-py2.py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\jedi\jedi-0.19.2-py2.py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/docutils/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/docutils/ is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\docutils.lock`
TRACE Received package metadata for: docutils
TRACE Selecting candidate for docutils with range >=0.16.0 with 55 remote versions
TRACE Found candidate for package docutils with range >=0.16.0 after 1 steps: 0.22.3 version
TRACE Returning candidate for package docutils with range >=0.16.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\docutils\docutils-0.22.3-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\docutils\docutils-0.22.3-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\docutils\docutils-0.22.3-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/itsdangerous/ is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\itsdangerous.lock`
TRACE Received package metadata for: itsdangerous
TRACE Selecting candidate for itsdangerous with range >=2.0.0 with 27 remote versions
TRACE Found candidate for package itsdangerous with range >=2.0.0 after 1 steps: 2.2.0 version
TRACE Returning candidate for package itsdangerous with range >=2.0.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\itsdangerous\itsdangerous-2.2.0-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\itsdangerous\itsdangerous-2.2.0-py3-none-any.lock`
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/tomlkit/ is storable because it has a heuristically cacheable status code 200
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\itsdangerous\itsdangerous-2.2.0-py3-none-any.lock`
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/packaging/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/packaging/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/pyyaml/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/msgspec-m/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/narwhals/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/starlette/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/starlette/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/psutil/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/psutil/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/websockets/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/websockets/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/loro/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/loro/ is storable because it has a heuristically cacheable status code 200
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\tomlkit.lock`
TRACE Received package metadata for: tomlkit
TRACE Selecting candidate for tomlkit with range >=0.12.0 with 53 remote versions
TRACE Found candidate for package tomlkit with range >=0.12.0 after 1 steps: 0.13.3 version
TRACE Returning candidate for package tomlkit with range >=0.12.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\tomlkit\tomlkit-0.13.3-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\tomlkit\tomlkit-0.13.3-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\tomlkit\tomlkit-0.13.3-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl does not have a fresh cache entry because its age is 288939 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\packaging.lock`
TRACE Received package metadata for: packaging
TRACE Selecting candidate for packaging with range * with 47 remote versions
TRACE Found candidate for package packaging with range * after 1 steps: 25.0 version
TRACE Returning candidate for package packaging with range * after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\packaging\packaging-25.0-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\packaging\packaging-25.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\packaging\packaging-25.0-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl does not have a fresh cache entry because its age is 285539 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.5.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.0.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.10.win32-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.3.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py2.6.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.1.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.2.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.3.exe
WARN Skipping file for pyyaml: PyYAML-3.11.win32-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win-amd64-py3.5.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-3.12.win32-py3.5.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win-amd64-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win-amd64-py3.4.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win32-py2.7.exe
WARN Skipping file for pyyaml: PyYAML-4.2b4.win32-py3.4.exe
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\pyyaml.lock`
TRACE Received package metadata for: pyyaml
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\msgspec-m.lock`
TRACE Received package metadata for: msgspec-m
TRACE Selecting candidate for pyyaml with range >=6.0 with 31 remote versions
TRACE Found candidate for package pyyaml with range >=6.0 after 1 steps: 6.0.3 version
TRACE Returning candidate for package pyyaml with range >=6.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pyyaml\pyyaml-6.0.3-cp313-cp313-win_amd64.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pyyaml\pyyaml-6.0.3-cp313-cp313-win_amd64.lock`
TRACE Selecting candidate for msgspec-m with range >=0.19.2 with 4 remote versions
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pyyaml\pyyaml-6.0.3-cp313-cp313-win_amd64.lock`
TRACE Found candidate for package msgspec-m with range >=0.19.2 after 1 steps: 0.19.3 version
TRACE Returning candidate for package msgspec-m with range >=0.19.2 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\msgspec-m\msgspec_m-0.19.3-cp313-cp313-win_amd64.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\msgspec-m\msgspec_m-0.19.3-cp313-cp313-win_amd64.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\msgspec-m\msgspec_m-0.19.3-cp313-cp313-win_amd64.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\narwhals.lock`
TRACE Received package metadata for: narwhals
TRACE Selecting candidate for narwhals with range >=2.0.0 with 226 remote versions
TRACE Found candidate for package narwhals with range >=2.0.0 after 1 steps: 2.12.0 version
TRACE Returning candidate for package narwhals with range >=2.0.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\narwhals-2.12.0-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\narwhals-2.12.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\narwhals-2.12.0-py3-none-any.lock`
TRACE No cache entry exists for C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\2.12.0-py3-none-any.msgpack
DEBUG No cache entry for: https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Sending fresh HEAD request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\starlette.lock`
TRACE Received package metadata for: starlette
TRACE Selecting candidate for starlette with range >=0.35.0, <0.36.0 | >=0.36.0+ with 186 remote versions
TRACE Found candidate for package starlette with range >=0.35.0, <0.36.0 | >=0.36.0+ after 1 steps: 0.50.0 version
TRACE Returning candidate for package starlette with range >=0.35.0, <0.36.0 | >=0.36.0+ after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\starlette\starlette-0.50.0-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\starlette\starlette-0.50.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\starlette\starlette-0.50.0-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl
WARN Skipping file for psutil: psutil-0.2.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.2.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.2.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.2.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.2.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.2.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.2.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.2.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.2.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.2.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.2.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.2.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.2.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.2.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.2.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.2.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.3.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.3.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.3.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.3.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.3.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.3.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.3.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.3.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.4.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.4.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.4.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.4.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.4.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.4.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.4.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.4.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.4.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.4.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.4.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.4.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.4.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.4.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.4.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.4.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.5.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.5.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.5.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.5.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.5.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.5.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.5.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.5.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.5.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.5.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.5.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.5.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.5.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.5.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.5.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.5.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.6.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.6.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.6.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.6.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.6.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.6.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.6.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.6.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.6.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.6.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.6.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.6.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.6.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.6.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.6.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.6.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.7.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.7.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.7.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.7.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.7.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.7.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.7.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.7.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-0.7.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-0.7.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-0.7.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-0.7.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-0.7.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-0.7.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-0.7.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-0.7.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-1.0.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-1.0.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-1.0.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-1.0.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-1.0.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-1.0.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-1.0.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-1.0.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-1.0.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-1.0.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-1.0.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-1.0.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-1.0.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-1.0.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-1.0.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-1.0.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-1.1.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-1.1.0.win-amd64-py3.2.exe
WARN Skipping file for psutil: psutil-1.1.0.win32-py2.4.exe
WARN Skipping file for psutil: psutil-1.1.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-1.1.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-1.1.0.win32-py3.2.exe
WARN Skipping file for psutil: psutil-1.1.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-1.1.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-1.1.1.win-amd64-py3.2.exe
WARN Skipping file for psutil: psutil-1.1.1.win32-py2.4.exe
WARN Skipping file for psutil: psutil-1.1.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-1.1.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-1.1.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-1.1.1.win32-py3.2.exe
WARN Skipping file for psutil: psutil-1.1.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-1.1.2.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-1.1.2.win-amd64-py3.2.exe
WARN Skipping file for psutil: psutil-1.1.2.win32-py2.4.exe
WARN Skipping file for psutil: psutil-1.1.2.win32-py2.5.exe
WARN Skipping file for psutil: psutil-1.1.2.win32-py2.6.exe
WARN Skipping file for psutil: psutil-1.1.2.win32-py2.7.exe
WARN Skipping file for psutil: psutil-1.1.2.win32-py3.2.exe
WARN Skipping file for psutil: psutil-1.1.2.win32-py3.3.exe
WARN Skipping file for psutil: psutil-1.1.3.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-1.1.3.win32-py2.4.exe
WARN Skipping file for psutil: psutil-1.1.3.win32-py2.5.exe
WARN Skipping file for psutil: psutil-1.1.3.win32-py2.6.exe
WARN Skipping file for psutil: psutil-1.1.3.win32-py2.7.exe
WARN Skipping file for psutil: psutil-1.1.3.win32-py3.2.exe
WARN Skipping file for psutil: psutil-1.1.3.win32-py3.3.exe
WARN Skipping file for psutil: psutil-1.2.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-1.2.0.win32-py2.4.exe
WARN Skipping file for psutil: psutil-1.2.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-1.2.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-1.2.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-1.2.0.win32-py3.2.exe
WARN Skipping file for psutil: psutil-1.2.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-1.2.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-1.2.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-1.2.1.win32-py2.4.exe
WARN Skipping file for psutil: psutil-1.2.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-1.2.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-1.2.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-1.2.1.win32-py3.2.exe
WARN Skipping file for psutil: psutil-1.2.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-2.0.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-2.0.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-2.0.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-2.0.0.win32-py2.4.exe
WARN Skipping file for psutil: psutil-2.0.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-2.0.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-2.0.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-2.0.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-2.0.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-2.1.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-2.1.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-2.1.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-2.1.0.win32-py2.4.exe
WARN Skipping file for psutil: psutil-2.1.0.win32-py2.5.exe
WARN Skipping file for psutil: psutil-2.1.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-2.1.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-2.1.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-2.1.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-2.1.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-2.1.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-2.1.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-2.1.1.win32-py2.4.exe
WARN Skipping file for psutil: psutil-2.1.1.win32-py2.5.exe
WARN Skipping file for psutil: psutil-2.1.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-2.1.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-2.1.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-2.1.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-2.1.2.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-2.1.2.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-2.1.2.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-2.1.2.win32-py2.4.exe
WARN Skipping file for psutil: psutil-2.1.2.win32-py2.5.exe
WARN Skipping file for psutil: psutil-2.1.2.win32-py2.6.exe
WARN Skipping file for psutil: psutil-2.1.2.win32-py2.7.exe
WARN Skipping file for psutil: psutil-2.1.2.win32-py3.3.exe
WARN Skipping file for psutil: psutil-2.1.2.win32-py3.4.exe
WARN Skipping file for psutil: psutil-2.1.3.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-2.1.3.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-2.1.3.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-2.1.3.win32-py2.4.exe
WARN Skipping file for psutil: psutil-2.1.3.win32-py2.5.exe
WARN Skipping file for psutil: psutil-2.1.3.win32-py2.6.exe
WARN Skipping file for psutil: psutil-2.1.3.win32-py2.7.exe
WARN Skipping file for psutil: psutil-2.1.3.win32-py3.3.exe
WARN Skipping file for psutil: psutil-2.1.3.win32-py3.4.exe
WARN Skipping file for psutil: psutil-2.2.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-2.2.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-2.2.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-2.2.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-2.2.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-2.2.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-2.2.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-2.2.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-2.2.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-2.2.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-2.2.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-2.2.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-2.2.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-2.2.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.0.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.0.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.0.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-3.0.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.0.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.0.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.0.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.0.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.0.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.0.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-3.0.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.0.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.0.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.0.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.1.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.1.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.1.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-3.1.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.1.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.1.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.1.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.1.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.1.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.1.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-3.1.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.1.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.1.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.1.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.2.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.2.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.2.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.2.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.2.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.2.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.2.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-3.2.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.2.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.2.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.2.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.2.2.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.2.2.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.2.2.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-3.2.2.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.2.2.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.2.2.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.2.2.win32-py3.5.exe
WARN Skipping file for psutil: psutil-3.3.0.win-amd64-py2.6.exe
WARN Skipping file for psutil: psutil-3.3.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.3.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.3.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-3.3.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-3.3.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.3.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.3.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.3.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.3.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-3.4.1.win-amd64-py2.6.exe
WARN Skipping file for psutil: psutil-3.4.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.4.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.4.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-3.4.1.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-3.4.1.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.4.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.4.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.4.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.4.1.win32-py3.5.exe
WARN Skipping file for psutil: psutil-3.4.2.win-amd64-py2.6.exe
WARN Skipping file for psutil: psutil-3.4.2.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-3.4.2.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-3.4.2.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-3.4.2.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-3.4.2.win32-py2.6.exe
WARN Skipping file for psutil: psutil-3.4.2.win32-py2.7.exe
WARN Skipping file for psutil: psutil-3.4.2.win32-py3.3.exe
WARN Skipping file for psutil: psutil-3.4.2.win32-py3.4.exe
WARN Skipping file for psutil: psutil-3.4.2.win32-py3.5.exe
WARN Skipping file for psutil: psutil-4.0.0.win-amd64-py2.6.exe
WARN Skipping file for psutil: psutil-4.0.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-4.0.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-4.0.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-4.0.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-4.0.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-4.0.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-4.0.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-4.0.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-4.0.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-4.1.0.win-amd64-py2.6.exe
WARN Skipping file for psutil: psutil-4.1.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-4.1.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-4.1.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-4.1.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-4.1.0.win32-py2.6.exe
WARN Skipping file for psutil: psutil-4.1.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-4.1.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-4.1.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-4.1.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-4.2.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-4.2.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-4.2.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-4.2.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-4.2.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-4.2.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-4.2.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-4.2.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-4.3.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-4.3.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-4.3.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-4.3.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-4.3.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-4.3.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-4.3.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-4.3.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-4.3.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-4.3.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-4.3.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-4.3.1.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-4.3.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-4.3.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-4.3.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-4.3.1.win32-py3.5.exe
WARN Skipping file for psutil: psutil-4.4.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-4.4.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-4.4.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-4.4.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-4.4.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-4.4.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-4.4.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-4.4.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-4.4.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-4.4.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-4.4.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-4.4.1.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-4.4.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-4.4.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-4.4.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-4.4.1.win32-py3.5.exe
WARN Skipping file for psutil: psutil-4.4.2.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-4.4.2.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-4.4.2.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-4.4.2.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-4.4.2.win32-py2.7.exe
WARN Skipping file for psutil: psutil-4.4.2.win32-py3.3.exe
WARN Skipping file for psutil: psutil-4.4.2.win32-py3.4.exe
WARN Skipping file for psutil: psutil-4.4.2.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.0.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.0.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.0.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.0.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.0.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.0.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.0.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.0.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.0.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.0.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.0.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.0.1.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.0.1.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.0.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.0.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.0.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.0.1.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.0.1.win32-py3.6.exe
WARN Skipping file for psutil: psutil-5.1.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.1.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.1.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.1.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.1.0.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.1.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.1.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.1.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.1.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.1.0.win32-py3.6.exe
WARN Skipping file for psutil: psutil-5.1.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.1.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.1.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.1.1.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.1.1.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.1.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.1.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.1.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.1.1.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.1.1.win32-py3.6.exe
WARN Skipping file for psutil: psutil-5.1.2.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.1.2.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.1.2.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.1.2.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.1.2.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.1.2.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.1.2.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.1.2.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.1.2.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.1.2.win32-py3.6.exe
WARN Skipping file for psutil: psutil-5.1.3.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.1.3.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.1.3.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.1.3.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.1.3.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.1.3.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.1.3.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.1.3.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.1.3.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.1.3.win32-py3.6.exe
WARN Skipping file for psutil: psutil-5.2.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.2.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.2.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.2.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.2.0.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.2.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.2.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.2.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.2.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.2.0.win32-py3.6.exe
WARN Skipping file for psutil: psutil-5.2.1.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.2.1.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.2.1.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.2.1.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.2.1.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.2.1.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.2.1.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.2.1.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.2.1.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.2.1.win32-py3.6.exe
WARN Skipping file for psutil: psutil-5.2.2.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.2.2.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.2.2.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.2.2.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.2.2.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.2.2.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.2.2.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.2.2.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.2.2.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.2.2.win32-py3.6.exe
WARN Skipping file for psutil: psutil-5.3.0.win-amd64-py2.7.exe
WARN Skipping file for psutil: psutil-5.3.0.win-amd64-py3.3.exe
WARN Skipping file for psutil: psutil-5.3.0.win-amd64-py3.4.exe
WARN Skipping file for psutil: psutil-5.3.0.win-amd64-py3.5.exe
WARN Skipping file for psutil: psutil-5.3.0.win-amd64-py3.6.exe
WARN Skipping file for psutil: psutil-5.3.0.win32-py2.7.exe
WARN Skipping file for psutil: psutil-5.3.0.win32-py3.3.exe
WARN Skipping file for psutil: psutil-5.3.0.win32-py3.4.exe
WARN Skipping file for psutil: psutil-5.3.0.win32-py3.5.exe
WARN Skipping file for psutil: psutil-5.3.0.win32-py3.6.exe
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\psutil.lock`
TRACE Received package metadata for: psutil
TRACE Selecting candidate for psutil with range >=5.0 with 99 remote versions
TRACE Found candidate for package psutil with range >=5.0 after 1 steps: 7.1.3 version
TRACE Returning candidate for package psutil with range >=5.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\psutil\psutil-7.1.3-cp37-abi3-win_amd64.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\psutil\psutil-7.1.3-cp37-abi3-win_amd64.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\psutil\psutil-7.1.3-cp37-abi3-win_amd64.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\websockets.lock`
TRACE Received package metadata for: websockets
TRACE Selecting candidate for websockets with range >=14.2.0 with 46 remote versions
TRACE Found candidate for package websockets with range >=14.2.0 after 1 steps: 15.0.1 version
TRACE Returning candidate for package websockets with range >=14.2.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\websockets\websockets-15.0.1-cp313-cp313-win_amd64.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\websockets\websockets-15.0.1-cp313-cp313-win_amd64.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\websockets\websockets-15.0.1-cp313-cp313-win_amd64.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for markdown-3.10-py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for click-8.3.1-py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for pymdown_extensions-10.17.1-py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for pygments-2.19.2-py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for uvicorn-0.38.0-py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for jedi-0.19.2-py2.py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for docutils-0.22.3-py3-none-any.whl by range request
TRACE Resource is not modified because old and new etag values ([34, 54, 54, 49, 101, 101, 100, 54, 101, 45, 51, 102, 54, 97, 34]) match
DEBUG Found not-modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/04/96/92447566d16df59b2a776c0fb82dbc4d9e07cd95062562af01e408583fc4/itsdangerous-2.2.0-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for tomlkit-0.13.3-py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for packaging-25.0-py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for pyyaml-6.0.3-cp313-cp313-win_amd64.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for msgspec_m-0.19.3-cp313-cp313-win_amd64.whl by range request
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for narwhals-2.12.0-py3-none-any.whl by range request
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for starlette-0.50.0-py3-none-any.whl by range request
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\itsdangerous\itsdangerous-2.2.0-py3-none-any.lock`
TRACE Received built distribution metadata for: itsdangerous==2.2.0
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\loro.lock`
TRACE Received package metadata for: loro
TRACE Selecting candidate for loro with range >=1.5.0 with 22 remote versions
TRACE Found candidate for package loro with range >=1.5.0 after 1 steps: 1.8.2 version
TRACE Returning candidate for package loro with range >=1.5.0 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\loro\loro-1.8.2-cp313-cp313-win_amd64.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\loro\loro-1.8.2-cp313-cp313-win_amd64.lock`
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl with authentication policy auto
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\loro\loro-1.8.2-cp313-cp313-win_amd64.lock`
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/70/81/54e3ce63502cd085a0c556652a4e1b919c45a446bd1e5300e10c44c8c521/markdown-3.10-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/98/78/01c019cdb5d6498122777c1a43056ebb3ebfeef2076d9d026bfe15583b2b/click-8.3.1-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/81/40/b2d7b9fdccc63e48ae4dbd363b6b89eb7ac346ea49ed667bb71f92af3021/pymdown_extensions-10.17.1-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/ee/d9/d88e73ca598f4f6ff671fb5fde8a32925c2e08a637303a1d12883c7305fa/uvicorn-0.38.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/bd/75/8539d011f6be8e29f339c42e633aae3cb73bffa95dd0f9adec09b9c58e85/tomlkit-0.13.3-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/20/12/38679034af332785aac8774540895e234f4d07f7545804097de4b666afd8/packaging-25.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/97/c9/39d5b874e8b28845e4ec2202b5da735d0199dbe5b8fb85f91398814a9a46/pyyaml-6.0.3-cp313-cp313-win_amd64.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/d5/38/90468da9a3af38a72d7bc4751ec62a1c812cdeb391b1f70d280c93561d1a/msgspec_m-0.19.3-cp313-cp313-win_amd64.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/d9/52/1064f510b141bd54025f9b55105e26d1fa970b9be67ad766380a3c9b74b0/starlette-0.50.0-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for psutil-7.1.3-cp37-abi3-win_amd64.whl by range request
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/55/4c/c3ed1a622b6ae2fd3c945a366e64eb35247a31e4db16cf5095e269e8eb3c/psutil-7.1.3-cp37-abi3-win_amd64.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for websockets-15.0.1-cp313-cp313-win_amd64.whl by range request
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/1b/6c/c65773d6cab416a64d191d6ee8a8b1c68a09970ea6909d16965d26bfed1e/websockets-15.0.1-cp313-cp313-win_amd64.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\markdown\markdown-3.10-py3-none-any.lock`
TRACE Received built distribution metadata for: markdown==3.10
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\click\click-8.3.1-py3-none-any.lock`
TRACE Received built distribution metadata for: click==8.3.1
DEBUG Adding transitive dependency for click==8.3.1: colorama{sys_platform == 'win32'}*
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1
TRACE Chose package for decision: jedi. remaining choices: loro, msgspec-m, markdown, pymdown-extensions, pygments, tomlkit, pyyaml, uvicorn, starlette, websockets, docutils, psutil, itsdangerous, narwhals, packaging, colorama
DEBUG Searching for a compatible version of jedi (>=0.18.0)
TRACE Selecting candidate for jedi with range >=0.18.0 with 33 remote versions
TRACE Found candidate for package jedi with range >=0.18.0 after 1 steps: 0.19.2 version
TRACE Returning candidate for package jedi with range >=0.18.0 after 1 steps
DEBUG Selecting: jedi==0.19.2 [compatible] (jedi-0.19.2-py2.py3-none-any.whl)
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pymdown-extensions\pymdown_extensions-10.17.1-py3-none-any.lock`
TRACE Received built distribution metadata for: pymdown-extensions==10.17.1
TRACE Fetching metadata for colorama from https://pypi.tuna.tsinghua.edu.cn/simple/colorama/
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\colorama.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\colorama.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\colorama.lock`
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\uvicorn\uvicorn-0.38.0-py3-none-any.lock`
TRACE Received built distribution metadata for: uvicorn==0.38.0
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\tomlkit\tomlkit-0.13.3-py3-none-any.lock`
TRACE Received built distribution metadata for: tomlkit==0.13.3
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/colorama/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/colorama/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/colorama/ does not have a fresh cache entry because its age is 285540 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/colorama/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/colorama/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/colorama/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/colorama/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/colorama/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/colorama/
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\packaging\packaging-25.0-py3-none-any.lock`
TRACE Received built distribution metadata for: packaging==25.0
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pyyaml\pyyaml-6.0.3-cp313-cp313-win_amd64.lock`
TRACE Received built distribution metadata for: pyyaml==6.0.3
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\msgspec-m\msgspec_m-0.19.3-cp313-cp313-win_amd64.lock`
TRACE Received built distribution metadata for: msgspec-m==0.19.3
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\starlette\starlette-0.50.0-py3-none-any.lock`
TRACE Received built distribution metadata for: starlette==0.50.0
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for loro-1.8.2-cp313-cp313-win_amd64.whl by range request
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\psutil\psutil-7.1.3-cp37-abi3-win_amd64.lock`
TRACE Received built distribution metadata for: psutil==7.1.3
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\websockets\websockets-15.0.1-cp313-cp313-win_amd64.lock`
TRACE Received built distribution metadata for: websockets==15.0.1
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/colorama/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/colorama/ is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\colorama.lock`
TRACE Received package metadata for: colorama
TRACE Selecting candidate for colorama with range * with 46 remote versions
TRACE Found candidate for package colorama with range * after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range * after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\colorama\colorama-0.4.6-py2.py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\colorama\colorama-0.4.6-py2.py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\colorama\colorama-0.4.6-py2.py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl does not have a fresh cache entry because its age is 285540 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\narwhals-2.12.0-py3-none-any.lock`
TRACE Received built distribution metadata for: narwhals==2.12.0
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c7/21/705964c7812476f378728bdf590ca4b771ec72385c533964653c68e86bdc/pygments-2.19.2-py3-none-any.whl
TRACE Resource is not modified because old and new etag values ([34, 54, 51, 53, 55, 52, 98, 97, 52, 45, 54, 50, 102, 55, 34]) match
DEBUG Found not-modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/d1/d6/3965ed04c63042e047cb6a3e6ed1a63a35087b6a609aa3a15ed8ac56c221/colorama-0.4.6-py2.py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/c0/5a/9cac0c82afec3d09ccd97c8b6502d48f165f9124db81b4bcb90b4af974ee/jedi-0.19.2-py2.py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/11/a8/c6a4b901d17399c77cd81fb001ce8961e9f5e04d3daf27e8925cb012e163/docutils-0.22.3-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\colorama\colorama-0.4.6-py2.py3-none-any.lock`
TRACE Received built distribution metadata for: colorama==0.4.6
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/79/68/2677ca414034f27a62fac7a504e776ba94167f9fb66c1c619b29ba6faa37/loro-1.8.2-cp313-cp313-win_amd64.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\pygments\pygments-2.19.2-py3-none-any.lock`
TRACE Received built distribution metadata for: pygments==2.19.2
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\jedi\jedi-0.19.2-py2.py3-none-any.lock`
TRACE Received built distribution metadata for: jedi==0.19.2
DEBUG Adding transitive dependency for jedi==0.19.2: parso>=0.8.4, <0.9.0
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2
TRACE Chose package for decision: markdown. remaining choices: loro, msgspec-m, parso, pymdown-extensions, pygments, tomlkit, pyyaml, uvicorn, starlette, websockets, docutils, psutil, itsdangerous, narwhals, packaging, colorama
DEBUG Searching for a compatible version of markdown (>=3.6, <4)
TRACE Selecting candidate for markdown with range >=3.6, <4 with 58 remote versions
TRACE Found candidate for package markdown with range >=3.6, <4 after 1 steps: 3.10 version
TRACE Returning candidate for package markdown with range >=3.6, <4 after 1 steps
DEBUG Selecting: markdown==3.10 [compatible] (markdown-3.10-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10
TRACE Chose package for decision: pymdown-extensions. remaining choices: loro, msgspec-m, parso, colorama, pygments, tomlkit, pyyaml, uvicorn, starlette, websockets, docutils, psutil, itsdangerous, narwhals, packaging
DEBUG Searching for a compatible version of pymdown-extensions (>=10.15, <11)
TRACE Selecting candidate for pymdown-extensions with range >=10.15, <11 with 109 remote versions
TRACE Found candidate for package pymdown-extensions with range >=10.15, <11 after 1 steps: 10.17.1 version
TRACE Returning candidate for package pymdown-extensions with range >=10.15, <11 after 1 steps
DEBUG Selecting: pymdown-extensions==10.17.1 [compatible] (pymdown_extensions-10.17.1-py3-none-any.whl)
DEBUG Adding transitive dependency for pymdown-extensions==10.17.1: markdown>=3.6
DEBUG Adding transitive dependency for pymdown-extensions==10.17.1: pyyaml*
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1
TRACE Chose package for decision: pygments. remaining choices: loro, msgspec-m, parso, colorama, packaging, tomlkit, pyyaml, uvicorn, starlette, websockets, docutils, psutil, itsdangerous, narwhals
DEBUG Searching for a compatible version of pygments (>=2.19, <3)
TRACE Selecting candidate for pygments with range >=2.19, <3 with 67 remote versions
TRACE Found candidate for package pygments with range >=2.19, <3 after 1 steps: 2.19.2 version
TRACE Returning candidate for package pygments with range >=2.19, <3 after 1 steps
DEBUG Selecting: pygments==2.19.2 [compatible] (pygments-2.19.2-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2
TRACE Chose package for decision: tomlkit. remaining choices: loro, msgspec-m, parso, colorama, packaging, narwhals, pyyaml, uvicorn, starlette, websockets, docutils, psutil, itsdangerous
DEBUG Searching for a compatible version of tomlkit (>=0.12.0)
TRACE Selecting candidate for tomlkit with range >=0.12.0 with 53 remote versions
TRACE Found candidate for package tomlkit with range >=0.12.0 after 1 steps: 0.13.3 version
TRACE Returning candidate for package tomlkit with range >=0.12.0 after 1 steps
DEBUG Selecting: tomlkit==0.13.3 [compatible] (tomlkit-0.13.3-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3
TRACE Chose package for decision: pyyaml. remaining choices: loro, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, uvicorn, starlette, websockets, docutils, psutil
DEBUG Searching for a compatible version of pyyaml (>=6.0)
TRACE Selecting candidate for pyyaml with range >=6.0 with 31 remote versions
TRACE Found candidate for package pyyaml with range >=6.0 after 1 steps: 6.0.3 version
TRACE Returning candidate for package pyyaml with range >=6.0 after 1 steps
DEBUG Selecting: pyyaml==6.0.3 [compatible] (pyyaml-6.0.3-cp313-cp313-win_amd64.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3
TRACE Chose package for decision: uvicorn. remaining choices: loro, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, psutil, starlette, websockets, docutils
DEBUG Searching for a compatible version of uvicorn (>=0.22.0)
TRACE Selecting candidate for uvicorn with range >=0.22.0 with 182 remote versions
TRACE Found candidate for package uvicorn with range >=0.22.0 after 1 steps: 0.38.0 version
TRACE Returning candidate for package uvicorn with range >=0.22.0 after 1 steps
DEBUG Selecting: uvicorn==0.38.0 [compatible] (uvicorn-0.38.0-py3-none-any.whl)
DEBUG Adding transitive dependency for uvicorn==0.38.0: click>=7.0
DEBUG Adding transitive dependency for uvicorn==0.38.0: h11>=0.8
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0
TRACE Chose package for decision: starlette. remaining choices: loro, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, psutil, h11, websockets, docutils
DEBUG Searching for a compatible version of starlette (>=0.35.0, <0.36.0 | >=0.36.0+)
TRACE Selecting candidate for starlette with range >=0.35.0, <0.36.0 | >=0.36.0+ with 186 remote versions
TRACE Found candidate for package starlette with range >=0.35.0, <0.36.0 | >=0.36.0+ after 1 steps: 0.50.0 version
TRACE Returning candidate for package starlette with range >=0.35.0, <0.36.0 | >=0.36.0+ after 1 steps
DEBUG Selecting: starlette==0.50.0 [compatible] (starlette-0.50.0-py3-none-any.whl)
DEBUG Adding transitive dependency for starlette==0.50.0: anyio>=3.6.2, <5
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0
TRACE Chose package for decision: websockets. remaining choices: loro, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, psutil, h11, anyio, docutils
DEBUG Searching for a compatible version of websockets (>=14.2.0)
TRACE Selecting candidate for websockets with range >=14.2.0 with 46 remote versions
TRACE Found candidate for package websockets with range >=14.2.0 after 1 steps: 15.0.1 version
TRACE Returning candidate for package websockets with range >=14.2.0 after 1 steps
DEBUG Selecting: websockets==15.0.1 [compatible] (websockets-15.0.1-cp313-cp313-win_amd64.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1
TRACE Chose package for decision: loro{python_full_version >= '3.11' and python_full_version < '3.14'}. remaining choices: loro, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, psutil, h11, anyio, docutils
DEBUG Searching for a compatible version of loro{python_full_version >= '3.11' and python_full_version < '3.14'} (>=1.5.0)
TRACE Selecting candidate for loro with range >=1.5.0 with 22 remote versions
TRACE Found candidate for package loro with range >=1.5.0 after 1 steps: 1.8.2 version
TRACE Returning candidate for package loro with range >=1.5.0 after 1 steps
DEBUG Selecting: loro==1.8.2 [compatible] (loro-1.8.2-cp313-cp313-win_amd64.whl)
DEBUG Adding transitive dependency for loro==1.8.2: loro==1.8.2
DEBUG Adding transitive dependency for loro==1.8.2: loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1
TRACE Chose package for decision: loro. remaining choices: loro{python_full_version >= '3.11' and python_full_version < '3.14'}, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, psutil, h11, anyio, docutils
DEBUG Searching for a compatible version of loro (==1.8.2)
TRACE Selecting candidate for loro with range ==1.8.2 with 22 remote versions
TRACE Found candidate for package loro with range ==1.8.2 after 1 steps: 1.8.2 version
TRACE Returning candidate for package loro with range ==1.8.2 after 1 steps
DEBUG Selecting: loro==1.8.2 [compatible] (loro-1.8.2-cp313-cp313-win_amd64.whl)
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\docutils\docutils-0.22.3-py3-none-any.lock`
TRACE Received built distribution metadata for: docutils==0.22.3
TRACE Fetching metadata for parso from https://pypi.tuna.tsinghua.edu.cn/simple/parso/
TRACE Fetching metadata for h11 from https://pypi.tuna.tsinghua.edu.cn/simple/h11/
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\parso.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\parso.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\parso.lock`
TRACE Fetching metadata for anyio from https://pypi.tuna.tsinghua.edu.cn/simple/anyio/
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\h11.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\h11.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\h11.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\anyio.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\anyio.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\anyio.lock`
TRACE Selecting candidate for loro with range ==1.8.2 with 22 remote versions
TRACE Found candidate for package loro with range ==1.8.2 after 1 steps: 1.8.2 version
TRACE Returning candidate for package loro with range ==1.8.2 after 1 steps
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/parso/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/parso/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/parso/ does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/parso/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/parso/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/parso/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/parso/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/parso/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/parso/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/h11/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/h11/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/h11/ does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/h11/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/h11/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/h11/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/h11/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/h11/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/h11/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/anyio/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/anyio/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/anyio/ does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/anyio/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/anyio/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/anyio/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/anyio/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/anyio/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/anyio/
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\loro\loro-1.8.2-cp313-cp313-win_amd64.lock`
TRACE Received built distribution metadata for: loro==1.8.2
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2
TRACE Chose package for decision: loro{python_full_version >= '3.11' and python_full_version < '3.14'}. remaining choices: docutils, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, psutil, h11, anyio
DEBUG Searching for a compatible version of loro{python_full_version >= '3.11' and python_full_version < '3.14'} (==1.8.2)
TRACE Selecting candidate for loro with range ==1.8.2 with 22 remote versions
TRACE Found candidate for package loro with range ==1.8.2 after 1 steps: 1.8.2 version
TRACE Returning candidate for package loro with range ==1.8.2 after 1 steps
DEBUG Selecting: loro==1.8.2 [compatible] (loro-1.8.2-cp313-cp313-win_amd64.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2
TRACE Chose package for decision: docutils. remaining choices: anyio, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, psutil, h11
DEBUG Searching for a compatible version of docutils (>=0.16.0)
TRACE Selecting candidate for docutils with range >=0.16.0 with 55 remote versions
TRACE Found candidate for package docutils with range >=0.16.0 after 1 steps: 0.22.3 version
TRACE Returning candidate for package docutils with range >=0.16.0 after 1 steps
DEBUG Selecting: docutils==0.22.3 [compatible] (docutils-0.22.3-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3
TRACE Chose package for decision: psutil. remaining choices: anyio, msgspec-m, parso, colorama, packaging, narwhals, itsdangerous, h11
DEBUG Searching for a compatible version of psutil (>=5.0)
TRACE Selecting candidate for psutil with range >=5.0 with 99 remote versions
TRACE Found candidate for package psutil with range >=5.0 after 1 steps: 7.1.3 version
TRACE Returning candidate for package psutil with range >=5.0 after 1 steps
DEBUG Selecting: psutil==7.1.3 [compatible] (psutil-7.1.3-cp37-abi3-win_amd64.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3
TRACE Chose package for decision: itsdangerous. remaining choices: anyio, msgspec-m, parso, colorama, packaging, narwhals, h11
DEBUG Searching for a compatible version of itsdangerous (>=2.0.0)
TRACE Selecting candidate for itsdangerous with range >=2.0.0 with 27 remote versions
TRACE Found candidate for package itsdangerous with range >=2.0.0 after 1 steps: 2.2.0 version
TRACE Returning candidate for package itsdangerous with range >=2.0.0 after 1 steps
DEBUG Selecting: itsdangerous==2.2.0 [compatible] (itsdangerous-2.2.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0
TRACE Chose package for decision: narwhals. remaining choices: anyio, msgspec-m, parso, colorama, packaging, h11
DEBUG Searching for a compatible version of narwhals (>=2.0.0)
TRACE Selecting candidate for narwhals with range >=2.0.0 with 226 remote versions
TRACE Found candidate for package narwhals with range >=2.0.0 after 1 steps: 2.12.0 version
TRACE Returning candidate for package narwhals with range >=2.0.0 after 1 steps
DEBUG Selecting: narwhals==2.12.0 [compatible] (narwhals-2.12.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0
TRACE Chose package for decision: packaging. remaining choices: anyio, msgspec-m, parso, colorama, h11
DEBUG Searching for a compatible version of packaging (*)
TRACE Selecting candidate for packaging with range * with 47 remote versions
TRACE Found candidate for package packaging with range * after 1 steps: 25.0 version
TRACE Returning candidate for package packaging with range * after 1 steps
DEBUG Selecting: packaging==25.0 [compatible] (packaging-25.0-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0
TRACE Chose package for decision: msgspec-m. remaining choices: anyio, h11, parso, colorama
DEBUG Searching for a compatible version of msgspec-m (>=0.19.2)
TRACE Selecting candidate for msgspec-m with range >=0.19.2 with 4 remote versions
TRACE Found candidate for package msgspec-m with range >=0.19.2 after 1 steps: 0.19.3 version
TRACE Returning candidate for package msgspec-m with range >=0.19.2 after 1 steps
DEBUG Selecting: msgspec-m==0.19.3 [compatible] (msgspec_m-0.19.3-cp313-cp313-win_amd64.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3
TRACE Chose package for decision: colorama{sys_platform == 'win32'}. remaining choices: anyio, h11, colorama, parso
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (*)
TRACE Selecting candidate for colorama with range * with 46 remote versions
TRACE Found candidate for package colorama with range * after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range * after 1 steps
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
DEBUG Adding transitive dependency for colorama==0.4.6: colorama==0.4.6
DEBUG Adding transitive dependency for colorama==0.4.6: colorama{sys_platform == 'win32'}==0.4.6
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3
TRACE Selecting candidate for colorama with range ==0.4.6 with 46 remote versions
TRACE Found candidate for package colorama with range ==0.4.6 after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range ==0.4.6 after 1 steps
TRACE Chose package for decision: colorama. remaining choices: anyio, h11, colorama{sys_platform == 'win32'}, parso
DEBUG Searching for a compatible version of colorama (==0.4.6)
TRACE Selecting candidate for colorama with range ==0.4.6 with 46 remote versions
TRACE Found candidate for package colorama with range ==0.4.6 after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range ==0.4.6 after 1 steps
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3, colorama==0.4.6
TRACE Chose package for decision: colorama{sys_platform == 'win32'}. remaining choices: anyio, h11, parso
DEBUG Searching for a compatible version of colorama{sys_platform == 'win32'} (==0.4.6)
TRACE Selecting candidate for colorama with range ==0.4.6 with 46 remote versions
TRACE Found candidate for package colorama with range ==0.4.6 after 1 steps: 0.4.6 version
TRACE Returning candidate for package colorama with range ==0.4.6 after 1 steps
DEBUG Selecting: colorama==0.4.6 [compatible] (colorama-0.4.6-py2.py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3, colorama==0.4.6, colorama{sys_platform == 'win32'}==0.4.6
TRACE Chose package for decision: parso. remaining choices: anyio, h11
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/parso/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/parso/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/h11/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/h11/ is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\parso.lock`
TRACE Received package metadata for: parso
DEBUG Searching for a compatible version of parso (>=0.8.4, <0.9.0)
TRACE Selecting candidate for parso with range >=0.8.4, <0.9.0 with 28 remote versions
TRACE Found candidate for package parso with range >=0.8.4, <0.9.0 after 1 steps: 0.8.5 version
TRACE Returning candidate for package parso with range >=0.8.4, <0.9.0 after 1 steps
DEBUG Selecting: parso==0.8.5 [compatible] (parso-0.8.5-py2.py3-none-any.whl)
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\h11.lock`
TRACE Received package metadata for: h11
TRACE Selecting candidate for parso with range >=0.8.4, <0.9.0 with 28 remote versions
TRACE Found candidate for package parso with range >=0.8.4, <0.9.0 after 1 steps: 0.8.5 version
TRACE Returning candidate for package parso with range >=0.8.4, <0.9.0 after 1 steps
TRACE Selecting candidate for h11 with range >=0.8 with 13 remote versions
TRACE Found candidate for package h11 with range >=0.8 after 1 steps: 0.16.0 version
TRACE Returning candidate for package h11 with range >=0.8 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\h11\h11-0.16.0-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\h11\h11-0.16.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\h11\h11-0.16.0-py3-none-any.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\parso\parso-0.8.5-py2.py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\parso\parso-0.8.5-py2.py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\parso\parso-0.8.5-py2.py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/anyio/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/anyio/ is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\anyio.lock`
TRACE Received package metadata for: anyio
TRACE Selecting candidate for anyio with range >=3.6.2, <5 with 64 remote versions
TRACE Found candidate for package anyio with range >=3.6.2, <5 after 1 steps: 4.11.0 version
TRACE Returning candidate for package anyio with range >=3.6.2, <5 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\anyio\anyio-4.11.0-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\anyio\anyio-4.11.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\anyio\anyio-4.11.0-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl does not have a fresh cache entry because its age is 288940 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for parso-0.8.5-py2.py3-none-any.whl by range request
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/16/32/f8e3c85d1d5250232a5d3477a2a28cc291968ff175caeadaf3cc19ce0e4a/parso-0.8.5-py2.py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for h11-0.16.0-py3-none-any.whl by range request
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/04/4b/29cac41a4d98d144bf5f6d33995617b185d14b22401f75ca86f384e87ff1/h11-0.16.0-py3-none-any.whl
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for anyio-4.11.0-py3-none-any.whl by range request
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/15/b3/9b1a8074496371342ec1e796a96f99c82c945a339cd81a8e73de28b4cf9e/anyio-4.11.0-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\parso\parso-0.8.5-py2.py3-none-any.lock`
TRACE Received built distribution metadata for: parso==0.8.5
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3, colorama==0.4.6, colorama{sys_platform == 'win32'}==0.4.6, parso==0.8.5
TRACE Chose package for decision: h11. remaining choices: anyio
DEBUG Searching for a compatible version of h11 (>=0.8)
TRACE Selecting candidate for h11 with range >=0.8 with 13 remote versions
TRACE Found candidate for package h11 with range >=0.8 after 1 steps: 0.16.0 version
TRACE Returning candidate for package h11 with range >=0.8 after 1 steps
DEBUG Selecting: h11==0.16.0 [compatible] (h11-0.16.0-py3-none-any.whl)
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\h11\h11-0.16.0-py3-none-any.lock`
TRACE Received built distribution metadata for: h11==0.16.0
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3, colorama==0.4.6, colorama{sys_platform == 'win32'}==0.4.6, parso==0.8.5, h11==0.16.0
TRACE Chose package for decision: anyio. remaining choices:
DEBUG Searching for a compatible version of anyio (>=3.6.2, <5)
TRACE Selecting candidate for anyio with range >=3.6.2, <5 with 64 remote versions
TRACE Found candidate for package anyio with range >=3.6.2, <5 after 1 steps: 4.11.0 version
TRACE Returning candidate for package anyio with range >=3.6.2, <5 after 1 steps
DEBUG Selecting: anyio==4.11.0 [compatible] (anyio-4.11.0-py3-none-any.whl)
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\anyio\anyio-4.11.0-py3-none-any.lock`
TRACE Received built distribution metadata for: anyio==4.11.0
DEBUG Adding transitive dependency for anyio==4.11.0: idna>=2.8
DEBUG Adding transitive dependency for anyio==4.11.0: sniffio>=1.1
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3, colorama==0.4.6, colorama{sys_platform == 'win32'}==0.4.6, parso==0.8.5, h11==0.16.0, anyio==4.11.0
TRACE Chose package for decision: idna. remaining choices: sniffio
TRACE Fetching metadata for idna from https://pypi.tuna.tsinghua.edu.cn/simple/idna/
TRACE Fetching metadata for sniffio from https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\idna.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\idna.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\idna.lock`
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\sniffio.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\sniffio.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\sniffio.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/ does not have a fresh cache entry because its age is 288941 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/idna/ is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/idna/ has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/simple/idna/ does not have a fresh cache entry because its age is 288941 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/simple/idna/
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/simple/idna/
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/simple/idna/ with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/simple/idna/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/simple/idna/
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/simple/idna/
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/sniffio/ is storable because it has a heuristically cacheable status code 200
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/simple/idna/
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/simple/idna/ is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\sniffio.lock`
TRACE Received package metadata for: sniffio
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\simple-v18\index\46901b1a4cb2cba0\idna.lock`
TRACE Received package metadata for: idna
TRACE Selecting candidate for sniffio with range >=1.1 with 6 remote versions
TRACE Found candidate for package sniffio with range >=1.1 after 1 steps: 1.3.1 version
TRACE Returning candidate for package sniffio with range >=1.1 after 1 steps
DEBUG Searching for a compatible version of idna (>=2.8)
TRACE Selecting candidate for idna with range >=2.8 with 33 remote versions
TRACE Found candidate for package idna with range >=2.8 after 1 steps: 3.11 version
TRACE Returning candidate for package idna with range >=2.8 after 1 steps
DEBUG Selecting: idna==3.11 [compatible] (idna-3.11-py3-none-any.whl)
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\sniffio\sniffio-1.3.1-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\sniffio\sniffio-1.3.1-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\sniffio\sniffio-1.3.1-py3-none-any.lock`
TRACE Selecting candidate for idna with range >=2.8 with 33 remote versions
TRACE Found candidate for package idna with range >=2.8 after 1 steps: 3.11 version
TRACE Returning candidate for package idna with range >=2.8 after 1 steps
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\idna\idna-3.11-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\idna\idna-3.11-py3-none-any.lock`
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl does not have a fresh cache entry because its age is 288941 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\idna\idna-3.11-py3-none-any.lock`
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Freshness lifetime heuristically assumed because of presence of last-modified header: 600s
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl has a cached response that does not allow staleness
TRACE Request to https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl does not have a fresh cache entry because its age is 288941 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl
DEBUG Sending revalidation request for: https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl
TRACE Resource is not modified because old and new etag values ([34, 54, 53, 100, 98, 99, 98, 50, 49, 45, 50, 55, 102, 98, 34]) match
DEBUG Found not-modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\sniffio\sniffio-1.3.1-py3-none-any.lock`
TRACE Received built distribution metadata for: sniffio==1.3.1
TRACE Resource is modified because status is 200 and not 304
DEBUG Found modified response for: https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
TRACE Getting metadata for idna-3.11-py3-none-any.whl by range request
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/0e/61/66938bbb5fc52dbdf84594873d5b51fb1f7c7794e9c0f5bd885f30bc507b/idna-3.11-py3-none-any.whl
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\idna\idna-3.11-py3-none-any.lock`
TRACE Received built distribution metadata for: idna==3.11
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3, colorama==0.4.6, colorama{sys_platform == 'win32'}==0.4.6, parso==0.8.5, h11==0.16.0, anyio==4.11.0, idna==3.11
TRACE Chose package for decision: sniffio. remaining choices:
DEBUG Searching for a compatible version of sniffio (>=1.1)
TRACE Selecting candidate for sniffio with range >=1.1 with 6 remote versions
TRACE Found candidate for package sniffio with range >=1.1 after 1 steps: 1.3.1 version
TRACE Returning candidate for package sniffio with range >=1.1 after 1 steps
DEBUG Selecting: sniffio==1.3.1 [compatible] (sniffio-1.3.1-py3-none-any.whl)
TRACE Assigned packages: root==0a0.dev0, marimo==0.17.8, click==8.3.1, jedi==0.19.2, markdown==3.10, pymdown-extensions==10.17.1, pygments==2.19.2, tomlkit==0.13.3, pyyaml==6.0.3, uvicorn==0.38.0, starlette==0.50.0, websockets==15.0.1, loro==1.8.2, loro{python_full_version >= '3.11' and python_full_version < '3.14'}==1.8.2, docutils==0.22.3, psutil==7.1.3, itsdangerous==2.2.0, narwhals==2.12.0, packaging==25.0, msgspec-m==0.19.3, colorama==0.4.6, colorama{sys_platform == 'win32'}==0.4.6, parso==0.8.5, h11==0.16.0, anyio==4.11.0, idna==3.11, sniffio==1.3.1
DEBUG Tried 24 versions: anyio 1, click 1, colorama 1, docutils 1, h11 1, idna 1, itsdangerous 1, jedi 1, loro 1, marimo 1, markdown 1, msgspec-m 1, narwhals 1, packaging 1, parso 1, psutil 1, pygments 1, pymdown-extensions 1, pyyaml 1, sniffio 1, starlette 1, tomlkit 1, uvicorn 1, websockets 1
DEBUG marker environment resolution took 3.130s
TRACE Resolution: ResolverEnvironment { kind: Specific { marker_env: ResolverMarkerEnvironment(MarkerEnvironment { inner: MarkerEnvironmentInner { implementation_name: "cpython", implementation_version: StringVersion { string: "3.13.9", version: "3.13.9" }, os_name: "nt", platform_machine: "AMD64", platform_python_implementation: "CPython", platform_release: "11", platform_system: "Windows", platform_version: "10.0.26100", python_full_version: StringVersion { string: "3.13.9", version: "3.13.9" }, python_version: StringVersion { string: "3.13", version: "3.13" }, sys_platform: "win32" } }) } }
TRACE Resolution edge: ROOT -> marimo
TRACE Resolution edge:     0a0.dev0 -> 0.17.8
TRACE Resolution edge: starlette -> anyio
TRACE Resolution edge:     0.50.0 -> 4.11.0
TRACE Resolution edge: anyio -> idna
TRACE Resolution edge:     4.11.0 -> 3.11
TRACE Resolution edge: anyio -> sniffio
TRACE Resolution edge:     4.11.0 -> 1.3.1
TRACE Resolution edge: jedi -> parso
TRACE Resolution edge:     0.19.2 -> 0.8.5
TRACE Resolution edge: uvicorn -> click
TRACE Resolution edge:     0.38.0 -> 8.3.1
TRACE Resolution edge: uvicorn -> h11
TRACE Resolution edge:     0.38.0 -> 0.16.0
TRACE Resolution edge: marimo -> click
TRACE Resolution edge:     0.17.8 -> 8.3.1
TRACE Resolution edge: marimo -> jedi
TRACE Resolution edge:     0.17.8 -> 0.19.2
TRACE Resolution edge: marimo -> markdown
TRACE Resolution edge:     0.17.8 -> 3.10
TRACE Resolution edge: marimo -> pymdown-extensions
TRACE Resolution edge:     0.17.8 -> 10.17.1
TRACE Resolution edge: marimo -> pygments
TRACE Resolution edge:     0.17.8 -> 2.19.2
TRACE Resolution edge: marimo -> tomlkit
TRACE Resolution edge:     0.17.8 -> 0.13.3
TRACE Resolution edge: marimo -> pyyaml
TRACE Resolution edge:     0.17.8 -> 6.0.3
TRACE Resolution edge: marimo -> uvicorn
TRACE Resolution edge:     0.17.8 -> 0.38.0
TRACE Resolution edge: marimo -> starlette
TRACE Resolution edge:     0.17.8 -> 0.50.0
TRACE Resolution edge: marimo -> websockets
TRACE Resolution edge:     0.17.8 -> 15.0.1
TRACE Resolution edge: marimo -> loro
TRACE Resolution edge:     0.17.8 -> 1.8.2 ; python_full_version >= '3.11' and python_full_version < '3.14'
TRACE Resolution edge: marimo -> docutils
TRACE Resolution edge:     0.17.8 -> 0.22.3
TRACE Resolution edge: marimo -> psutil
TRACE Resolution edge:     0.17.8 -> 7.1.3
TRACE Resolution edge: marimo -> itsdangerous
TRACE Resolution edge:     0.17.8 -> 2.2.0
TRACE Resolution edge: marimo -> narwhals
TRACE Resolution edge:     0.17.8 -> 2.12.0
TRACE Resolution edge: marimo -> packaging
TRACE Resolution edge:     0.17.8 -> 25.0
TRACE Resolution edge: marimo -> msgspec-m
TRACE Resolution edge:     0.17.8 -> 0.19.3
TRACE Resolution edge: click -> colorama
TRACE Resolution edge:     8.3.1 -> 0.4.6 ; sys_platform == 'win32'
TRACE Resolution edge: pymdown-extensions -> markdown
TRACE Resolution edge:     10.17.1 -> 3.10
TRACE Resolution edge: pymdown-extensions -> pyyaml
TRACE Resolution edge:     10.17.1 -> 6.0.3
Resolved 24 packages in 3.13s
DEBUG Using base executable for virtual environment: C:\Users\zhoumeng\AppData\Roaming\uv\python\cpython-3.13.9-windows-x86_64-none\python.exe
TRACE Using empty directory at `C:\Users\zhoumeng\AppData\Local\uv\cache\builds-v0\.tmpLVYlPM` for virtual environment
DEBUG Using request timeout of 30s
DEBUG Registry requirement already cached: itsdangerous==2.2.0
DEBUG Registry requirement already cached: pyyaml==6.0.3
DEBUG Registry requirement already cached: jedi==0.19.2
DEBUG Registry requirement already cached: marimo==0.17.8
DEBUG Registry requirement already cached: uvicorn==0.38.0
DEBUG Registry requirement already cached: docutils==0.22.3
DEBUG Registry requirement already cached: h11==0.16.0
DEBUG Registry requirement already cached: loro==1.8.2
DEBUG Registry requirement already cached: tomlkit==0.13.3
DEBUG Registry requirement already cached: sniffio==1.3.1
DEBUG Registry requirement already cached: pymdown-extensions==10.17.1
DEBUG Registry requirement already cached: markdown==3.10
DEBUG Registry requirement already cached: starlette==0.50.0
DEBUG Registry requirement already cached: websockets==15.0.1
DEBUG Registry requirement already cached: idna==3.11
DEBUG Registry requirement already cached: click==8.3.1
DEBUG Registry requirement already cached: msgspec-m==0.19.3
DEBUG Registry requirement already cached: parso==0.8.5
DEBUG Registry requirement already cached: pygments==2.19.2
DEBUG Registry requirement already cached: colorama==0.4.6
DEBUG Registry requirement already cached: anyio==4.11.0
DEBUG Registry requirement already cached: psutil==7.1.3
DEBUG Identified uncached distribution: narwhals==2.12.0
DEBUG Registry requirement already cached: packaging==25.0
TRACE Checking lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\narwhals-2.12.0-py3-none-any.lock` at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\narwhals-2.12.0-py3-none-any.lock`
DEBUG Acquired lock for `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\narwhals-2.12.0-py3-none-any.lock`
TRACE No cache entry exists for C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\2.12.0-py3-none-any.http
DEBUG No cache entry for: https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Sending fresh GET request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Handling request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl with authentication policy auto
TRACE Request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl
TRACE Response from https://pypi.tuna.tsinghua.edu.cn/packages/0b/9a/c6f79de7ba3a0a8473129936b7b90aa461d3d46fec6f1627672b1dccf4e9/narwhals-2.12.0-py3-none-any.whl is storable because it has a heuristically cacheable status code 200
DEBUG Released lock at `C:\Users\zhoumeng\AppData\Local\uv\cache\wheels-v5\index\46901b1a4cb2cba0\narwhals\narwhals-2.12.0-py3-none-any.lock`
Prepared 1 package in 887ms
TRACE Extracting file name=PackageName("narwhals")
TRACE Extracting file name=PackageName("sniffio")
TRACE Extracting file name=PackageName("tomlkit")
TRACE Extracting file name=PackageName("click")
TRACE Extracting file name=PackageName("h11")
TRACE Extracting file name=PackageName("markdown")
TRACE Extracting file name=PackageName("parso")
TRACE Extracting file name=PackageName("itsdangerous")
TRACE Extracting file name=PackageName("msgspec-m")
TRACE Extracting file name=PackageName("jedi")
TRACE Extracting file name=PackageName("docutils")
TRACE Extracting file name=PackageName("pygments")
TRACE Extracting file name=PackageName("colorama")
TRACE Extracting file name=PackageName("idna")
TRACE Extracting file name=PackageName("pymdown-extensions")
TRACE Extracting file name=PackageName("packaging")
TRACE Extracting file name=PackageName("anyio")
TRACE Extracting file name=PackageName("starlette")
TRACE Extracting file name=PackageName("loro")
TRACE Extracting file name=PackageName("marimo")
TRACE Extracting file name=PackageName("psutil")
TRACE Extracting file name=PackageName("uvicorn")
TRACE Extracting file name=PackageName("pyyaml")
TRACE Extracting file name=PackageName("websockets")
TRACE Extracted 18 files name=PackageName("tomlkit")
TRACE No entrypoints name=PackageName("tomlkit")
TRACE No data name=PackageName("tomlkit")
TRACE Writing installer metadata name=PackageName("tomlkit")
TRACE Extracted 13 files name=PackageName("sniffio")
TRACE No entrypoints name=PackageName("sniffio")
TRACE No data name=PackageName("sniffio")
TRACE Writing installer metadata name=PackageName("sniffio")
TRACE Extracted 13 files name=PackageName("itsdangerous")
TRACE No entrypoints name=PackageName("itsdangerous")
TRACE No data name=PackageName("itsdangerous")
TRACE Writing installer metadata name=PackageName("itsdangerous")
TRACE Extracted 8 files name=PackageName("loro")
TRACE No entrypoints name=PackageName("loro")
TRACE No data name=PackageName("loro")
TRACE Writing installer metadata name=PackageName("loro")
TRACE Extracted 21 files name=PackageName("msgspec-m")
TRACE No entrypoints name=PackageName("msgspec-m")
TRACE No data name=PackageName("msgspec-m")
TRACE Writing installer metadata name=PackageName("msgspec-m")
TRACE Extracted 17 files name=PackageName("h11")
TRACE No entrypoints name=PackageName("h11")
TRACE No data name=PackageName("h11")
TRACE Writing installer metadata name=PackageName("h11")
TRACE Writing record name=PackageName("tomlkit")
TRACE Extracted 13 files name=PackageName("idna")
TRACE No entrypoints name=PackageName("idna")
TRACE No data name=PackageName("idna")
TRACE Writing installer metadata name=PackageName("idna")
TRACE Extracted 22 files name=PackageName("click")
TRACE No entrypoints name=PackageName("click")
TRACE No data name=PackageName("click")
TRACE Writing installer metadata name=PackageName("click")
TRACE Writing record name=PackageName("sniffio")
TRACE Writing record name=PackageName("itsdangerous")
TRACE Writing record name=PackageName("h11")
TRACE Writing record name=PackageName("loro")
TRACE Writing record name=PackageName("msgspec-m")
TRACE Extracted 17 files name=PackageName("colorama")
TRACE No entrypoints name=PackageName("colorama")
TRACE No data name=PackageName("colorama")
TRACE Writing installer metadata name=PackageName("colorama")
TRACE Writing record name=PackageName("idna")
TRACE Writing record name=PackageName("click")
TRACE Extracted 23 files name=PackageName("packaging")
TRACE No entrypoints name=PackageName("packaging")
TRACE No data name=PackageName("packaging")
TRACE Extracted 24 files name=PackageName("pyyaml")
TRACE Writing installer metadata name=PackageName("packaging")
TRACE No entrypoints name=PackageName("pyyaml")
TRACE No data name=PackageName("pyyaml")
TRACE Writing installer metadata name=PackageName("pyyaml")
TRACE Writing record name=PackageName("colorama")
TRACE Extracted 39 files name=PackageName("markdown")
TRACE Writing entrypoints name=PackageName("markdown")
TRACE Extracted 46 files name=PackageName("pymdown-extensions")
TRACE No entrypoints name=PackageName("pymdown-extensions")
TRACE No data name=PackageName("pymdown-extensions")
TRACE Writing installer metadata name=PackageName("pymdown-extensions")
TRACE Extracted 37 files name=PackageName("parso")
TRACE Writing record name=PackageName("packaging")
TRACE Writing record name=PackageName("pyyaml")
TRACE No entrypoints name=PackageName("parso")
TRACE No data name=PackageName("parso")
TRACE Writing installer metadata name=PackageName("parso")
TRACE Writing record name=PackageName("pymdown-extensions")
TRACE Extracted 35 files name=PackageName("psutil")
TRACE No entrypoints name=PackageName("psutil")
TRACE No data name=PackageName("psutil")
TRACE Writing installer metadata name=PackageName("psutil")
TRACE Writing record name=PackageName("parso")
TRACE Extracted 39 files name=PackageName("starlette")
TRACE No entrypoints name=PackageName("starlette")
TRACE No data name=PackageName("starlette")
TRACE Writing installer metadata name=PackageName("starlette")
TRACE Writing record name=PackageName("psutil")
TRACE No data name=PackageName("markdown")
TRACE Writing installer metadata name=PackageName("markdown")
TRACE Writing record name=PackageName("starlette")
TRACE Extracted 46 files name=PackageName("uvicorn")
TRACE Writing entrypoints name=PackageName("uvicorn")
TRACE Extracted 48 files name=PackageName("anyio")
TRACE No entrypoints name=PackageName("anyio")
TRACE No data name=PackageName("anyio")
TRACE Writing installer metadata name=PackageName("anyio")
TRACE Writing record name=PackageName("markdown")
TRACE Extracted 57 files name=PackageName("websockets")
TRACE Writing entrypoints name=PackageName("websockets")
TRACE Writing record name=PackageName("anyio")
TRACE No data name=PackageName("uvicorn")
TRACE Writing installer metadata name=PackageName("uvicorn")
TRACE Writing record name=PackageName("uvicorn")
TRACE Extracted 165 files name=PackageName("narwhals")
TRACE No entrypoints name=PackageName("narwhals")
TRACE No data name=PackageName("narwhals")
TRACE Writing installer metadata name=PackageName("narwhals")
TRACE Writing record name=PackageName("narwhals")
TRACE No data name=PackageName("websockets")
TRACE Writing installer metadata name=PackageName("websockets")
TRACE Writing record name=PackageName("websockets")
TRACE Extracted 212 files name=PackageName("docutils")
TRACE Writing entrypoints name=PackageName("docutils")
TRACE Extracted 344 files name=PackageName("pygments")
TRACE Writing entrypoints name=PackageName("pygments")
TRACE No data name=PackageName("pygments")
TRACE Writing installer metadata name=PackageName("pygments")
TRACE Writing record name=PackageName("pygments")
TRACE No data name=PackageName("docutils")
TRACE Writing installer metadata name=PackageName("docutils")
TRACE Writing record name=PackageName("docutils")
TRACE Extracted 1720 files name=PackageName("marimo")
TRACE Writing entrypoints name=PackageName("marimo")
TRACE No data name=PackageName("marimo")
TRACE Writing installer metadata name=PackageName("marimo")
TRACE Writing record name=PackageName("marimo")
TRACE Extracted 1880 files name=PackageName("jedi")
TRACE No entrypoints name=PackageName("jedi")
TRACE No data name=PackageName("jedi")
TRACE Writing installer metadata name=PackageName("jedi")
TRACE Writing record name=PackageName("jedi")
Installed 24 packages in 1.04s
 + anyio==4.11.0
 + click==8.3.1
 + colorama==0.4.6
 + docutils==0.22.3
 + h11==0.16.0
 + idna==3.11
 + itsdangerous==2.2.0
 + jedi==0.19.2
 + loro==1.8.2
 + marimo==0.17.8
 + markdown==3.10
 + msgspec-m==0.19.3
 + narwhals==2.12.0
 + packaging==25.0
 + parso==0.8.5
 + psutil==7.1.3
 + pygments==2.19.2
 + pymdown-extensions==10.17.1
 + pyyaml==6.0.3
 + sniffio==1.3.1
 + starlette==0.50.0
 + tomlkit==0.13.3
 + uvicorn==0.38.0
 + websockets==15.0.1
DEBUG Checking for Python environment at: `C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi`
TRACE Querying interpreter executable at C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Scripts\python.exe
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\anyio-4.11.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\click-8.3.1.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\colorama-0.4.6.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\docutils-0.22.3.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\h11-0.16.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\idna-3.11.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\itsdangerous-2.2.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\jedi-0.19.2.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\loro-1.8.2.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\marimo-0.17.8.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\markdown-3.10.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\msgspec_m-0.19.3.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\narwhals-2.12.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\packaging-25.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\parso-0.8.5.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\psutil-7.1.3.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\pygments-2.19.2.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\pymdown_extensions-10.17.1.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\pyyaml-6.0.3.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\sniffio-1.3.1.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\starlette-0.50.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\tomlkit-0.13.3.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\uvicorn-0.38.0.dist-info
DEBUG Looking at `.dist-info` at: C:\Users\zhoumeng\AppData\Local\uv\cache\archive-v0\E6cXtESvq0vsjlMNtg2fi\Lib\site-packages\websockets-15.0.1.dist-info
DEBUG Running `marimo edit --sandbox .\marimo_notes.py`
Running in a sandbox: C:\ProgramData\chocolatey\lib\uv\tools\uv.exe run --isolated --no-project --compile-bytecode --with-requirements C:\Users\zhoumeng\AppData\Local\Temp\tmpufik46vb.txt --python >=3.13 --index https://pypi.tuna.tsinghua.edu.cn/simple marimo edit .\marimo_notes.py
Installed 50 packages in 21.37s
Bytecode compiled 2520 files in 1.00s

        Edit marimo_notes.py in your browser 📝

        ➜  URL: http://127.0.0.1:2718?access_token=xnbz4ph-7ssSXAaOe8sz2g

---
