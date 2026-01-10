```yaml
number: 11355
title: Requesting specific Python version leads to error
type: issue
state: closed
author: ImahnShekhzadeh
labels:
  - question
assignees: []
created_at: 2025-02-09T14:42:56Z
updated_at: 2025-06-04T05:46:53Z
url: https://github.com/astral-sh/uv/issues/11355
synced_at: 2026-01-10T03:41:47Z
```

# Requesting specific Python version leads to error

---

_Issue opened by @ImahnShekhzadeh on 2025-02-09 14:42_

### Summary

Hi, 

Following the instructions from [the docs](https://docs.astral.sh/uv/concepts/python-versions/#managed-and-system-python-installations), I'm trying to install Python 3.10.3 as follows:

```dockerfile
FROM nvidia/cuda:12.2.2-cudnn8-devel-ubuntu22.04

RUN apt-get update && apt-get install -y curl ca-certificates git && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

WORKDIR /app

COPY setup.py .
COPY pyproject.toml .

# Install `uv` acc. to the instructions https://docs.astral.sh/uv/guides/integration/docker/#installing-uv
ADD https://astral.sh/uv/install.sh /uv-installer.sh
RUN sh /uv-installer.sh && rm /uv-installer.sh
ENV PATH="/root/.local/bin/:$PATH"
RUN uv self update && \
    uv python install 3.10.3 && \
    uv venv --python 3.10.3 && \
    uv pip install -e .

# Expose network port
EXPOSE 80

RUN git config --global --add safe.directory /app
```
When building the Dockerfile via 
```bash
docker build -f Dockerfile -t test:0.0.1 .
```
and then running the container interactively via
```bash
docker run --shm-size 512m --rm -v $(pwd):/app --gpus all -it test:0.0.1 /bin/bash
```
and then typing
```bash
uv pip list --verbose
```
I get the error:
```
root@482aab32b5fa:/app# uv pip list --verbose
DEBUG uv 0.5.29
DEBUG Searching for default Python interpreter in virtual environments or search path
DEBUG Failed to inspect Python interpreter from virtual environment at `.venv/bin/python3` 
error: Failed to inspect Python interpreter from virtual environment at `.venv/bin/python3`
  Caused by: Python interpreter not found at `/app/.venv/bin/python3`
```
The weird thing is that **when not specifyng a minor Python version**, i.e. writing 
```Dockerfile
RUN uv self update && \
    uv python install 3.10 && \
    uv venv --python 3.10 && \
    uv pip install -e .
```
instead, there's no issue with the above command.

### Platform

Ubuntu 22.04

### Version

uv 0.5.29

### Python version

Python 3.10.3

---

_Label `bug` added by @ImahnShekhzadeh on 2025-02-09 14:42_

---

_Comment by @charliermarsh on 2025-02-09 18:09_

Seems odd! I'll tag @zanieb who has the most context here.

---

_Comment by @zanieb on 2025-02-09 18:21_

It looks like you're mounting your working directory with `-v $(pwd):/app`, this will include the `.venv` in your current directory which overlaps with the one in the image and is broken because the `python3` symlink points to an installation that does not exist, hence the error:

```
error: Failed to inspect Python interpreter from virtual environment at `.venv/bin/python3`
  Caused by: Python interpreter not found at `/app/.venv/bin/python3`
```

I don't know why it'd work when you drop the minor version.

You should avoid mounting `.venv`, either via `.dockerignore` or as demonstrated via an anonymous volume in https://github.com/astral-sh/uv-docker-example/blob/1c593aa32621eacd0125b55bd8d2796b86e8ea73/run.sh#L11

---

_Label `bug` removed by @zanieb on 2025-02-09 18:21_

---

_Label `question` added by @zanieb on 2025-02-09 18:21_

---

_Comment by @ImahnShekhzadeh on 2025-02-09 20:00_

Yep - adding the `.dockerignore` file and `.venv` in it solves the issue.
I first did not specify a minor version, which is why when not specifying a minor version, no error occurred. 

---

_Closed by @ImahnShekhzadeh on 2025-02-09 20:00_

---

_Comment by @davideuler on 2025-06-04 05:46_

Test if python version can be detected corrected by uv:

```
/app/.venv/bin/python3 --version
/app/.venv/bin/python3 -c "import sys; print(sys.version); print(sys.executable)"

```

In my case, I put a sitecustomize.py at the site-packages directory, which caused the second command output extra line so that uv failed. 

```
# /app/.venv/bin/python3 -c "import sys; print(sys.version); print(sys.executable)"
✅ Gradio v5.31.0 share=True blocking active for 2 components.
3.12.8 (main, Jan 14 2025, 22:38:09) [GCC 6.3.0 20170516]
/app/.venv/bin/python3
```

---
