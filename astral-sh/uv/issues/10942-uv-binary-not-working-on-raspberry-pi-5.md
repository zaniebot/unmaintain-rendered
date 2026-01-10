---
number: 10942
title: UV binary not working on raspberry pi 5
type: issue
state: closed
author: nhumrich
labels:
  - bug
  - releases
assignees: []
created_at: 2025-01-24T18:18:51Z
updated_at: 2025-01-29T22:06:01Z
url: https://github.com/astral-sh/uv/issues/10942
synced_at: 2026-01-10T01:24:59Z
---

# UV binary not working on raspberry pi 5

---

_Issue opened by @nhumrich on 2025-01-24 18:18_

### Summary

Apologies if this is a duplicate. I found two potential duplicates, but they were closed a while ago, and this issue is recent. 

Possible duplicates: https://github.com/astral-sh/uv/issues/4647 https://github.com/astral-sh/uv/issues/6528

When building a docker image with UV on a raspberry pi 5, uv crashes. The error is:

```
0.081 <jemalloc>: Unsupported system page size
0.081 <jemalloc>: Unsupported system page size
0.081 memory allocation of 96 bytes failed
0.084 Aborted (core dumped)
```

This issue is new, as I have my automated builds run on a raspberry pi. I haven't changed my dockerfile, and my last successful build was on Dec 5. So the bug appears to be introduced after Dec 5. 

I have tried updating the raspberry pi 5 packages and that doesn't solve anything. 

Unfortunately, you might need a RPi5 to reproduce the issue. However, if you have one, you can reproduce by running `docker build` on this Dockerfile:

```dockerfile
FROM python:3.12-slim

RUN --mount=from=ghcr.io/astral-sh/uv,source=/uv,target=/bin/uv \
    uv pip install pandas
```

I am unsure of the UV version, as it crashes and won't run, not even to show version. But the sha of the docker image its being mounted from is:
sha256:2381d6aa60c326b71fd40023f921a0a3b8f91b14d5db6b90402e65a635053709

If the uv version is the same as on a non-arm machine pulling, then its 0.5.24

### Platform

raspberry pi 5

### Version

0.5.24

### Python version

3.12.8

---

_Label `bug` added by @nhumrich on 2025-01-24 18:18_

---

_Comment by @zanieb on 2025-01-24 18:28_

Just to confirm, that SHA is for the latest ARM image

```
❯ docker run --platform linux/arm64 -it ghcr.io/astral-sh/uv
Unable to find image 'ghcr.io/astral-sh/uv:latest' locally
latest: Pulling from astral-sh/uv
dfc710471e71: Pull complete 
e567a4ebf4fe: Pull complete 
Digest: sha256:2381d6aa60c326b71fd40023f921a0a3b8f91b14d5db6b90402e65a635053709
Status: Downloaded newer image for ghcr.io/astral-sh/uv:latest
uv 0.5.24
```

---

_Label `releases` added by @zanieb on 2025-01-24 18:29_

---

_Comment by @zanieb on 2025-01-24 19:21_

I don't think we set `JEMALLOC_SYS_WITH_LG_PAGE=16` in the Docker build.

Can you check if installing from the GitHub Release works instead?

---

_Referenced in [astral-sh/uv#10943](../../astral-sh/uv/pulls/10943.md) on 2025-01-24 19:36_

---

_Comment by @zanieb on 2025-01-24 19:50_

Similarly, it'd be helpful if you tested https://github.com/astral-sh/uv/pull/10943

---

_Comment by @nhumrich on 2025-01-24 20:54_

Modified the dockerfile to look like this

```FROM python:3.12-slim

RUN apt update && apt install curl -y
RUN curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.5.24/uv-installer.sh | sh
RUN ~/.local/bin/uv version
```

And doing that, it DOES in fact work fine. 

---

_Comment by @zanieb on 2025-01-24 20:58_

Cool thanks!

Surprised our Docker image ever worked on your machine then, tbh — something else must have changed.

---

_Comment by @zanieb on 2025-01-29 22:05_

Should be fixed in https://github.com/astral-sh/uv/pull/10943 — lmk if not.

---

_Closed by @zanieb on 2025-01-29 22:06_

---

_Referenced in [astral-sh/uv#11084](../../astral-sh/uv/issues/11084.md) on 2025-01-29 22:06_

---

_Referenced in [astral-sh/uv#15401](../../astral-sh/uv/issues/15401.md) on 2025-08-21 07:11_

---
