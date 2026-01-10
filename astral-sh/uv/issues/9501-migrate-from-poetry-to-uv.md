---
number: 9501
title: Migrate from Poetry to UV
type: issue
state: closed
author: lluisCM
labels:
  - question
assignees: []
created_at: 2024-11-28T12:43:07Z
updated_at: 2025-05-15T17:50:24Z
url: https://github.com/astral-sh/uv/issues/9501
synced_at: 2026-01-10T01:24:41Z
---

# Migrate from Poetry to UV

---

_Issue opened by @lluisCM on 2024-11-28 12:43_

Hello, I'm not sure if this is the place to post a question, but I would like to get some feedback/experience on relation of replacing poetry to uv.

I'm looking to migrate from Poetry to UV. But my only concern is that UV seems to not provide a build backend, so what is the recommended way to structure the pyproject.toml file? Should I still install poetry, pdm or hatchling as a build systems? In that case, isn't a better option to just use poetry (for example) to manage the project and uv for venv etc... but not for the pyproject.toml file?

I'm specifically referring to this section:

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"

Thank you.

---

_Comment by @FishAlchemist on 2024-11-28 13:08_

UV currently doesn't have a backend built, so it should be difficult to handle functions that require a backend.
* https://github.com/astral-sh/uv/issues/8779

However, besides build backend, there are still many replaceable functions, so it depends on whether the functions you need require a backend.

---

_Comment by @lluisCM on 2024-11-28 14:21_

Be able to generate the .whl file, building and install the current project and probably prepare it to publish to PyPi requires a backend, right? 
So what is the recommended way to proceed? which backend is recommended/supported to use?
Thank you.

---

_Comment by @konstin on 2024-11-28 14:30_

Until uv has its own backend, you can use hatchling or the pdm backend:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

```toml
[build-system]
requires = ["pdm-backend"]
build-backend = "pdm.backend"
```

uv works with all of them, you can use the backend you like best. For new projects, `uv init` has a `--build-backend` option, the default is hatchling, which should cover most use cases (also coming from poetry).

---

_Label `question` added by @konstin on 2024-11-28 14:30_

---

_Comment by @laudmt on 2024-11-29 17:25_

Having personally passed a few projects from poetry to uv, I encountered an issue with hatchling, which had trouble to find the correct compiler.

Adding this fixed it:
```bash
export CC=gcc 
export AR=ar
```

I believe it is related to this issue : https://github.com/astral-sh/uv/issues/8429

---

_Comment by @lluisCM on 2024-12-02 08:24_

> Until uv has its own backend, you can use hatchling or the pdm backend:
> 
> [build-system]
> requires = ["hatchling"]
> build-backend = "hatchling.build"
> [build-system]
> requires = ["pdm-backend"]
> build-backend = "pdm.backend"
> uv works with all of them, you can use the backend you like best. For new projects, `uv init` has a `--build-backend` option, the default is hatchling, which should cover most use cases (also coming from poetry).

Thank you for the clarification.

---

_Closed by @zanieb on 2024-12-02 23:32_

---

_Comment by @qiuxiaomu on 2025-05-15 17:48_

Hope uv's build system is on the schedule! 

---

_Comment by @zanieb on 2025-05-15 17:50_

We're working on it!

- #8779 
- #3957 
- #12804 

---
