```yaml
number: 6734
title: "feat: publish tag without patch version"
type: pull_request
state: merged
author: samypr100
labels:
  - releases
assignees: []
merged: true
base: main
head: docker-semver
created_at: 2024-08-28T00:08:06Z
updated_at: 2024-08-28T13:53:33Z
url: https://github.com/astral-sh/uv/pull/6734
synced_at: 2026-01-10T12:53:34Z
```

# feat: publish tag without patch version

---

_Pull request opened by @samypr100 on 2024-08-28 00:08_

## Summary

Closes https://github.com/astral-sh/uv/issues/6678

This change would publish an additional tag that includes only `major.minor`.

For a release with x.y.z, this would publish the tags:
* `ghcr.io/astral-sh/uv:latest`
* `ghcr.io/astral-sh/uv:x.y.z`
* `ghcr.io/astral-sh/uv:x.y`


---

_@zanieb approved on 2024-08-28 00:10_

Awesome, thank you! Did you test this in a fork?

---

_Label `release` added by @zanieb on 2024-08-28 00:10_

---

_Comment by @samypr100 on 2024-08-28 00:19_

> Awesome, thank you! Did you test this in a fork?

Yup, here's a run from a test branch https://github.com/samypr100/uv/actions/runs/10587568664

and published packages https://github.com/samypr100/uv/pkgs/container/uv (0.3.5)



---

_@konstin reviewed on 2024-08-28 10:05_

---

_Review comment by @konstin on `.github/workflows/build-docker.yml`:139 on 2024-08-28 10:05_

Can you add a comment what this does?

---

_@zanieb reviewed on 2024-08-28 12:49_

---

_Review comment by @zanieb on `.github/workflows/build-docker.yml`:139 on 2024-08-28 12:49_

👍 a comment would be nice here

---

_@samypr100 reviewed on 2024-08-28 12:57_

---

_Review comment by @samypr100 on `.github/workflows/build-docker.yml`:139 on 2024-08-28 12:57_

Makes sense, added in 764c1df99d1a267d33fbbf4b5187b50ff5d7ecfe

---

_Merged by @zanieb on 2024-08-28 13:31_

---

_Closed by @zanieb on 2024-08-28 13:31_

---

_Branch deleted on 2024-08-28 13:53_

---
