---
number: 3117
title: Add environment variable for require-hashes
type: issue
state: closed
author: sspans-sbp
labels:
  - configuration
assignees: []
created_at: 2024-04-18T07:31:03Z
updated_at: 2024-04-18T21:07:09Z
url: https://github.com/astral-sh/uv/issues/3117
synced_at: 2026-01-10T01:23:24Z
---

# Add environment variable for require-hashes

---

_Issue opened by @sspans-sbp on 2024-04-18 07:31_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Similar to `UV_SYSTEM_PYTHON=true` it would be quite helpful to also have `UV_REQUIRE_HASHES=true`
That would make it easier to ensure hash checking is always enabled when building containers, especially when stacking images.

My scenario is:

python_3.12_base/Dockerfile:

```dockerfile
FROM python:3.12.3-alpine3.19

ENV UV_SYSTEM_PYTHON=true
ENV UV_NO_CACHE=true
ENV UV_REQUIRE_HASHES=true

# We know we're running in a docker container as root
# Unlike the rest of the world, we never run privileged
ENV PIP_ROOT_USER_ACTION=ignore

RUN apk upgrade --no-cache && \
    apk add --no-cache git gnupg openssh-client curl jq unzip && \
    # renovate: datasource=pypi versioning=pep440
    pip install --no-cache-dir uv==0.1.32
```

infrastructure_ci/Dockerfile:

```dockerfile
FROM python_3.12_slim

SHELL ["/bin/ash", "-eo", "pipefail", "-c"]

# Add Python packages
RUN apk add --no-cache build-base libffi-dev && \
    uv pip install -r requirements.txt && \
    apk del build-base libffi-dev
```

---

_Label `configuration` added by @AlexWaygood on 2024-04-18 07:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-18 19:59_

---

_Comment by @charliermarsh on 2024-04-18 19:59_

I can do this.

---

_Referenced in [astral-sh/uv#3125](../../astral-sh/uv/pulls/3125.md) on 2024-04-18 20:57_

---

_Closed by @charliermarsh on 2024-04-18 21:07_

---
