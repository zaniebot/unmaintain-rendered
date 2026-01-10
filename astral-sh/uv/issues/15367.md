```yaml
number: 15367
title: Extended Docker images for 0.8.12 are not published to DockerHub
type: issue
state: closed
author: zanieb
labels: []
assignees: []
created_at: 2025-08-19T00:02:35Z
updated_at: 2025-08-19T16:45:43Z
url: https://github.com/astral-sh/uv/issues/15367
synced_at: 2026-01-10T03:32:46Z
```

# Extended Docker images for 0.8.12 are not published to DockerHub

---

_Issue opened by @zanieb on 2025-08-19 00:02_

There was an authentication issue with Depot and DockerHub for the 0.8.12 release and the extended Docker images, e.g., `uv:alpine` or `uv:python3.12-trixie`, were not published to DockerHub for 0.8.12. Pinned images like `docker.io/astral/uv:0.8.12-alpine` will be missing. Images without a patch pin, like `docker.io/astral/uv:0.8-alpine`, will point to 0.8.11. The base uv images, e.g., `uv:latest`, `uv:0.8`, and `uv:0.8.12`, are not affected and are available on DockerHub.

If you are affected, we recommend using the GitHub Container Registry images or staying on 0.8.11.

We will resolve this issue in a subsequent release, but we probably won't backfill the images.

---

_Comment by @zanieb on 2025-08-19 13:36_

The root cause here is that the BuildKit cache has some very weird behavior where it will store `docker.io` repositories and a subsequent attempt to push to a specific `docker.io` repository can be poisoned by the that cache and include the additional repositories.

In this case, @samypr100 was testing the Docker release process and published to his own `docker.io` repository from the same Depot "project" (i.e., shared cache). Then, our release process attempted to publish both to our official repository and Sam's repository — the latter of which we do not have write permissions for. (This is certainly a simplified representation of what happened, we're digging deeper into the details of the BuildKit behavior here).

The solution is to clear the cache and create distinct Depot projects for production and testing. These items have been actioned, so the next release should go out fine.

---

_Closed by @zanieb on 2025-08-19 16:45_

---
