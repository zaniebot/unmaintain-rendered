```yaml
number: 11398
title: Dockerizing a package
type: issue
state: closed
author: sergeykad
labels:
  - question
assignees: []
created_at: 2025-02-10T19:38:43Z
updated_at: 2025-07-16T13:51:37Z
url: https://github.com/astral-sh/uv/issues/11398
synced_at: 2026-01-12T16:00:35Z
```

# Dockerizing a package

---

_@sergeykad_

### Question

I have a project that uses workspaces and contains microservices and libraries used by the microservices.

What is the recommended approach to build a Docker image for a single microservice with all the packages it depends on?

I checked the examples and the documentation but didn't find anything similar. 

### Platform

_No response_

### Version

Latest 

---

_Label `question` added by @sergeykad on 2025-02-10 19:38_

---

_Comment by @konstin on 2025-02-27 17:49_

It should be very similar to https://github.com/astral-sh/uv-docker-example, with a Dockerfile in the root of the workspace. The different is that you add the workspace directories in the docker build and that you potentially have to add arguments to `uv sync` to select the right service to install.

---

_Comment by @sergeykad on 2025-03-04 17:58_

Hi @konstin, thanks for the feedback!

I'm working on implementing your suggestions, but I have a couple of quick questions about it:

1.  **Service Selection:** When you mentioned arguments for `uv sync` to select a service, were you referring to `--package`?  How can I use `--package` in a workflow with two `uv sync` commands (before and after source copy) to avoid installing all workspace dependencies in the first step?

