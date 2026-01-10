```yaml
number: 1374
title: "Cannot `uv pip install` without virtualenv"
type: issue
state: closed
author: hauntsaninja
labels:
  - enhancement
assignees: []
created_at: 2024-02-15T22:37:25Z
updated_at: 2024-07-09T15:56:03Z
url: https://github.com/astral-sh/uv/issues/1374
synced_at: 2026-01-10T05:31:36Z
```

# Cannot `uv pip install` without virtualenv

---

_Issue opened by @hauntsaninja on 2024-02-15 22:37_

It's common to not use virtual environments in containers, e.g. see discussion around PEP 704. I don't mind the default being to prevent installation into a base environment, but it would be nice to have some low level flag to override it.

```
error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.
```

(Maybe I can lie about `VIRTUAL_ENV` to work around...)

---

_Renamed from "Cannot `uv pip install` in container without virtualenv" to "Cannot `uv pip install` without virtualenv" by @hauntsaninja on 2024-02-15 22:38_

---

_Comment by @charliermarsh on 2024-02-15 22:46_

👍 Agreed, we need some solution to this. I believe setting `VIRTUAL_ENV` would work, but it should be a first-class feature.

---

_Label `enhancement` added by @charliermarsh on 2024-02-15 22:47_

---

_Comment by @batazor on 2024-02-16 23:11_

It would also be cool to do a couple of examples on how to create a Dockerfile

especially with multi-stage builds, as I don't want to carry all the dependencies (c-library) that are needed when building, previously I used `--wheel-dir /app/wheels` to do this

---

_Comment by @dmwyatt on 2024-02-17 17:42_

Here's how I setup my Dockerfile to work until we get a feature in `uv` to specifically support this:

```
ENV VIRTUAL_ENV /usr/local/

RUN curl -LsSf https://astral.sh/uv/install.sh | sh \
    && . $HOME/.cargo/env \
    && uv pip compile requirements.in -o requirements.txt \
    && uv pip sync requirements.txt
```

`/usr/local/` is correct on `slim-bullseye` images, you'll have to modify for whatever you image uses.

---

_Comment by @hauntsaninja on 2024-02-17 21:56_

Since this issue is somewhat popular, just noting that I've been doing `VIRTUAL_ENV=$(python -c "import sys; print(sys.prefix)") uv pip ...`

---

_Comment by @ofek on 2024-02-18 23:21_

This probably would be the ultimate fix https://github.com/astral-sh/uv/issues/1396

---

_Comment by @ericbn on 2024-02-19 14:58_

pip has the --require-virtualenv option which makes it behave like uv:

```
$ pip install --require-virtualenv attrs
ERROR: Could not find an activated virtualenv (required).
```
but --no-require-virtualenv seems too long of a name for an option in uv. And sure the default uv behavior is much saner default!



---

_Comment by @zanieb on 2024-02-19 15:46_

I think we can track this in #1396 and #1526 since those are concrete solutions.

Thanks everyone!

---

_Closed by @zanieb on 2024-02-19 15:46_

---

_Comment by @SzilvasiPeter on 2024-07-09 15:56_

I will mention the simplest solution, the `--system` option since other comments do not mention it. Also, the [documentation ](https://github.com/astral-sh/uv/?tab=readme-ov-file#installing-into-arbitrary-python-environments) highlights this option for containerized environments:

```bash
uv pip install --system
```

> WARNING: `--system` is intended for use in continuous integration (CI) environments and should be used with caution, as it can modify the system Python installation.

---
