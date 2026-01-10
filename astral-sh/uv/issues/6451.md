```yaml
number: 6451
title: "Docker: feedback on docs and workflow with frozen/locked dependencies and `--require-hashes`"
type: issue
state: closed
author: bofm
labels: []
assignees: []
created_at: 2024-08-22T17:29:01Z
updated_at: 2025-04-01T23:56:17Z
url: https://github.com/astral-sh/uv/issues/6451
synced_at: 2026-01-10T03:41:46Z
```

# Docker: feedback on docs and workflow with frozen/locked dependencies and `--require-hashes`

---

_Issue opened by @bofm on 2024-08-22 17:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello Astral team! Thank you for your work on Ruff and UV! It makes a huge difference.

Please clarify the best way to install the locked dependencies and validate by the hashes when building a Docker image.

## Key points when building a Docker image of an app:
* We want minimum extra stuff inside the container.
* No need to create a venv.
* No need to install uv, because we can mount it (assume we use modern Docker with BuildKit).
* For production usage, we should use frozen/locked dependencies and validate them by hashes.

## Problems:
* `uv sync` creates a venv. This is redundant in a Docker container with a single app.
* The `uv sync`-way in Docker also requires using `uv run` as cmd or setting the venv env var or override PATH.
* The Docker section of the docs on the website seems to mix the new (UV-only) workflow and the old workflow when the UV is used as a faster Pip.
* It is unclear which way is preferred. A new user will be confused.
* [Installing a project](https://docs.astral.sh/uv/guides/integration/docker/#installing-a-project_1) suggests doing `uv pip install -r pyproject.toml`. But this way the lock/freeze files are not used and the build may be non-idempotent. And the hashes are not checked.

## Workaround

This is what I ended up with right now with uv 0.3.1.

```Dockerfile
FROM python:3.12-slim as uv
# Installing via pip because there is no (yet) docker image for armv7 
RUN --mount=type=cache,target=/root/.cache pip install -U uv

FROM python:3.12-slim

RUN mkdir /tmp/app
WORKDIR /tmp/app

COPY requirements.lock .
RUN --mount=type=cache,target=/root/.cache \
    --mount=from=uv,source=/usr/local/bin/uv,target=/bin/uv \
#  !!!! this grep hack !!!!
    cat requirements.lock | grep -vE '^-e' > requirements.txt \ 
    && uv pip install --system --require-hashes --link-mode copy -r requirements.txt
```

## What it should be like

I felt like I need something like `uv sync —system --frozen-only` which would not create a venv and would only use `uv.lock` to install the dependencies.

## Versions

* uv 0.3.1
* Dev: MacOS Sonoma (14.3.1) on MacBook Pro with Apple Silicone arch.
* Prod: Ubuntu Linux 22.04 on armv7 arch.

---

_Comment by @charliermarsh on 2024-08-22 17:31_

There's something like this brewing in https://github.com/astral-sh/uv/pull/6398, designed for this very purpose (with discussion here: https://github.com/astral-sh/uv/issues/4028). It won't solve the `--system` part yet, that will come separately.

---

_Comment by @charliermarsh on 2024-08-22 17:32_

(Sorry, can't give a full reply now, I just wanted to point you there since it creates `uv sync --frozen --no-locals` to the project itself.)

---

_Comment by @charliermarsh on 2024-08-22 17:32_

You can run `uv sync --frozen` today though to install from only `uv.lock`.

---

_Comment by @bofm on 2024-08-22 17:41_

> You can run `uv sync --frozen` today though to install from only `uv.lock`.

Yes, but this way it creates a venv and installs in there. The desired way is to install into the system python in the docker container.

UPD1: and it also requires the pyproject.toml to be copied into the workdir.

---

_Comment by @charliermarsh on 2024-08-22 17:43_

Yeah the `--no-locals` from that PR would make it such that we _don't_ require the `pyproject.toml`.

We don't yet support installing into system Python but I suspect we will soon for this reason.

---

_Comment by @bofm on 2024-08-22 18:03_

There is a good point in https://github.com/astral-sh/uv/issues/4028#issuecomment-2305322593

> uv sync is not a command designed for application install

What would be left after `--no-locals` is merged and the project deps are installed is that we'd still need to install the project itself (currently `uv pip install .`) which mixed the two worlds: pip and non-pip. We got ourselves another P vs NP problem.

---

_Comment by @charliermarsh on 2024-08-22 20:47_

Not quite -- you can see the example in the PR:

```dockerfile
# Copy the lockfile into the image
ADD uv.lock /app/uv.lock

# Install remote dependencies
WORKDIR /app
RUN uv sync --frozen --no-locals

# Copy the project into the image
ADD . /app
WORKDIR /app

# Sync the remaining dependencies
RUN uv sync --frozen
```

The benefit here is that you can create a cacheable layer with your project's dependencies.

---

_Comment by @bofm on 2024-08-22 22:09_

Nice! What's still missing? Omitting venv creation?

---

_Comment by @albertotb on 2024-08-24 14:18_

I could not find any issue that tracks the feature request of omitting venv creation so I will share another use case I found here (besides the Docker one).

### Context
Mypy is notoriusly difficult to run correctly using pre-commit, since pre-commit uses an isolated env for every hook but mypy wants to have all your dependencies installed. To get around that, one can use `language: system` to omit venv creation and use the project's venv (with mypy and all dependencies installed):

```{yaml}
- repo: https://github.com/pre-commit/mirrors-mypy
  rev: v1.6.1
  hooks:
    - id: mypy
      language: system
```

### Problem
In CI you need to install the dependencies to run mypy. If using uv, currently it looks something like this:

```
jobs:
  pre-commit:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-python@v5
        with:
          python-version-file: ".python-version"
      - name: Set up uv
        run: curl -LsSf https://astral.sh/uv/0.3.1/install.sh | sh
      - name: Install dependencies
        run: uv sync --dev --frozen
      - uses: pre-commit/action@v3.0.1
```

However, that does not work because pre-commit uses the system Python and dependencies (and mypy) are installed into a venv. 

### Current solutions
As a workaround, you can either run pre-commit manually or add an extra step before pre-commit that adds the venv to the path and installs pip (needed for pre-commit to install the tools into the isolated venvs):

```
      - run: |
          echo "$PWD/.venv/bin" >> $GITHUB_PATH
          uv pip install pip
```

The solution would be straightforward if a `--system` flag existed in `uv sync`.

EDIT: I've just found issue https://github.com/astral-sh/uv/issues/5964 that seems to be asking exactly for this

---

_Comment by @inflation on 2024-08-28 02:28_

Furthermore, some Python libraries with native extensions must be built from sources, which require a compiler (or more than one, e.g. nvcc, rustc, etc). We want to keep that in the builder image only. 

For now, we copy the `.venv` folder and have a workaround to fix the symlink:
```dockerfile
RUN uv python install 3.10
RUN ln -sf $(uv python find 3.10) $HOME/$DIR_NAME/.venv/bin/python 
```

It would be great to have a mechanism to "bundle" all the dependencies and python itself, so we could copy it to the runtime image and run the app without uv.

---

_Comment by @zanieb on 2025-04-01 23:56_

There's `UV_PROJECT_ENVIRONMENT` for omitting the virtual environment now. I'm not sure what else is actionable here.



---

_Closed by @zanieb on 2025-04-01 23:56_

---
