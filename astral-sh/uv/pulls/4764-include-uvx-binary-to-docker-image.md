```yaml
number: 4764
title: Include uvx binary to docker image.
type: pull_request
state: merged
author: Di-Is
labels:
  - releases
assignees: []
merged: true
base: main
head: copy-uvx-docker-final-stage
created_at: 2024-07-03T11:38:35Z
updated_at: 2024-07-03T12:40:01Z
url: https://github.com/astral-sh/uv/pull/4764
synced_at: 2026-01-12T16:06:26Z
```

# Include uvx binary to docker image.

---

_@Di-Is_

Fix #4763

## Summary

Modify the Dockerfile to include the uvx binary in the docker image `ghcr.io/astral-sh/uv`.

## Test Plan

I have verified that the uvx binary is included in the Docker image built with the following command and can be executed.

```bash
# cd repository root
docker build -t uv:add_uvx .
docker run --rm --entrypoint /uvx uv -V

uv-tool-run 0.2.21
```

---

_Review requested from @zanieb by @konstin on 2024-07-03 11:38_

---

_@konstin approved on 2024-07-03 11:39_

---

_Renamed from "Docker image `ghcr.io/astral-sh/uv` to include uvx binary" to "docker image `ghcr.io/astral-sh/uv` to include uvx binary" by @Di-Is on 2024-07-03 11:39_

---

_Renamed from "docker image `ghcr.io/astral-sh/uv` to include uvx binary" to "Include uvx binary to docker image." by @Di-Is on 2024-07-03 11:41_

---

_@charliermarsh approved on 2024-07-03 11:51_

---

_Merged by @charliermarsh on 2024-07-03 11:51_

---

_Closed by @charliermarsh on 2024-07-03 11:51_

---

_Comment by @charliermarsh on 2024-07-03 11:51_

Thank you.

---

_Branch deleted on 2024-07-03 11:52_

---

_Label `release` added by @zanieb on 2024-07-03 12:40_

---
