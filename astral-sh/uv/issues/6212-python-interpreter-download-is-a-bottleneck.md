---
number: 6212
title: Python interpreter download is a bottleneck 
type: issue
state: open
author: konstin
labels:
  - performance
assignees: []
created_at: 2024-08-19T16:49:20Z
updated_at: 2024-08-20T00:28:22Z
url: https://github.com/astral-sh/uv/issues/6212
synced_at: 2026-01-10T01:23:57Z
---

# Python interpreter download is a bottleneck 

---

_Issue opened by @konstin on 2024-08-19 16:49_

When performing an uncached install, the majority of the time is spent on waiting for python to download and unpack. This download is a bottleneck, because nothing else happens in parallel. We should parallelize the python download to run in parallel with package downloads and unpacks, while stalling package builds. We have to add the interpreter info for each interpreter we can download to uv (or make it available online) so we start evaluating wheel tags without having a physical interpreter.

Reproducer:

[pyproject.toml](https://gist.github.com/konstin/8f1b09131885699c059973f58443463c)
[uv.lock](https://gist.github.com/konstin/d028dbb5430a4346bca546c988b847e5)

```dockerfile
FROM ubuntu:latest
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates
RUN curl -LsSf https://astral.sh/uv/install.sh > /tmp/uv-installer.sh && sh /tmp/uv-installer.sh && rm /tmp/uv-installer.sh
ENV PATH="/root/.cargo/bin/:$PATH"
ADD pyproject.toml pyproject.toml
ADD uv.lock uv.lock
RUN mkdir -p github_wikidata_bot
RUN touch github_wikidata_bot/__init__.py && echo "cache bump"
RUN uv python install 3.12
RUN uv sync
```

`uv python install 3.12` takes 2.5s on my machine, and `uv sync` takes 1.5s, while only `uv sync` takes 4s. On a shared server i tried, i get 6.5s for python and 3.5s for the sync. While both use network, disk and io, they are still much faster running in parallel, just like parallel download and unpack for wheels is much faster than sequential wheel install.

---

_Label `performance` added by @konstin on 2024-08-19 16:49_

---

_Comment by @zanieb on 2024-08-19 16:52_

Oo that sounds cool.

---

_Comment by @zanieb on 2024-08-19 16:52_

Though wouldn't people have the `uv python install` cached in their Docker image and make this optimization irrelevant?

---

_Comment by @konstin on 2024-08-19 17:11_

The docker is more for illustration purposes, i'm thinking about any uncached deployment enviroment where you also want to install python, but also the first run on a new machine experience of uv.

---

_Comment by @konstin on 2024-08-19 17:13_

For docker specifically, i'd recommend, in order:
* Use an image we provide that has python installed
* Run `uv python install 3.x` in a separate cached step like the `apt-get` invocations 
* Install python through your image's mechanism and have it in a cached layer (make sure build stage and run stage match _exactly_ on their python)

---

_Comment by @samypr100 on 2024-08-20 00:28_

> Use an image we provide that has python installed

🤞 https://github.com/astral-sh/uv/pull/6053

---

_Referenced in [astral-sh/uv#4242](../../astral-sh/uv/issues/4242.md) on 2024-08-22 09:07_

---

_Referenced in [astral-sh/uv#16289](../../astral-sh/uv/issues/16289.md) on 2025-10-13 23:07_

---
