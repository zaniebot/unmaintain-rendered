---
number: 6323
title: uv cache populate / export cache
type: issue
state: open
author: adriangb
labels:
  - cache
  - needs-design
assignees: []
created_at: 2024-08-21T12:34:06Z
updated_at: 2024-08-21T23:52:03Z
url: https://github.com/astral-sh/uv/issues/6323
synced_at: 2026-01-10T01:23:59Z
---

# uv cache populate / export cache

---

_Issue opened by @adriangb on 2024-08-21 12:34_

It would be nice to have the ability to pre-cache dependencies. In particular I would find this useful in a monorepo context to:
1. Have a docker layer where I populate the cache with all dependencies.
2. In another service specific layer I do a `--cache-from` or similar from this cache layer and tell `uv` to use the cache that was built in that layer.

This is important because if I have services with the following deps:
```
tensorflow
some-small-package
```
And
```
tensorflow
some-other-small-package
```
If I don't have a "common" step where I download `tensorflow` I'd end up downloading it twice (once for each service) which is wasteful. Yes you can parallelize it but IMO efficient serial >> inefficient parallel.

Alternatively something like https://github.com/python-poetry/poetry/issues/5983 and functionality in uv to populate that folder of wheels could do the trick.

---

_Label `cache` added by @charliermarsh on 2024-08-21 13:35_

---

_Label `needs-design` added by @charliermarsh on 2024-08-21 13:35_

---

_Comment by @charliermarsh on 2024-08-21 13:35_

What's the typical mechanism that you'd use to ensure that those two layers use the same TensorFlow version?

---

_Comment by @adriangb on 2024-08-21 14:14_

Currently it's a PITA with all package managers that I know of that support monorepos.

Hence why I think something like:

```
FROM python:3.12-slim-bullseye AS deps
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv
COPY uv.lock .
RUN --mount=type=cache,target=/root/.cache/uv \
  uv cache populate

FROM python:3.12-slim-bullseye AS service
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv
COPY . /app
RUN --mount=from=deps,source=/root/.cache/uv,target=/root/.cache/uv \
  uv install --package service
```

Would be nice.

---

_Comment by @samypr100 on 2024-08-21 23:50_

I'd say this is similar in the venue/spirit of https://github.com/astral-sh/uv/issues/1681 / https://github.com/astral-sh/uv/issues/3163 as populating the cache will involve downloading and possibly a build step which is a common workflow achieved with pip wheel or download.

---

_Referenced in [astral-sh/uv#9345](../../astral-sh/uv/issues/9345.md) on 2024-11-22 05:05_

---
