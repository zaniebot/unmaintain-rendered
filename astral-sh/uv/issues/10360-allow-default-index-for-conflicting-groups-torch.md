```yaml
number: 10360
title: Allow default index for conflicting groups (torch cpu/cuda)
type: issue
state: open
author: sglbl
labels:
  - needs-design
assignees: []
created_at: 2025-01-07T13:14:09Z
updated_at: 2025-04-09T02:26:28Z
url: https://github.com/astral-sh/uv/issues/10360
synced_at: 2026-01-12T16:00:11Z
```

# Allow default index for conflicting groups (torch cpu/cuda)

---

_@sglbl_

Suggestion, not a bug: Allow default index for conflicting groups (torch cpu/cuda)

```toml
# This template toml file works with pytorch cpu and gpu versions @sglbl
# using 'uv sync --extra cpu' or 'uv sync --extra gpu' command
[project]
name = "digit-classification"
version = "0.1.0"
description = "Digit classification project"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "gradio==5.6.0",
    "matplotlib==3.9.2",
    "numpy==2.1.3",
    "opencv-python>=4.10.0.84",
    "transformers>=4.46.3",
]

[dependency-groups]
dev = [
    "jupyter>=1.1.1",
    "pytest>=8.3.3",
]

[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
]
gpu = [
  "torch>=2.5.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "gpu" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu", marker = "platform_system != 'Darwin'" },
  { index = "pytorch-gpu", extra = "gpu" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-gpu"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[tool.pytest.ini_options]
pythonpath = "."
filterwarnings = ["ignore::DeprecationWarning", "ignore::FutureWarning"]
```

Is there a way to set a default extra so that it installs pytorch gpu/cuda version without adding extra flag? 
(something like putting the flag `set-default = true` under one of the `[[tool.uv.index]]`)

---

_Label `needs-design` added by @charliermarsh on 2025-01-07 13:46_

---

_Comment by @zanieb on 2025-01-07 18:23_

