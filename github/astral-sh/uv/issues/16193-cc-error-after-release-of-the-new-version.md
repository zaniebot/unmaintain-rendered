---
number: 16193
title: cc error after release of the new version
type: issue
state: closed
author: fihbrs
labels:
  - question
assignees: []
created_at: 2025-10-08T15:29:39Z
updated_at: 2025-10-08T16:00:22Z
url: https://github.com/astral-sh/uv/issues/16193
synced_at: 2026-01-07T13:12:19-06:00
---

# cc error after release of the new version

---

_Issue opened by @fihbrs on 2025-10-08 15:29_

### Question

We have been using ghcr.io/astral-sh/uv:bookworm-slim as a builder image and after the update we can't build our Image anymore it saying can't find "cc":

<img width="1022" height="197" alt="Image" src="https://github.com/user-attachments/assets/37a0c317-fbb8-4b80-9102-aec187b33d19" />
<img width="984" height="167" alt="Image" src="https://github.com/user-attachments/assets/26d299e6-c4db-4ae6-8387-21e5fd6d4770" />

we have changed it to 0.8.23 and it is working back but I would like to know if we have to add gcc and so how or is it an image problem

### Platform

ubuntu 24.04

### Version

_No response_

---

_Label `question` added by @fihbrs on 2025-10-08 15:29_

---

_Comment by @zanieb on 2025-10-08 15:34_

My guess is that you're unintentionally using Python 3.14 and the package doesn't have a wheel for 3.14 so it's being built whereas before it was not.

Do you have a `.python-version` file? Can you share verbose logs? (Please use code blocks instead of screenshots)

---

_Comment by @fihbrs on 2025-10-08 15:43_

> My guess is that you're unintentionally using Python 3.14 and the package doesn't have a wheel for 3.14 so it's being built whereas before it was not.
> 
> Do you have a `.python-version` file? Can you share verbose logs? (Please use code blocks instead of screenshots)

