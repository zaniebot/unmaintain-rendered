```yaml
number: 13274
title: "ci(docker): incorporate docker release enhancements from uv"
type: pull_request
state: merged
author: samypr100
labels:
  - release
  - ci
assignees: []
merged: true
base: main
head: docker-release-improvements
created_at: 2024-09-06T23:33:24Z
updated_at: 2024-10-27T13:49:18Z
url: https://github.com/astral-sh/ruff/pull/13274
synced_at: 2026-01-12T15:55:43Z
```

# ci(docker): incorporate docker release enhancements from uv

---

_@samypr100_

## Summary

This PR updates `ruff` to match `uv` updated [docker releases approach](https://github.com/astral-sh/uv/blob/main/.github/workflows/build-docker.yml). It's a combined PR with changes from these PR's
* https://github.com/astral-sh/uv/pull/6053
* https://github.com/astral-sh/uv/pull/6556
* https://github.com/astral-sh/uv/pull/6734
* https://github.com/astral-sh/uv/pull/7568

Summary of changes / features

1. This change would publish an additional tags that includes only `major.minor`.

    For a release with `x.y.z`, this would publish the tags:

    * ghcr.io/astral-sh/ruff:latest
    * ghcr.io/astral-sh/ruff:x.y.z
    * ghcr.io/astral-sh/ruff:x.y

2. Parallelizes multi-platform builds using multiple workers (hence the new docker-build / docker-publish jobs), which cuts docker releases time in half.

3. This PR introduces additional images with the ruff binaries from scratch for both amd64/arm64 and makes the mapping easy to configure by generating the Dockerfile on the fly. This approach focuses on minimizing CI time by taking advantage of dedicating a worker per mapping (20-30s~ per job). For example, on release `x.y.z`, this will publish the following image tags with format `ghcr.io/astral-sh/ruff:{tag}` with manifests for both amd64/arm64. This also include `x.y` tags for each respective additional tag. Note, this version does not include the python based images, unlike `uv`.

    * From **scratch**: `latest`, `x.y.z`, `x.y` (currently being published)
    * From **alpine:3.20**: `alpine`, `alpine3.20`, `x.y.z-alpine`, `x.y.z-alpine3.20`
    * From **debian:bookworm-slim**: `debian-slim`, `bookworm-slim`, `x.y.z-debian-slim`, `x.y.z-bookworm-slim`
    * From **buildpack-deps:bookworm**: `debian`, `bookworm`, `x.y.z-debian`, `x.y.z-bookworm`

4. This PR also fixes `org.opencontainers.image.version` for all tags (including the one from `scratch`) to contain the right release version instead of branch name `main` (current behavior).

    ```
    > docker inspect ghcr.io/astral-sh/ruff:0.6.4 | jq -r '.[0].Config.Labels'
    {
      ...
      "org.opencontainers.image.version": "main"
    }
    ```

Closes https://github.com/astral-sh/ruff/issues/13481

## Test Plan

Approach mimics `uv` with almost no changes so risk is low but I still tested the full workflow.

* I have a working CI release pipeline on my fork run https://github.com/samypr100/ruff/actions/runs/10966657733
* The resulting images were published to https://github.com/samypr100/ruff/pkgs/container/ruff

---

_Comment by @samypr100 on 2024-09-06 23:33_

CC @zanieb

---

_Marked ready for review by @samypr100 on 2024-09-06 23:41_

---

_Assigned to @zanieb by @zanieb on 2024-09-07 18:37_

---

_Review requested from @zanieb by @MichaReiser on 2024-09-08 07:44_

---

_Comment by @jvacek on 2024-10-22 12:04_

@zanieb does this need docs for gitlab again?

---

_Comment by @zanieb on 2024-10-22 12:05_

We haven't restructured the Ruff documentation to have integration guides like uv, so... eventually but not yet.

---

_@zanieb approved on 2024-10-22 12:06_

Thanks for your patience Sam!

---

_Merged by @zanieb on 2024-10-22 12:06_

---

_Closed by @zanieb on 2024-10-22 12:06_

---

_Label `release` added by @dhruvmanila on 2024-10-24 14:59_

---

_Label `ci` added by @dhruvmanila on 2024-10-24 14:59_

---

_Branch deleted on 2024-10-27 13:49_

---
