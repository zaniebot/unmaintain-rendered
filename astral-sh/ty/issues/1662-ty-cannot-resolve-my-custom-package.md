```yaml
number: 1662
title: Ty cannot resolve my custom package
type: issue
state: closed
author: hermeschen1116
labels:
  - question
  - imports
assignees: []
created_at: 2025-11-28T08:40:59Z
updated_at: 2025-12-02T01:19:14Z
url: https://github.com/astral-sh/ty/issues/1662
synced_at: 2026-01-10T01:58:59Z
```

# Ty cannot resolve my custom package

---

_Issue opened by @hermeschen1116 on 2025-11-28 08:40_

### Question

I’m using both uv and ty together. When I install my own custom packages using uv, ty doesn’t recognise them, whereas other third-party packages work perfectly with ty. As the docs mention, I use uv to manage my environment, so I don’t need to configure anything in my pyproject.toml. Is there anything else I should be aware of?

### Version

0.0.0-alpha.28

---

_Label `question` added by @hermeschen1116 on 2025-11-28 08:41_

---

_Comment by @AlexWaygood on 2025-11-28 08:43_

Are you using setuptools for your build backend, by any chance? (If you haven't specified a build backend, then uv will use setuptools by default for your project.)

---

_Label `imports` added by @AlexWaygood on 2025-11-28 08:49_

---

_Comment by @hermeschen1116 on 2025-11-28 08:50_

@AlexWaygood No, my build backend is nuitka. I use it in all my python project.

```toml
[build-system]
requires = ["Nuitka[build-wheel]", "toml", "tqdm"]
build-backend = "nuitka.distutils.Build"
```

---

_Comment by @hermeschen1116 on 2025-11-28 14:37_

@AlexWaygood 
I hope you don’t mind me asking again. I’m not really familiar with how Ty or any LSP gets third-party package details, but could it be that it’s because the package is built as a binary wheel? Also, it looks like Pyright is having trouble resolving my package too.

---

_Comment by @AlexWaygood on 2025-11-28 14:46_

Yeah, I'm not familiar with nuitka, I was planning on looking into it :-)

it would be great if you could show me what files are installed into your `site-packages` directory for your custom package. Is it only C extensions? Or does it also have some Python files? If it's only C extensions, then this issue is probably a duplicate of https://github.com/astral-sh/ty/issues/487

---

_Comment by @hermeschen1116 on 2025-11-28 14:54_

> Yeah, I'm not familiar with nuitka, I was planning on looking into it :-)
> 
> it would be great if you could show me what files are installed into your `site-packages` directory for your custom package. Is it only C extensions? Or does it also have some Python files? If it's only C extensions, then this issue is probably a duplicate of [#487](https://github.com/astral-sh/ty/issues/487)

Yeah, you're right. There's no .py file is including in both wheel file (nuitka compiles the code into a .so file) and in site-packages.

---

_Comment by @AlexWaygood on 2025-11-28 14:56_

Great, thanks for the confirmation!!

---

_Closed by @AlexWaygood on 2025-11-28 14:56_

---

_Comment by @hermeschen1116 on 2025-11-28 15:00_

@AlexWaygood BTW, just let you know because I wrote .pyi file for some function in my package and it's include in wheel file and installed in site-packages. I'm not sure if Ty use .pyi files. Even the functions are specified in .pyi but Ty still cannot resolve them.

---

_Comment by @AlexWaygood on 2025-11-28 15:04_

we do inspect `.pyi` files, yes. I assumed you didn't have any because you said you only had C extensions in `site-packages` 😄

Can you please list out all the files for your package that are included in `site-packages`?

---

_Reopened by @AlexWaygood on 2025-11-28 15:04_

---

_Comment by @hermeschen1116 on 2025-11-28 15:05_

> we do inspect `.pyi` files, yes. I assumed you didn't have any because you said you only had C extensions in `site-packages` 😄
> 
> Can you please list out all the files for your package that are included in `site-packages`?

Sure. utils is my custom package.

<img width="403" height="435" alt="Image" src="https://github.com/user-attachments/assets/f7847e36-1fa0-45a8-bfe7-f331a725c9ff" />

---

_Comment by @MichaReiser on 2025-11-28 16:37_

Can you show us how you import the module and what ty's diagnostic shows (ideally, run ty with `-v` so that it logs all search paths)

---

_Comment by @hermeschen1116 on 2025-11-28 17:45_

@MichaReiser 
<img width="514" height="77" alt="Image" src="https://github.com/user-attachments/assets/33fc31f7-e3b0-455f-b9a8-e91ceed37b1f" />

And what ty shows by diagnosis the same file.
```shell
INFO Defaulting to python-platform `darwin`
INFO Python version: Python 3.12, platform: darwin
INFO Indexed 1 file(s) in 0.003s
error[unresolved-import]: Cannot resolve imported module `utils.Connection`
  --> src/tlpr/internal/service/Detect.py:11:6
   |
 9 | import msgspec.json
10 | from structlog.typing import FilteringBoundLogger
11 | from utils.Connection import create_client
   |      ^^^^^^^^^^^^^^^^
12 | from utils.concurrent import execute_async, gather
13 | from utils.typing.URL import HttpURL
   |
info: Searched in the following paths during module resolution:
info:   1. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition/src (first-party code)
info:   2. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition (first-party code)
info:   3. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   4. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition/.venv/lib/python3.12/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `utils.concurrent`
  --> src/tlpr/internal/service/Detect.py:12:6
   |
10 | from structlog.typing import FilteringBoundLogger
11 | from utils.Connection import create_client
12 | from utils.concurrent import execute_async, gather
   |      ^^^^^^^^^^^^^^^^
13 | from utils.typing.URL import HttpURL
14 | from yolo_utils.server.internal.Typing import OBB
   |
info: Searched in the following paths during module resolution:
info:   1. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition/src (first-party code)
info:   2. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition (first-party code)
info:   3. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   4. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition/.venv/lib/python3.12/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `utils.typing.URL`
  --> src/tlpr/internal/service/Detect.py:13:6
   |
11 | from utils.Connection import create_client
12 | from utils.concurrent import execute_async, gather
13 | from utils.typing.URL import HttpURL
   |      ^^^^^^^^^^^^^^^^
14 | from yolo_utils.server.internal.Typing import OBB
   |
info: Searched in the following paths during module resolution:
info:   1. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition/src (first-party code)
info:   2. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition (first-party code)
info:   3. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   4. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition/.venv/lib/python3.12/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

error[unresolved-import]: Cannot resolve imported module `yolo_utils.server.internal.Typing`
  --> src/tlpr/internal/service/Detect.py:14:6
   |
12 | from utils.concurrent import execute_async, gather
13 | from utils.typing.URL import HttpURL
14 | from yolo_utils.server.internal.Typing import OBB
   |      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
15 |
16 | from tlpr import BASE_LOGGER
   |
info: Searched in the following paths during module resolution:
info:   1. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition/src (first-party code)
info:   2. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition (first-party code)
info:   3. vendored://stdlib (stdlib typeshed stubs vendored by ty)
info:   4. /Users/hermeschen/Repo/work/taiwan-license-plate-recognition/.venv/lib/python3.12/site-packages (site-packages)
info: make sure your Python environment is properly configured: https://docs.astral.sh/ty/modules/#python-environment
info: rule `unresolved-import` is enabled by default

Found 4 diagnostics
```

---

_Comment by @hermeschen1116 on 2025-11-28 17:53_

> > we do inspect `.pyi` files, yes. I assumed you didn't have any because you said you only had C extensions in `site-packages` 😄
> > Can you please list out all the files for your package that are included in `site-packages`?
> 
> Sure. utils is my custom package.
> 
> <img alt="Image" width="403" height="435" src="https://private-user-images.githubusercontent.com/108386417/520207559-f7847e36-1fa0-45a8-bfe7-f331a725c9ff.png?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NjQzNTI2MjQsIm5iZiI6MTc2NDM1MjMyNCwicGF0aCI6Ii8xMDgzODY0MTcvNTIwMjA3NTU5LWY3ODQ3ZTM2LTFmYTAtNDVhOC1iZmU3LWYzMzFhNzI1YzlmZi5wbmc_WC1BbXotQWxnb3JpdGhtPUFXUzQtSE1BQy1TSEEyNTYmWC1BbXotQ3JlZGVudGlhbD1BS0lBVkNPRFlMU0E1M1BRSzRaQSUyRjIwMjUxMTI4JTJGdXMtZWFzdC0xJTJGczMlMkZhd3M0X3JlcXVlc3QmWC1BbXotRGF0ZT0yMDI1MTEyOFQxNzUyMDRaJlgtQW16LUV4cGlyZXM9MzAwJlgtQW16LVNpZ25hdHVyZT1iMGMyOWExOTA4NWIyNWM4MWE5NDU1MzRiMmUzZTE5NTNmMDI3ZDc5MDQ2MjYzYzRjMzAzYzAzY2JhMTQ3NWQ3JlgtQW16LVNpZ25lZEhlYWRlcnM9aG9zdCJ9.YquGJFTtiJeoCZ34f98zQwIZLfI7S6PziBobhACBh0U">

<img width="411" height="124" alt="Image" src="https://github.com/user-attachments/assets/6c393734-45ec-4914-b9d3-8d597efc8336" />

I found extra .pyi for my package outside the dir I showed in the previous image, and it looks like below.

```python
# This file was generated by Nuitka

# Stubs included by default
from __future__ import annotations
from __future__ import annotations
import importlib.resources

PROJECT_ROOT = PACKAGE_ROOT.parent.parent

__name__ = ...



# Modules used internally, to allow implicit dependencies to be seen:
import os
import __future__
import importlib
import importlib.resources
import concurrent
import concurrent.futures
import typing
import aiologic
import aiologic.lowlevel
import culsans
import culsans._queues
import structlog
import structlog.typing
import utils.concurrent.gather
import contextlib
import pathlib
import httpx
import httpx_retries
import asgi_lifespan
import pytest
import functools
import inspect
import multiprocessing
import time
import anyio
import enum
import anyio.abc
import anyio.from_thread
import anyio.to_thread
import concurrent.futures.thread
import io
import cv2
import cv2.typing
import numpy
import numpy.typing
import collections
import collections.abc
import logging
import colorama
import orjson
import structlog.contextvars
import structlog.dev
import structlog.processors
import abc
import concurrent.futures.Future
import concurrent.futures.ThreadPoolExecutor
import concurrent.futures.as_completed
import itertools
import granian
import granian._imports
import torch
import torch.utils
import torch.utils.data
import utils.concurrent.RaiseBehavior
import utils.concurrent.as_async
import utils.concurrent.as_sync
import utils.concurrent.execute_async
import gc
import fastapi
import sys
import granian.constants
import granian.log
import msgspec
import msgspec.toml
import copy
import urllib
import urllib.parse
import attrs
```

---

_Comment by @carljm on 2025-12-02 00:42_

> I found extra .pyi for my package

Yes, I think this is the problem. ty is finding `utils.pyi` and prioritizing it over `utils/concurrent/__init__.pyi`, and `utils.pyi` has no submodule `concurrent`. (Note the fact that it contains `import concurrent` is not the same as having an actual submodule named `concurrent`, and is not sufficient for `from utils.concurrent import ...` to work -- though it should be sufficient for `from utils import concurrent` to work.)

ty's behavior here is consistent with the behavior at runtime (assuming these were all `.py` instead of `.pyi` files), so I don't think this is a bug in ty. If you want the pyi files under `utils/` directory to be used, remove the outer `utils.pyi`.

There is one more wrinkle here, which ty is also accurately modeling from runtime. That is that a package `foo/__init__.py` does take priority over `foo.py`, but a _namespace_ package `foo/` (with no `foo/__init__.py`) is lower priority than `foo.py`. So another solution to your problem could be to add `utils/__init__.pyi` file; then the outer `utils.pyi` would be ignored in favor of the `utils/` package. But I'm not sure why you'd want to keep the unused `utils.pyi` in that case.

Thanks for the report! I will close it since currently I don't see a bug in ty here. If you believe there's a bug, please comment! It would be helpful if you can provide a complete reproducible example.

---

_Closed by @carljm on 2025-12-02 00:42_

---

_Comment by @hermeschen1116 on 2025-12-02 01:19_

I see. I think the outer utils.pyi is generated by nuitka. I found there's some content about it. 
Thank you for the help.

---
