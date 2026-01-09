---
number: 12248
title: Building a project with dev dependencies does not make those dev dependencies installable.
type: issue
state: closed
author: boatcoder
labels:
  - question
assignees: []
created_at: 2025-03-17T21:35:48Z
updated_at: 2025-03-18T22:21:17Z
url: https://github.com/astral-sh/uv/issues/12248
synced_at: 2026-01-07T13:12:18-06:00
---

# Building a project with dev dependencies does not make those dev dependencies installable.

---

_Issue opened by @boatcoder on 2025-03-17 21:35_

### Summary

`uv add --dev` places packages in the `dependency-groups` section of  pyproject.toml 
`uv build` does not build them into an installable section `djadmin[dev]`
You have to move/copy  them from `dependency-groups` to `[project.optional-dependencies]` so that you can get them to install.

This might be as designed or desired but it is definitely puzzling when you try to package things and then they won't install.  Hopefully this will help someone else that runs into this.

```
[project]
name = "djadmin"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "django>=5.1.6",
    "django-extensions>=3.2.3",
    "django-hint>=0.3.2",
    "django-nested-admin>=4.1.1",
    "django-stubs-ext>=5.1.3",
    "djantic>=0.7.0",
    "pgvector>=0.3.6",
    "psycopg2-binary>=2.9.10",
]

[project.optional-dependencies]
dev = [
    "model-bakery>=1.20.4",
    "pyyaml>=6.0.2",
    "requests>=2.32.3",
    "django-debug-toolbar>=5.0.1",
]

[dependency-groups]
dev = [
    "responses>=0.25.7",
]
```


### Platform

mac0S 14.2.1

### Version

uv 0.6.6

### Python version

_No response_

---

_Label `bug` added by @boatcoder on 2025-03-17 21:35_

---

_Comment by @zanieb on 2025-03-17 21:46_

This is intentional. `dependency-groups` and development dependencies are not part of the published project.

https://docs.astral.sh/uv/concepts/projects/dependencies/#development-dependencies

It sounds like you want `uv add --optional dev` instead.

---

_Comment by @zanieb on 2025-03-17 21:47_

Note this is not just a detail of uv, but the intent of the Python standard for development dependency groups https://peps.python.org/pep-0735/

---

_Label `bug` removed by @zanieb on 2025-03-17 21:47_

---

_Label `question` added by @zanieb on 2025-03-17 21:47_

---

_Closed by @charliermarsh on 2025-03-18 22:21_

---
