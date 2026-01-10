---
number: 4385
title: Running uv in an interactive docker container
type: issue
state: closed
author: braaannigan
labels:
  - question
assignees: []
created_at: 2024-06-18T15:14:52Z
updated_at: 2024-08-21T06:16:56Z
url: https://github.com/astral-sh/uv/issues/4385
synced_at: 2026-01-10T01:23:37Z
---

# Running uv in an interactive docker container

---

_Issue opened by @braaannigan on 2024-06-18 15:14_

Thanks for this great crate! I've been working with a recent version and trying to capture my best practices here: https://www.rhosignal.com/posts/uv-in-docker/

I want to highlight some issues with the more recent versions of uv when working in interactive mode in docker. The issues arise because of the way a virtual env is now required. I want to use the following dockerfile approach (as opposed to a curl installation) as it has better caching:
```dockerfile
FROM ghcr.io/astral-sh/uv:0.2.12 as uv
# Choose python version here
FROM python:3.10.1-slim-buster
# Create a virtual environment with uv inside the container
RUN --mount=from=uv,source=/uv,target=./uv \
    ./uv venv /opt/venv
# We need to set this environment variable so that uv knows where
# the virtual environment is to install packages
ENV VIRTUAL_ENV=/opt/venv
# Copy the requirements file into the container
COPY requirements.txt .
# Install the packages with uv using --mount=type=cache to cache the downloaded packages
RUN --mount=type=cache,target=/root/.cache/uv \
    --mount=from=uv,source=/uv,target=./uv \
    ./uv pip install  -r requirements.txt
```
This works fine for building the image. The problem is when I want to develop in interactive mode in the container (which is my whole day basically).

- The first issue is that when you run the container you need to activate the virtual env every time (which is manageable but less convenient than before)
- The second is that if I want to try out additional package installs while running in interactive mode I have neither uv nor pip available! uv isn't available because it's just a docker build layer and pip is not in the virtual env. The options here are to install pip into the virtual env or to manually install uv from curl when runnng interactively.

Have I missed any other possible solutions for dealing with these issues?

Thanks again


---

_Comment by @zanieb on 2024-06-18 15:19_

Thanks for sharing!

> The first issue is that when you run the container you need to activate the virtual env every time 

You could add the virtual environment's `bin` to the `PATH` then you shouldn't need to do any activation.

You can also use `--system` and just skip the virtual environment entirely.

> The second is that if I want to try out additional package installs while running in interactive mode I have neither uv nor pip available! uv isn't available because it's just a docker build layer and pip is not in the virtual env. The options here are to install pip into the virtual env or to manually install uv from curl when runnng interactively.

Sounds about right. I'd opt to just keep uv around, but the alternative is to run `uv venv` with `--seed` to install `pip` or install `pip` manually.



---

_Label `question` added by @zanieb on 2024-06-18 15:19_

---

_Comment by @hoechenberger on 2024-06-18 18:31_

> You can also use --system and just skip the virtual environment entirely.

FWIW this is what I'm doing and it works flawlessly.

---

_Referenced in [ferrocene/ferrocene#694](../../ferrocene/ferrocene/pulls/694.md) on 2024-06-19 20:37_

---

_Comment by @braaannigan on 2024-06-20 10:15_

@zanieb  @hoechenberger 
I've done the add-venv-to-path approach: https://www.rhosignal.com/posts/uv-in-docker/

Any feeling for whether there is a preference for this over the --system approach?

---

_Comment by @hoechenberger on 2024-06-20 10:23_

@braaannigan In my dev containers I try to keep complexity low, and I feel like setting up a venv adds another layer of complexity. I therefore set the `UV_SYSTEM_PYTHON` env var and install things globally. No need for a venv in an isolated environment like a dev container. I can share a sample dev container config with you if you're interested!

---

_Comment by @zanieb on 2024-06-20 13:18_

I think both approaches are totally reasonable. We see a "add venv to path" approach more often when you don't have write access to the system.

---

_Comment by @zanieb on 2024-06-21 00:29_

I'm adding some documentation based on this discussion #4433 

---

_Comment by @kennycontreras on 2024-07-12 15:45_

> @braaannigan I can share a sample dev container config with you if you're interested!

Could you please share the container configuration? That would be really helpful. Thanks!


---

_Closed by @zanieb on 2024-08-19 17:25_

---

_Comment by @braaannigan on 2024-08-20 08:25_

> @braaannigan In my dev containers I try to keep complexity low, and I feel like setting up a venv adds another layer of complexity. I therefore set the `UV_SYSTEM_PYTHON` env var and install things globally. No need for a venv in an isolated environment like a dev container. I can share a sample dev container config with you if you're interested!

I'd like to see this as well

---

_Comment by @hoechenberger on 2024-08-21 06:16_

Hello, sorry for not getting back to you, this entirely slipped my mind. Can you please ping me again if you don't hear back from me by the end of this week? Best wishes 

---
