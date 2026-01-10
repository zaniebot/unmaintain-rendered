---
number: 13071
title: optional-dependency not synced with docker
type: issue
state: closed
author: gabriel-vanzandycke
labels:
  - question
assignees: []
created_at: 2025-04-23T16:23:53Z
updated_at: 2025-04-28T10:19:19Z
url: https://github.com/astral-sh/uv/issues/13071
synced_at: 2026-01-10T01:25:28Z
---

# optional-dependency not synced with docker

---

_Issue opened by @gabriel-vanzandycke on 2025-04-23 16:23_

### Summary

I'm running `jupyterlab` within a docker container for my pytorch experiments. All was working well until I added an optional dependency `customlib` which depends on a private index:

**Note: Edited to take [charliermarsh's comment](https://github.com/astral-sh/uv/issues/13071#issuecomment-2824998485) into account**

Here is my (simplified for sake of clarity) setup, I'm not sure whether it is a bug or a misconfiguration:

**Dockerfile**
```Dockerfile
FROM pytorch/pytorch:2.2.0-cpu
COPY --from=ghcr.io/astral-sh/uv:0.6.16 /uv /bin/uv

# Setting working directory to the root user default home directory
WORKDIR /home/app

# Handle cache and target being on different filesystems
ENV UV_LINK_MODE=copy

# Install dependencies
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-install-workspace --extra jupyter --extra optional
```

**pyproject.toml**
```pyproject.toml
[project]
name = "experiment"
version = "0.1.0"
requires-python = "==3.11.*"
dependencies = [
    "numpy>=1.22",
    "lightning>=2.4.0",
    "astconfig==0.2.0",
]

[project.optional-dependencies]
jupyterlab = ['jupyterlab>=4.3.3']
optional = ["customlib==1.0.0"]

[tool.uv]
package = true # forces packaging this project

[tool.uv.sources]
customlib = { index = "private" }

[[tool.uv.index]]
name = "private"
url = "https://gitlab.private.com/pypi/simple"
```


**docker-compose.yaml**
```yaml
services:
  jupyter:
    build: . # Builds the image from the Dockerfile in the current directory
    image: app:latest
    hostname: "${HOSTNAME}" # environment variable defined in your .profile for instance
    command: ["uv", "run", "--with", "jupyter", "jupyter", "lab", "--port=${JUPYTERLAB_PORT}", "--ip=${HOSTNAME}", "--allow-root", "--no-browser"]
    ports:
        - ${JUPYTERLAB_PORT}:${JUPYTERLAB_PORT}
    environment:
      JUPYTERLAB_PORT: ${JUPYTERLAB_PORT}
    volumes:
      - .:/home/app
      - /home/app/.venv
    shm_size: '1g'
    env_file:
      - .env # Load environment variables from .env file which includes keys from the private index
```

After creating the container from scratch
```bash
docker compose build --no-cache
```

The dependencies resolver of `uv` tries to search `wheel` into the private index, while it should be fetched from default Pypi.
```
0.386   × Failed to download `wheel==0.43.0`
0.386   ├─▶ Failed to fetch:
0.386   │   `https://gitlab.private.com/api/pypi/files/<random-key>/wheel-0.43.0-py3-none-any.whl`
0.386   ╰─▶ HTTP status client error (401 Unauthorized) for url
0.386       (https://gitlab.private.com/api/pypi/files/<random-key>/wheel-0.43.0-py3-none-any.whl)
0.386   help: `wheel` (v0.43.0) was included because `experiment` (v0.1.0) depends
0.386         on `astconfig` (v0.2.0) which depends on `astunparse` (v1.6.3) which
0.386         depends on `wheel`
------
failed to solve: process "/bin/sh -c uv sync --frozen --no-install-project --no-install-workspace --extra jupyterlab --extra das" did not complete successfully: exit code: 1
```

The `.env` file, loaded with the `docker-compose.yaml` holds the credentials for the private index:
```
UV_INDEX_PRIVATE_USERNAME=__token__
UV_INDEX_PRIVATE_PASSWORD=key
```

Anyone could help ?
Thanks in advance.

### Platform

Ubuntu 22.04.4 LTS

### Version

0.6.16

### Python version

Python 3.11.9

---

_Label `bug` added by @gabriel-vanzandycke on 2025-04-23 16:23_

---

_Comment by @charliermarsh on 2025-04-23 17:17_

Thanks for filing. You def need to include `--extra optional` in your `uv sync` command -- otherwise we'll omit all of the extras. I think your `jupyterlab` command works because it's using `"uv", "run", "--with", "jupyter"`, so you're "manually" injecting Jupyter into the environment.

---

_Label `bug` removed by @charliermarsh on 2025-04-23 17:17_

---

_Label `question` added by @charliermarsh on 2025-04-23 17:17_

---

_Comment by @gabriel-vanzandycke on 2025-04-24 12:03_

Thanks!
I'm now facing another issue I was mentioning. I updated the OP, but in a nutshell, `docker compose build` searches for `wheel` in the private index from which `customlib` originates, rather than looking for it into PyPi 🤷 

---

_Comment by @charliermarsh on 2025-04-24 16:19_

Consider adding `explicit = true` under `[[tool.uv.index]]`. That way, only packages marked like `customlib = { index = "private" }` will come from that index.

---

_Comment by @gabriel-vanzandycke on 2025-04-24 18:49_

Thanks. I had tried this; however it leads to another problem, a dependency of `customlib` also on the private index cannot be found:
```
  × No solution found when resolving dependencies for split (python_full_version >= '3.11' and sys_platform == 'darwin'):
  ╰─▶ Because cvcolors was not found in the package registry and customlib==1.0.0 depends on cvcolors<1.0.0, we can conclude that customlib==1.0.0 cannot be used.
      And because experiment[optional] depends on customlib==1.0.0 and your workspace requires experiment[das], we can conclude that your workspace's requirements are unsatisfiable.
```

I tested adding `cvcolors` to the explicit list of dependencies (although I shouldn't have to), but it led to other problems...

---

_Comment by @charliermarsh on 2025-04-24 18:51_

Sorry, that's all consistent with the design. If you want a package to come from an explicit index, you need to mark it as such in `tool.uv.sources` and mark it as a dependency. Adding `cvcolors` to your list of dependencies shouldn't cause issues if that's a transitive dependency anyway? I also don't know why you're failing to fetch from `https://gitlab.private.com/api/pypi/files/<random-key>/wheel-0.43.0-py3-none-any.whl` in the first place. That just looks like a PyPI proxy?

---

_Comment by @charliermarsh on 2025-04-24 18:52_

You can also set `--index-strategy unsafe-best-match` if you want to trick uv into selecting `wheel==0.45.1` from PyPI. For whatever reason, your PyPI proxy suggests it has `wheel`, but only at version 0.43.0.

---

_Closed by @charliermarsh on 2025-04-24 18:52_

---

_Comment by @gabriel-vanzandycke on 2025-04-28 10:16_

I've investigated deeper and the issue was related to unavailable credentials at the moment of 
```
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    uv sync --frozen --no-install-project --no-install-workspace
```

It was resolved by adding a secret to my `docker-compose.yaml`
```
secrets:
  gitlab:
    file: ./.env
```

And loading them in my `Dockerfile` before the call to `uv sync`:
```
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    --mount=type=secret,id=gitlab,required \
    set -a && . /run/secrets/gitlab && set +a && \
    uv sync --frozen --no-install-project --no-install-workspace
```

Setting the extra index to `explicit=true` in `pyproject.toml` isn't necessary.

Hope that helps if anyone faces similar issues.

---
