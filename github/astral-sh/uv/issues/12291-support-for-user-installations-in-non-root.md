---
number: 12291
title: "Support for `--user` installations in non-root container environments"
type: issue
state: closed
author: slmg
labels:
  - enhancement
assignees: []
created_at: 2025-03-18T16:47:35Z
updated_at: 2025-03-20T17:54:23Z
url: https://github.com/astral-sh/uv/issues/12291
synced_at: 2026-01-07T13:12:18-06:00
---

# Support for `--user` installations in non-root container environments

---

_Issue opened by @slmg on 2025-03-18 16:47_

### Summary

#### Description

`uv` assumes that users will always want to operate inside a `.venv` directory by default, which is generally reasonable. However, in my development setup, this assumption creates friction.

I am using the VS Code [Dev Container](https://code.visualstudio.com/docs/devcontainers/containers) extension, operating inside a Debian-based container (`python:bookworm`). Following best practices, I [avoid running the container as root](https://www.docker.com/blog/understanding-the-docker-user-instruction/#:~:text=Use%20a%20non%2Droot%20user%20to%20limit%20root%20access) and instead created a non-root user (`moby`) for my local development.

Since I am already isolated inside a container, using a virtual environment inside it is redundant. I'd like to install packages directly to my user environment (e.g., `/home/moby/.local/[bin|lib]`), but `uv sync` will always create a `.venv` directory if none exists, with no available CLI option to bypass this behavior.

I noticed that `uv pip install` recently introduced a `--system` flag, but this still defaults to `/usr/local` which requires root, making it unsuitable for non-root container workflows like mine.

#### Requested Feature

I’d like to propose the addition of a `--user` flag for both `uv pip` and `uv sync` commands that:
- Skips the automatic `.venv` creation.
- Installs packages to the standard user-level directory: `$HOME/.local/[bin|lib]`.

This would align `uv` with `pip install --user` behavior, making it much more flexible for containerized, non-root development environments.

#### Example Dockerfile for context:

```dockerfile
FROM python:3.12-bookworm

RUN useradd -m moby -u 1000 -s /usr/bin/bash
USER moby
WORKDIR /home/moby
ENV PATH /home/moby/.local/bin:$PATH
```

#### Why this matters

- In containerized environments where isolation is already guaranteed by the container itself, virtual environments become an extra and sometimes unnecessary layer.
- Running containers as root is not recommended, yet currently `uv` assumes either root access or `.venv` usage.
- Having a `--user` option would streamline workflows for developers working in secure, non-root, containerized setups.

#### Current workaround

At the moment, my only option is to avoid `uv sync` or `uv pip install`, and manually rely on a traditional, slower, but reliable `pip install`.

---

_Label `enhancement` added by @slmg on 2025-03-18 16:47_

---

_Comment by @zanieb on 2025-03-18 17:53_

See also https://github.com/astral-sh/uv/issues/12197

It's _very_ unlikely that we'll add support for a `--user` flag. There's a great deal of discussion on this in https://github.com/astral-sh/uv/issues/2077

I understand that this adds complexity to the container use-case — we're still trying to find the best way to solve that — but I think the `--user` flag comes with additional problems.

I'd recommend using `UV_PROJECT_ENVIRONMENT` to set the path to your virtual environment then adding its `bin` to the front of your `PATH`.

---

_Comment by @notatallshaw on 2025-03-18 18:00_

I'm not strongly invested in whether uv provides `--user`, but I would strongly discourage it for most use cases, including this one. 

> * In containerized environments where isolation is already guaranteed by the container itself, virtual environments become an extra and sometimes unnecessary layer.

It should be noted that the docker container doesn't provide any isolation from the Python choices the base image makes, which absolutely can cause issues.

I would suggest reading this blog even if you don't agree with the conclusions: https://hynek.me/articles/docker-virtualenv/


---

_Closed by @charliermarsh on 2025-03-20 17:54_

---