First of all, thank you. We do have a `.python-version` `3.13` to be exact 
```
#0 building with "default" instance using docker driver

#1 [internal] load build definition from Dockerfile
#1 transferring dockerfile: 1.77kB done
#1 DONE 0.0s

#2 [internal] load metadata for ghcr.io/astral-sh/uv:bookworm-slim
#2 ...

#3 [internal] load metadata for docker.io/library/debian:bookworm-slim
#3 DONE 0.5s

#2 [internal] load metadata for ghcr.io/astral-sh/uv:bookworm-slim
#2 DONE 0.6s

#4 [internal] load .dockerignore
#4 transferring context: 2B done
#4 DONE 0.0s

#5 [builder 1/6] FROM ghcr.io/astral-sh/uv:bookworm-slim@sha256:8ffef4550cef7d6b9a3f52551f5e3fb8b61983fd02a19a9fd9ab0e7811a1baf5
#5 DONE 0.0s

#6 [stage-1 1/6] FROM docker.io/library/debian:bookworm-slim@sha256:7e490910eea2861b9664577a96b54ce68ea3e02ce7f51d89cb0103a6f9c386e0
#6 DONE 0.0s

#7 [builder 2/6] RUN uv python install 3.12
#7 CACHED

#8 [builder 3/6] WORKDIR /app
#8 CACHED

#9 [internal] load build context
#9 transferring context: 1.15MB 0.1s done
#9 DONE 0.1s

#10 [builder 4/6] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --locked --no-install-project --no-dev
#10 0.366 Downloading cpython-3.14.0-linux-x86_64-gnu (download) (33.1MiB)
#10 6.333  Downloading cpython-3.14.0-linux-x86_64-gnu (download)
#10 6.442 Using CPython 3.14.0
#10 6.442 Creating virtual environment at: .venv
#10 6.447 Resolved 82 packages in 0.96ms
#10 6.449    Building uvloop==0.21.0
#10 6.449    Building cffi==1.17.1
#10 6.449    Building pydantic-core==2.33.2
#10 6.449    Building httptools==0.6.4
#10 6.449    Building pyyaml==6.0.2
#10 6.978   × Failed to build `httptools==0.6.4`
#10 6.978   ├─▶ The build backend returned an error
#10 6.978   ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit
#10 6.978       status: 1)
#10 6.978 
#10 6.978       [stdout]
#10 6.978       running bdist_wheel
#10 6.978       running build
#10 6.978       running build_py
#10 6.978       copying httptools/__init__.py ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools
#10 6.978       copying httptools/_version.py ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools
#10 6.978       copying httptools/parser/__init__.py ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools/parser
#10 6.978       copying httptools/parser/errors.py ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools/parser
#10 6.978       running egg_info
#10 6.978       writing httptools.egg-info/PKG-INFO
#10 6.978       writing dependency_links to httptools.egg-info/dependency_links.txt
#10 6.978       writing requirements to httptools.egg-info/requires.txt
#10 6.978       writing top-level names to httptools.egg-info/top_level.txt
#10 6.978       reading manifest file 'httptools.egg-info/SOURCES.txt'
#10 6.978       reading manifest template 'MANIFEST.in'
#10 6.978       adding license file 'LICENSE'
#10 6.978       writing manifest file 'httptools.egg-info/SOURCES.txt'
#10 6.978       copying httptools/parser/cparser.pxd ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools/parser
#10 6.978       copying httptools/parser/parser.pyx ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools/parser
#10 6.978       copying httptools/parser/python.pxd ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools/parser
#10 6.978       copying httptools/parser/url_cparser.pxd ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools/parser
#10 6.978       copying httptools/parser/url_parser.pyx ->
#10 6.978       build/lib.linux-x86_64-cpython-314/httptools/parser
#10 6.978       running build_ext
#10 6.978       building 'httptools.parser.parser' extension
#10 6.978       cc -pthread -fno-strict-overflow -Wsign-compare
#10 6.978       -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -fPIC
#10 6.978       -I/root/.cache/uv/sdists-v9/pypi/httptools/0.6.4/WkmIRZTjQl4jhJmctE_1J/src/vendor/llhttp/include
#10 6.978       -I/root/.cache/uv/sdists-v9/pypi/httptools/0.6.4/WkmIRZTjQl4jhJmctE_1J/src/vendor/llhttp/src
#10 6.978       -I/root/.cache/uv/builds-v0/.tmp4bRat4/include
#10 6.978       -I/python/cpython-3.14.0-linux-x86_64-gnu/include/python3.14
#10 6.978       -c httptools/parser/parser.c -o
#10 6.978       build/temp.linux-x86_64-cpython-314/httptools/parser/parser.o -O2
#10 6.978 
#10 6.978       [stderr]
#10 6.978       /root/.cache/uv/builds-v0/.tmp4bRat4/lib/python3.14/site-packages/setuptools/_distutils/dist.py:289:
#10 6.978       UserWarning: Unknown distribution option: 'test_suite'
#10 6.978         warnings.warn(msg)
#10 6.978       /root/.cache/uv/builds-v0/.tmp4bRat4/lib/python3.14/site-packages/setuptools/dist.py:759:
#10 6.978       SetuptoolsDeprecationWarning: License classifiers are deprecated.
#10 6.978       !!
#10 6.978 
#10 6.978       
#10 6.978       ********************************************************************************
#10 6.978               Please consider removing the following classifiers in favor of a
#10 6.978       SPDX license expression:
#10 6.978 
#10 6.978               License :: OSI Approved :: MIT License
#10 6.978 
#10 6.978               See
#10 6.978       https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license
#10 6.978       for details.
#10 6.978       
#10 6.978       ********************************************************************************
#10 6.978 
#10 6.978       !!
#10 6.978         self._finalize_license_expression()
#10 6.978       error: command 'cc' failed: No such file or directory
#10 6.978 
#10 6.978       hint: This usually indicates a problem with the package or the build
#10 6.978       environment.
#10 6.978   help: `httptools` (v0.6.4) was included because `backend` (v0.1.0) depends
#10 6.978         on `uvicorn[standard]` (v0.35.0) which depends on `httptools`
#10 ERROR: process "/bin/sh -c uv sync --locked --no-install-project --no-dev" did not complete successfully: exit code: 1
------
 > [builder 4/6] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     uv sync --locked --no-install-project --no-dev:
6.978       ********************************************************************************
6.978 
6.978       !!
6.978         self._finalize_license_expression()
6.978       error: command 'cc' failed: No such file or directory
6.978 
6.978       hint: This usually indicates a problem with the package or the build
6.978       environment.
6.978   help: `httptools` (v0.6.4) was included because `backend` (v0.1.0) depends
6.978         on `uvicorn[standard]` (v0.35.0) which depends on `httptools`
------
Dockerfile:18
--------------------
  17 |     WORKDIR /app
  18 | >>> RUN --mount=type=cache,target=/root/.cache/uv \
  19 | >>>     --mount=type=bind,source=uv.lock,target=uv.lock \
  20 | >>>     --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
  21 | >>>     uv sync --locked --no-install-project --no-dev
  22 |     
--------------------
ERROR: failed to build: failed to solve: process "/bin/sh -c uv sync --locked --no-install-project --no-dev" did not complete successfully: exit code: 1
```

