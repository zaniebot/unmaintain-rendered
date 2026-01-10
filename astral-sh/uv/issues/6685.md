```yaml
number: 6685
title: "`--no-install-workspace` needs the pyproject.toml of each workspace member to be present"
type: issue
state: closed
author: konstin
labels:
  - bug
assignees: []
created_at: 2024-08-27T12:54:25Z
updated_at: 2025-07-22T16:46:55Z
url: https://github.com/astral-sh/uv/issues/6685
synced_at: 2026-01-10T03:32:44Z
```

# `--no-install-workspace` needs the pyproject.toml of each workspace member to be present

---

_Issue opened by @konstin on 2024-08-27 12:54_

`uv sync --no-dev --no-install-workspace --locked` doesn't work when only the root `pyproject.toml` and `uv.lock` are present. We should allow installing the entire workspace with `--no-install-workspace --locked` when only the top level `uv.lock` is present.

Reproducer failing:

```dockerfile
FROM ubuntu AS builder

RUN mkdir /app
WORKDIR /app
ENV UV_PYTHON_INSTALL_DIR=/app/python

COPY --from=ghcr.io/astral-sh/uv /uv /bin/uv

# Install the dependencies
ADD pyproject.toml uv.lock /app/
RUN uv sync --no-dev --no-install-workspace --locked

# Install the project itself
ADD src /app/src
ADD packages /app/packages
RUN uv sync --no-dev --locked
```

Passing:
```dockerfile
FROM ubuntu AS builder

RUN mkdir /app
WORKDIR /app
ENV UV_PYTHON_INSTALL_DIR=/app/python

COPY --from=ghcr.io/astral-sh/uv /uv /bin/uv

# Install the dependencies
ADD pyproject.toml uv.lock /app/
ADD packages/bird-feeder/pyproject.toml /app/packages/bird-feeder/pyproject.toml
ADD packages/seeds/pyproject.toml /app/packages/seeds/pyproject.toml
RUN uv sync --no-dev --no-install-workspace --locked

# Install the project itself
ADD src /app/src
ADD packages /app/packages
RUN uv sync --no-dev --locked
```

---

_Comment by @charliermarsh on 2024-08-27 12:56_

The same in spirit as #6573 though makes sense to keep separate; it might be separately fixable.

---

_Comment by @konstin on 2024-08-27 12:59_

Will probably fix them in one go but i wanted to document the test case.

---

_Comment by @charliermarsh on 2024-08-27 19:38_

I _think_ this one is easier to fix.

---

_Label `bug` added by @charliermarsh on 2024-08-27 20:03_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-28 02:20_

---

_Closed by @charliermarsh on 2024-08-28 11:58_

---

_Closed by @charliermarsh on 2024-08-28 11:58_

---

_Comment by @AnkitSiva on 2025-05-07 18:46_

I'm still running into this issue. I have a minimal reproducible setup [here](https://github.com/AnkitSiva/uv_minimal_repro). I think you may have fixed the `--frozen` but not the `--locked` case?

---

_Comment by @charliermarsh on 2025-05-07 18:48_

Locked requires all the members to be present. Locked requires that we validate the lockfile, which requires a resolution, which requires the member pyproject.toml files.

---

_Comment by @AnkitSiva on 2025-05-07 18:50_

Gotcha, thanks for the blazing fast reply!

---

_Comment by @leonardocustodio on 2025-07-22 16:44_

In that case, this guide should be reviewed: https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers

> If you're using a [workspace](https://docs.astral.sh/uv/concepts/projects/workspaces/), then use the --no-install-workspace flag which excludes the project and any workspace members.

It gives the impression that you should use `--no-install-workspace` and it will work out of the box, even if it is being suggested to use `--lock` just above

Though I also get the error without the `--lock`, maybe there is some other issue?

---

_Comment by @charliermarsh on 2025-07-22 16:46_

I believe we only fixed this for `--frozen`. I don't think we should or even can fix it for `--locked`.

---

_Comment by @leonardocustodio on 2025-07-22 16:46_

I mean without it it gives the same error, I suppose it defaults to `--locked` if you don't specify? I think just adding that in the highlight, which needs to be used with frozen, would be sufficient.

---
