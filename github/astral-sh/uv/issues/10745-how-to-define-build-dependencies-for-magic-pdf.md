---
number: 10745
title: "How to define & build dependencies for `magic-pdf[full]` in pyproject.toml?"
type: issue
state: closed
author: hongbo-miao
labels: []
assignees: []
created_at: 2025-01-19T01:00:15Z
updated_at: 2025-01-19T12:28:12Z
url: https://github.com/astral-sh/uv/issues/10745
synced_at: 2026-01-07T13:12:18-06:00
---

# How to define & build dependencies for `magic-pdf[full]` in pyproject.toml?

---

_Issue opened by @hongbo-miao on 2025-01-19 01:00_

# Background

I have succeed installing `magic-pdf[full]` with Python 3.12 in Ubuntu 22 by using below way:

**pyproject.toml**

```toml
[project]
name = "mineru"
version = "1.0.0"
requires-python = "~=3.12.0"
dependencies = [
  "detectron2",
  "magic-pdf[full]==1.0.1"
]

[tool.uv]
package = false
prerelease = "allow"

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git" }
```

Then I can install by

```sh
uv venv
uv pip install torch
uv pip install --no-build-isolation git+https://github.com/facebookresearch/detectron2.git
uv sync --dev
```

**Notes:**

- `magic-pdf[full]` has a dependency `detectron2` which does not have wheel, which is why I run `uv pip install --no-build-isolation git+https://github.com/facebookresearch/detectron2.git`. 
- `detectron2` didn't define the dependency `torch` but requires it, which is why I manually run `uv pip install torch` before `detectron2`.
-  `magic-pdf[full] 1.0.1` currently is using a pre-release dependency `paddlepaddle 3.0.0b1` which is why we need `prerelease = "allow"`, but in future, once magic-pdf relies on a formal release version of `paddlepaddle`, then no need `prerelease = "allow"`.

# What I Try to Achieve

Now I am trying to fully use `pyproject.toml` to define everything for `magic-pdf[full]` without manual running things like `uv pip install --no-build-isolation`.
For `detectron2` is a dependency for `magic-pdf[full]`, ideally if no need define in `dependencies = []` list in pyproject.toml, that would be great, as in the code, there is no code import `detectron2` directly.

Any guide would be appreciate, thanks! ☺️

I have read

- https://docs.astral.sh/uv/concepts/projects/config/#build-isolation
- https://docs.astral.sh/uv/concepts/projects/dependencies/#default-groups

Below are what I have tried.

## Experiments

For below three experiments, when I try to run

```sh
uv sync --extra build
uv sync --extra build --extra compile
```

They all failed at first step `uv sync --extra build` with error

```sh
Resolved 220 packages in 2.83s
  × Failed to build `detectron2 @
  │ git+[https://github.com/facebookresearch/detectron2.git@9604f5995cc628619f0e4fd913453b4d7d61db3f`](https://github.com/facebookresearch/detectron2.git@9604f5995cc628619f0e4fd913453b4d7d61db3f%60)
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit
      status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
      ModuleNotFoundError: No module named 'setuptools'

      hint: This usually indicates a problem with the package or the build
      environment.
  help: `detectron2` was included because `mineru` (v1.0.0) depends on
        `detectron2`
```

## Experiment 1 - using `project.optional-dependencies`

```toml
[project]
name = "mineru"
version = "1.0.0"
requires-python = "~=3.12.0"
dependencies = [
  "detectron2",
  "magic-pdf[full]==1.0.1",
]

[project.optional-dependencies]
build = ["setuptools", "torch"]
compile = ["detectron2"]

[tool.uv]
package = false
prerelease = "allow"
no-build-isolation-package = ["detectron2"]

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git", branch = "main" }
```

## Experiment 2 - using `[[tool.uv.dependency-metadata]]`

```toml
[project]
name = "mineru"
version = "1.0.0"
requires-python = "~=3.12.0"
dependencies = [
  "detectron2",
  "magic-pdf[full]==1.0.1",
]

[project.optional-dependencies]
build = ["setuptools", "torch"]
compile = ["detectron2"]

[tool.uv]
package = false
prerelease = "allow"
no-build-isolation-package = ["detectron2"]

[[tool.uv.dependency-metadata]]
name = "detectron2"
version = "0.6"
requires-dist = ["torch"]

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git", branch = "main" }
```

## Experiment 3 - using `[build-system]`

```toml
[project]
name = "mineru"
version = "1.0.0"
requires-python = "~=3.12.0"
dependencies = [
  "detectron2",
  "magic-pdf[full]==1.0.1",
]

[build-system]
requires = ["setuptools"]
build-backend = "setuptools.build_meta"

[project.optional-dependencies]
build = ["torch"]
compile = ["detectron2"]

[tool.uv]
package = false
prerelease = "allow"
no-build-isolation-package = ["detectron2"]

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git", branch = "main" }
```


---

_Renamed from "How to install `magic-pdf[full]` without manual `uv pip install --no-build-isolation`?" to "How to define dependencies for `magic-pdf[full]` in pyproject.toml?" by @hongbo-miao on 2025-01-19 01:28_

---

_Renamed from "How to define dependencies for `magic-pdf[full]` in pyproject.toml?" to "How to define & build dependencies for `magic-pdf[full]` in pyproject.toml?" by @hongbo-miao on 2025-01-19 02:02_

---

_Comment by @hongbo-miao on 2025-01-19 12:22_

I just succeed by doing this way. Please let me know if any place can be improved, thanks! ☺️

```toml
[project]
name = "mineru"
version = "1.0.0"
requires-python = "~=3.12.0"

[project.optional-dependencies]
build = ["setuptools", "torch"]
compile = ["detectron2", "magic-pdf[full]==1.0.1"]

[tool.uv]
package = false
prerelease = "allow"
no-build-isolation-package = ["detectron2"]

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git", branch = "main" }
```

Now I just need install by

```sh
uv sync --extra=build
uv sync --extra=build --extra=compile
```

---

_Closed by @hongbo-miao on 2025-01-19 12:22_

---
