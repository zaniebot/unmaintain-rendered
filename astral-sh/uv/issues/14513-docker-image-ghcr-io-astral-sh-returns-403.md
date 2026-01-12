```yaml
number: 14513
title: Docker image ghcr.io/astral-sh/... returns 403 Forbidden when pulling
type: issue
state: closed
author: kbukum1
labels:
  - question
assignees: []
created_at: 2025-07-08T23:54:09Z
updated_at: 2025-07-09T15:06:18Z
url: https://github.com/astral-sh/uv/issues/14513
synced_at: 2026-01-12T16:01:50Z
```

# Docker image ghcr.io/astral-sh/... returns 403 Forbidden when pulling

---

_@kbukum1_

### Summary

The official uv Docker image `ghcr.io/astral-sh/uv:0.7.1` is returning a 403 Forbidden error when attempting to pull it from GitHub Container Registry. This prevents using the image in Dockerfiles or pulling it directly.

### Minimal reproducible example:
```bash
docker pull ghcr.io/astral-sh/uv:0.7.1
```

Full error output:

```
ERROR: failed to solve: ghcr.io/astral-sh/uv:0.7.1: failed to resolve source metadata for ghcr.io/astral-sh/uv:0.7.1: failed to authorize: failed to fetch oauth token: unexpected status from GET request to https://ghcr.io/token?scope=repository%3Aastral-sh%2Fuv%3Apull&service=ghcr.io: 403 Forbidden
```


Additional context:
  - This also fails when using the image in a Dockerfile with COPY --from=ghcr.io/astral-sh/uv:0.7.1
  - The error suggests an authentication/authorization issue with the GitHub Container Registry
  - Version 0.7.19 appears to have the same issue


### Platform

macOS 14 arm64 

### Version

uv 0.7.19 (attempting to access via Docker image)

### Python version

N/A

---

_Label `bug` added by @kbukum1 on 2025-07-08 23:54_

---

_Comment by @zanieb on 2025-07-09 00:04_

I cannot reproduce

```
❯ docker pull ghcr.io/astral-sh/uv:0.7.1
0.7.1: Pulling from astral-sh/uv
```

Are you rate limited or something..?

---

_Label `bug` removed by @zanieb on 2025-07-09 00:04_

---

_Label `question` added by @zanieb on 2025-07-09 00:04_

---

_Comment by @kbukum1 on 2025-07-09 00:12_

@zanieb , 

Here are some examples:

```
=> [internal] load metadata for docker.io/library/python:3.13.3-bookworm                                                0.9s
 => [auth] astral-sh/uv:pull token for ghcr.io                                                                           0.0s
------
 > [internal] load metadata for ghcr.io/astral-sh/uv:0.7.1:
------
Dockerfile:181
--------------------
 179 |     
 180 |     # Install uv by copying from the official image
 181 | >>> COPY --from=ghcr.io/astral-sh/uv:0.7.1 /uv /uvx /usr/local/bin/
 182 |     
 183 |     COPY --chown=dependabot:dependabot cargo $DEPENDABOT_HOME/cargo
--------------------
ERROR: failed to solve: ghcr.io/astral-sh/uv:0.7.1: failed to resolve source metadata for ghcr.io/astral-sh/uv:0.7.1: failed to authorize: failed to fetch oauth token: unexpected status from GET request to https://ghcr.io/token?scope=repository%3Aastral-sh%2Fuv%3Apull&service=ghcr.io: 403 Forbidden

View build details: docker-desktop://dashboard/build/des
```


```
git:(kamil/cleanup_cooldown) docker pull ghcr.io/astral-sh/uv:0.7.1
Error response from daemon: error from registry: denied
```

---

_Comment by @kbukum1 on 2025-07-09 00:14_

Checked with my colleague, he is also getting error. 

---

_Comment by @zanieb on 2025-07-09 11:53_

Maybe try `docker logout ghcr.io`? Do you an expired token?

Regardless, the image is public.

---

_Comment by @kbukum1 on 2025-07-09 15:03_

> docker logout ghcr.io

Thanks, @zanieb. `docker logout ghcr.io` worked out. After that I was able to rerun the docker to build the image.

---

_Closed by @kbukum1 on 2025-07-09 15:06_

---
