```yaml
number: 6921
title: "docs `docker.md`: example to use bind mounts"
type: pull_request
state: merged
author: odie5533
labels:
  - documentation
assignees: []
merged: true
base: main
head: docker-example-bind-mounts
created_at: 2024-09-01T19:05:41Z
updated_at: 2024-09-03T14:44:54Z
url: https://github.com/astral-sh/uv/pull/6921
synced_at: 2026-01-12T16:07:35Z
```

# docs `docker.md`: example to use bind mounts

---

_@odie5533_

## Summary

Update the extended docker example to use bind mounts and avoid creating extra layers and avoid copying files into layers

This is in line with the official Docker templates for Python applications (you can try them out using `docker init`. I found them very helpful!)

---

_Renamed from "Update docker.md example to use bind mounts" to "docs `docker.md`: example to use bind mounts" by @odie5533 on 2024-09-01 19:08_

---

_Review requested from @zanieb by @charliermarsh on 2024-09-02 18:01_

---

_Label `documentation` added by @charliermarsh on 2024-09-02 18:01_

---

_Comment by @charliermarsh on 2024-09-02 18:01_

Thank you! Tagging @zanieb for review.

---

_Assigned to @zanieb by @zanieb on 2024-09-03 14:35_

---

_@zanieb approved on 2024-09-03 14:36_

---

_Merged by @zanieb on 2024-09-03 14:36_

---

_Closed by @zanieb on 2024-09-03 14:36_

---

_Comment by @zanieb on 2024-09-03 14:37_

If you're interested, this could be changed over in https://github.com/astral-sh/uv-docker-example/blob/main/Dockerfile too!

---

_Comment by @konstin on 2024-09-03 14:39_

How does this interact with caching, will `uv sync --frozen --no-install-project` rerun when `uv.lock` changed? https://docs.docker.com/engine/storage/bind-mounts/ doesn't talk about caching and https://docs.docker.com/build/cache/ doesn't say anything about mounts.

---

_Comment by @odie5533 on 2024-09-03 14:44_

> How does this interact with caching, will `uv sync --frozen --no-install-project` rerun when `uv.lock` changed? https://docs.docker.com/engine/storage/bind-mounts/ doesn't talk about caching and https://docs.docker.com/build/cache/ doesn't say anything about mounts.

It has the same behavior as would adding them to the layer, so if `uv.lock` or `pyproject.toml` changes it will re-run the layer (re-run `uv sync --frozen --no-install-project`).

---
