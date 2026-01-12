```yaml
number: 8848
title: "`uv build` does not work if UV_PROJECT_ENVIRONMENT is a sub path of `.venv`"
type: issue
state: open
author: zwing99
labels:
  - bug
assignees: []
created_at: 2024-11-06T00:34:10Z
updated_at: 2025-04-01T16:51:15Z
url: https://github.com/astral-sh/uv/issues/8848
synced_at: 2026-01-12T15:59:36Z
```

# `uv build` does not work if UV_PROJECT_ENVIRONMENT is a sub path of `.venv`

---

_@zwing99_

## uv version
uv version = 0.4.29

## steps to replicate
```bash
# create dir
mkdir uv-test-build; cd uv-test-build
# init and add click as dependancy so that .venv/.docker get initialized
uv init --lib --name test-uv-build; UV_PROJECT_ENVIRONMENT=.venv/.docker uv add click
# run uv build
UV_PROJECT_ENVIRONMENT=.venv/.docker uv build
```
error:
```bash
error: Broken virtual environment `/path/to/dir/test-uv-build/.venv`: `pyvenv.cfg` is missing
```

This does not happen if i change to `UV_PROJECT_ENVIRONMENT=.venv.docker`

The main motivation for this is to seperate the virtualenv in docker vs out of docker during local dev in case there are package difference between MacOS and Linux container. By making it a .subfolder of .venv i can lean on standard gitignore from [toptal gitignoreio](https://www.toptal.com/developers/gitignore/) without having to customize. I can successfully do this trick in other package managers like poetry. 

---

_Assigned to @zanieb by @zanieb on 2024-11-06 01:25_

---

_Label `bug` added by @charliermarsh on 2024-11-07 13:35_

---
