```yaml
number: 13390
title: "feat(docker): add 3.14 beta images to uv docker"
type: pull_request
state: merged
author: samypr100
labels:
  - enhancement
assignees: []
merged: true
base: main
head: docker-py-314-rc
created_at: 2025-05-12T00:14:26Z
updated_at: 2025-05-18T14:10:42Z
url: https://github.com/astral-sh/uv/pull/13390
synced_at: 2026-01-12T16:10:40Z
```

# feat(docker): add 3.14 beta images to uv docker

---

_@samypr100_

## Summary

Now that Python 3.14 first beta is out, I think it's worth adding support for the official upstream RC images.

Once 3.14 is released, we can remove the `-rc-` infix from the images we pull from.

## Test Plan

Upstream images verified to be functional with uv.

---

_Comment by @konstin on 2025-05-12 09:49_

Should we tag them only a `python:3.14-rc` for now, to avoid giving the impression that they can be used as stable releases?

---

_Comment by @samypr100 on 2025-05-12 20:37_

> Should we tag them only a `python:3.14-rc` for now, to avoid giving the impression that they can be used as stable releases?

Makes sense, happy to do either way. Requested change is in 53b4ca4f721289b00cd3759af3ffbf411aa9eaa2

---

_@zanieb approved on 2025-05-12 20:44_

---

_@konstin approved on 2025-05-13 07:50_

---

_Label `enhancement` added by @konstin on 2025-05-13 07:52_

---

_Comment by @konstin on 2025-05-13 12:49_

@samypr100 Could you verify that the changed CI job passes? It's a release job so it doesn't run with default CI

---

_Comment by @samypr100 on 2025-05-13 18:13_

> @samypr100 Could you verify that the changed CI job passes? It's a release job so it doesn't run with default CI

Here's an example publish docker release workflow working https://github.com/samypr100/uv/actions/runs/15003664973

Here's the 3.14 images that were published
* https://github.com/samypr100/uv/pkgs/container/uv/414779084?tag=python3.14-rc-bookworm
* https://github.com/samypr100/uv/pkgs/container/uv/414779414?tag=python3.14-rc-bookworm-slim
* https://github.com/samypr100/uv/pkgs/container/uv/414778900?tag=python3.14-rc-alpine



---

_Merged by @konstin on 2025-05-13 18:27_

---

_Closed by @konstin on 2025-05-13 18:27_

---

_Branch deleted on 2025-05-13 18:47_

---

_Comment by @reneleonhardt on 2025-05-18 06:48_

Why tag them as release candidate when there will be 3 more betas before rc1? 🤔
https://peps.python.org/pep-0745/

---

_Comment by @samypr100 on 2025-05-18 14:07_

> Why tag them as release candidate when there will be 3 more betas before rc1? 🤔 https://peps.python.org/pep-0745/

That's how 3.14 releases are labeled in https://hub.docker.com/_/python/

---

_Comment by @reneleonhardt on 2025-05-18 14:10_

Oh, my eyes only saw b1 everywhere 😅
[3.14.0b1-slim-bookworm, 3.14-rc-slim-bookworm, 3.14.0b1-slim, 3.14-rc-slim⁠](https://github.com/docker-library/python/blob/90aa427282f3f8283652c97011a511a77ea699b8/3.14-rc/slim-bookworm/Dockerfile)

---
