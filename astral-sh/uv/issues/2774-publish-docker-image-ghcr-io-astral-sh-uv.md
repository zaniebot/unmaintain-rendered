---
number: 2774
title: "Publish Docker image: ghcr.io/astral-sh/uv"
type: issue
state: closed
author: bschoenmaeckers
labels:
  - releases
assignees: []
created_at: 2024-04-02T13:16:04Z
updated_at: 2024-04-23T14:52:29Z
url: https://github.com/astral-sh/uv/issues/2774
synced_at: 2026-01-10T01:23:22Z
---

# Publish Docker image: ghcr.io/astral-sh/uv

---

_Issue opened by @bschoenmaeckers on 2024-04-02 13:16_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

There is a Dockerfile available in the repo but it is not (yet) published to the package registry.

---

_Label `release` added by @zanieb on 2024-04-02 19:19_

---

_Comment by @zanieb on 2024-04-02 19:20_

We had this during development but turned it off on launch — to be revisited.

Can you share more about your use-case for the image?

---

_Comment by @bschoenmaeckers on 2024-04-03 09:56_

I would like to use it in a slim docker image like shown below, this way docker will cache the latest uv binary but does not include it into the final image.

```Dockerfile
FROM ghcr.io/astral-sh/uv as uv
FROM python:alpine
RUN --mount=type=cache,target=/root/.cache/uv \
        --mount=from=uv,source=/uv,target=./uv \
        ./uv pip install .....
```

---

_Comment by @helderco on 2024-04-05 13:52_

Yeah, much easier and efficient installation method when working with containers!

I'm currently doing a `pip install` and pulling that binary in multi-stage, just to avoid installing curl for the install script. That mostly just tries to figure the current OS and arch, but with a multi-platformat container images that's handled automatically already. Also, pulling from uv and python base image can be done in parallel here, while doing the install in a RUN doesn't.

---

_Comment by @zanieb on 2024-04-05 14:49_

cc @charliermarsh who has more context on how hard it would be to enable this again

---

_Comment by @bschoenmaeckers on 2024-04-06 08:58_

It would also be nice to have an image with the latest python for building wheels in the CI.

```bash
uv pip install build[uv] twine
python -m build --installer uv
python -m twine upload dist/*
```

---

_Comment by @helderco on 2024-04-06 11:02_

You can use multi stage to copy uv into any image. That'll keep the uv image as small as possible for faster download.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-20 02:20_

---

_Referenced in [astral-sh/uv#3155](../../astral-sh/uv/pulls/3155.md) on 2024-04-20 02:21_

---

_Closed by @charliermarsh on 2024-04-21 16:02_

---

_Comment by @helderco on 2024-04-23 13:50_

It's great that this is in 🙌

Can you tag the image with the release version though? That would help with pinning.

---

_Comment by @zanieb on 2024-04-23 14:52_

We will, there was just a permissions issue in the last release #3195 

---
