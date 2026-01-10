---
number: 15271
title: Remove manual Docker image annotations
type: issue
state: open
author: zanieb
labels:
  - internal
assignees: []
created_at: 2025-08-14T04:36:18Z
updated_at: 2025-08-26T16:18:03Z
url: https://github.com/astral-sh/uv/issues/15271
synced_at: 2026-01-10T01:25:54Z
---

# Remove manual Docker image annotations

---

_Issue opened by @zanieb on 2025-08-14 04:36_

Now that Depot supports these https://github.com/depot/build-push-action/pull/38 we can drop manual push of annotations (originally added in https://github.com/astral-sh/uv/pull/14165)

---

_Label `internal` added by @zanieb on 2025-08-14 04:36_

---

_Comment by @samypr100 on 2025-08-18 17:03_

I can take this if you'd like, although I'll need a depot token for testing as my trial expired 😅 

---

_Comment by @samypr100 on 2025-08-25 20:28_

I tested two releases on my fork, one with "current" [workflow](https://github.com/samypr100/uv/actions/runs/17050325414) and one with "modified" [workflow](https://github.com/samypr100/uv/actions/runs/17192775708) with the new annotation support per https://github.com/depot/build-push-action/pull/38

Sadly, it seems the `--annotation` parameters are ignored entirely by the depot build backend. The annotations don't work for either GHCR or DockerHub. My guess is that the version of buildx being used by depot when pushing is using is too old to support annotations. It's possible the buildx backend baseline on depot needs to be at least [0.16.0](https://docs.docker.com/build/release-notes/#0160), although possibly greater.

The frontend cli commands seem correct at a glance when compared to the old buildx workflow.

depot-build-action:

```shell
/opt/hostedtoolcache/depot/2.98.0/x64/depot build \
--annotation index:org.opencontainers.image.created=2025-08-24T19:26:43.337Z \
--annotation index:org.opencontainers.image.description=An extremely fast Python package installer and resolver, written in Rust. \
--annotation index:org.opencontainers.image.licenses=Apache-2.0 \
--annotation index:org.opencontainers.image.revision=9815132a8bc31286fc8b2189f9c1ca79820f5326 \
--annotation index:org.opencontainers.image.source=https://github.com/samypr100/uv \
--annotation index:org.opencontainers.image.title=uv \
--annotation index:org.opencontainers.image.url=https://github.com/samypr100/uv \
--annotation index:org.opencontainers.image.version=0.8.13-alpine3.21 \
--iidfile /tmp/depot-build-push-PqozBu/iidfile \
--label org.opencontainers.image.created=2025-08-24T19:26:43.337Z \
--label org.opencontainers.image.description=An extremely fast Python package installer and resolver, written in Rust. \
--label org.opencontainers.image.licenses=Apache-2.0 \
--label org.opencontainers.image.revision=9815132a8bc31286fc8b2189f9c1ca79820f5326 \
--label org.opencontainers.image.source=https://github.com/samypr100/uv \
--label org.opencontainers.image.title=uv \
--label org.opencontainers.image.url=https://github.com/samypr100/uv \
--label org.opencontainers.image.version=0.8.13-alpine3.21 \
--metadata-file /tmp/depot-build-push-PqozBu/metadata-file \
--platform linux/amd64,linux/arm64 \
--push \
--tag ghcr.io/samypr100/uv:0.8.13-alpine3.21 \
--tag ghcr.io/samypr100/uv:0.8-alpine3.21 \
--tag ghcr.io/samypr100/uv:0.8.13-alpine \
--tag ghcr.io/samypr100/uv:0.8-alpine \
--tag ghcr.io/samypr100/uv:alpine3.21 \
--tag ghcr.io/samypr100/uv:alpine \
--tag docker.io/samypr100/uv:0.8.13-alpine3.21 \
--tag docker.io/samypr100/uv:0.8-alpine3.21 \
--tag docker.io/samypr100/uv:0.8.13-alpine \
--tag docker.io/samypr100/uv:0.8-alpine \
--tag docker.io/samypr100/uv:alpine3.21 \
--tag docker.io/samypr100/uv:alpine \
--provenance mode=max,builder-id=https://github.com/samypr100/uv/actions/runs/17192775708 \
--project s46lwks5ft .
```

docker-build-action (translated from old buildx workflows we had)

```shell
/usr/bin/docker buildx build \
--annotation index:org.opencontainers.image.created=2025-08-24T19:26:43.337Z \
--annotation index:org.opencontainers.image.description=An extremely fast Python package installer and resolver, written in Rust. \
--annotation index:org.opencontainers.image.licenses=Apache-2.0 \
--annotation index:org.opencontainers.image.revision=9815132a8bc31286fc8b2189f9c1ca79820f5326 \
--annotation index:org.opencontainers.image.source=https://github.com/samypr100/uv \
--annotation index:org.opencontainers.image.title=uv \
--annotation index:org.opencontainers.image.url=https://github.com/samypr100/uv \
--annotation index:org.opencontainers.image.version=0.8.13-alpine3.21 \
--iidfile /home/runner/work/_temp/docker-actions-toolkit-LlDxC1/build-iidfile-cd95da4225.txt \
--label org.opencontainers.image.created=2025-08-24T19:26:43.337Z \
--label org.opencontainers.image.description=An extremely fast Python package installer and resolver, written in Rust. \
--label org.opencontainers.image.licenses=Apache-2.0 \
--label org.opencontainers.image.revision=9815132a8bc31286fc8b2189f9c1ca79820f5326 \
--label org.opencontainers.image.source=https://github.com/samypr100/uv \
--label org.opencontainers.image.title=uv \
--label org.opencontainers.image.url=https://github.com/samypr100/uv \
--label org.opencontainers.image.version=0.8.13-alpine3.21 \
--platform linux/amd64,linux/arm64 \
--tag ghcr.io/samypr100/uv:0.8.13-alpine3.21 \
--tag ghcr.io/samypr100/uv:0.8-alpine3.21 \
--tag ghcr.io/samypr100/uv:0.8.13-alpine \
--tag ghcr.io/samypr100/uv:0.8-alpine \
--tag ghcr.io/samypr100/uv:alpine3.21 \
--tag ghcr.io/samypr100/uv:alpine \
--tag docker.io/samypr100/uv:0.8.13-alpine3.21 \
--tag docker.io/samypr100/uv:0.8-alpine3.21 \
--tag docker.io/samypr100/uv:0.8.13-alpine \
--tag docker.io/samypr100/uv:0.8-alpine \
--tag docker.io/samypr100/uv:alpine3.21 \
--tag docker.io/samypr100/uv:alpine \
--metadata-file /home/runner/work/_temp/docker-actions-toolkit-LlDxC1/build-metadata-34da053294.json \
--push .
```

Output from current  workflow (manual annotations added):

```shell
$ docker buildx imagetools inspect --format '{{ json .Manifest.Annotations }}' ghcr.io/samypr100/uv:0.8.11-alpine
{
  "org.opencontainers.image.created": "2025-08-18T19:31:51.421Z",
  "org.opencontainers.image.description": "An extremely fast Python package installer and resolver, written in Rust.",
  "org.opencontainers.image.licenses": "Apache-2.0",
  "org.opencontainers.image.revision": "617b75b85c7792774ba8576bf46546a766386a73",
  "org.opencontainers.image.source": "https://github.com/samypr100/uv",
  "org.opencontainers.image.title": "uv",
  "org.opencontainers.image.url": "https://github.com/samypr100/uv",
  "org.opencontainers.image.version": "0.8.11-alpine3.21"
}
```

Output from modified workflow (no manual annotations added):

```shell
$docker buildx imagetools inspect --format '{{ json .Manifest.Annotations }}' ghcr.io/samypr100/uv:0.8.13-alpine
null
```



---
