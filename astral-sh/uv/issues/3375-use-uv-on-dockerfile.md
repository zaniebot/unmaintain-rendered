```yaml
number: 3375
title: "Use `uv` on Dockerfile"
type: issue
state: closed
author: brunolnetto
labels:
  - question
assignees: []
created_at: 2024-05-04T19:04:28Z
updated_at: 2025-09-23T16:08:52Z
url: https://github.com/astral-sh/uv/issues/3375
synced_at: 2026-01-12T15:58:43Z
```

# Use `uv` on Dockerfile

---

_@brunolnetto_

Can we use `uv` on a Dockerfile? I tried using as follows, but got this error: `0.258 error: failed to canonicalize path /code/.venv/bin/python 258   Caused by: No such file or directory (os error 2)`


```
FROM python:3.11.3-slim-buster AS base

WORKDIR /code

# Set env variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONPATH="/code"

# Create non-root user
RUN adduser --disabled-password appuser

# Install uv
RUN pip install uv

# Create virtual environment
RUN python3 -m venv /code/venv

# Activate virtual environment (assuming it's named 'venv')
RUN bash -c "source /code/venv/bin/activate"

# Copy application code and requirements (assuming requirements.txt is in the same directory)
COPY . .

# Install dependencies with uv
RUN pip install --upgrade pip
RUN uv pip install -r requirements.txt

# Switch to non-root user
USER appuser

# Run the application
CMD ["bash", "scripts/run.sh"]

```

---

_Comment by @charliermarsh on 2024-05-04 19:15_

I ran this myself and saw the error I expected:

```
#14 0.134 error: Failed to locate a virtualenv or Conda environment (checked: `VIRTUAL_ENV`, `CONDA_PREFIX`, and `.venv`). Run `uv venv` to create a virtualenv.
```

The problem is that `RUN bash -c "source /code/venv/bin/activate"` doesn't affect any of the commands that run after it.

There are a lot of different options here to fix it.

First, you can use `.venv` as the environment name, and `uv` will discover it automatically:

```
FROM python:3.11.3-slim-buster AS base

WORKDIR /code

# Set env variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONPATH="/code"

# Create non-root user
RUN adduser --disabled-password appuser

# Install uv
RUN pip install uv

# Create virtual environment
RUN python -m venv /code/.venv

# Copy application code and requirements (assuming requirements.txt is in the same directory)
COPY . .

# Install dependencies with uv
RUN uv pip install -r requirements.txt
```

Alternatively, you can use `uv` to create the environment, to simplify things further:

```
FROM python:3.11.3-slim-buster AS base

WORKDIR /code

# Set env variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONPATH="/code"

# Create non-root user
RUN adduser --disabled-password appuser

# Install uv
RUN pip install uv

# Create virtual environment
RUN uv venv

# Copy application code and requirements (assuming requirements.txt is in the same directory)
COPY . .

# Install dependencies with uv
RUN uv pip install -r requirements.txt
```

Alternatively, you can tell `uv` to install into that specific virtualenv:

```
FROM python:3.11.3-slim-buster AS base

WORKDIR /code

# Set env variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONPATH="/code"

# Create non-root user
RUN adduser --disabled-password appuser

# Install uv
RUN pip install uv

# Create virtual environment
RUN python3 -m venv /code/venv

# Copy application code and requirements (assuming requirements.txt is in the same directory)
COPY . .

# Install dependencies with uv
RUN uv pip install -r requirements.txt --python /code/venv/bin/python
```

Alternatively, you can "activate" the `VIRTUAL_ENV` via environment variables:

```
FROM python:3.11.3-slim-buster AS base

WORKDIR /code

# Set env variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONPATH="/code"

# Create non-root user
RUN adduser --disabled-password appuser

# Install uv
RUN pip install uv

# Create virtual environment
RUN python3 -m venv /code/venv

# Copy application code and requirements (assuming requirements.txt is in the same directory)
COPY . .

ENV VIRTUAL_ENV /code/venv
ENV PATH /code/venv/bin:$PATH

# Install dependencies with uv
RUN uv pip install -r requirements.txt
```

Finally, you can use `--system` and skip the virtualenv entirely, since you're already containerized:

```
FROM python:3.11.3-slim-buster AS base

WORKDIR /code

# Set env variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONPATH="/code"

# Create non-root user
RUN adduser --disabled-password appuser

# Install uv
RUN pip install uv

# Copy application code and requirements (assuming requirements.txt is in the same directory)
COPY . .

# Install dependencies with uv
RUN uv pip install -r requirements.txt --system
```


---

_Comment by @charliermarsh on 2024-05-04 19:15_

Personally, I would do this one:

```docker
FROM python:3.11.3-slim-buster AS base

WORKDIR /code

# Set env variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    PYTHONPATH="/code"

# Create non-root user
RUN adduser --disabled-password appuser

# Install uv
RUN pip install uv

# Create virtual environment
RUN uv venv

# Copy application code and requirements (assuming requirements.txt is in the same directory)
COPY . .

# Install dependencies with uv
RUN uv pip install -r requirements.txt
```

---

_Label `question` added by @charliermarsh on 2024-05-04 19:15_

---

_Comment by @brunolnetto on 2024-05-04 19:18_

I am flattered to have this issue answered by you. I will portrait this on my fridge.

---

_Comment by @charliermarsh on 2024-05-04 21:05_

Happy to help :)

---

_Closed by @charliermarsh on 2024-05-04 21:05_

---

_Comment by @brunolnetto on 2024-05-07 01:38_

Another related question:  I found [this outstanding work](https://github.com/tiangolo/full-stack-fastapi-template/blob/master/backend/Dockerfile) at fastapi repository. It uses poetry, a long-standing friend, and below commands. I would like to give some refreshing refactor and a pull request with `uv`. What do you suggest? :-)

```
ARG INSTALL_DEV=false
RUN bash -c "if [ $INSTALL_DEV == 'true' ] ; then poetry install --no-root ; else poetry install --no-root --only main ; fi"
```




---

_Comment by @blfpd on 2025-09-23 16:08_

> Personally, I would do this one:
> 
> ```
> FROM python:3.11.3-slim-buster AS base
> 
> WORKDIR /code
> ```

Maybe you never encountered this issue, but I’d advise against using `/code` as a workdir for Python projects, as it is a valid Python module and might mess with your imports.  
See:  
https://docs.python.org/3/library/code.html  
https://github.com/python/cpython/blob/main/Lib/code.py

Popular options: `/app`, `/src`, `/srv`…

---
