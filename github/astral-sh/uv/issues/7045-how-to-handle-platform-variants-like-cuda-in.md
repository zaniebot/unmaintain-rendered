---
number: 7045
title: How to handle platform variants like cuda in workspaces
type: issue
state: open
author: PhilipVinc
labels:
  - question
  - needs-design
assignees: []
created_at: 2024-09-04T20:31:08Z
updated_at: 2025-06-30T18:12:07Z
url: https://github.com/astral-sh/uv/issues/7045
synced_at: 2026-01-07T13:12:17-06:00
---

# How to handle platform variants like cuda in workspaces

---

_Issue opened by @PhilipVinc on 2024-09-04 20:31_

Hi. I'm evaluating the use of uv, which looks amazing, for some of our data science needs.

We have a monorepo that we use for all our projects, which depends on `jax`. 
I would like to migrate it to an uv workspace. 

However the way we currently use it is that on our personal machines (Mac) we simply depend on jax, while when running on HPC computers we want to depend on `jax[cuda]`. 
I'm not sure how to handle this on uv's side. Adding `jax[cuda]` makes the project unresolvable on Mac. 
But simply adding `jax` is not enough on the other machines.

(To further complicate the situation, we sometimes run it on some AMD machines where we would like to depend on custom wheels from `https://github.com/ROCm/jax/releases/download/rocm-jaxlib-v0.4.30/jaxlib-0.4.30+rocm611-cp310-cp310-manylinux2014_x86_64.whl`, but we can set this aside for now).



---

_Comment by @charliermarsh on 2024-09-04 23:31_

Hi!

> Adding `jax[cuda]` makes the project unresolvable on Mac.

Do you mean: there's no valid resolution, or that it can't be resolved on a Mac machine, i.e., the project can't be built?


---

_Label `question` added by @charliermarsh on 2024-09-04 23:31_

---

_Comment by @plainerman on 2024-10-04 21:58_

I would like to chip in here because this is something I (and a lot of other data scientists/researchers) need. As @PhilipVinc already said, a typical use case is to install the jax cuda version (or pytorch cuda version or whatever) if cuda is available on the system, otherwise one wants to install the CPU version (usually a different package). This behavior is different whether you are on an x86 machine or Mac, so I would like to outline here what happens when you try to add the dependencies.

If you do uv add 'jax[cuda]' on:
1. An x86 system with cuda supported. Everything works fine and the package is installed.
2. On an x86 system with no cuda installed. It installs the jax cuda package just fine but all operations are executed on CPU. This is in principle okay, but the jax[cuda] package is quite large. So the best solution would be to install jax instead of jax[cuda]. 

3. On an apple silicon mac, the following happens:

```
error: distribution jax-cuda12-plugin==0.4.34 @ registry+https://pypi.org/simple can't be installed because it doesn't have a source distribution or wheel for the current platform
```
Meaning that the jax developers have not published a cuda wheel for mac (only jax). Which makes sense because cuda does not work on mac.

----

Meaning it behaves differently depending on the system. What one could do (but feels hacky) is to run `nvcc --version` and see if this returns 0. If cuda is installed, this program is installed with it.

I am not sure how one would include this check into the dependencies and there are for sure better ways to do this. But I am just not familiar enough with uv or python packaging in general. Any solution or workaround would be much appreciated.

If there is anything I can run or help with, please let me know. I am eager to find a solution (even if it is hacky for now)

---

_Comment by @plainerman on 2024-10-08 11:54_

A very rudimentary version would be to have the following dependencies based on [PEP508](https://peps.python.org/pep-0508/)

```toml
dependencies = [
    "jax; platform_system == 'Darwin'",
    "jax[cuda]; platform_system != 'Darwin'",
]
```

But this does not check if cuda is installed and available. So if you are not developing on a Mac, this does not add anything. 

Being able to run a command like `nvcc --version` could solve this problem. Any different ideas or suggestions?

---

_Label `needs-design` added by @zanieb on 2024-10-21 21:42_

---

_Comment by @aschleck on 2025-02-07 19:33_

Another related usecase: on Linux TPU machines we want `jax[tpu]` (which we sniff using `ps ax | grep '[t]pu.googleapis.com'`) whereas for the remainder of Linux machines we want `jax[cuda]`. I don't see any good way to deal with this at the moment

---

_Comment by @mstarodub on 2025-06-30 18:12_

> Another related usecase: on Linux TPU machines we want `jax[tpu]` (which we sniff using `ps ax | grep '[t]pu.googleapis.com'`) whereas for the remainder of Linux machines we want `jax[cuda]`. I don't see any good way to deal with this at the moment

maybe something like `"jax[tpu]; sys_platform != 'darwin' and 'gcp' in platform_release"`?
need something to identify the gcp machine from uname -a:
https://packaging.python.org/en/latest/specifications/dependency-specifiers/#environment-markers

---
