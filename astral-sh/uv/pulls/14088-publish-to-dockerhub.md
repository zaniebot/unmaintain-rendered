```yaml
number: 14088
title: Publish to DockerHub
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/dockerhub
created_at: 2025-06-16T19:37:28Z
updated_at: 2025-06-18T16:30:39Z
url: https://github.com/astral-sh/uv/pull/14088
synced_at: 2026-01-12T16:11:01Z
```

# Publish to DockerHub

---

_@zanieb_

The primary motivation here is to avoid confusion with non-official repositories, e.g., https://github.com/astral-sh/uv/issues/13958 which could lead to attacks against our users.

Resolves

- https://github.com/astral-sh/uv/issues/12679
- #8699 

---

_Label `releases` added by @zanieb on 2025-06-16 19:37_

---

_Marked ready for review by @zanieb on 2025-06-17 01:23_

---

_Comment by @zanieb on 2025-06-17 01:25_

The test plan here is unclear to me, I might release these from a fork and just delete the images?

---

_Assigned to @Gankra by @zanieb on 2025-06-17 01:26_

---

_Review requested from @samypr100 by @zanieb on 2025-06-17 01:26_

---

_Review requested from @Gankra by @zanieb on 2025-06-17 01:26_

---

_Comment by @samypr100 on 2025-06-17 02:31_

I think `docker-retag-base` should be skipped when publishing to docker hub, or at least filter out the new `docker.io/astral/uv` tags that will show up in `needs.docker-publish-base.outputs.image-tags` unless the intent is to do the re-push for both?

---

_Comment by @zanieb on 2025-06-17 13:24_

> I think `docker-retag-base` should be skipped when publishing to docker hub, or at least filter out the new `docker.io/astral/uv` tags that will show up in `needs.docker-publish-base.outputs.image-tags` unless the intent is to do the re-push for both?

I was wondering that. I'm not sure if it matters? It seems easy enough to skip it, so I added it.

---

_Review comment by @Gankra on `.github/workflows/build-docker.yml`:93 on 2025-06-17 14:13_

oh boy back in my element with github ci yml fake-js ternary exprs


---

_Review comment by @Gankra on `.github/workflows/build-docker.yml`:90 on 2025-06-17 14:14_

Wait why is this comment obsolete but the code didn't change?

---

_Review comment by @Gankra on `.github/workflows/build-docker.yml`:273 on 2025-06-17 14:18_

Is there a reason to not attest the other image? Only github's hub cares about this?

---

_@Gankra approved on 2025-06-17 14:20_

---

_Comment by @Gankra on 2025-06-17 14:22_

The nice thing about testing this live is that the docker publish stuff runs during the build step, so if the docker releases bomb out we will only have docker images to mop up and not like, pypi.

---

_@zanieb reviewed on 2025-06-17 14:32_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:273 on 2025-06-17 14:32_

uhh maybe we should attest the DockerHub ones, I'm not sure how if it works tbh.

---

_@zanieb reviewed on 2025-06-17 14:32_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:90 on 2025-06-17 14:32_

I can restore that

---

_@zanieb reviewed on 2025-06-17 14:33_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:90 on 2025-06-17 14:33_

The latter block does not have the comment, so I think I just made them the same. It's sort of apparent? idk.

---

_@samypr100 reviewed on 2025-06-18 03:16_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:273 on 2025-06-18 03:16_

It would be a tad different as we'd have to use `push-to-registry: true` for DockerHub alongside with subject name must start with `index.docker.io` rather than `docker.io`.

---

_@samypr100 approved on 2025-06-18 03:17_

---

_@zanieb reviewed on 2025-06-18 12:06_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:273 on 2025-06-18 12:06_

I think I'd prefer to do that afterwards

---

_Merged by @zanieb on 2025-06-18 16:30_

---

_Closed by @zanieb on 2025-06-18 16:30_

---

_Branch deleted on 2025-06-18 16:30_

---
