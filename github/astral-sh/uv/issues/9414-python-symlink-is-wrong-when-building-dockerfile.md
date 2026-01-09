---
number: 9414
title: Python symlink is wrong when building Dockerfile
type: issue
state: closed
author: timvink
labels: []
assignees: []
created_at: 2024-11-25T11:08:22Z
updated_at: 2024-11-25T12:55:41Z
url: https://github.com/astral-sh/uv/issues/9414
synced_at: 2026-01-07T13:12:18-06:00
---

# Python symlink is wrong when building Dockerfile

---

_Issue opened by @timvink on 2024-11-25 11:08_

Consider this Dockerfile:

```Dockerfile
# adapted from https://github.com/astral-sh/uv-docker-example/blob/main/Dockerfile
FROM mcr.microsoft.com/azureml/openmpi4.1.0-ubuntu20.04

ENV UV_COMPILE_BYTECODE=1
ENV UV_LINK_MODE=copy

WORKDIR /code

# Copy the lockfile and settings
COPY uv.lock .
COPY pyproject.toml .
COPY .python-version .

# Setup python environment
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=type=bind,source=uv.lock,target=uv.lock \
    --mount=type=bind,source=pyproject.toml,target=pyproject.toml \
    --mount=type=bind,source=.python-version,target=.python-version \
    --mount=type=secret,id=uv.toml,dst=/root/.config/uv/uv.toml \
    uv sync --frozen --no-install-project --link-mode=copy --python-preference=only-managed

# Place executables in the environment at the front of the path
ENV PATH="/code/.venv/bin:$PATH"
```

If I build this and open a shell inside the image, and run `ls -lah .venv/bin/`;

<img width="779" alt="image" src="https://github.com/user-attachments/assets/dc1d567f-757e-49ab-8058-727586e2b40f">

and compare it with `uv python list` you can see the wrong symlink (notice the root, `/home/azureuser/` instead of `/root/`:

<img width="698" alt="image" src="https://github.com/user-attachments/assets/76d47107-9685-4392-b795-bfd485f57d96">

The consequence is that `which python` points to the wrong python version, because the python version in the .venv does not exist.

I suspect something is going wrong with `~` expansion.. indeed on my host machine that points to `/home/azureuser/`. 
Using [0.5.4](https://github.com/astral-sh/uv/releases/tag/0.5.4)

---

_Comment by @FishAlchemist on 2024-11-25 11:26_

I'm just double-checking this Dockerfile. Isn't the default user supposed to be root? And with uv set to only-managed, I thought it would automatically download managed Python. So, if the Docker user is root, shouldn't the location of managed Python be correct?

---

_Comment by @timvink on 2024-11-25 11:32_

Yes, I even have an additional `USER root:root` in my docker. The location of managed python is correct:

![image](https://github.com/user-attachments/assets/4e11eccf-0d52-43b5-8277-b2ee2e429751)

The location of python in the `.venv` folder inside the docker container isnt:

![image](https://github.com/user-attachments/assets/afcd0c82-7a9c-4875-8a97-a04b56d55289)
![image](https://github.com/user-attachments/assets/bf1b2ada-853f-47ff-8124-345117b9de19)

I've also tried:

- uv 0.4.30, same issue.
- setting `HOME` env var explicit in Docker container
- Removing cache from host system (remove `--mount=type=cache,target=/root/.cache/uv`)

---

_Comment by @FishAlchemist on 2024-11-25 12:00_

If we don't explicitly set the value of $HOME in Docker, what is the  value of $HOME?

---

_Comment by @timvink on 2024-11-25 12:53_

I'm not convinced anymore this is `uv` related. I think it's related to the base image I am using. Closing issue.

In case anyone hits a similar problem, I solved it by explicitly setting the VIRTUALENV variable:

```Dockerfile
# Place executables in the environment at the front of the path
ENV PATH="/code/.venv/bin:$PATH"
ENV VIRTUAL_ENV=/uv/.venv/
```

---

_Closed by @timvink on 2024-11-25 12:53_

---
