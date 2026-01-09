---
number: 12075
title: "Can I use `uv` to install only the dependencies of a package?"
type: issue
state: open
author: mkleinbort-wl
labels:
  - question
assignees: []
created_at: 2025-03-09T13:40:33Z
updated_at: 2025-03-12T13:37:42Z
url: https://github.com/astral-sh/uv/issues/12075
synced_at: 2026-01-07T13:12:18-06:00
---

# Can I use `uv` to install only the dependencies of a package?

---

_Issue opened by @mkleinbort-wl on 2025-03-09 13:40_

### Question

I'd be interested in being able to install only the dependencies of a python package (not the python package itself).

I imagine this api as something like:

`uv add scikit-learn --only-deps`

Which would install the dependencies from the `pyproject.toml` [here](https://github.com/scikit-learn/scikit-learn/blob/main/pyproject.toml), but not scikit-learn itself.

Use case:
- Easier caching of layers in docker builds

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @mkleinbort-wl on 2025-03-09 13:40_

---

_Comment by @zanieb on 2025-03-09 15:36_

I think the expected interface would be `uv add -r https://github.com/scikit-learn/scikit-learn/blob/main/pyproject.toml` but we ban that because the interaction with extras and such is confusing. Perhaps we should allow it though.

> Easier caching of layers in docker builds

Hm why are you using `uv add` in Docker builds?

We have `uv sync --no-install-project` for layer caching. https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers

---

_Comment by @mkleinbort-wl on 2025-03-09 15:39_

I think `uv sync --no-install-project` is exactly what I was looking for, I'll test it and close the issue.

In my Dockerfile I'm directly installing from a private git repo, hence the ask - vs. having the code in the image itself.

Roughly...
```Dockerfile
FROM python:3.11 AS builder

WORKDIR /app

RUN uv venv
RUN uv pip install git+ssh://git@github.com/{ORG}/{REPO}.git

FROM python:3.11
WORKDIR /app
COPY --from=builder /app/.venv .venv/
COPY . .

# More code
```


---

_Comment by @mkleinbort-wl on 2025-03-09 16:01_

To explain further, the dependencies of `git+ssh://git@github.com/{ORG}/{REPO}.git` change slowly, but `git+ssh://git@github.com/{ORG}/{REPO}.git` itself changes frequently. 

---

_Comment by @zanieb on 2025-03-09 16:24_

You might want to clone / download the `uv.lock` and `pyproject.toml` first so you can do something like https://github.com/astral-sh/uv-docker-example/blob/c16a61fb3e6ab568ac58d94b73a7d79594a5d570/Dockerfile#L15-L17 then clone the rest of the code

---

_Comment by @charliermarsh on 2025-03-09 17:08_

I think `--no-install-package scikit-learn` should also work. It will install the dependencies, but not `scikit-learn`.

---

_Comment by @mkleinbort-wl on 2025-03-12 13:37_

Not sure that works

```
# In a docker file
RUN uv pip install --no-install-package git+ssh://git@github.com/{{ ORG_NAME }}/{{ REPO_NAME }}.git 
```

Results in Exit code 2 (troubleshooting it now)

---
