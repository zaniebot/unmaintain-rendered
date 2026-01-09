---
number: 11887
title: Custom installation options present in the pyproject.toml
type: issue
state: open
author: hongbo-miao
labels:
  - enhancement
assignees: []
created_at: 2025-03-01T23:41:41Z
updated_at: 2025-03-16T01:39:15Z
url: https://github.com/astral-sh/uv/issues/11887
synced_at: 2026-01-07T13:12:18-06:00
---

# Custom installation options present in the pyproject.toml

---

_Issue opened by @hongbo-miao on 2025-03-01 23:41_

### Summary

I have some projects that need install by following certain steps

```sh
uv sync --extra=build
uv sync --extra=build --extra=compile
```

base on https://docs.astral.sh/uv/concepts/projects/config/#build-isolation

Currently Renovate cannot upgrade dependencies for these projects.

I originally asked at https://github.com/renovatebot/renovate/discussions/33811

This would need uv side to support to let Renovate know what is proper steps to install. It would be great to support a field so that Renovate knows how to install, thanks! ☺️


---

_Label `enhancement` added by @hongbo-miao on 2025-03-01 23:41_

---

_Comment by @nathanjmcdougall on 2025-03-02 21:34_

Related: https://peps.python.org/pep-0771/

As a workaround for now, you could duplicate the `extra` information in a dependency group, and set the dependency to install by default:
https://docs.astral.sh/uv/concepts/projects/dependencies/#default-groups

---

_Comment by @hongbo-miao on 2025-03-02 21:45_

Thanks @nathanjmcdougall . In my case, the installation order is also important. For example,

```toml
[project]
name = "mineru"
version = "1.0.0"
requires-python = "~=3.12.0"

[project.optional-dependencies]
build = [
  "setuptools",
  "torch",
]
compile = [
  "detectron2",
  "magic-pdf[full]==1.1.0",
]
paddlepaddle-gpu = [
  "paddlepaddle-gpu==3.0.0b2",
]

[dependency-groups]
dev = [
  "poethepoet==0.32.2",
  "pytest-cov==6.0.0",
  "pytest==8.3.4",
]

[tool.uv]
package = false
prerelease = "allow"
no-build-isolation-package = ["detectron2"]

[tool.uv.sources]
detectron2 = { git = "https://github.com/facebookresearch/detectron2.git", branch = "main" }
paddlepaddle-gpu = { index = "paddlepaddle-gpu" }

[[tool.uv.index]]
name = "paddlepaddle-gpu"
url = "https://www.paddlepaddle.org.cn/packages/stable/cu118"
explicit = true

[tool.poe.tasks]
dev = "python src/main.py"
test = "pytest --verbose --verbose"
test-coverage = "pytest --cov=. --cov-report=xml"

```

I have to install based on order
```sh
uv sync --extra=build
uv sync --extra=build --extra=compile
uv sync --extra=build --extra=compile --extra=paddlepaddle-gpu
uv sync --extra=build --extra=compile --extra=paddlepaddle-gpu --dev
```

I wrote the reason why have to install like this at [here](https://github.com/opendatalab/MinerU/discussions/1374#discussion-7750775) if you are curious about why.

I assume you are referring to [project.optional-dependencies](https://docs.astral.sh/uv/concepts/projects/dependencies/#optional-dependencies):

![Image](https://github.com/user-attachments/assets/9d04e3b2-1404-46aa-a00e-42824d03ad53)

which I do not think can help installing based on certain order. 😔

---

_Comment by @nathanjmcdougall on 2025-03-02 22:38_

Ah I understand the issue now. An issue is the no-build-isolation workaround for `detectron2` which current requires multiple steps per
https://docs.astral.sh/uv/concepts/projects/config/#build-isolation
(see #2252 for other examples).

So you want some declarative configuration which `renovate` could use to know the correct sequence of `uv` commands to run. You're right, my suggestion won't help with that.

---

_Comment by @hongbo-miao on 2025-03-02 22:58_

Yup, that will help Renovate, otherwise all dependencies failed to bump versions and just stuck there. Thanks! ☺️

---

_Referenced in [hongbo-miao/hongbomiao.com#24323](../../hongbo-miao/hongbomiao.com/pulls/24323.md) on 2025-03-02 22:59_

---

_Referenced in [hongbo-miao/hongbomiao.com#24528](../../hongbo-miao/hongbomiao.com/pulls/24528.md) on 2025-03-02 22:59_

---

_Referenced in [hongbo-miao/hongbomiao.com#24628](../../hongbo-miao/hongbomiao.com/pulls/24628.md) on 2025-03-02 22:59_

---

_Referenced in [ultralytics/ultralytics#17527](../../ultralytics/ultralytics/pulls/17527.md) on 2025-03-15 14:58_

---

_Referenced in [hongbo-miao/hongbomiao.com#25189](../../hongbo-miao/hongbomiao.com/pulls/25189.md) on 2025-03-16 01:39_

---
