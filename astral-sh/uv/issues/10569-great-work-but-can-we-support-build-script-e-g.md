---
number: 10569
title: "Great work, but can we support build script e.g. `build.py` for building simple C extension modules?"
type: issue
state: open
author: jymchng
labels:
  - enhancement
  - build-backend
assignees: []
created_at: 2025-01-13T16:03:49Z
updated_at: 2025-04-16T10:29:46Z
url: https://github.com/astral-sh/uv/issues/10569
synced_at: 2026-01-10T01:24:55Z
---

# Great work, but can we support build script e.g. `build.py` for building simple C extension modules?

---

_Issue opened by @jymchng on 2025-01-13 16:03_

Hi, great work with `uv`! I love Rust and love seeing how it is revolutionizing the Python's ecosystem.

Anyway, I use `poetry` with a simple `build.py` build script to build my C extension module for a Python library.

When I attempt to switch over to `uv`, I find that `uv` has no known support for such a `build.py` build script, unlike `poetry`.

I understand it has support for `maturin` for building Rust extension modules and also its documentations say using the `scikit-learn-backend` for building both C and C++ extension modules is the best bet.

I know `cmake`, horrible beast but I know it. But I really do not want to use `cmake` just to build a straightforward C extension module, `build.py` build script has successfully built my package very well.

I ended up having this `pyproject.toml`.

```
[project]
name = "pure-python-lib"
version = "0.1.0"
description = "Add your description here"
authors = [
    { name = "Jim Chng", email = "jimchng@outlook.com" }
]
readme = "README.md"
requires-python = ">=3.8"

# [tool.poetry]
# name = "pure-python-lib"
# version = "0.1.0"
# description = "Add your description here"
# authors = ["Jim Chng <jimchng@outlook.com>"]
# readme = "README.md"
# requires-python = ">=3.8"

[build-system] # poetry-dynamic-versioning
requires = ["poetry-core>=1.0.0", "poetry-dynamic-versioning>=1.0.0,<2.0.0"]
build-backend = "poetry_dynamic_versioning.backend"

[tool.poetry.build]
script = "build.py"
generate-setup-file = false
```

And a simple `build.py` for illustration:

```python; build.py
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

logger.info("`build.py` accessed!")
```

It does work in the sense that `build.py` is 'accessed' when I run `uv build`, like so:

```bash
$ uv build
Building source distribution...
Building wheel from source distribution...
INFO:__main__:`build.py` accessed! # here, `build.py` is indeed accessed
Successfully built dist/pure_python_lib-0.1.0.tar.gz
Successfully built dist/pure_python_lib-0.1.0-cp38-cp38-manylinux_2_35_x86_64.whl
```

But of course, now the ugly thing is `poetry` only recognizes `[tool.poetry]` and **NOT** `[project]` in `pyproject.toml` which `uv` recognizes. I end up having two chunks (both `[tool.poetry]` and `[project]`) that are ugly duplicates of each other.

Not sure how to resolve this.

---

