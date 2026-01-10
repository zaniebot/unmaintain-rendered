---
number: 16757
title: how to use uv and UV_CACHE_DIR  to pre-install common dependencies in a docker image?
type: issue
state: open
author: kforner
labels:
  - question
assignees: []
created_at: 2025-11-17T09:20:48Z
updated_at: 2025-11-20T15:11:03Z
url: https://github.com/astral-sh/uv/issues/16757
synced_at: 2026-01-10T01:26:09Z
---

# how to use uv and UV_CACHE_DIR  to pre-install common dependencies in a docker image?

---

_Issue opened by @kforner on 2025-11-17 09:20_

### Question

I want to build a docker image that will be used as a base image for multiple python-based analysis projects.
The idea is to fix the Python version, and preinstall some commonly used packages to speed up downstream use.

I tried using a common cache dir, via the $UV_CACHE_DIR var.
It works when I do a `uv sync` or a `uv venv; uv pip install pandas`, that actually make use of the cache.
But I have a case, where some packages are under a pkgs/ folder, and running the tests with `uv run --all-packages   pytest -v` insists in re-downloading all dependencies, by-passing the cache.

So my questions are:

- is it the right way to pre-install/pre-populate Python packages? 
- How to run the pytests without re-downloading the dependencies.







### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @kforner on 2025-11-17 09:20_

---

_Comment by @pcastellazzi on 2025-11-18 13:55_

Take a look at:
* <https://docs.astral.sh/uv/guides/integration/docker/>
* <https://github.com/astral-sh/uv-docker-example/>
* <https://github.com/astral-sh/uv/issues/8025>


---

_Comment by @kforner on 2025-11-19 10:03_

Thanks. I had already looked at those docs, but I'm not sure it solves my particular problem. 
I want to pre-populate a docker image with common packages, like pandas. 
Then this image should be used with different python projects. 
I wonder what is the best way to achieve this.

---

_Comment by @pcastellazzi on 2025-11-19 18:18_

The simplest solution would be to se use something like:

```Dockerfile
# imageA
FROM python:3.14-trixie

# install your shared dependencies globally
RUN pip3 install pandas
```

```Dockerfile
# imageB
FROM imageA
ADD /path/to/project /app
WORKSPACE /app

# I am using multiple RUN commands for clarity but they should be one

# export uv managed dependancies in a form pip can install
RUN uv export --requirements.txt >requirements.txt

# install globally, does not matter because we are using the container for isolation
RUN python3 -m pip install -r requirements.txt

ENTRYPOINT ["python3", "/app/myapp.py"]
```

The reason i think caching would not solve your particular problem is data science libraries tend to have very long compile times. Caching the downloaded package is not enough, you need to also save the generated binaries.

Having said that, you can try `uv run --offline --no-python-downloads --verbose --all-packages -- pytest -v` to help you debug what's going on with your setup. That will print a lot of information about what `uv` is trying to do and why.

---

_Comment by @kforner on 2025-11-20 08:52_

Thanks!
So if I understand, your proposal is to by-pass uv?


---

_Comment by @pcastellazzi on 2025-11-20 15:11_

For this particular case, yes. It seems there are no plans to allow `uv` to interact with python outside a virtual environment. <https://github.com/astral-sh/uv/issues/1550>. There are other paths you could take (like having your own PyPI mirror, think of [Artifactory](https://jfrog.com/artifactory/) or [Bandersnatch](https://bandersnatch.readthedocs.io/en/latest/index.html); or using [Conda](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) which support environment cloning out of the box and it was designed for data science in particular, so probably most of what you need is pre compiled out of the box). 

---
