```yaml
number: 13959
title: "How to install `flash-attn` and `transformer-engine[pytorch]` using `uv sync` in a clean environment (e.g., Docker)"
type: issue
state: closed
author: pplmx
labels:
  - question
assignees: []
created_at: 2025-06-11T02:50:33Z
updated_at: 2025-08-19T08:39:30Z
url: https://github.com/astral-sh/uv/issues/13959
synced_at: 2026-01-10T03:32:45Z
```

# How to install `flash-attn` and `transformer-engine[pytorch]` using `uv sync` in a clean environment (e.g., Docker)

---

_Issue opened by @pplmx on 2025-06-11 02:50_

### Question

Here is my trial:

```toml
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "megatron-core==0.11.0; sys_platform == 'linux'",
    "numpy>=2.2.3",
    "torch>=2.6.0",
    "flash-attn",  
    "transformer-engine[pytorch]",  
]

[tool.uv]
no-build-isolation-package = ["transformer-engine-torch", "flash-attn"]

[[tool.uv.dependency-metadata]]
name = "flash-attn"
requires-dist = ["torch", "einops"]

[[tool.uv.dependency-metadata]]
name = "transformer-engine-torch"
requires-dist = ["setuptools"]
```

### Platform

Linux 5.4.0-42-generic x86_64 GNU/Linux

### Version

uv 0.7.8

---

_Label `question` added by @pplmx on 2025-06-11 02:50_

---

_Comment by @charliermarsh on 2025-06-11 02:55_

Did you read https://docs.astral.sh/uv/concepts/projects/config/#build-isolation? There is a `flash-attn` example in there.

---

_Comment by @pplmx on 2025-06-11 03:06_

> Did you read [docs.astral.sh/uv/concepts/projects/config#build-isolation](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation)? There is a `flash-attn` example in there.

Yes, I tried. But I couldn’t install `flash-attn` with `uv sync` on the first attempt.
I had to comment out `flash-attn` and `transformer-engine[pytorch]`, run `uv sync` to install the rest, and then uncomment them and run `uv sync` again.

---

_Comment by @pplmx on 2025-06-11 03:20_

First attempt, it will throw

```log
Resolved 110 packages in 2ms
  x Failed to build `flash-attn==2.7.4.post1`
  |-> The build backend returned an error
  `-> Call to `setuptools.build_meta:__legacy__.build_wheel` failed (exit status: 1)

      [stderr]
      Traceback (most recent call last):
        File "<string>", line 8, in <module>
      ModuleNotFoundError: No module named 'setuptools'

      hint: This usually indicates a problem with the package or the build environment.
  help: `flash-attn` (v2.7.4.post1) was included because `test` (v0.1.0) depends on `flash-attn`
```

---

_Comment by @pplmx on 2025-06-11 04:17_

By the way, the issue only occurs in a truly clean environment.
Even for a brand new project with the same `pyproject.toml`, `uv sync` works fine if the required packages have already been installed before (e.g., in the same container).
That's why using a clean container is necessary to reliably reproduce the problem.

---

_Comment by @piyifan123 on 2025-06-16 23:04_

Also observing this problem and the guide [docs.astral.sh/uv/concepts/projects/config#build-isolation](https://docs.astral.sh/uv/concepts/projects/config/#build-isolation) is not very clear to see how it can be configured to play with https://docs.astral.sh/uv/guides/integration/pytorch/ (i.e., multiple torch backends). 

It would be nice to update the pytorch integration doc to include how to setup flash_attn as well since it's almost always being configured together with pytorch. 

Thanks!

---

_Comment by @marcelroed on 2025-07-31 22:08_

It's possible to do `uv sync --no-install-package flash-attn --no-install-package transformer-engine` first, then to do `uv sync`, but this is extremely annoying when all I want to do is run `uv run` and have my environment solve itself. This shows up a lot in my SLURM scripts, where I need to this sort of thing in every single launch script for things that use Huggingface (most ML experiments).

```
srun --nodes=$SLURM_JOB_NUM_NODES \
     --ntasks=$SLURM_JOB_NUM_NODES \
     --ntasks-per-node=1 \ 
     uv sync --no-install-package flash-attn
```


@charliermarsh is there a way this could be a part of the `tool.uv` config? I'm thinking something like

```
[tool.uv]
no-build-isolation-package = ["flash-attn"]
install-last-packages = ["flash-attn"]  # maybe these are installed in order?
```

---

_Comment by @charliermarsh on 2025-08-02 15:36_

> I had to comment out flash-attn and transformer-engine[pytorch], run uv sync to install the rest, and then uncomment them and run uv sync again.

I think this sounds right at the moment, though. You have to do this two-phase install right now (and the docs suggest the same, IIRC?). We have some work in-progress right now that will make it possible to do this in a single command though.

---

_Comment by @marcelroed on 2025-08-02 20:35_

What is the proposed solution? I'm really looking forward to this!

---

_Comment by @charliermarsh on 2025-08-02 20:49_

We now support declaring "extra" build dependencies that you can include in the environment for a given package. So you can declare `torch` as an "extra" build dependency for `flash-attn`. Then we'll be adding an option to ensure that the same version of `torch` is used in the build environment as will be used at runtime (https://github.com/astral-sh/uv/pull/14944), but that PR hasn't merged yet -- we need to make a few improvements to the caching behavior first. The net effect will be that you can say, "`flash-attn` needs to have access to `torch`, and it needs to be the same version of `torch` as in the lockfile", which will let `flash-attn` pull in the right pre-built wheel from GitHub (as per its `setup.py` behavior). So, in short, you can add that small bit of configuration to your `pyproject.toml` and `uv sync` will "just work" without multiple steps.

---

_Comment by @marcelroed on 2025-08-03 18:31_

Awesome, this is incredible work! For now, I got this working by specifying a narrow enough torch version that they match up:

```toml
[tool.uv.extra-build-dependencies]
# Will require these extra packages when building flash-attn.
# It's important that the torch version matches the one used by this package.
# Consider using == for an exact match.
flash-attn = ["setuptools>=80.9.0", "torch>=2.6,<2.7"]
```

With this I can just do a single `uv sync`!

---

_Comment by @charliermarsh on 2025-08-03 21:20_

Oh yeah, that should also work in the meantime as long as you constrain the torch versions like you’ve done there :)

---

_Comment by @charliermarsh on 2025-08-16 16:04_

I believe in the next release you should just be able to do:
```toml
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    "megatron-core==0.11.0; sys_platform == 'linux'",
    "numpy>=2.2.3",
    "torch>=2.6.0",
    "flash-attn",
    "transformer-engine[pytorch]",  
]

