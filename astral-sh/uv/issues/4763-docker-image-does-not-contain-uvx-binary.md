```yaml
number: 4763
title: Docker image does not contain uvx binary
type: issue
state: closed
author: Di-Is
labels: []
assignees: []
created_at: 2024-07-03T11:38:16Z
updated_at: 2024-07-03T11:51:57Z
url: https://github.com/astral-sh/uv/issues/4763
synced_at: 2026-01-12T15:58:51Z
```

# Docker image does not contain uvx binary

---

_@Di-Is_

It seems that the docker image `ghcr.io/astral-sh/uv` does not contain uvx binary.

```bash
docker run --rm --entrypoint /uvx ghcr.io/astral-sh/uv:0.2.21

docker: Error response from daemon: failed to create task for container: failed to create shim task: OCI runtime create failed: runc create failed: unable to start container process: exec: "/uvx": stat /uvx: no such file or directory: unknown.
```

---

_Closed by @charliermarsh on 2024-07-03 11:51_

---

_Closed by @charliermarsh on 2024-07-03 11:51_

---
