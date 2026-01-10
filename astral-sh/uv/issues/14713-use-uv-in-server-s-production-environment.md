---
number: 14713
title: "Use uv in server's production environment"
type: issue
state: open
author: cedric-sun
labels:
  - question
assignees: []
created_at: 2025-07-18T11:17:32Z
updated_at: 2025-07-18T12:03:51Z
url: https://github.com/astral-sh/uv/issues/14713
synced_at: 2026-01-10T01:25:48Z
---

# Use uv in server's production environment

---

_Issue opened by @cedric-sun on 2025-07-18 11:17_

### Question

Hi uv team,

We have a few python scripts in a git repo managed by uv. These scripts will be packaged into a docker image and invoked by a cron job on a daily basis.

We plan to use install uv within the image and run `uv sync` upon image build, and just `uv run path/to/script.py` as the invocation command as well did during development. The problem is that we don't want uv to access internet once the image was built, or perform any disk IO at runtime. (specifically, we are aware of the requirement of the cache directory).

I'd like to have your advice on this practice. It it okay to do so? Or shall we `source .venv/bin/activate` manually and just use python?

### Platform

Ubuntu 24.04 in docker.

### Version

uv 0.7.11

---

_Label `question` added by @cedric-sun on 2025-07-18 11:17_

---

_Renamed from "Use uv in production environment" to "Use uv in server's production environment" by @cedric-sun on 2025-07-18 11:17_

---

_Comment by @paveldikov on 2025-07-18 11:58_

For a proper, non-brittle production environment, what we do (with pretty good success) is to model the scripts as a true `pyproject.toml`-driven package.

Model all of your scripts as `[project.scripts]` entrypoints, and your dependencies as `[project.dependencies]`, as one would in general.

You can then take this project and install it into the Docker image. You will end up with a complete set of all of your dependencies, baked right into your image, and all of your script entrypoints will be easily executable (if you install onto the base image's system python, they will end up on `$PATH` too, which is neat)

---

_Comment by @zanieb on 2025-07-18 12:03_

You can use `uv run --no-sync` or `UV_NO_SYNC=1`, which I believe addresses your use-case.

---

_Comment by @zanieb on 2025-07-18 12:03_

(We might perform disk IO to like.. find the Python interpreter to use.. I'm not sure if that matters to you)

---