[tool.uv]
no-build-isolation-package = ["transformer-engine-torch", "flash-attn"]
```

This should work with a single `uv sync`.

---

_Comment by @charliermarsh on 2025-08-16 16:43_

Even better (and this should work in the latest, existing release -- a single `uv sync`, no need for build isolation):

```toml
[project]
name = "test"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
    "megatron-core==0.11.0; sys_platform == 'linux'",
    "numpy>=2.2.3",
    "torch>=2.6.0",
    "flash-attn",
    "transformer-engine[pytorch]",  
]

[tool.uv.extra-build-dependencies]
flash-attn = [{ requirement = "torch", match-runtime = true}]
transformer-engine-torch = [{ requirement = "torch", match-runtime = true}]
```

I just confirmed this on a Linux machine.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-16 16:43_

---

_Closed by @zanieb on 2025-08-18 21:15_

---

_Comment by @marcelroed on 2025-08-18 21:31_

Thanks so much for this @charliermarsh and @zanieb! 

---

_Comment by @pplmx on 2025-08-19 08:39_

> I believe in the next release you should just be able to do:
> 
> [project]
> name = "test"
> version = "0.1.0"
> description = "Add your description here"
> readme = "README.md"
> requires-python = ">=3.10"
> dependencies = [
>     "megatron-core==0.11.0; sys_platform == 'linux'",
>     "numpy>=2.2.3",
>     "torch>=2.6.0",
>     "flash-attn",
>     "transformer-engine[pytorch]",  
> ]
> 
> [tool.uv]
> no-build-isolation-package = ["transformer-engine-torch", "flash-attn"]
> This should work with a single `uv sync`.


Thanks for the fix, @charliermarsh @zanieb ! I tested it with `v0.8.12` and it's working correctly now.


---
