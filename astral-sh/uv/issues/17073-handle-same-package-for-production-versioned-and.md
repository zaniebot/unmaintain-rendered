---
number: 17073
title: handle same package for production(versioned) and development (editable)
type: issue
state: closed
author: donniedarko-zen
labels:
  - question
assignees: []
created_at: 2025-12-10T15:07:57Z
updated_at: 2025-12-19T08:43:19Z
url: https://github.com/astral-sh/uv/issues/17073
synced_at: 2026-01-10T01:26:13Z
---

# handle same package for production(versioned) and development (editable)

---

_Issue opened by @donniedarko-zen on 2025-12-10 15:07_

### Question

I am trying to handle two different environments using uv: 
* Production environment → uses a **versioned dependency** from a private package index (e.g., CodeArtifact) 
* Local development environment → uses an **editable version of the same private package** from a local path
At the same time, I want local development to automatically use the editable local path version of that same package. Currently, uv always requires explicit flags to choose between these two setups, and exporting without options does not include the production dependency.

I want to be able to run:
`uv export --no-dev -o requirements.txt`
and get a production-ready requirements.txt that includes the versioned package, without needing to pass any extra flags. 

what is the recommended uv workflow for cleanly separating:
1. production versioned dependencies, and 
2. local editable dependencies,
when both refer to a package with the same name?

------
The ideal would be like :

```Pyproject
[project]
name = "my-repo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "shapely==2.0.4",
    "brain==0.11.100",
]

[dependency-groups]
dev = [
    "brain",
    "pytest>=9.0.1",
]

[tool.uv.sources]
brain= { path = "../brain", editable = true, group='dev'}
```

So once I do `uv export --no-dev -o requirements.txt` , the dev dependencies will be excluded and only the versioned `brain` will be exported with other packages. (`shapely` will be included and `pytest` will be excluded)

-----

I used `[dependency-groups]` and resolved the [conflicts](https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies) in the `pyproject.toml` file. However, both production and development environments are created under `[package.dev-dependencies]` in the lock file. This doesn't seem right, because the production packages shouldn't be under the dev dependencies. 

### Platform

_No response_

### Version

uv 0.9.13

---

_Label `question` added by @donniedarko-zen on 2025-12-10 15:07_

---

_Comment by @konstin on 2025-12-11 10:01_

Does `uv export --no-sources` work for your case? It's not intended for that, but it may just work here.

---

_Comment by @donniedarko-zen on 2025-12-19 08:43_

> Does `uv export --no-sources` work for your case? It's not intended for that, but it may just work here.

Thank you, it seems like this is what I need to use with the above `pyproject.toml` file. 
`uv export ---no-sources -o requirements.txt` gives the expected `requirements.txt` file without any editable versions and dev-group. 

---

_Closed by @donniedarko-zen on 2025-12-19 08:43_

---
