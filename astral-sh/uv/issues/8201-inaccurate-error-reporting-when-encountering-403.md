```yaml
number: 8201
title: Inaccurate error reporting when encountering 403 error
type: issue
state: closed
author: christiansegercrantz
labels:
  - error messages
assignees: []
created_at: 2024-10-15T08:18:30Z
updated_at: 2024-10-16T19:03:09Z
url: https://github.com/astral-sh/uv/issues/8201
synced_at: 2026-01-12T15:59:21Z
```

# Inaccurate error reporting when encountering 403 error

---

_@christiansegercrantz_

I noticed when trying to create a Docker image that installs required dependencies (generated with `rye sync`) from an internal repository that requires authentication, that  `uv` inaccurately reports the 403 error as the package not being available. 

Reproducible example:
1. Create a requirements.lock with any requirements to a remote artifactory that requires authentication. 
2. Create docker image with the following code
 ```
FROM python:3.12-slim

RUN pip install uv

WORKDIR /app
COPY requirements.lock ./

ENTRYPOINT [ "bash" ]
```
3. Build the image, go into it and run `uv pip install --no-cache --system --verbose -r requirements.lock` in it. This is the output (using Altair as example):

```
DEBUG uv 0.4.21
DEBUG Searching for default Python interpreter in system path
DEBUG Found `cpython-3.12.7-linux-aarch64-gnu` at `/usr/local/bin/python` (search path)
Using Python 3.12.7 environment at /usr/local
DEBUG Acquired lock for `/usr/local`
DEBUG At least one requirement is not satisfied: altair==5.4.1
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.7
DEBUG Solving with target Python version: >=3.12.7
DEBUG Adding direct dependency: altair==5.4.1
DEBUG No cache entry for: <REDACTED INTERNAL ARTIFACTORY>
DEBUG Searching for a compatible version of altair (==5.4.1)
DEBUG No compatible version found for: altair
  × No solution found when resolving dependencies:
  ╰─▶ Because altair was not found in the package registry and you require altair==5.4.1, we can conclude that your requirements are
      unsatisfiable.
DEBUG Released lock at `/tmp/uv-1b696e695b7c17a7.lock`
```
If I run the same operation with `pip install --no-cache -r requirements.lock` I get either a clear 403 error (if inside the Dockerfile) or a request for credentials. 

I did not see if this has been reported already or if it's intentional. Whichever, I find this quite confusing when debugging. 


---

_Comment by @paveldikov on 2024-10-15 10:12_

This relates to my use case for #5260.

Essentially 4xx errors (especially 403, but 401 and 405 are also known to happen) are a fact of life for many user demographics, so it would be nice if `uv` had specific error handling for these scenarios:

* acknowledge 403s as a distinct class of failure in the error messaging, instead of squashing down to 'not found'
* opportunistically pretty-print 4xx response bodies if they are of a JSON `Content-Type`, and contain certain well-known keys e.g. `message`

(those are the two that come to my mind -- I am sure there could be other improvements w.r.t. troubleshooting ergonomics)

---

_Comment by @charliermarsh on 2024-10-16 18:47_

We now show dedicated hints here, which I think is reasonable for now: https://github.com/astral-sh/uv/pull/8264.

---

_Closed by @charliermarsh on 2024-10-16 18:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-16 18:47_

---

_Label `error messages` added by @charliermarsh on 2024-10-16 18:47_

---

_Comment by @paveldikov on 2024-10-16 19:02_

> We now show dedicated hints here, which I think is reasonable for now: #8264.

Nice, this is a good start! I have put some minor comments but happy to track in a separate issue?


---
