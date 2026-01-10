---
number: 7712
title: Changing libraries while working on a project syncs heavy dependencies
type: issue
state: open
author: jbohnslav
labels: []
assignees: []
created_at: 2024-09-26T15:07:21Z
updated_at: 2024-09-26T15:07:21Z
url: https://github.com/astral-sh/uv/issues/7712
synced_at: 2026-01-10T01:24:18Z
---

# Changing libraries while working on a project syncs heavy dependencies

---

_Issue opened by @jbohnslav on 2024-09-26 15:07_

Hi, I'm looking for advice on how to use UV in my workflow. Right now, changing any code in local library requires 7GB of changes in my Docker image.

My project looks like this
```
libs
    my_lib/
       # requires pytorch, cuda, etc. 7GB
projects
    my_project/
         # requires my_lib
```

Docker section, as per the UV docs
```docker
RUN mkdir /app 
COPY libs /app/libs 

WORKDIR /app/projects/my_project 
ENV UV_LINK_MODE=copy \ 
    UV_COMPILE_BYTECODE=1 
RUN --mount=type=cache,target=/root/.cache/uv \ 
    --mount=type=bind,source=projects/caption/.python-version,target=.python-version \ 
    --mount=type=bind,source=projects/caption/uv.lock,target=uv.lock \ 
    --mount=type=bind,source=projects/caption/pyproject.toml,target=pyproject.toml \ 
    uv sync --frozen --no-install-project --no-editable 

COPY projects/my_project /app/projects/my_project 
# Sync the project 
RUN --mount=type=cache,target=/root/.cache/uv \ 
    uv sync --frozen --no-editable
```

And here's the chunk of the pyproject.toml: 

```toml 
dependencies = [ "my_lib",  "ruff>=0.6.4", "setuptools>=75.1.0", ] 

[tool.uv.sources] 
my_lib = { path = "../../libs/my_lib", editable = true }
```

My issue right now is that when I change library code and rebuild my docker container, changing library code is a change to the docker layer that also installs the large dependencies, so I have to build and push the entire thing. This is pushing like 7GB when I change a single line of text in a local library. 

How do I fix it? 




---
