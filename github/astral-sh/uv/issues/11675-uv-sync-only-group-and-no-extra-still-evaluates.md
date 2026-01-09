---
number: 11675
title: uv sync --only-group and --no-extra still evaluates other package sources not specified, source = path
type: issue
state: open
author: JWCS
labels:
  - question
assignees: []
created_at: 2025-02-20T19:13:49Z
updated_at: 2025-08-08T22:24:22Z
url: https://github.com/astral-sh/uv/issues/11675
synced_at: 2026-01-07T13:12:18-06:00
---

# uv sync --only-group and --no-extra still evaluates other package sources not specified, source = path

---

_Issue opened by @JWCS on 2025-02-20 19:13_

### Summary

MRE:
`chmod +x Dockerfile && ./Dockerfile && docker run --network host -it --rm --entrypoint sh mre-exclude-groups:latest`
``` Dockerfile
#!/usr/bin/env -S docker build -t mre-exclude-groups:latest . -f
FROM ghcr.io/astral-sh/uv:python3.10-alpine

WORKDIR /app

COPY pyproject.toml .
# ideally, reusing existing uv.lock file; but the chicken-egg problem is without uv.lock file; see --inexact

ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy

RUN uv venv

# BUG 1
# This doesn't work because "distribution not found at <this path>", for unspecified group/package groundingdino
#RUN uv sync --inexact --only-group legacy-setup-req

# Ok, install, try again
RUN apk add git && git clone --depth 1 --single-branch https://github.com/IDEA-Research/GroundingDINO.git

# BUG 2 (might be the same)
#RUN uv sync --inexact --only-group legacy-setup-req
# Failed to build groundingdino at <this path>, no module named torch

# What I want to do
#RUN uv sync --inexact --only-group legacy-setup-req
#RUN uv sync --inexact --no-default-groups --no-install-project
#RUN uv sync --inexact --all-groups --no-install-project
#RUN uv sync -U
```
``` toml
[project]
name = "uv-exclude-groups-mre"
dynamic = ["version"]
requires-python = ">=3.10"
dependencies = [
  "numpy<2",
  "torch==1.13.1+cu116",
  "torchvision==0.14.1+cu116",
]
[dependency-groups]
legacy-setup-req = [ "setuptools", "torch" ]
groundingdino = [ {include-group="legacy-setup-req"}, "groundingdino" ]

[tool.uv]
package = false
environments = [
  "sys_platform == 'linux' and implementation_name == 'cpython' and platform_machine == 'x86_64'",
]

[tool.uv.sources]
torch       = { index = "pytorch-cu116" }
torchvision = { index = "pytorch-cu116" }
groundingdino = { path = "GroundingDINO", editable = false }

[[tool.uv.index]]
name = "pytorch-cu116"
url = "https://download.pytorch.org/whl/cu116"
explicit = true

# stop uv from building workaround https://github.com/astral-sh/uv/issues/6321
[tool.setuptools]
py-modules = []
```
Granted, I know that FWIW, these are all just "temporary" (but crucial) hacks around some cleaner mechanism for dealing with setup.py/pytorch packages; there's a handful of such issues around the repo. Ex #7390 #7722
It's a pain to uv `pip install torch` not pinned to the version/source specified in the pyproject.toml, and then tier the builds. Ex, this does work:

