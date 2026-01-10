---
number: 7748
title: Allow sync to install to system python
type: issue
state: closed
author: nhumrich
labels:
  - duplicate
  - question
assignees: []
created_at: 2024-09-27T23:29:44Z
updated_at: 2025-04-26T03:33:16Z
url: https://github.com/astral-sh/uv/issues/7748
synced_at: 2026-01-10T01:24:19Z
---

# Allow sync to install to system python

---

_Issue opened by @nhumrich on 2024-09-27 23:29_

In docker environments, it's quite common to install all python packages to the system. Virtual environments are not needed because the current project is the only thing installed. 

The docker documentation for UV recommends that you use the venv entrypoint for all container commands:
```
CMD ["/app/.venv/bin/python"]
```
However, this makes migrating to UV a bit harder when so many things already reference "python" directly in. 

Also, using a virtual environment in these situations complicates the container, because devs now have to remember to use the .venv python always. 

Would it be possible to install into the system during `uv sync` somehow. I assumed `--python-preference only-system` would do this, but that doesn't seem to prevent the creating of a virtual environment. `uv pip install` support `--system`, so its sort of strange that `sync` does not. 

My current workaround is doing this in the dockerfile instead:

```
RUN --mount=from=ghcr.io/astral-sh/uv,source=/uv,target=/bin/uv \
    uv pip compile pyproject.toml -o requirements.txt && \
    uv pip install -r requirements.txt --system
```

---

_Comment by @zanieb on 2024-09-28 03:52_

See [`UV_PROJECT_ENVIRONMENT`](https://docs.astral.sh/uv/concepts/projects/#configuring-the-project-environment-path) and #6834 

---

_Label `question` added by @zanieb on 2024-09-28 03:52_

---

_Label `duplicate` added by @zanieb on 2024-09-28 03:52_

---

_Closed by @zanieb on 2024-10-21 22:02_

---

_Comment by @nhumrich on 2024-10-22 15:31_

@zanieb maybe I am misunderstanding, but it appears as if UV_PROJECT_ENVIRONMENT wants the location of a virtualenv.

The intent is not to tell UV to use an existing virtual environment, but to use the system directly.

The docs also say:
> uv sync will remove extraneous packages from the environment by default and, as such, may leave the system in a broken state.

This is not wanted. I would conclude that this feature request does not appear to be the same as the UV_PROJECT_ENVIRONMENT feature.

---

_Comment by @zanieb on 2024-10-22 15:49_

> uv sync will remove extraneous packages from the environment by default and, as such, may leave the system in a broken state.

You can use `--inexact` then, but it still might change package versions if it needs to — which is the problem with using a system environment generally. Why are you using `uv sync` to a system environment if you don't have control over the environment?

---

_Comment by @nhumrich on 2024-10-26 23:21_

@zanieb I do have control over the environment. Its a docker container.

---

_Comment by @zanieb on 2024-10-27 00:41_

Then why are you worried about packages being removed? Sorry, I must be missing something.

---

_Comment by @eric-gitta-moore on 2025-04-26 03:33_

> [@zanieb](https://github.com/zanieb) I do have control over the environment. Its a docker container.

Same here.

---