_Referenced in [python-poetry/poetry#10029](../../python-poetry/poetry/issues/10029.md) on 2025-01-13 16:10_

---

_Comment by @tpgillam on 2025-01-13 21:27_

Just a workaround, but you could use the setuptools backend to build the extension module. This necessitates a `setup.py` rather than `build.py`.

I do this in a small package I maintain (https://github.com/tpgillam/mt2) :
- [setup.py](https://github.com/tpgillam/mt2/blob/main/setup.py).
- relevant part of [pyproject.toml](https://github.com/tpgillam/mt2/blob/3ebb30b52b8318628a4aaea760787bc5bf6f0752/pyproject.toml#L29-L31) .. NB in this case we've got a build-time dependency on `numpy` expressed here.

---

_Comment by @zanieb on 2025-01-13 21:46_

I believe @konstin is thinking about this.

---

_Label `build-backend` added by @konstin on 2025-01-14 08:31_

---

_Comment by @jymchng on 2025-01-18 09:41_

@konstin Good day to you sir

Understand that you are looking at it. Thank you.

---

_Comment by @jymchng on 2025-01-18 09:46_

@konstin 

May I suggest two aspects of having an arbitrary `build.py` which may be a source of bugs:

1) As `uv build` builds the package / 'binary' (a.k.a. application, e.g. a FastAPI application) in a temporary directory, importing modules (e.g. a `build_utils.py` for helper functions to be shared across `build.py` and other scripts like files, e.g. `noxfile.py`) from current project in `build.py` may not work. Although it is uncustomary for `build.py` to import modules from current project, it is worthwhile to note how `uv` wants to handle this.

2) Do allow arbitrary positional arguments to be passed into `build.py`, e.g. `uv build -- -a --b` where `-a` and `--b` are then passed into `python -m build`.

Both for your considerations.

---

_Referenced in [stanford-centaur/PyPantograph#90](../../stanford-centaur/PyPantograph/issues/90.md) on 2025-03-29 18:37_

---

_Label `enhancement` added by @konstin on 2025-04-11 09:51_

---

_Comment by @futurewasfree on 2025-04-13 17:38_

I could only add +1 on this feature request that is currently blocking us from poetry to uv migration.
Saw two issues  personally:
1. Similar to what was described above, package builds in a temporary directory are breaking `PyBind11Extension` build (at least with setuptools) in a presence of `include_dirs`:
```
    Pybind11Extension(
        "extension_name",
        sources=sources,
        include_dirs=include_dirs)
```
The latter are usually relative to package path and it looks like it's impossible to get correct absolute path within setup.py file (it's already executed from a temporary directory, as the copy)
2.  Sometimes you just want to run arbitrary script in a package folder, e.g. to pre-populate cached files. This seems not possible due to the same temp dir restriction and might require quite few overloads of `setuptools.Command` and `setuptools.build` (if possible at all).
I think it could be streamlined as well..

Thanks in advance for your consideration!

---

_Comment by @konstin on 2025-04-14 08:57_

Have you checked hatchling? It provides build hooks (https://hatch.pypa.io/latest/config/build/#build-hooks) that can be used to build native extensions.

---

_Comment by @futurewasfree on 2025-04-14 09:55_

Yes, I've did. And I saw some examples running shell scripts as build hook there.
But let's say that I  want to run python under build backend's env, how could I do this with hatchling?

---

_Comment by @jorenham on 2025-04-14 12:51_

> Yes, I've did. And I saw some examples running shell scripts as build hook there. But let's say that I want to run python under build backend's env, how could I do this with hatchling?

See https://hatch.pypa.io/dev/plugins/build-hook/reference/



---

_Comment by @futurewasfree on 2025-04-14 13:19_

Could you point at more concrete example that I can not see?
I've followed that page and got there: https://github.com/rmorshea/hatch-build-scripts that's what I was referring to about shell scripts

---

_Comment by @jorenham on 2025-04-14 13:33_

> Could you point at more concrete example that I can not see? I've followed that page and got there: [rmorshea/hatch-build-scripts](https://github.com/rmorshea/hatch-build-scripts?rgh-link-date=2025-04-14T13%3A19%3A50.000Z) that's what I was referring to about shell scripts

That plugin is actually a pretty good example; as it's written in Python itself

---

_Comment by @futurewasfree on 2025-04-15 10:16_

Ok, understood now, I'll take a look into custom hatch plugin then..

Wondering if there is any mitigation for the 1st issue I've reported with `include_dirs` and setuptools build?

---

_Comment by @jymchng on 2025-04-16 10:29_

https://github.com/winstxnhdw/KinematicBicycleModel/blob/main/pyproject.toml

Take a look at this guys, maybe we can use `setuptools` to 'natively' help us to build extension modules between a dedicated build script.

---

_Referenced in [astral-sh/uv#16438](../../astral-sh/uv/issues/16438.md) on 2025-10-24 15:16_

---
