---
number: 7722
title: "`uv sync` handling third party dependencies in `setup.py`"
type: issue
state: closed
author: jamesbraza
labels: []
assignees: []
created_at: 2024-09-26T18:51:26Z
updated_at: 2025-10-30T19:00:16Z
url: https://github.com/astral-sh/uv/issues/7722
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv sync` handling third party dependencies in `setup.py`

---

_Issue opened by @jamesbraza on 2024-09-26 18:51_

I am using `uv==0.4.16`, and there are two packages that make my life annoying because their `setup.py` depends on [`torch`](https://pypi.org/project/torch/):

- https://github.com/facebookresearch/xformers/blob/v0.0.28/setup.py#L24-L31 ([PyPI](https://pypi.org/project/xformers/))
- https://github.com/Dao-AILab/flash-attention/blob/v2.6.3/setup.py#L21-L29 (not on PyPI)

How can `uv sync` work with this scenario? Currently, it successfully makes a `uv.lock`, but then fails on `uv sync`:

```
[[package]]
name = "xformers"
version = "0.0.28"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "numpy" },
    { name = "torch" },
]
sdist = { url = "https://files.pythonhosted.org/packages/be/8e/1b9bc5317a7a26d33af2db63f5db112c0cfe373befe7d5653e1d639db231/xformers-0.0.28.tar.gz", hash = "sha256:bc74e7033d2675962d4ddc708fc66328b6f6cbe5aedff7491eab98c338623bf2", size = 7757812 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/d1/a0/bb5a668186f5f77a73043a8ff8b86176c7d3cbed10cbdafcca67237ee432/xformers-0.0.28-cp311-cp311-manylinux_2_28_x86_64.whl", hash = "sha256:c7c6b1887d6bb71fe26464492b84d6cdbfbfdada5669a4a606c186a384e226f5", size = 16653563 },
    { url = "https://files.pythonhosted.org/packages/9b/e4/efead2c5c89cd5766c7c4f5320448c80a51f3a497bbe560b8000e2ebc930/xformers-0.0.28-cp312-cp312-manylinux_2_28_x86_64.whl", hash = "sha256:9b3a5e6f2e02d6b2bb84a38351dcb181cfdb037451ab8cd57f39c161af33d83b", size = 16653138 },
]
```

```none
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: xformers==0.0.28
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/user/Library/Caches/uv/builds-v0/.tmpCHWnFA/lib/python3.12/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/user/Library/Caches/uv/builds-v0/.tmpCHWnFA/lib/python3.12/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/Users/user/Library/Caches/uv/builds-v0/.tmpCHWnFA/lib/python3.12/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/user/Library/Caches/uv/builds-v0/.tmpCHWnFA/lib/python3.12/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 24, in <module>
ModuleNotFoundError: No module named 'torch'
---
```

---

_Comment by @charliermarsh on 2024-09-26 18:52_

There are some tips on that here: https://docs.astral.sh/uv/concepts/projects/#build-isolation. It's still a bit of a pain though.

---

_Comment by @jamesbraza on 2024-09-26 18:59_

Ah I see, thank you! Yeah so currently installation is a two-step process:

1. `pip install torch`
2. `pip install xformers`

I was sort of hoping `uv` could somehow convert that to be a one-step process, but it doesn't seem to be the case:

1. `uv sync --extra build`
    - Note: the extra `build= ["torch"]`
2. `uv sync --extra xformers`
    - Note: relies on `no-build-isolation-package = ["xformers"]` being in `tool.uv`

Do you think it's a reasonable request to `uv` to somehow support this use case as a one-step process? In other words, make it less of a special case from an installer's perspective

---

_Comment by @charliermarsh on 2024-09-27 12:25_

I would like it to be one-step, yeah. It's a reasonable request. It likely won't happen immediately though.

---

_Comment by @vwxyzjn on 2024-12-04 21:59_

Hi @charliermarsh just to confirm, the current best approach still requires two commands, right? I needed to run

```
uv sync
uv sync --compile
```

with 

```
[project]
name = "open-instruct"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "numpy<2",
    "packaging>=24.2",
    "setuptools>=75.6.0",
    "torch>=2.5.1",
    # "flash-attn>=2.7.0.post2",
]

