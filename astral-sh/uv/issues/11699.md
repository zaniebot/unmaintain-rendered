```yaml
number: 11699
title: "Intermediate layer multi-arch Docker builds have intermittent hang during \"uv sync\""
type: issue
state: closed
author: vicchi
labels:
  - bug
assignees: []
created_at: 2025-02-21T15:15:13Z
updated_at: 2025-02-28T16:51:15Z
url: https://github.com/astral-sh/uv/issues/11699
synced_at: 2026-01-10T03:50:31Z
```

# Intermediate layer multi-arch Docker builds have intermittent hang during "uv sync"

---

_Issue opened by @vicchi on 2025-02-21 15:15_

### Summary

Using the [intermediate layers approach](https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers) with a multi-arch Docker build causes the build to frequently hang during the second `uv sync` when building for an architecture which is not the one the build is being invoked from.

Or to put it another way, building for `linux/arm64/v8` and `linux/amd64` on a `linux/amd64` machine, the build will succeed on the native architecture and roughly 50-60% of the time hang on the non-native, `linux/arm64/v8` build. And by hang I mean the build was still "running" an hour after it commenced where the average successful full rebuild time, without the Docker cache, is just over 1 minute.

Limiting the build architectures to a single target and the builds run to completion, both for native and non-native architectures. Enabling multiple target architectures and the hangs will start to recur.

In the `Dockerfile` below, it's the second invocation of `uv sync` where the hang, when it happens, will reliably occur.

This is happening as part of migrating our Docker builds for `<day-job>` which makes a reproducible test case ... challenging as there's lots of components and private depedencies in our private GitHub repos. However, a redacted set of build components are below.

`pyproject.toml` :-

```
[project]
name = "refdata-api"
version = "2.7.0"
description = "The Kamma Reference Data API"
authors = [
    {name = "Gary Gale", email = "gary@<redacted>"}
]
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "authlib==1.2.0",
    "cachetools==5.3.0",
    "diskcache==5.4.0",
    "fastapi==0.109.1",
    "gunicorn==22.0.0",
    "httpx==0.23.3",
    "itsdangerous==2.1.2",
    "jinja2==3.1.4",
    "meilisearch==0.25.0",
    "phonenumbers==8.13.25",
    "psycopg2-binary==2.9.10",
    "pydantic==1.10.13",
    "python-dateutil==2.8.2",
    "python-dotenv==1.0.0",
    "python-ulid==1.1.0",
    "refdata-machinetag==1.1.1",
    "refdata-models==2.1.1",
    "requests==2.31.0",
    "setproctitle==1.3.2",
    "typer[all]==0.7.0",
    "uvicorn==0.21.1",
]

[project.scripts]
server = "refdata_api.server:main"

[project.urls]
Homepage = "https://github.com/<redacted>refdata-api"
Repository = "https://github.com/<redacted>/refdata-api.git"

[dependency-groups]
dev = [
    "autopep8>=2.3.2",
    "flake8>=7.1.2",
    "flake8-alphabetize>=0.0.21",
    "flake8-bugbear>=24.12.12",
    "flake8-pyproject>=1.2.3",
    "pylint>=3.3.4",
    "pytest>=8.3.4",
    "pytest-cov>=6.0.0",
    "pytest-dotenv>=0.5.2",
    "pytest-env>=1.1.5",
]

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[tool.setuptools]
package-dir = {"" = "src"}

[tool.setuptools.packages.find]
where = ["src"]

[tool.uv]
package = true

[tool.uv.sources]
refdata-machinetag = { git = "ssh://git@github.com/<redacted>/refdata-machinetag.git", rev = "v1.1.1" }
refdata-models = { git = "ssh://git@github.com/<redacted>/refdata-models.git", rev = "v2.1.1" }
```

Build command (from our `Makefile`) :-

```
[ -x /usr/bin/docker-credential-ecr-login ] || aws ecr get-login-password --region "eu-west-1" --profile default | \
        docker login --username AWS --password-stdin <redacted>.dkr.ecr.eu-west-1.amazonaws.com
docker buildx inspect refdata-builder > /dev/null 2>&1 || \
        docker buildx create --name refdata-builder --bootstrap --use
docker buildx use refdata-builder
docker buildx build --platform="linux/arm64/v8,linux/amd64" \
        --file ./docker/refdata-api/Dockerfile \
        --push \
        --tag <redacted>.dkr.ecr.eu-west-1.amazonaws.com/refdata/refdata-api:latest \
        --provenance=false \
        --progress=plain \
        --ssh default \
        --build-arg VERSION=2.7.0-beta.1 \
        --no-cache --build-arg UBUNTU_RELEASE=22.04 .
```

