```yaml
number: 2026
title: Local dependencies cannot specify its optional dependency
type: issue
state: closed
author: Andrew-Chen-Wang
labels:
  - needs-mre
assignees: []
created_at: 2024-02-27T22:20:12Z
updated_at: 2024-02-28T02:16:04Z
url: https://github.com/astral-sh/uv/issues/2026
synced_at: 2026-01-12T15:58:34Z
```

# Local dependencies cannot specify its optional dependency

---

_@Andrew-Chen-Wang_

(Feature request): I have a pyproject.toml specifying:

```toml
dependencies = [
    "models @ file:///${PROJECT_ROOT}/projects/backend/libs/models"
]
```

but I'd like to specify:

```
dependencies = [
    "models[all] @ file:///${PROJECT_ROOT}/projects/backend/libs/models"
]
```

Where models is defined as

```
[project]
name = "models"
version = "0.0.1"
description = "Relational database tables"
dependencies = [
    "sqlalchemy==2.0.27",
]

[project.optional-dependencies]
dev = [
    "pytest"
]
all = ["models[dev]"]
```

I think in a requirements.txt file, I'm able to specify:

```
file:///home/you/src/foo#egg=foo[bar,baz]
```

I tried adding that directly to requirements.txt and then run `uv pip install` but not working:

```
error: Failed to build editables
  Caused by: Failed to build editable: file:///Users/andrew/projects/backend/libs/models%23egg=models
  Caused by: Source distribution not found at: /Users/andrew/projects/backend/libs/models#egg=models
```

---

_Comment by @charliermarsh on 2024-02-27 22:46_

Hey sorry -- just trying to follow the request. Did `uv pip install -r requirements.txt` fail, with `requirements.txt` being:

```
models[all] @ file:///${PROJECT_ROOT}/projects/backend/libs/models
```

If not, what should we be doing to reproduce the error?

---

_Label `needs-mre` added by @charliermarsh on 2024-02-27 22:46_

---

_Comment by @Andrew-Chen-Wang on 2024-02-27 23:40_

Apologies for being unclear. I'm directly writing in my requirements.txt

```
# The following packages were excluded from the output:
# models
# background
# web
# tasks

-e file:///${PROJECT_ROOT}/projects/backend/libs/models
-e file:///${PROJECT_ROOT}/projects/backend/libs/background
-e file:///${PROJECT_ROOT}/projects/backend/services/web
-e file:///${PROJECT_ROOT}/projects/tools/tasks
```

where the comments were written by uv but the `-e` lines were written as part of a task runner.

I want to be able to do:

```
-e file:///${PROJECT_ROOT}/projects/backend/libs/models#egg=models[all]
```

But this doesn't work and based on the readme doesn't sound like it will be supported. I want to be able to install optinal dependencies of local packages, but I'm not sure how to do that today with uv

---

_Comment by @Andrew-Chen-Wang on 2024-02-27 23:46_

I think in retrospect, I'm kind of confusing my goal with this issue. What I really want is a compiled requirements.txt that incorporates optional dependencies for local packages. I want to be able to have in my pyproject.toml:

```
dependencies = [
    "models[all] @ file:///${PROJECT_ROOT}/projects/backend/libs/models"
]
```

But this isn't supported yet, only third party packages can do this like uvicorn[standard]

---

_Comment by @charliermarsh on 2024-02-28 00:05_

Hmm, I thought this was supported though. For example, you can `uv pip install -r requirements.txt` given this file in the root of the uv repo:

```
black[dev] @ ./scripts/editable-installs/black_editable
```

---

_Comment by @Andrew-Chen-Wang on 2024-02-28 01:56_

> Hmm, I thought this was supported though. For example, you can uv pip install -r requirements.txt given this file in the root of the uv repo:
> black[dev] @ ./scripts/editable-installs/black_editable

Having difficulty doing that in a subfolder like this (note the `-e` for editable if that makes a difference):

```
(.venv) andrew@Andrew changelog % uv pip install -r bin/requirements/requirements-dev.txt
error: Failed to build editables
  Caused by: Failed to build editable: file:///Users/andrew/Work/Code/projects/changelog/changelog/models[all]%20@%20file:/Users/andrew/Work/Code/projects/changelog/changelog/projects/backend/libs/models
```

Requirements file:

```
-e models[all] @ file:///${PROJECT_ROOT}/projects/backend/libs/models
```

---

For my true request, I'm looking to ensure all my monorepo's local package's dependencies are pinned to one version. For instance, let's say I have "pytest" pinned in a bunch of my local package's pyproject.toml's optional dependencies. I want to make sure they're all on my version specified in requirements.txt

But in pyproject.toml, I can't specify something like this:

```
dependencies = [
    "models[all] @ file:///${PROJECT_ROOT}/projects/backend/libs/models"
]
```

such that my requirements.txt file can output

```
pytest==1.0.0
```

---

_Comment by @charliermarsh on 2024-02-28 01:57_

I believe you want `uv pip install -e "./scripts/editable-installs/black_editable[dev]"`.

---

_Comment by @Andrew-Chen-Wang on 2024-02-28 02:13_

Ah I feel very stupid; this is an amazing package. Thank you so much!

root level pyproject.toml:

```
dependencies = [
    "tasks[all] @ file:///${PROJECT_ROOT}/projects/tools/tasks"
]
```

includes all dependencies. my tasks pyproject.toml:

```
[project]
name = "tasks"
version = "0.0.1"
description = "Task directory for running commands more easily"
dependencies = [
     "click==8.1.7"
]

[project.scripts]
inv = "tasks.tasks:main"

[tool.setuptools.packages.find]
include = ["tasks*"]  # package names should match these glob patterns (["*"] by default)
exclude = ["tasks.tests*"]  # exclude packages matching these glob patterns (empty by default)

[project.optional-dependencies]
dev = ["pytest", "pytest-asyncio]
all = ["tasks[dev]"]
```

and my resulting requirements.txt (including a post-process command that I ran in my task runner to append the final -e so that future users can just run `uv pip install requirements.txt`):

```
pytest==version
pytest-asyncio==version
-e file:///${PROJECT_ROOT}/projects/backend/tools/tasks
```

---

_Closed by @Andrew-Chen-Wang on 2024-02-28 02:13_

---

_Comment by @charliermarsh on 2024-02-28 02:16_

Not at all, I didn't know this syntax either until I had to implement it!

---
