```yaml
number: 6657
title: "[Doc] Fix using uv temporarily on docker"
type: pull_request
state: merged
author: 2xyo
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-08-26T20:34:53Z
updated_at: 2024-08-26T20:36:38Z
url: https://github.com/astral-sh/uv/pull/6657
synced_at: 2026-01-10T13:09:51Z
```

# [Doc] Fix using uv temporarily on docker

---

_Pull request opened by @2xyo on 2024-08-26 20:34_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The following Dockerfile command fails:

```
[...]
RUN --mount=from=uv,source=/uv,target=/bin/uv \
    cd /opt/opencti-connector-webhook && \
    uv pip install --system -r requirements.txt && \
    apk del git build-base
[...]
```
Result 

```
yo@opencti:~/connectors/stream/webhook$ docker build -t opencti/connector-webhook:d .
[+] Building 1.0s (3/3) FINISHED                                                                                                                                            docker:default
 => [internal] load build definition from Dockerfile                                                                                                                                  0.1s
 => => transferring dockerfile: 557B                                                                                                                                                  0.1s
 => ERROR [internal] load metadata for docker.io/library/uv:latest                                                                                                                    0.8s
 => [internal] load metadata for docker.io/library/python:3.11-alpine                                                                                                                 0.8s
------
 > [internal] load metadata for docker.io/library/uv:latest:
------
ERROR: failed to solve: uv: failed to resolve source metadata for docker.io/library/uv:latest: pull access denied, repository does not exist or may require authorization: server message: insufficient_scope: authorization failed
```

Fix:

```
[...]
RUN --mount=from=ghcr.io/astral-sh/uv,source=/uv,target=/bin/uv \
    cd /opt/opencti-connector-webhook && \
    uv pip install --system -r requirements.txt && \
    apk del git build-base
[...]
```


## Test Plan

<!-- How was it tested? -->
```
$ docker --version
Docker version 26.0.0, build 2ae903e
$ date
Mon Aug 26 20:31:53 UTC 2024

$ docker build -t opencti/connector-webhook:e .
[+] Building 41.8s (13/13) FINISHED                                                                                                                                         docker:default
 => [internal] load build definition from Dockerfile                                                                                                                                  0.0s
 => => transferring dockerfile: 587B                                                                                                                                                  0.0s
 => [internal] load metadata for ghcr.io/astral-sh/uv:latest                                                                                                                          0.5s
 => [internal] load metadata for docker.io/library/python:3.11-alpine                                                                                                                 0.5s
 => [internal] load .dockerignore                                                                                                                                                     0.1s
 => => transferring context: 2B                                                                                                                                                       0.0s
 => [stage-0 1/6] FROM docker.io/library/python:3.11-alpine@sha256:700b4aa84090748aafb348fc042b5970abb0a73c8f1b4fcfe0f4e3c2a4a9fcca                                                   0.0s
 => [internal] load build context                                                                                                                                                     0.1s
 => => transferring context: 130B                                                                                                                                                     0.0s
 => CACHED FROM ghcr.io/astral-sh/uv:latest@sha256:f6b18f4a7408c5244374b00c8832089258d130f7a77a38807348072e714ffa0c                                                                   0.0s
 => CACHED [stage-0 2/6] COPY src /opt/opencti-connector-webhook                                                                                                                      0.0s
 => CACHED [stage-0 3/6] RUN apk --no-cache add git build-base libmagic libffi-dev libxml2-dev libxslt-dev                                                                            0.0s
 => [stage-0 4/6] RUN --mount=from=ghcr.io/astral-sh/uv,source=/uv,target=/bin/uv     cd /opt/opencti-connector-webhook &&     uv pip install --system -r requirements.txt    38.3s
 => [stage-0 5/6] COPY entrypoint.sh /                                                                                                                                                0.1s
 => [stage-0 6/6] RUN chmod +x /entrypoint.sh                                                                                                                                         0.8s
 => exporting to image                                                                                                                                                                1.7s
 => => exporting layers                                                                                                                                                               1.6s
 => => writing image sha256:aa6810f883d104c838f35e848c0d7d8b4df5c7c3929f18a88b7139d0ec892a0b                                                                                          0.0s
 => => naming to docker.io/opencti/connector-webhook:e                                                                                                                                0.0s
```

---

_@zanieb approved on 2024-08-26 20:36_

Thank you! I presume this worked in my testing because it was cached locally?

---

_Label `documentation` added by @zanieb on 2024-08-26 20:36_

---

_Merged by @zanieb on 2024-08-26 20:36_

---

_Closed by @zanieb on 2024-08-26 20:36_

---