2.  **Docker Image Size:** To avoid containerizing the whole UV workspace, is a multi-stage build the way to go?  And if so, which directories should I copy to the final image? Is [this example](https://docs.astral.sh/uv/guides/integration/docker/#non-editable-installs) the correct approach?

Thanks again!

---

_Comment by @konstin on 2025-03-06 10:55_

1. Do you mean like `--no-install-project`? It depends a bit on how your workspace is layed out.
2. Yes, a multi-stage build like in that example is the way to go.

---

_Comment by @stefanadelbert on 2025-03-31 14:17_

@sergeykad I'm faced with exactly the same situation. I'm working on a project which uses docker compose with multiple services. I want to use a `uv` workspace for the project where each service is a member of the workspace. I want to build an image for each service which only has the necessary dependencies installed in it.

My project structure looks something like this:

```
- pyproject.toml
- uv.lock
- .venv
- compose.yaml
- packages
  - package1
    - pyproject.toml
    - Dockerfile
  - package2
    - pyproject.toml
    - Dockerfile
```

The docker image for `package1` should only have the necessary packages installed in it, i.e. not all the workspace packages.

I can see how I could run `uv sync --frozen --no-install-project --package package1` and that will install just the dependencies for package1, but the `uv.lock` file is outside of the build context, i.e. it's at `../../uv.lock` relative to the Dockerfile, so I can't get it into the docker image nicely. I would prefer to bind mount uv.lock, as the `uv` docs suggest.

I would be really interested to know how you've solved this.

---

_Comment by @sergeykad on 2025-03-31 17:43_

@stefanadelbert I put Dockerfile in the root directory, but you can try using Docker Bake instead. I don't know though how well it works with compose. 

---

_Comment by @zanieb on 2025-04-01 20:41_

@stefanadelbert 

Why can't you use `docker build . --file subdir/Dockerfile` to build from the project root?

---

_Comment by @stefanadelbert on 2025-04-02 06:51_

@zanieb @sergeykad 

Thanks for the responses.

I've thought about this quite a bit over the last couple of days. The more I think about it, the more I realise that the build context for the various Docker images needs to be the workspace root. This will mean that wherever the Dockefiles are located, they will need to use paths that are relative to the workspace root.

It feels natural to me to have the Dockerfile in the same location as the build context, so that paths in the Dockerfile behave as if they are relative to the Dockerfile. This would suggest Dockerfiles in the workspace root. And that idea is feeling less uncomfortable to me the more I think about it.

So, the revised structure would look like below, where the compose file specifies build context of `.` (which is the default anyway) and also specifies specific Dockerfiles.

```
- pyproject.toml
- uv.lock
- .venv
- compose.yaml
- package1.Dockerfile
- package2.Dockerfile
- packages
  - package1
    - pyproject.toml
  - package2
    - pyproject.toml
```

`compose.yaml`
```
...
services:
  package1:
    build:
      context: .
      dockerfile: package1.Dockerfile
...
```

Furthermore, I actually want `package2` to have `package1` as a dependency, which means at build time for `package2`'s image, the code for `package1` will need to be present. So I will copy the entire `packages` source tree into the build stage of each image and then inter-stage copy just the `.venv` into the run stage for each image.

It feels like it will work. Any comments on this approach would be much appreciated.

---

_Comment by @zanieb on 2025-04-02 14:17_

That makes sense to me!

---

_Comment by @stefanadelbert on 2025-04-09 09:07_

*Update*

I'm following through with this plan and it's working  well.

I'm using multi-layer and multi-stage Dockerfile to keep the image build process as lite as possible. However... I run into a situation where for one of my images, I need to install a workspace member and I wondering if there are any good ideas out there to keep things efficient.

Imagine a workspace (`W`) with two members (`A` and `B`). `A` is an app and `B` is a lib. `A` depends on `B`. Let's say I want to build a single docker image for the app, `A`.

The workspace structured is,

```
workspace
├── packages
│   ├── A
│   │   ├── pyproject.toml
│   │   └── src
│   │       └── A
│   │           ├── __init__.py
│   │           └── foo.py
│   └── B
│       ├── pyproject.toml
│       └── src
│           └── B
│               ├── __init__.py
│               └── bar.py
├── pyproject.toml
├── README.md
├── uv.lock
├── A.Dockerfile
```

The dockerfile might look something like 

```dockerfile
# Use a Python image with uv pre-installed
FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim AS builder
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy
WORKDIR /app

RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=packages/A/pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-editable --no-dev

ADD ./packages/A /app
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=packages/A/pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-dev 

FROM python:3.12-slim-bookworm AS runner
WORKDIR /app
COPY --from=builder /app/.venv/ /app/.venv
COPY --from=builder /app/src /app/src
ENV PATH="/app/.venv/bin:$PATH"
```

The problem is that the workspace member `B` is required by the first `uv sync` command, but it's not there yet. I could obviously copy the code for `B` into the image beforehand (`ADD ./packages/ /app/packages`), and this does work, but that undoes the separation of the two distinct `uv sync` layers.

Any advice or ideas on this?

---

_Comment by @konstin on 2025-04-11 10:42_

Unless you have a strong incentive to do otherwise, I'd keep the workspace structure as-is in the production image. You can bind mount `packages` in their entirety and then install both B and A in one step, while separating the installation of the external dependencies into a separate step with `--no-install-workspace`. Usually, the external dependencies are well cacheable, while it's not worth separating the installation of in-repo packages.

---

_Comment by @stefanadelbert on 2025-04-11 15:25_

@konstin This is exactly what I'm after. Thank you.

I'm now doing a `uv sync --no-install-workspace` as the first install step, which is the key.
I'm then bind mounting in the whole `packages` directory and running `uv sync` to install the workspace members.
In the second `runner` stage I'm doing an ADD of just the application sources.

I see a potential problem where this image will rebuild from the second `uv sync` layer in the `builder` stage if any files change in `packages/`, including files that don't actually affect this image. In this contrived example this is irrelevant, but image that there are `packages/[A,B,C,D]`, and if something changes in `packages/C`, then this image would do a redundant rebuild. I could be more selective about the subdirectories in `packages/` that I bind mount in, but I'm trying to find a general form for a Dockerfile that will work correctly and efficiently (in that order), so I'd prefer to keep things general.

```dockerfile
FROM ghcr.io/astral-sh/uv:python3.12-bookworm-slim AS builder
ENV UV_COMPILE_BYTECODE=1 UV_LINK_MODE=copy
WORKDIR /app
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=packages/A/pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-workspace --no-dev
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=packages/A/pyproject.toml,target=pyproject.toml \
    --mount=type=bind,source=packages,target=/app/packages \
    uv sync --frozen --no-editable --no-dev 

FROM python:3.12-slim-bookworm AS runner
WORKDIR /app
COPY --from=builder /app/.venv/ /app/.venv
ADD ./packages/A /app/src
ENV PATH="/app/.venv/bin:$PATH"
```

---

_Comment by @konstin on 2025-04-15 11:21_

It's true that a single package change triggers a complete rebuild, but unless there is a package that expensive to build (e.g. due to native code), the second RUN step should usually be fast enough to not need separate caching.

---

_Closed by @charliermarsh on 2025-07-16 13:51_

---
