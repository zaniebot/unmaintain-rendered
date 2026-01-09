---
number: 12789
title: "`uv lock` isn't updating dependencies pinned versions"
type: issue
state: closed
author: ihelmer07
labels:
  - question
assignees: []
created_at: 2025-04-09T21:10:55Z
updated_at: 2025-05-12T21:20:44Z
url: https://github.com/astral-sh/uv/issues/12789
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv lock` isn't updating dependencies pinned versions

---

_Issue opened by @ihelmer07 on 2025-04-09 21:10_

### Summary

@zanieb - Maybe you can help further as I'm now noticing an issue with keeping the dependencies current.

I created a single repo `build-reqs` with a single `pyproject.toml` file
```toml
#build-reqs/pyproject.toml
[project]
name = "build-reqs"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
  "boto3>=1.36.15",
  "celery==5.4.0"
]
```

I have an app repo that I ran `uv add git+https://path.to.repo`
this is the `pyproject.toml` for app repo
```toml
#app/pyproject.toml
[project]
name = "app"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "build-reqs",
]

[tool.uv.sources]
build-reqs = { git = "https://gitlab.com/URL/build-reqs.git" }

```

If I run `uv sync` in the app folder it will pull in all the correct dependencies.

However if I now update the `build-reqs` repo to a new dependency version `"celery==5.1.0"`.

```toml
#build-reqs/pyproject.toml
[project]
name = "build-reqs"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
  "boto3>=1.36.15",
  "celery==5.1.0"
]
```

The `app` repo doesn't update by running `uv sync` or any other command.
if I run `uv lock` in the app folder - it will still show celery 5.4.0.

I can get a correct requirements list if I use `uv pip compile -o reqs.txt pyproject.toml`
but I can't get `uv sync` or `uv lock` to 'refresh' the build-req dependencies.

### Platform

windows

### Version

uv 0.5.29

### Python version

3.12.9

---

_Label `bug` added by @ihelmer07 on 2025-04-09 21:10_

---

_Comment by @zanieb on 2025-04-09 23:00_

This isn't supported. There's some previous discussion in https://github.com/astral-sh/uv/issues/6781 that may be helpful.

#6794 will also be relevant if you want this workflow.

---

_Label `bug` removed by @zanieb on 2025-04-09 23:00_

---

_Label `question` added by @zanieb on 2025-04-09 23:00_

---

_Closed by @charliermarsh on 2025-05-12 21:20_

---
