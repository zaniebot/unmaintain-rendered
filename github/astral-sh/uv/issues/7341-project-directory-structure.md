---
number: 7341
title: Project directory structure
type: issue
state: closed
author: pkucmus
labels:
  - question
assignees: []
created_at: 2024-09-12T20:17:29Z
updated_at: 2025-06-22T14:14:00Z
url: https://github.com/astral-sh/uv/issues/7341
synced_at: 2026-01-07T13:12:17-06:00
---

# Project directory structure

---

_Issue opened by @pkucmus on 2024-09-12 20:17_

Hello.

[UV: 0.4.6, Python: 3.12.4]

(I'm using Poetry since it's inception, so I might be opinionated, but I'm learning a lot when going through the issues here - if something might sound like a "Poetry user's opinion" feel free to call me out :))

I'm also quite opinionated when it comes to the project directory structure, what you propose [here](https://docs.astral.sh/uv/guides/projects/#project-structure), for projects leads into other challenges down the line, like being forced to "dance around" the `.venv` directory in Docker.

```
.
├── .venv
│   ├── bin
│   ├── lib
│   └── pyvenv.cfg
├── .python-version
├── README.md
├── hello.py
├── pyproject.toml
└── uv.lock
```

Where if you were to "`src` the project", like so:

```
.
├── .venv
│   ├── bin
│   ├── lib
│   └── pyvenv.cfg
├── src
│   └── my_project
│       └── hello.py
├── .python-version
├── README.md
├── pyproject.toml
└── uv.lock
```

Then a few things can happen:

1. You can Docker volume everything that's in `src` 

```yaml
services:
  my_project:
    ...
    volumes:
      - ./src:/app/src
```

2. You can leverage Docker cache in build time if the dependencies did not change

```Dockerfile
FROM python:3.12
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/uv

...

WORKDIR /app

COPY uv.lock pyproject.toml ./

ARG INSTALL_DEV=false
RUN uv sync --frozen --no-install-project
COPY . .  
RUN uv sync --frozen
```

don't redo the dependency installation if uv.lock or pyproject.toml did not change

3. you can explicitly reference your root project with 

```python
from my_project import hello
```

This approach should be doable with a pyproject.toml addition of:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[tool.hatch.build]
include = [
    "src/my_project",
]
```

but for some reason my_project is not available for import.

---

_Referenced in [astral-sh/uv#7342](../../astral-sh/uv/issues/7342.md) on 2024-09-12 20:20_

---

_Comment by @zanieb on 2024-09-12 20:38_

Have you tried `uv init --package`? I believe it creates the structure you're looking for. 

See https://docs.astral.sh/uv/concepts/projects/#packaged-applications

---

_Label `question` added by @zanieb on 2024-09-12 20:38_

---

_Comment by @pkucmus on 2024-09-12 21:32_

Thanks for all you help on these, the issue was different and it's more likely related to the `hatchling.build` - `__init__.py` was missing from my root module (which Poetry did not care about). 
This "opinionated setup" of mine should be doable now but I'm curious about one more thing: can I have more than one root package in `src`? Same as https://python-poetry.org/docs/pyproject/#packages?

Also I'm curious how does everyone find the `src` approach for projects in general, is it at least worth mentioning in the docs?

---

_Comment by @zanieb on 2024-09-13 02:13_

Yeah totally. This is all build backend configuration. We don't provide our own build backend yet (#3957) but I believe hatchling supports what you're describing or you could use another build backend like setuptools: https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

There's also https://docs.astral.sh/uv/concepts/workspaces/ if you have more complicated setups.

---

_Comment by @sirosc on 2024-09-13 17:50_

One way of doing it in `pyproject.toml` with hatch is:
```
[tool.hatch.build.targets.wheel]
# Defines the targets for the wheel build.
include = ["src/my_project1/**", "src/my_project2/**"]
```


---

_Closed by @zanieb on 2024-09-14 03:13_

---

_Referenced in [tbe-team/raybot#5](../../tbe-team/raybot/issues/5.md) on 2025-02-23 17:11_

---

_Comment by @fabioz on 2025-04-03 12:38_

Note: In my own project I had to provide both `include` and `sources`:

i.e.:

```
[tool.hatch.build.targets.wheel]
sources = ["src", "tests", "etc"]
include = ["src/**", "tests/**", "etc/**"]
```

---

_Comment by @milanzmitrovic on 2025-05-02 07:44_

Does anyone know what is difference between package and library in UV parlance?

I mean, diff between "--lib" and "--package"?

https://docs.astral.sh/uv/concepts/projects/init/#libraries 


Examples:
```
uv init --lib example-lib

uv init --package example-pkg
```


---

_Comment by @mmohajer9 on 2025-06-22 14:14_

> Does anyone know what is difference between package and library in UV parlance?
> 
> I mean, diff between "--lib" and "--package"?
> 
> https://docs.astral.sh/uv/concepts/projects/init/#libraries
> 
> Examples:
> 
> ```
> uv init --lib example-lib
> 
> uv init --package example-pkg
> ```

In essence, **every library is considered a package**, as outlined in the documentation, but **not all packages are intended to function as libraries**. Additionally, when you choose the library option, a `py.typed` file is generated. This file serves as a marker indicating that the package supports static type checking. It informs type checkers such as `mypy` that the package includes type annotations and should be analyzed for type correctness. By including `py.typed`, you enable external tools to validate your type hints and help ensure the reliability of your code.

---
