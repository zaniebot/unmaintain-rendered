```yaml
number: 9800
title: "What's the uv equivalent of tool.hatch.build.include = [...]?"
type: issue
state: closed
author: rdong8
labels: []
assignees: []
created_at: 2024-12-11T04:47:38Z
updated_at: 2024-12-11T20:59:50Z
url: https://github.com/astral-sh/uv/issues/9800
synced_at: 2026-01-10T04:36:21Z
```

# What's the uv equivalent of tool.hatch.build.include = [...]?

---

_Issue opened by @rdong8 on 2024-12-11 04:47_

I want to include extra non-code files into my package (ie. JSON configuration files), and access them in code as `import resources; config_path = resources.files("config")`. In hatch I can do stuff this:

```toml
[tool.hatch.build]
include = ["config/**/*.json", "src/mypackage/**/*.py"]

[tool.hatch.build.targets.wheel]
packages = ["src/package"]

[tool.hatch.build.targets.wheel.force-include]
"config/" = "/config"
```

Is there an equivalent for uv?

---

_Comment by @FishAlchemist on 2024-12-11 10:26_

If I'm not mistaken, ``[tool.hatch.build]`` refers to the backend settings of **hatchling.build**, right? uv doesn't have its own officially released backend yet, so you might consider continuing to use **hatchling.build**, as it can still be used.
```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
uv build supports other build backends, not just uv's build backend.

Here are the supported backends during ``uv init``.
```
--build-backend build-backend
Initialize a build-backend of choice for the project.

Implicitly sets --package.

Possible values:

hatch: Use hatchling as the project build backend
flit: Use flit-core as the project build backend
pdm: Use pdm-backend as the project build backend
setuptools: Use setuptools as the project build backend
maturin: Use maturin as the project build backend
scikit: Use scikit-build-core as the project build backend
```

 Are you using UV as your build  backend right now?

---

_Comment by @rdong8 on 2024-12-11 14:09_

Ah ok that makes total sense, I can just continue to use Hatch. 

How would I be able to use uv to build though? It would be nice if I can eliminate one more extra tool. 

---

_Comment by @charliermarsh on 2024-12-11 14:11_

You can use `uv build` with the hatchling build backend. Like, you should be able to run `uv build` in your project even with those settings (unless they're specific to _hatch_ rather than hatchling).

---

_Closed by @rdong8 on 2024-12-11 14:13_

---

_Comment by @FishAlchemist on 2024-12-11 14:21_

@rdong8 If you want to remove the dependency on hatchling, you can keep an eye on the state of the UV build backend.
Here: https://github.com/astral-sh/uv/issues/3957#issuecomment-2517967239

---

_Comment by @rdong8 on 2024-12-11 14:51_

Awesome exactly what I was looking for

---

_Comment by @rdong8 on 2024-12-11 20:59_

I'm using uv 0.5.8, is this the correct way to do it:

```toml
[build-system]
requires = ["uv>=0.5,<0.6"]
build-backend = "uv"

[tool.uv.build-backend.data]
data = "config/"
```

I can't seem to be able to access it as `importlib.resources.files("config")` in my code. 

---