`Dockerfile` :-

```
ARG UBUNTU_RELEASE=latest
ARG VERSION=latest
FROM ubuntu:${UBUNTU_RELEASE} AS builder

### --------------------------------------------------------------------------------
### Start - build prep

ARG UBUNTU_RELEASE
ARG VERSION

SHELL ["/bin/sh", "-exc"]
ENV DEBIAN_FRONTEND=noninteractive

# - install Python 3.10 including installer tools
# - install Git and SSH to allow access to private GitHub repos as dependencies

RUN <<EOT
apt-get update -qy
apt-get install -qyy \
    -o APT::Install-Recommends=false \
    -o APT::Install-Suggests=false \
    git \
    openssh-client \
    python3 \
    python3-setuptools
mkdir -p "${HOME}"/.ssh
chmod 0600 "${HOME}"/.ssh
ssh-keyscan github.com >> "${HOME}"/.ssh/known_hosts
EOT

# - install uv, but just for this layer
COPY --from=ghcr.io/astral-sh/uv:latest /uv /usr/local/bin/uv

# - setup uv for this build ...
# - copy instead of hard linking (which uv will complain about)
# - compile to bytecode after installation for faster startup times
# - use the just installed Python version and don't (un-helpfully) download one
# - tell uv which Python binary to use
# - tell uv sync where to sync to

ENV UV_LINK_MODE=copy \
    UV_COMPILE_BYTECODE=1 \
    UV_PYTHON_DOWNLOADS=never \
    UV_PYTHON=/usr/bin/python3.10 \
    UV_PROJECT_ENVIRONMENT=/opt/kamma

### End - build prep

### --------------------------------------------------------------------------------
### Start - dependencies and build

# - install runtime dependencies only, without the application itself
# - this layer will be cached until either uv.lock or pyproject.toml
#   change (these are mounted here into the builder layer as we
#   don't need them at runtime)


RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    --mount=type=ssh \
    uv sync \
    --frozen \
    --no-dev \
    --no-install-project

# - install everything else (the application itself), without dependencies
# - /build is temporary to this layer and won't be copied into the runtime layer

ADD . /build
WORKDIR /build

RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=ssh \
    uv sync \
    --frozen \
    --no-dev \
    --no-editable

### End - dependencies and build
### --------------------------------------------------------------------------------
### Start - runtime

ARG UBUNTU_RELEASE
FROM ubuntu:${UBUNTU_RELEASE}

ARG UBUNTU_RELEASE
ARG VERSION

SHELL ["/bin/sh", "-exc"]
ENV DEBIAN_FRONTEND=noninteractive

# - setup runtime search paths

ENV PYTHONPATH=/opt/kamma
ENV PATH=/opt/kamma/bin:${PATH}
ENV VIRTUAL_ENV=/opt/kamma

# - install curl for health checks
# - install Python 3.13 without installer or development tools
# - install tini as a container init to allow signals to be correctly propagated

RUN <<EOT
apt-get update -qy
apt-get install -qyy \
    -o APT::Install-Recommends=false \
    -o APT::Install-Suggests=false \
    curl \
    python3 \
    tini
apt-get clean
rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
EOT

COPY ./docker/refdata-api/files/docker-entrypoint.sh /opt/kamma/
COPY --from=builder /opt/kamma /opt/kamma

EXPOSE 80

WORKDIR /opt/kamma

ENTRYPOINT ["/usr/bin/tini", "-v", "--", "/opt/kamma/docker-entrypoint.sh"]
CMD ["gunicorn", "refdata_api.server:api", "--bind", "0.0.0.0:80"]
STOPSIGNAL SIGINT
HEALTHCHECK CMD curl --silent --fail http://localhost:80/ping || exit 1

### End - runtime

### --------------------------------------------------------------------------------
### Start - smoke test

# - check that the installed Python version is what we expect
# - check that one of the dependencies can be imported
# - check that what we've installed can actually be imported

RUN <<EOT
python3 --version
python3 -Ic 'import uvicorn'
python3 -Ic 'import refdata_api'
EOT

### End - smoke test
```

### Platform

Ubuntu 22.04 - Linux 6.8.0-49-generic x86_64 GNU/Linux

### Version

uv 0.6.2 (Homebrew 2025-02-19)

### Python version

Python 3.10.12

---

_Label `bug` added by @vicchi on 2025-02-21 15:15_

---

_Comment by @vicchi on 2025-02-24 10:02_

Update ... I've managed to come up with a reproducible test case; see https://github.com/vicchi/buildtest

In terms of test environments:

To run the build, clone the repo above, copy `.env.sample` to `.env` and update with your GitHub API credentials. The repo assumes `direnv`, `uv` and Docker are pre-installed and available. Run `make build` to build with Docker layer caching and `make rebuild` to build without layer caching.

