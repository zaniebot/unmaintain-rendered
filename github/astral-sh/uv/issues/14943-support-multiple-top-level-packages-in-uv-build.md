---
number: 14943
title: "Support multiple top-level packages in `uv_build`"
type: issue
state: closed
author: johnthagen
labels:
  - enhancement
assignees: []
created_at: 2025-07-28T16:30:48Z
updated_at: 2025-07-28T17:34:59Z
url: https://github.com/astral-sh/uv/issues/14943
synced_at: 2026-01-07T13:12:19-06:00
---

# Support multiple top-level packages in `uv_build`

---

_Issue opened by @johnthagen on 2025-07-28 16:30_

### Summary

We have been able to move most of our applications from `hatchling` to `uv_build` and would like to move the rest, but we are missing one feature: the ability to configure `uv_build` to install editable installs for multiple top level packages outside of a `src` directory.

The is a feature supported by a number of build backends already, which I think signals that it's a much used feature.

### Hatchling

```toml
[tool.hatch.build]
packages = [
    "extra1",
    "extra2",
]
```

- https://hatch.pypa.io/latest/config/build/#packages

### Poetry

```toml
[tool.poetry]
packages = [
    { include = "extra1" },
    { include = "extra2" },
]
```

- https://python-poetry.org/docs/pyproject#packages

### Setuptools

```py
# setup.py

setup(
    name="seabird",
    packages=['albatross', 'albatross.beak', 'albatross.wings', 'albatross.tail'],
    # ...
)
```

### Motivation

In the use cases I've used and seen, it's less about building multiple packages into a wheel for a _library_ and more about having the build tool install editable installations for multiple top level packages for an _application_ that doesn't follow the `src/` layout.

For some applications like FastAPI, Django, etc. it's common not use a `src/` directory. In these applications, you sometimes want to have multiple top level packages, some of which might contain development-only utilities and such. Having them live in separate paths relative to `pyproject.toml` is convenient for only `COPY`ing certain folders into a container image and easily knowing you've excluded others.

In this scenario, we might still want the standard `uv sync` to editable install multiple top level "packages" into the virtual environment so the `PYTHONPATH` all works.

This is just one use case, I'm sure others have many more given that hatchling, Poetry, and setuptools at least all support this. It provides a lot of flexibility for applications that aren't following the standard `src/` + 1 package into a wheel format that most libraries on PyPI do.

### Previous Posts

- https://github.com/astral-sh/uv/issues/3957#issuecomment-2827475439
- https://github.com/astral-sh/uv/issues/3957#issuecomment-2836831150
- https://github.com/astral-sh/uv/issues/3957#issuecomment-2970497563

### Example

_No response_

---

_Label `enhancement` added by @johnthagen on 2025-07-28 16:30_

---

_Comment by @zanieb on 2025-07-28 16:37_

Isn't this supported and documented at https://docs.astral.sh/uv/concepts/build-backend/#modules ?

---

_Comment by @zanieb on 2025-07-28 16:38_

(Perhaps I'm missing something)

---

_Comment by @johnthagen on 2025-07-28 17:04_

@zanieb So https://docs.astral.sh/uv/concepts/build-backend/#modules is definitely in the right direction, but `module-name` is singular, right? I think what we're looking for is something like:

```toml
[tool.uv.build-backend]
module-names = ["FOO", "BAR", "BAZ"]
module-root = ""
```

Does that make sense?

---

_Comment by @zanieb on 2025-07-28 17:06_

We document support for multiple module names in the next section

---

_Comment by @zanieb on 2025-07-28 17:06_

See https://github.com/astral-sh/uv/pull/14460

---

_Comment by @johnthagen on 2025-07-28 17:34_

Wow, yes I missed that this was added recently! Thanks!

---

_Closed by @johnthagen on 2025-07-28 17:34_

---
