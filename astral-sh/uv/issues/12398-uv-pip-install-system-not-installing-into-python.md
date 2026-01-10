---
number: 12398
title: "`uv pip install --system` not installing into python environment"
type: issue
state: closed
author: droidnoob
labels:
  - question
assignees: []
created_at: 2025-03-22T21:27:19Z
updated_at: 2025-07-11T02:30:40Z
url: https://github.com/astral-sh/uv/issues/12398
synced_at: 2026-01-10T01:25:19Z
---

# `uv pip install --system` not installing into python environment

---

_Issue opened by @droidnoob on 2025-03-22 21:27_

### Summary

`uv pip install --system` does not seem to install the packages into the python environment. I'm using the latest release (`uv 0.6.9`).

I using a multi stage Dockerfile in the final image I am installing `uvicorn`

```Dockerfile
# final stage
FROM python:3.12-slim-bookworm

# Copy UV binaries to the final image
COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

# Create app user
RUN groupadd -r app || true && useradd -r -g app app || true

# Copy the Python packages from the builder stage
COPY --from=builder /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages

# Copy the application code
COPY . /app

WORKDIR /app

# Install uvicorn and gunicorn system-wide
RUN uv pip install --system uvicorn gunicorn

CMD ["uvicorn", "src.main:app", "--host", "0.0.0.0", "--port", "8000"]
```

Both build and final stages uses `ghcr.io/astral-sh/uv:latest`. While building and running the dockerfile I am getting the following error

```
bash: line 1: uvicorn: command not found
```

I changed the `latest` to `0.6.9` had the same issue.
This was resolved when I changed to `0.6.8`


### Platform

Linux 6.8.0-38-generic x86_64 GNU/Linux

### Version

uv 0.6.9

### Python version

Python 3.12.3

---

_Label `bug` added by @droidnoob on 2025-03-22 21:27_

---

_Comment by @zanieb on 2025-03-22 23:21_

Can you share logs for `uv pip install --system uvicorn gunicorn -v` in the image?

---

_Comment by @droidnoob on 2025-03-23 04:45_

```
DEBUG uv 0.6.9
DEBUG Searching for default Python interpreter in search path
DEBUG Found `cpython-3.12.9-linux-x86_64-gnu` at `/usr/local/bin/python` (first executable in the search path)
Using Python 3.12.9 environment at: /usr/local
DEBUG Acquired lock for `/usr/local`
DEBUG Requirement satisfied: click>=7.0
DEBUG Requirement satisfied: gunicorn
DEBUG Requirement satisfied: h11>=0.8
DEBUG Requirement satisfied: packaging
DEBUG Requirement satisfied: uvicorn
Audited 2 packages in 27ms
DEBUG Released lock at `/tmp/uv-1b696e695b7c17a7.lock`

```

I'm getting similar results in `latest` and `0.6.8`

---

_Comment by @droidnoob on 2025-03-23 05:02_

In `0.6.8`

```sh
$ which uvicorn

 /usr/local/bin/uvicorn

$ python -m pip show uvicorn
Name: uvicorn
Version: 0.34.0
Summary: The lightning-fast ASGI server.
Home-page: https://www.uvicorn.org/
Author: 
Author-email: Tom Christie <tom@tomchristie.com>, Marcelo Trylesinski <marcelotryle@gmail.com>
License: BSD-3-Clause
Location: /usr/local/lib/python3.12/site-packages
Requires: click, h11
Required-by: 
```

In `0.6.9`

```sh
$ which uvicorn

$ python -m pip show uvicorn
Name: uvicorn
Version: 0.34.0
Summary: The lightning-fast ASGI server.
Home-page: https://www.uvicorn.org/
Author: 
Author-email: Tom Christie <tom@tomchristie.com>, Marcelo Trylesinski <marcelotryle@gmail.com>
License: BSD-3-Clause
Location: /usr/local/lib/python3.12/site-packages
Requires: click, h11
Required-by: 
```
uvicorn binary is not being installed in  `/usr/local/bin`. So the issue is not it's not being installed in python environment, but not getting installed as binary.

---

_Comment by @droidnoob on 2025-03-25 13:24_

@zanieb Is this a bug with uv? 

---

_Comment by @zanieb on 2025-03-25 14:26_

It looks like you're copying the site packages but not `/usr/local/bin` so we see the package as installed already and don't reinstall? I would recommend you copy over the package executable too? I don't think this is a bug with uv.

---

_Label `bug` removed by @zanieb on 2025-03-25 14:39_

---

_Label `question` added by @zanieb on 2025-03-25 14:39_

---

_Closed by @charliermarsh on 2025-07-11 02:30_

---