However, I'm also getting this issue with "optional-dependencies":
``` diff
> git diff
diff --git a/Dockerfile b/Dockerfile
index e350a25..f37b630 100755
--- a/Dockerfile
+++ b/Dockerfile
@@ -17,8 +17,14 @@ RUN uv venv
 # Ok, install, try again
 RUN apk add git && git clone --depth 1 --single-branch https://github.com/IDEA-Research/GroundingDINO.git

-# BUG 2 (might be the same)
-#RUN uv sync --inexact --only-group legacy-setup-req
+# BUG 3: considers optionals!
+RUN --mount=type=cache,target=/root/.cache/uv \
+   uv sync --inexact --no-extra compile # FAILS, tries to parse GroundingDino/setup.py
+RUN --mount=type=cache,target=/root/.cache/uv \
+   uv sync --inexact # FAILS, tries to parse GroundingDino/setup.py
+RUN --mount=type=cache,target=/root/.cache/uv \
+   uv sync --inexact --extra build # FAILS, tries to parse GroundingDino/setup.py
+RUN --mount=type=cache,target=/root/.cache/uv \
+   uv sync --inexact --extra build --extra compile
 # Failed to build groundingdino at <this path>, no module named torch

 # What I want to do
diff --git a/pyproject.toml b/pyproject.toml
index 4c1d845..9b9ea3b 100644
--- a/pyproject.toml
+++ b/pyproject.toml
@@ -7,9 +7,10 @@ dependencies = [
   "torch==1.13.1+cu116",
   "torchvision==0.14.1+cu116",
 ]
-[dependency-groups]
-legacy-setup-req = [ "setuptools", "torch" ]
-groundingdino = [ {include-group="legacy-setup-req"}, "groundingdino" ]
+
+[project.optional-dependencies]
+build = ["setuptools", "torch"]
+compile = ["groundingdino"]

 [tool.uv]
 package = false
```
The 3 uv sync commands which shouldn't be considering groundingdino all try evaluating its setup.py, which on first run, no torch is installed. 

### Platform

Alpine x64 (uv docker image)

### Version

uv 0.6.1

### Python version

Python 3.10

---

_Label `bug` added by @JWCS on 2025-02-20 19:13_

---

_Comment by @zanieb on 2025-02-20 19:45_

This looks similar to

- https://github.com/astral-sh/uv/issues/11363
- https://github.com/astral-sh/uv/issues/11104
- https://github.com/astral-sh/uv/issues/9654

We need to resolve all of the packages (including groups and extras) even if you do not want to include them in the `sync` operation. I don't understand why you can't use the existing lockfile?

Perhaps I'm misunderstanding?

---

_Label `bug` removed by @zanieb on 2025-02-20 19:45_

---

_Label `question` added by @zanieb on 2025-02-20 19:45_

---

_Comment by @JWCS on 2025-02-20 19:57_

It's the chicken-and-egg problem. If I have a lockfile, then I can `uv sync --frozen` install the dependencies (at least, say, setuptools/pytorch). Which enables unlimited further variants of `uv sync`. But if I don't have a lock file, and I wanted to create it from scratch... then my only option is commenting out the optional or group dependencies, `uv sync` that to get the initial lock file, then uncomment the optionals/group deps and then `uv sync` install them. 

It's not impossible... but I didn't realize this was the only real way to set this up from scratch, as of 0.6.1. 
I guess, the bug is that the documentation (and the recommendations from other gh issues) _seems_ like the behavior of --only-group/--no-extra would even exclude resolving of such packages, especially if they're known to be broken when resolved out of order. There's no mechanism right now to prevent the resolving of packages without commenting out / sed-ing the pyproject.toml file.

---

_Referenced in [astral-sh/uv#12309](../../astral-sh/uv/issues/12309.md) on 2025-03-19 08:07_

---

_Comment by @konstin on 2025-04-11 11:06_

fwiw you can use `uv lock` to generate a lockfile without syncing.

---

_Comment by @eremeevfd on 2025-06-30 13:50_

We also found ourselves in a similar situation. Our case is: we need the libraries from private registry only for testing, but for production build, we don't want to expose credentials for registry and have these dependencies present at all. But it's currently impossible to install only non-private dependencies unfortunately.

---

_Comment by @mrezzamoradi on 2025-08-08 22:24_

To my understanding, `uv sync` by default tries to update the lock file to make sure it's in sync with `pyproject.toml` **and that's the reason it needs to resolve the dependencies in every group**. Now if you don't want to update the lock file, which is usually the case in Dockerifles and in CI pipelines, just pass `--frozen` flag to the sync command. That should remove the necessity to resolve the dependencies since it is assuming the paths in the lock file are already correct.

I tried it in a test project and managed to reproduce the issue and solve it with `--frozen`

---