Similar to [`default-groups`](https://docs.astral.sh/uv/reference/settings/#default-groups)?

---

_Comment by @sglbl on 2025-01-07 18:56_

@zanieb Yes. Actually I tried to solve the torch cpu/gpu issue with `dependency-groups`, but it didn't work because of the conflicts. 
So something like default-groups would be really nice

---

_Comment by @zanieb on 2025-01-07 18:59_

We support conflicting group declarations in the same was as extras.

---

_Comment by @sglbl on 2025-01-08 14:21_

@zanieb Also, is there a way to set a global default python version (like `asdf` does)?
Because now when I initialize uv, it initializes with my latest python version installed (if I haven't created .python-version file already). It's the same for virtual environment.

I have `Ubuntu 24.04 Gnome/Wayland` with `python3.12` preinstalled. And in every project I use `python3.10`. That's why I'm trying to set a default global python version without needing uninstall my Ubuntu Python. 

---

_Comment by @zanieb on 2025-01-08 21:12_

You can `cd ~ && uv python pin 3.10`

---

_Comment by @HichemAK on 2025-01-09 11:03_

I still have a problem with `default-groups`. For example, in the following setup:

```toml
[project]
name = "example"
version = "0.1.0"
description = "XXXXX"
readme = "README.md"
authors = [
    { name = "Hichem Ammar Khodja", email = "hichem5696@gmail.com" }
]
requires-python = ">=3.9.7"
dependencies = [
]

[tool.uv]
default-groups = ["cu118"]
conflicts = [
  [
    {extra = "cpu"},
    {extra = "cu118"},
    {extra = "cu121"},
    {extra = "cu124"},
  ],
]

[dependency-groups]
cpu = [
  "torch",
]
cu118 = [
  "torch",
]
cu121 = [
  "torch",
]
cu124 = [
  "torch",
]


[tool.uv.sources]
torch = [
  { index = "torch-cpu", extra = "cpu"},
  { index = "torch-cu118", extra = "cu118"},
  { index = "torch-cu121", extra = "cu121"},
  { index = "torch-cu124", extra = "cu124"},
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[[tool.uv.index]]
name = "torch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[[tool.uv.index]]
name = "torch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

uv returns the following error:
```
error: Failed to generate package metadata for `example==0.1.0 @ editable+.`
  Caused by: Source entry for `torch` only applies to extra `cpu`, but the `cpu` extra does not exist. When an extra is present on a source (e.g., `extra = "cpu"`), the relevant package must be included in the `project.optional-dependencies` section for that extra (e.g., `project.optional-dependencies = { "cpu" = ["torch"] }`).
```

However, if I replace `[dependency-groups]` to `[project.optional-dependencies]` (as the error suggests) to include all `extra`s in optional dependencies, uv tells me that default groups must be mentioned in the dependency table group :
```
error: Default group `cu118` (from `tool.uv.default-groups`) is not defined in the project's `dependency-group` table
```

Am I missing something ?

**EDIT:** I added `tool.uv.conflicts` section but the results is the same.

---

_Comment by @zanieb on 2025-01-09 17:18_

In

```
conflicts = [
  [
    {extra = "cpu"},
    {extra = "cu118"},
    {extra = "cu121"},
    {extra = "cu124"},
  ],
]
```

these should be `group =` not `extra =`

---

_Comment by @sglbl on 2025-01-10 13:42_

> You can `cd ~ && uv python pin 3.10`

Sorry, it didn't work.

![Image](https://github.com/user-attachments/assets/5296663e-66fd-452f-8018-60d45f31d4f6)

---

_Comment by @maxstrobel on 2025-01-17 11:10_

~~~toml
[project]
name = "..."
version = "0.1.0"
description = "..."
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[dependency-groups]
cpu = ["torch>=2.5.1"]
cu118 = ["torch>=2.5.1"]


[tool.uv]
default-groups = ["cpu"]
conflicts = [[{ group = "cpu" }, { group = "cu118" }]]
[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", group = "cpu" },
    { index = "pytorch-cu118", group = "cu118" },
]
~~~

@sglbl - This works for me. It installs by default `cpu`. You can enable the CUDA installation via `uv sync --group cu118 --no-group cpu` - it's a bit complicated that you have to disable `cpu` with `--no-group cpu`, but that's the best I've come up so far.

@zanieb Is it possible to simplify this further, s.t. a `uv sync --group cu118` would be enough?

---

_Comment by @maxstrobel on 2025-01-17 14:13_

Now I found another issue.

Adding PyTorch standalone with the configuration in `pyproject.toml` shown above works.

However, if I add now a package that has PyTorch as a requirement, e.g. `transformers`, some entries in `uv.lock` will be overwritten and `uv sync` is no longer possible:

~~~bash
$ uv sync
Resolved 51 packages in 1ms
error: Distribution `torch==2.5.1 @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
~~~

Updated `pyproject.toml` (with `transformers` on `torch`):
~~~toml
[project]
name = "xxx"
version = "0.1.0"
description = "xxx"
readme = "README.md"
requires-python = ">=3.11"
dependencies = []

[dependency-groups]
cpu = ["torch>=2.5.1"]
cu118 = ["torch>=2.5.1"]


[tool.uv]
default-groups = ["cpu"]
conflicts = [[{ group = "cpu" }, { group = "cu118" }]]
[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true
[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", group = "cpu" },
    { index = "pytorch-cu118", group = "cu118" },
]
~~~



[uv-without-transformer.lock.txt](https://github.com/user-attachments/files/18455629/uv-without-transformer.lock.txt)
[uv-with-transformer.lock.txt](https://github.com/user-attachments/files/18455628/uv-with-transformer.lock.txt)

---

_Comment by @sglbl on 2025-01-17 14:22_

@maxstrobel That's why I use `extra` with conflicts instead of `group`. When I was using group I needed to remove uv.lock all the time. It's fine with `extra`. Only thing is missing is a default one.

---

_Comment by @maxstrobel on 2025-01-20 08:55_

@sglbl Thanks, I can confirm that this works also on my end.

@charliermarsh @zanieb What do you think, should I open a new issue for the conflict described above that comes with groups?

---

_Comment by @HichemAK on 2025-01-21 10:09_

> In
> 
> ```
> conflicts = [
>   [
>     {extra = "cpu"},
>     {extra = "cu118"},
>     {extra = "cu121"},
>     {extra = "cu124"},
>   ],
> ]
> ```
> 
> these should be `group =` not `extra =`

It works perfectly thanks 👌 

---

_Comment by @mcdiarmid on 2025-04-09 02:26_

> @zanieb Is it possible to simplify this further, s.t. a uv sync --group cu118 would be enough?

I'm interested in this too.  The ability to choose one of the groups in a defined conflict, rather than appending to the list of groups containing `default-groups`.  Maybe outside the scope of this issue though.  

Perhaps adding a parameter like `additional-group-behavior=[append|override]`, where default is the current behavior of `append`.

---
