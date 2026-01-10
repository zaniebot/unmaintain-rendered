---
number: 5610
title: Publish variants of Docker image
type: issue
state: closed
author: zanieb
labels:
  - help wanted
  - releases
assignees: []
created_at: 2024-07-30T16:44:07Z
updated_at: 2024-09-03T13:44:02Z
url: https://github.com/astral-sh/uv/issues/5610
synced_at: 2026-01-10T01:23:50Z
---

# Publish variants of Docker image

---

_Issue opened by @zanieb on 2024-07-30 16:44_

Instead of only supplying a bare image, we should publish an alpine variant and a variant with Python so people can use our image without creating a custom derivative.

---

_Comment by @zanieb on 2024-07-30 16:44_

For example, this is important for GitLab CI which requires `sh`.

This applies to Ruff as well.

---

_Label `release` added by @zanieb on 2024-07-30 16:44_

---

_Comment by @zanieb on 2024-07-30 21:14_

This should basically be trivial, we just need to setup a folder with a few variants, copy in the executable from the scratch image, and publish them each release.

---

_Label `help wanted` added by @zanieb on 2024-07-30 21:14_

---

_Comment by @konstin on 2024-07-31 07:42_

When building an optimized production image, i'd need two base images: A builder with uv, my python version and ideally a bunch of dev libraries (curl, git, gcc, dev headers), and a runner image that has python in the same location as the builder, in which i only copy the venv and call `python` as entrypoint.

---

_Comment by @zanieb on 2024-07-31 13:16_

I think we can describe a few goals here:

1. Allow uv to be used in GitLab CI, i.e. provide a image with a shell
2. Allow uv to be used as a Python image, i.e. provide an image for each Python version with development tools
3. Document best practices for deriving a minimal production image from (2)

---

_Comment by @mayuresh-quartic on 2024-07-31 17:18_

I can suggest few approach to this:
- Pull latest python slim images
- Install uv latest version in global system

I can create Dockerfiles quickly for this

This will be minimal with no extra apt or system packages 

@zanieb 

---

_Comment by @mayuresh-quartic on 2024-07-31 17:21_

Later I can think of some CI to automate this:

On official release publish these Python version images with UV installed:
Python3.8
Python3.9
Python3.10
Python3.11
Python3.12

or as per UV support matrix


---

_Comment by @zanieb on 2024-07-31 17:26_

Sure, a couple notes:

- I think I'd use a single Dockerfile parameterized on Python version. 
- We should copy the `uv` executable from our scratch Docker image instead of installing from, e.g., curl

---

_Comment by @mayuresh-quartic on 2024-07-31 17:30_

For example here is the uv image i created based on Python3.9 version: mayuresh14/uv-python39-slim

Dockerfile looks like this
```
FROM --platform=linux/amd64 python:3.9-slim

RUN pip install uv
```

I will keep in mind your mentioned notes and create a PR soon


---

_Referenced in [astral-sh/uv#6053](../../astral-sh/uv/pulls/6053.md) on 2024-08-13 05:29_

---

_Closed by @zanieb on 2024-09-03 13:44_

---
