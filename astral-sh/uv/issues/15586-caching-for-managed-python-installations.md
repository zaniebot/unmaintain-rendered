```yaml
number: 15586
title: Caching for managed python installations
type: issue
state: closed
author: aokellermann
labels:
  - enhancement
assignees: []
created_at: 2025-08-29T22:36:09Z
updated_at: 2025-09-04T18:50:23Z
url: https://github.com/astral-sh/uv/issues/15586
synced_at: 2026-01-12T16:02:13Z
```

# Caching for managed python installations

---

_@aokellermann_

### Summary

The current guidance for [caching in docker images](https://docs.astral.sh/uv/guides/integration/docker/#caching) is:

```dockerfile
ENV UV_LINK_MODE=copy
RUN --mount=type=cache,target=/root/.cache/uv \
    uv sync
```

The `UV_LINK_MODE=COPY` is crucial, as the cache is not mounted during container runtime.

However, this only works for package cache, and not for managed python installations. So if my image doesn't have python already installed, uv downloads it and puts it in a non-cached filesystem (`/root/.local/share/uv` by default). It would be nice if there was some way to have a `UV_LINK_MODE` analog, but for managed python so that if one mounts a python cache, `uv sync` will copy python to the image filesystem.



---

_Label `enhancement` added by @aokellermann on 2025-08-29 22:36_

---

_Comment by @zanieb on 2025-08-29 22:42_

There's a `UV_PYTHON_CACHE_DIR`, it's just not on by default.

---

_Comment by @aokellermann on 2025-09-04 18:48_

thanks! opened documentation PR for this

---

_Closed by @zanieb on 2025-09-04 18:50_

---
