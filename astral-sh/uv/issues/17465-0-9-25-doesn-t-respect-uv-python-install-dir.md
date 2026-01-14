```yaml
number: 17465
title: "0.9.25 doesn't respect UV_PYTHON_INSTALL_DIR"
type: issue
state: closed
author: wm-eitc
labels:
  - question
assignees: []
created_at: 2026-01-14T15:43:28Z
updated_at: 2026-01-14T16:07:14Z
url: https://github.com/astral-sh/uv/issues/17465
synced_at: 2026-01-14T16:39:11Z
```

# 0.9.25 doesn't respect UV_PYTHON_INSTALL_DIR

---

_@wm-eitc_

### Summary

Here's a snippet from a Dockerfile we build locally:
```
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/bin/

WORKDIR /work
ENV UV_PYTHON_INSTALL_DIR=/work
COPY pyproject.toml .python-version uv.lock ./

RUN uv python install --install-dir . && \
    uv sync --locked --no-cache
```

and the log from building that container (using Kaniko):
```
INFO[0312] COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/bin/ 
INFO[0313] Taking snapshot of files...                  
INFO[0314] WORKDIR /work
INFO[0314] Cmd: workdir
INFO[0314] Changed working directory to /work
INFO[0314] Creating directory /work with uid 4242 and gid 4242 
INFO[0314] Taking snapshot of files...                  
INFO[0314] ENV UV_PYTHON_INSTALL_DIR=/work
INFO[0314] COPY pyproject.toml .python-version uv.lock ./ 
INFO[0314] Taking snapshot of files...                  
INFO[0314] RUN uv python install --install-dir . &&     uv sync --locked --no-cache 
INFO[0314] Initializing snapshotter ...                 
INFO[0314] Taking snapshot of full filesystem...        
INFO[0315] Cmd: /bin/bash                               
INFO[0315] Args: [-o pipefail -c uv python install --install-dir . &&     uv sync --locked --no-cache] 
INFO[0315] Util.Lookup returned: &{Uid:4242 Gid: Username: Name: HomeDir:/} 
INFO[0315] Performing slow lookup of group ids for      
INFO[0315] Running: [/bin/bash -o pipefail -c uv python install --install-dir . &&     uv sync --locked --no-cache] 
error: failed to create directory `/.cache/uv`: Permission denied (os error 13)
```
Although we're setting `UV_PYTHON_INSTALL_DIR=/work`, `uv` is attempting to install in `/`. This is new behavior, version 0.9.24 did not exhibit this problem.

### Platform

Ubuntu 24.04

### Version

0.9.25

### Python version

3.13

---

_Label `bug` added by @wm-eitc on 2026-01-14 15:43_

---

_Comment by @zanieb on 2026-01-14 15:48_

Looking...

---

_Comment by @zanieb on 2026-01-14 15:56_

I think this is failing from #17088 which now initializes the cache for `python install` operations. You can use `--no-cache` there or set a target `UV_CACHE_DIR`?

Hence the error

```
error: failed to create directory `/.cache/uv`: Permission denied (os error 13)
```

---

_Label `bug` removed by @zanieb on 2026-01-14 15:59_

---

_Label `question` added by @zanieb on 2026-01-14 15:59_

---

_Comment by @wm-eitc on 2026-01-14 16:07_

Yes, setting `--no-cache` fixes the issue. Thanks for the quick response!

---

_Closed by @wm-eitc on 2026-01-14 16:07_

---
