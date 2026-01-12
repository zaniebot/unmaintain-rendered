```yaml
number: 7583
title: Error when pointing UV_PROJECT_ENVIRONMENT to /usr/local/ on latest UV version 0.4.13
type: issue
state: closed
author: rosssgill
labels:
  - bug
assignees: []
created_at: 2024-09-20T12:08:02Z
updated_at: 2024-09-20T17:40:35Z
url: https://github.com/astral-sh/uv/issues/7583
synced_at: 2026-01-12T15:59:15Z
```

# Error when pointing UV_PROJECT_ENVIRONMENT to /usr/local/ on latest UV version 0.4.13

---

_@rosssgill_

Hi there, 

I've been previously using the docker image `ghcr.io/astral-sh/uv:0.4.12-python3.11-bookworm-slim` and successfully setting `UV_PROJECT_ENVIRONMENT="/usr/local/"` to point to the container's system python environment, similar to how @zanieb has done here: https://github.com/astral-sh/uv/pull/6834#issuecomment-2319253359. I noticed however that with the latest UV version `0.4.13`, for example, using `ghcr.io/astral-sh/uv:python3.11-bookworm-slim` image, this is no longer possible and during the container build I get the following error:
```
[svc dev 6/8] RUN --mount=type=cache,target=/root/.cache/uv     --mount=type=bind,source=uv.lock,target=uv.lock     --mount=type=bind,source=pyproject.toml,target=pyproject.toml     --mount=type=bind,source=setup.py,target=setup.py     uv sync --frozen --no-install-project:
0.422 error: Project virtual environment directory `/usr/local/` cannot be used because it is not a virtual environment and is non-empty
```
I believe it may be related to this pull request https://github.com/astral-sh/uv/pull/7527?

---

_Comment by @zanieb on 2024-09-20 13:16_

Ah sorry that's an oversight from the protections we added in #7522.

l'll fix this right away.

---

_Assigned to @zanieb by @zanieb on 2024-09-20 13:16_

---

_Comment by @zanieb on 2024-09-20 14:15_

I have a fix up in #7585 

I haven't tested it yet though — if want to make sure it works for you that'd be helpful. I'll be adding test coverage for this functionality before merging.

---

_Comment by @zanieb on 2024-09-20 17:40_

This will be fixed in the next release (sometime today)

New integration test in #7591

---

_Closed by @zanieb on 2024-09-20 17:40_

---

_Label `bug` added by @zanieb on 2024-09-20 17:40_

---