# Build Test 1

* Build host: Ubuntu 22.04 `Linux 6.8.0-49-generic x86_64 GNU/Linux`
* uv version:`uv 0.6.2 (Homebrew 2025-02-19)`
* Docker: `Docker version 28.0.0, build f9ced58`
* Buildx: `github.com/docker/buildx v0.21.0 34ed52e`

Build for `linux/amd64` only succeeds: https://gist.github.com/vicchi/9df29f32d822a0454bba8c56e32830a0
Build for `linux/arm64` only succeeds: https://gist.github.com/vicchi/9bbb41113ea991ef7f9a31b61de9abeb
Build for `linux/amd64` and `linux/arm64` hangs: https://gist.github.com/vicchi/101c521e080fcb9f572b8f8540503039

# Build Test 2

* Build host: Ubuntu 24.04 `Linux 6.8.0-49-generic x86_64 GNU/Linux`
* uv version:`uv 0.6.2 (Homebrew 2025-02-19)`
* Docker: `Docker version 28.0.0, build f9ced58`
* Buildx: `github.com/docker/buildx v0.21.0 34ed52e`

Build for `linux/amd64` only succeeds
Build for `linux/arm64` only succeeds
Build for `linux/amd64` and `linux/arm64` hangs

# Build Test 3

* Build host: macOS Sequoia 15.3.1 `Darwin 24.3.0 arm64`
* uv version:`uv 0.6.2 (Homebrew 2025-02-19)`
* Docker: `Docker version 28.0.0, Docker version 28.0.0, build f9ced58158`
* Buildx: `github.com/docker/buildx v0.20.1-desktop.2 aaf7c2bc7f9ec3afee1cec77d671845a4b57a0c8`

Build for `linux/amd64` only succeeds
Build for `linux/arm64` only succeeds
Build for `linux/amd64` and `linux/arm64` succeeds


---

_Comment by @konstin on 2025-02-24 10:13_

Thanks for the clear reproducer, this seems to be another instance of https://github.com/astral-sh/uv/issues/6105. I've transformed your example into an MRE: https://github.com/astral-sh/uv/issues/6105.

Am I reading your comment correctly that only arm64 ever fails, and only if the host is also linux?

---

_Comment by @vicchi on 2025-02-24 12:28_

>Am I reading your comment correctly that only arm64 ever fails, and only if the host is also linux?

@konstin Thanks for the reply and yes ... if the build host is `linux/amd64` then the build only hangs if a) the target architecture is `linux/arm64` and b) this is a multi-arch build which also targets the build host's architecture as `linux/amd64`.

This issue doesn't occur if the build host if `linux/arm64` (macOS).

That said, these two architectures are the only ones we target, but I'll add another CPU architecture into the mix to see if it's `linux/arm64` which is the root cause in a multi-arch build or any architecture which isn't the host's.

---

_Comment by @vicchi on 2025-02-24 13:02_

@konstin Ah ... `ghcr.io/astral-sh/uv` only supports `linux/amd64` and `linux/arm64` which makes testing other CPU architectures ... interesting 😄 

---

_Comment by @konstin on 2025-02-24 13:09_

With cargo-zigbuild, building should work with `rustup target add <...> && cargo zigbuild --profile profiling --target <...>`.

---

_Comment by @vicchi on 2025-02-24 13:17_

@konstin I read through #6105 and on a hunch disabled bytecode compilation in my `Dockerfile` ... so `UV_COMPILE_BYTECODE=0` ... and both architecture's builds complete successfully as evidenced by https://github.com/vicchi/buildtest/pkgs/container/buildtest

It's not a fix but it's a viable workaround in the interim.

---

_Comment by @konstin on 2025-02-24 13:34_

I assume it has something with qemu not properly handling the way we spawn the worker processes and pass their stdin/stdout, but i couldn't yet figure out yet where. With `RUST_LOG="uv_installer::compile=trace"`, we show that some of the workers exit while others get stuck, but i couldn't yet figure out how to attach gdb to properly debug where its stuck (https://github.com/astral-sh/uv/issues/6105#issuecomment-2677954298). Deactivating bytecode compilation on aarch64 is a workaround for now.

---

_Comment by @vicchi on 2025-02-24 14:17_

@konstin I'll take a fully functioning build, albeit one with slightly slower startup times, over a build which doesn't build any day.

If there's any more diagnositics, tests or changes you'd like me to make on my environment please shout loudly at me

---

_Comment by @konstin on 2025-02-28 16:51_

I'm merging this with #6105 since we found the cause

---

_Closed by @konstin on 2025-02-28 16:51_

---