# pytroch related setups
[tool.uv.sources]
torch = [
  { index = "pytorch-cu121", marker = "platform_system != 'Darwin'"},
]
[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true
[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

# flash-attn related setups
[project.optional-dependencies]
compile = ["flash-attn>=2.7.0.post2"]
[tool.uv]
no-build-isolation-package = ["flash-attn"]
[[tool.uv.dependency-metadata]]
name = "flash-attn"
version = "2.7.0.post2"
requires-dist = ["torch", "setuptools"]
```

I am slightly puzzled as I thought the `tool.uv.dependency-metadata` would make it a one command process, because it already tells uv to wait for `torch` and `setuptools` 👀

---

_Comment by @AyhamSaffar on 2025-01-08 15:41_

I am struggling with the same issue.

xformers is needed to run any hugging face models in PyTorch, so i would imagine alot of people are getting stuck on this issue.

I am sure xformers' environment is really poorly configured given it doesn't automatically install when you install the hugging face libraries that depend on it, but supporting these kinds of libraries without any hard-to-find tricks would be amazing.

---

_Comment by @AyhamSaffar on 2025-01-13 23:05_

Also @charliermarsh couldn't get xformers to install using your linked page.

Setup:

```
[project]
name = "demo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.12.*"
dependencies = []

[project.optional-dependencies]
build = ["torch", "setuptools"]
compile = ["xformers"]

[tool.uv]
no-build-isolation-package = ["xformers"]

[[tool.uv.dependency-metadata]]
name = "xformers"
version = "0.0.29.post1"
requires-dist = ["torch", "setuptools"]
```

Then running this:

```
uv sync --extra build
uv sync --extra build --extra compile
```

The following keeps getting raised:

> building 'xformers._C' extension
> ... UserWarning: Attempted to use ninja as the BuildExtension backend but we could not find ninja.. Falling back to using the slow distutils backend.
> ... UserWarning: Error checking compiler version for cl: [WinError 2] The system cannot find the file specified
> error: Microsoft Visual C++ 14.0 or greater is required. Get it with ...

Every time I add more to the pyproject.toml dependencies, a different error comes up with the same final line.

Is this a UV thing or an xformers thing?

---

_Comment by @charliermarsh on 2025-01-13 23:27_

That would be an xformers thing... Unfortunately they only ship Linux wheels, so you have to build from source.=, and your machine doesn't seem to have the dependencies it needs.

---

_Comment by @AyhamSaffar on 2025-01-13 23:51_

thanks! everything works fine on wsl.  You do not even need the whole no-build-isolation trick.

I guess the library setup.py is only run when there isn't a dist and wheel / whatever binaries for your platform on PyPI?

---

_Comment by @zanieb on 2025-01-14 00:00_

You'll probably want the MS C++ toolchain for other packages as well, https://visualstudio.microsoft.com/visual-cpp-build-tools/

---

_Comment by @charliermarsh on 2025-01-14 00:00_

Yeah that's right. You can see [here](https://pypi.org/project/xformers/#files) that they only ship Linux-compatible "wheels":

![Image](https://github.com/user-attachments/assets/a3d355d6-56e3-4699-acf6-15935da3d54d)

So if you're not on Linux, we have to download the source distribution and try to build it.

---

_Comment by @AyhamSaffar on 2025-01-14 00:04_

Cheers @zanieb and @charliermarsh. I appreciate the help.

---

_Referenced in [astral-sh/uv#11675](../../astral-sh/uv/issues/11675.md) on 2025-02-20 19:13_

---

_Referenced in [stfc/janus-core#421](../../stfc/janus-core/pulls/421.md) on 2025-03-14 19:47_

---

_Comment by @muhammad-fiaz on 2025-10-30 18:23_

is there any good solution for this?? or any example working reference,  for me the main issue is with `xformers` it always conflict with `cuda` enabled `pytorch` when installing with uv `pyproject.toml`

---

_Comment by @zanieb on 2025-10-30 18:46_

We made this work in one-step now, see https://docs.astral.sh/uv/concepts/projects/config/#augmenting-build-dependencies

@muhammad-fiaz if you have questions after reading that please open a new issue with details. I don't think this one is specific enough for us to help you.

---

_Closed by @zanieb on 2025-10-30 18:46_

---

_Comment by @muhammad-fiaz on 2025-10-30 18:59_

Hello @zanieb Thanks for replying, i also saw that [docs](https://docs.astral.sh/uv/concepts/projects/config/#augmenting-build-dependencies) but it does automate some features specific to `pytorch` and `flash attention` but for me the main issue is with `xformer` usage with uv i am not sure it always conflict with `pytorch` and installing incorrectly  when ever there is some new cuda so i needed to manually install using `pip` for each dependencies based dependencies so i have created an #16522 feature request support for multiple automated version for same dependencies 

BTW! i am using uv version (v0.9.6)

---
