```yaml
number: 265
title: Support resolving modules from conda or pixi environments
type: issue
state: closed
author: pavelzw
labels:
  - help wanted
  - imports
assignees: []
created_at: 2025-05-08T08:54:03Z
updated_at: 2025-05-23T18:00:43Z
url: https://github.com/astral-sh/ty/issues/265
synced_at: 2026-01-12T15:54:22Z
```

# Support resolving modules from conda or pixi environments

---

_@pavelzw_

### Summary

When using pixi with ty, you get `lint:unresolved-import` errors like this:

```
❯ pixi run ty check
...
error: lint:unresolved-import: Cannot resolve imported module `pydantic_settings`
 --> pixi_diff_to_markdown/settings.py:3:6
  |
1 | from enum import Enum
2 |
3 | from pydantic_settings import (
  |      ^^^^^^^^^^^^^^^^^
4 |     BaseSettings,
5 |     PydanticBaseSettingsSource,
  |
info: `lint:unresolved-import` is enabled by default
❯ pixi run python -c "import pydantic_settings"
# works
❯ pixi run which ty
/Users/pavel/projects/pixi-diff-to-markdown/.pixi/envs/default/bin/ty
❯ pixi run which python
/Users/pavel/projects/pixi-diff-to-markdown/.pixi/envs/default/bin/python
❯ pixi run python -m site
sys.path = [
    '/Users/pavel/projects/pixi-diff-to-markdown',
    '/Users/pavel/projects/pixi-diff-to-markdown/.pixi/envs/default/lib/python313.zip',
    '/Users/pavel/projects/pixi-diff-to-markdown/.pixi/envs/default/lib/python3.13',
    '/Users/pavel/projects/pixi-diff-to-markdown/.pixi/envs/default/lib/python3.13/lib-dynload',
    '/Users/pavel/projects/pixi-diff-to-markdown/.pixi/envs/default/lib/python3.13/site-packages',
]
USER_BASE: '/Users/pavel/.local' (exists)
USER_SITE: '/Users/pavel/.local/lib/python3.13/site-packages' (doesn't exist)
ENABLE_USER_SITE: True
```

see https://github.com/pavelzw/pixi-diff-to-markdown/pull/66 for a reproducer.
pixi (and conda envs in general) stores its python site packages in `.pixi/envs/<env-name>/lib/python3.13/site-packages` (on windows). (note: this is the same environment that `ty` is installed in)

When running `uv sync` and afterwards `pixi run ty check`, I don't get the error anymore so it seems to me that `ty` is only checking `.venv` but not the actual environment that i'm using.

There are a lot of scenarios where it's not possible for me to use `uv`, for example in corporate settings where i only have a conda-forge mirror and not a pypi mirror, so ty supporting pixi environments would be very nice.

### Version

ty 0.0.0-alpha.7

---

_Renamed from "lint:unresolved-import when using pixi or conda environments" to "`lint:unresolved-import` when using pixi or conda environments" by @pavelzw on 2025-05-08 09:02_

---

_Comment by @pavelzw on 2025-05-08 09:29_

With `pixi run ty check --extra-search-path .pixi/envs/default/lib/python3.13/site-packages --python-version 3.13` you can get it to work but i think a solution that doesn't require me to add this manually would be nicer.

---

_Comment by @MichaReiser on 2025-05-08 10:36_

Does pixi set the `VIRTUAL_ENV`? You should also be able to use `ty check --python=.pixi`

---

_Label `module-resolution` added by @MichaReiser on 2025-05-08 10:36_

---

_Comment by @pavelzw on 2025-05-08 11:51_

> Does pixi set the `VIRTUAL_ENV`?

Nope, but both conda and pixi set `CONDA_PREFIX` which is the canonical way to the environment:

```
❯ pixi run env | grep CONDA_PREFIX
CONDA_PREFIX=/Users/pavel/projects/pixi-diff-to-markdown/.pixi/envs/default
```

> You should also be able to use `ty check --python=.pixi`

```
❯ pixi run ty check --python=.pixi/envs/default
ty failed
  Cause: Invalid search path settings
  Cause: Failed to discover the site-packages directory: `--python` argument points to a broken venv with no pyvenv.cfg file
```

---

_Comment by @pavelzw on 2025-05-08 12:18_

Even if pixi would set `VIRTUAL_ENV` it currently doesn't work:

```
❯ pixi run "VIRTUAL_ENV=.pixi/envs/default ty check"
ty failed
  Cause: Invalid search path settings
  Cause: Failed to discover the site-packages directory: `VIRTUAL_ENV` environment variable points to a broken venv with no pyvenv.cfg file
```

You should be able to get most stuff from `pyenv.cfg` in a conda env by inspecting `$CONDA_PREFIX/conda-meta/python-3.*.json`, for example `python-3.13.3-h81fe080_101_cp313.json`

---

_Comment by @JaRoSchm on 2025-05-08 12:29_

The necessary functionality for detecting and handling conda environments is already present in `uv`. Maybe some of that could be reused for `ty`.

---

_Comment by @MichaReiser on 2025-05-08 12:33_

We do plan for a uv integration to discover workspace packages, virtual environemnt, and the python version. 

---

_Comment by @AlexWaygood on 2025-05-08 12:36_

Currently we only support virtual environments. https://github.com/astral-sh/ty/issues/193 is already open to track supporting system installations, and we'll probably consider supporting other popular installations of Python such as conda installations too.

---

_Renamed from "`lint:unresolved-import` when using pixi or conda environments" to "Support resolving modules from conda or pixi environments" by @AlexWaygood on 2025-05-08 12:36_

---

_Label `wish` added by @AlexWaygood on 2025-05-08 12:36_

---

_Comment by @AlexWaygood on 2025-05-19 15:43_

Now that https://github.com/astral-sh/ty/issues/193 is complete, this issue should be easy to tackle. We just need to add support for the `CONDA_PREFIX` environment variable: if no `--python` flag has been passed (and no configuration option has been set on the command line) _and_ `VIRTUAL_ENV` has not been set _and_ there's no `.venv` directory in the project root, _then_ we should check to see if `CONDA_PREFIX` has been set. If so, we should understand that the value of `CONDA_PREFIX` is the currently active Python environment. We should treat these environments identically to the way we treat system environments.

---

_Label `wish` removed by @AlexWaygood on 2025-05-19 15:43_

---

_Label `help wanted` added by @AlexWaygood on 2025-05-19 15:43_

---

_Closed by @MichaReiser on 2025-05-23 18:00_

---