---

_Comment by @zanieb on 2025-10-08 15:53_

Hm, that's surprising.

If I do this trivial setup it succeeds:

```
❯ uv init --package example -p 3.13
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv add httptools==0.6.4
Using CPython 3.13.3 interpreter at: /opt/homebrew/opt/python@3.13/bin/python3.13
Creating virtual environment at: .venv
Resolved 2 packages in 132ms
      Built example @ file:///Users/zb/workspace/uv/example
Prepared 1 package in 8ms
Installed 2 packages in 5ms
 + example==0.1.0 (from file:///Users/zb/workspace/uv/example)
 + httptools==0.6.4
❯ vi Dockerfile
❯ cat Dockerfile
FROM ghcr.io/astral-sh/uv:bookworm-slim

ADD . /app
WORKDIR /app
RUN uv sync
❯ docker build .
[+] Building 3.4s (9/9) FINISHED                                                         docker:orbstack
 => [internal] load build definition from Dockerfile                                                0.0s
 => => transferring dockerfile: 115B                                                                0.0s
 => [internal] load metadata for ghcr.io/astral-sh/uv:bookworm-slim                                 0.0s
 => [internal] load .dockerignore                                                                   0.0s
 => => transferring context: 2B                                                                     0.0s
 => [internal] load build context                                                                   0.0s
 => => transferring context: 396.83kB                                                               0.0s
 => CACHED [1/4] FROM ghcr.io/astral-sh/uv:bookworm-slim                                            0.0s
 => [2/4] ADD . /app                                                                                0.1s
 => [3/4] WORKDIR /app                                                                              0.1s
 => [4/4] RUN uv sync                                                                               2.9s
 => exporting to image                                                                              0.2s
 => => exporting layers                                                                             0.2s
 => => writing image sha256:5d7f350b3cc42f30152a8585698d399be2940bc535c2d597ae1ccefb7a75c8d8        0.0s 
                                                                                                         
What's next:                                                                                             
    View a summary of image vulnerabilities and recommendations → docker scout quickview                 
```

If I pin to 3.14 before running `sync`

```
RUN uv python pin 3.14
```

or remove the version file

```
RUN rm .python-version
```

I get the aforementioned build failure.

Can you confirm your python version file is getting copied in?

---

_Comment by @zanieb on 2025-10-08 15:54_

It looks like you're not mounting it during the sync

---

_Comment by @fihbrs on 2025-10-08 15:57_

Yes! that was the problem once I've mounted it, builds correctly. Thank you for your time

---

_Comment by @zanieb on 2025-10-08 15:59_

Great! We'll add that to our documentation. Thanks for reaching out.

---

_Closed by @zanieb on 2025-10-08 16:00_

---
