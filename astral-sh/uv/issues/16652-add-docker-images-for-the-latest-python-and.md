```yaml
number: 16652
title: Add Docker images for the latest Python and Debian versions
type: issue
state: closed
author: GreenK173
labels:
  - enhancement
assignees: []
created_at: 2025-11-09T14:01:58Z
updated_at: 2025-12-31T18:47:00Z
url: https://github.com/astral-sh/uv/issues/16652
synced_at: 2026-01-12T16:02:35Z
```

# Add Docker images for the latest Python and Debian versions

---

_@GreenK173_

### Summary

It'd be great to have Docker images `uv:python-debian` and `uv:python-debian-slim` with the latest Python and Debian versions.

It would be useful for GitLab CI integration, because the solution given in https://docs.astral.sh/uv/guides/integration/gitlab/#using-the-uv-image requires to manually bump the versions whenever there's a new one.

More generally, there could be latest Python images for each distro, like `uv:python-alpine`, `uv:python-trixie`, `uv:python-slim-trixie`, etc.

Somewhat related to #16213.

### Example

```yml
uv:
  image: ghcr.io/astral-sh/uv:python-debian
```

---

_Label `enhancement` added by @GreenK173 on 2025-11-09 14:02_

---

_Comment by @zanieb on 2025-11-09 15:02_

I'm wary to expand our Docker image matrix. Why do you want the Python version to be able to change without warning? It seems like that'd be breaking?

---

_Comment by @GreenK173 on 2025-11-09 21:58_

Thanks for the reply. I wanted to use it to create a template that wouldn't need to be updated with new versions. But eventually I realized that for my use case it's better to use `uv:debian` anyway.



---

_Comment by @hemna on 2025-12-30 05:17_

Will there be a free thread build of the docker image?  I don't see a tag for 3.14t-alpine ?

---

_Comment by @zanieb on 2025-12-30 14:47_

@hemna that seems like a separate request. We're just mirroring the upstream images though, so unless one exists upstream we probably won't add one.

---

_Closed by @zanieb on 2025-12-30 14:47_

---

_Comment by @samypr100 on 2025-12-31 18:47_

Also see https://github.com/docker-library/python/issues/1082

---
