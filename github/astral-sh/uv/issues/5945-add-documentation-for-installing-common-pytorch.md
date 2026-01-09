---
number: 5945
title: Add documentation for installing common pytorch variants
type: issue
state: closed
author: zanieb
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2024-08-09T00:11:36Z
updated_at: 2024-12-17T03:53:51Z
url: https://github.com/astral-sh/uv/issues/5945
synced_at: 2026-01-07T13:12:17-06:00
---

# Add documentation for installing common pytorch variants

---

_Issue opened by @zanieb on 2024-08-09 00:11_

😭 but it must be done!

---

_Label `documentation` added by @zanieb on 2024-08-09 00:11_

---

_Comment by @nikhilweee on 2024-08-09 20:02_

I think the most common difficulty would be to specify the index url, but I'm glad `uv` makes it as easy as possible.

```shell
uv add torch --index-url https://download.pytorch.org/whl/cpu # within a project
uv pip install torch --index-url https://download.pytorch.org/whl/cpu # otherwise
```

---

_Comment by @FishAlchemist on 2024-08-21 17:21_

However, I've always used ``uv pip install`` for both PyTorch and TensorFlow, because on my Windows system, PyTorch uses CUDA 12.1 and TensorFlow includes [tensorflow-intel](https://pypi.org/project/tensorflow-intel/), both of which are platform-dependent.

---

_Comment by @baggiponte on 2024-08-21 18:59_

Available to help :) I am on ARM macOS and `uv add torch` recently has always been nice to me 😅 Regular version (w/o CUDA dependencies) are installed.

---

_Comment by @baggiponte on 2024-08-22 06:21_

Would also like to point out there's been this python package (recently recommended by @muellerzr, HuggingFace Accelerate maintainer) to install torch with automatic backend detection. Might be a good candidate for a first uv plugin, I guess?

---

_Referenced in [astral-sh/uv#6498](../../astral-sh/uv/issues/6498.md) on 2024-08-23 12:44_

---

_Label `help wanted` added by @zanieb on 2024-08-23 13:34_

---

_Referenced in [astral-sh/uv#6523](../../astral-sh/uv/pulls/6523.md) on 2024-08-23 14:54_

---

_Comment by @valentincalomme on 2024-08-23 15:30_

Sharing my thoughts to help with the discussion.

I feel that the biggest challenge here is that the source is platform dependent. On MacOs, PyPi is sufficient. But for Linux and Windows, if one wishes to install a cuda enabled version, they need to specify a URL.

I would personally already be helped if I could specify platform-dependent sources for packages.

Now the cuda/not cuda question is difficult. I think that theoretically, using optional dependencies can be a way to deal with this. If you want the cuda version (for any platform), install it as my_package[cuda]. If you only need the cpu version (i.e. Github actions), then install it as my_package[cpu].

But it's more of a flag then an optional dependency. It's either/or. So not sure how to deal with that. Maybe there can be "modes" for dependencies, with a default. And if the "cuda" mode is specified, dependencies are installed based on that.

---

_Comment by @zanieb on 2024-08-23 15:42_

> I would personally already be helped if I could specify platform-dependent sources for packages.

Regarding this.. https://github.com/astral-sh/uv/issues/3397

---

_Comment by @zanieb on 2024-09-11 13:27_

Example at https://github.com/astral-sh/uv/issues/7245#issuecomment-2340970983

---

_Comment by @dan-jacobson on 2024-09-21 22:40_

I've been banging my head against this for a couple days and can only report a list of things that don't work.

For reference, my goal is to install `torch` cpu-only, in two environments:
- my m2 macbook
- `linux/amd64` (so I can build my image in github ci and run on Cloud Run).

Here's what I've tried so far:

- `uv add torch` : doesn't work because torch defaults to installing a bunch of cuda on `linux/amd64`
- `uv add torch --extra-index-url https://download.pytorch.org/whl/cpu` doesn't work because `torch==2.4.1+cpu` doesn't exist for macos
-  `find-links = ["https://download.pytorch.org/whl/torch_stable.html"]` somehow downloads cuda stuff on my mac?? this also fails if declare `torch==2.4.1+cpu` because the `macos` whl's in that registry don't have the `+cpu` local version identifier. (edit: i did also have to downgrade to `torch==2.3.1` because 2.4.X isn't in the `torch_stable` registry)
- `dependencies = ["torch==2.4.1 ; sys_platform == 'darwin'", "torch==2.4.1+cpu ;  sys_platform != 'darwin'"]`, with `extra-index-url = "https://download.pytorch.org/whl/cpu"` in `[tool.uv]`.
-  `dependencies = ["torch @ https://download.pytorch.org/whl/cpu/torch-2.4.1-cp312-none-macosx_11_0_arm64.whl ; sys_platform == 'darwin' and platform_machine == 'arm64'", "torch==2.4.1+cpu ; sys_platform !='darwin'"]` again, with `extra-index-url` pointing to the torch cpu-only registry. i was pretty surprised this one didn't work, since i thought i was yoinking from @charliermarsh in [this comment](https://github.com/astral-sh/uv/issues/7245#issuecomment-2340836442).

I've also tried all variations of the above with `index-strategy = "unsafe-best-match"`, just to make sure that it's not a registry order problem.

anyways, i'm sure there's a way. i just haven't found it yet. i'll keep plugging away. i think my next idea is to define the `macos` as a `dev-dependency`, although that feels a little bit hacky.

---

_Comment by @charliermarsh on 2024-09-21 23:08_

Thanks, that’s a good concrete example. I’ll take a look at it (not tonight, but soon) and report back.

---

_Comment by @dan-jacobson on 2024-09-22 00:06_

Update: it seems that the last version I tried, `dependencies = ["torch @ whatever ; sys_platform == 'darwin' and platform_machine == 'arm64'", "torch @ whatever+cpu ; sys_platform !='darwin'"]` DOES work if I'm just careful about the paths. Also it works if I relax to just the local version identifier.

For anyone trying this in the future, here's the specific `pyproject.toml` that worked for me:

```
dependencies = [
    "torch==2.4.1 ; sys_platform == 'darwin' and platform_machine == 'arm64'",
    "torch==2.4.1+cpu ; sys_platform !='darwin'",
]

[tool.uv]
index-strategy = "unsafe-best-match"
extra-index-url = ["https://download.pytorch.org/whl/cpu"]
```

Edit: note that `index-strategy = "unsafe-best-match"` isn't strictly necessary for just `torch`, but some of the other deps in the `https://download.pytorch.org/whl/cpu` registry are behind their `pypi` equivalents. `pillow`, for example. so it was necessary to either 1) downgrade my `pillow` version to the one in the `pytorch/cpu` registry, or 2) use `unsafe-best-match`.

Edit 2: looking back at my logs, the reason I hadn't gotten this approach to work before was actually because I hadn't wrapped `extra-index-url` in a `List` -- I'd just been passing it as a string. This broke the TOML parsing, but the proximate error I was seeing was: 
```
No solution found when resolving dependencies for split (sys_platform != 'darwin'):
  ╰─▶ Because there is no version of torch{sys_platform != 'darwin'}==2.4.1+cpu and your project depends on torch{sys_platform != 'darwin'}==2.4.1+cpu, we can
      conclude that your project's requirements are unsatisfiable.
```
And I had concluded I was writing my `dependencies` wrong. So, whoops, skill issue for not reading the full stacktrace.

---

_Comment by @charliermarsh on 2024-09-23 19:21_

> And I had concluded I was writing my dependencies wrong. So, whoops, skill issue for not reading the full stacktrace.

We had a few releases where we dropped this error on the floor, so it might not have even been visible to you sadly.

---

_Comment by @charliermarsh on 2024-09-23 19:21_

I do think that what you ended up with is right, though:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12"
dependencies = ["torch==2.4.1 ; sys_platform == 'darwin'", "torch==2.4.1+cpu ;  sys_platform != 'darwin'"]

[tool.uv]
index-strategy = "unsafe-best-match"
extra-index-url = ["https://download.pytorch.org/whl/cpu"]
```

---

_Comment by @charliermarsh on 2024-09-23 19:27_

Does anyone know _why_ PyTorch has some wheels with `+cpu` and some without here? This is on the `https://download.pytorch.org/whl/cpu` index.

![Screenshot 2024-09-23 at 3 26 14 PM](https://github.com/user-attachments/assets/4de1dc89-edd0-4390-9034-fb86ce350657)


---

_Comment by @dan-jacobson on 2024-09-28 18:27_

Honestly, I'm not entirely sure -- my vague understanding is that the wheels that *could* have CUDA support have explicit `+cpu` versions, but the ones without CUDA support are implicitly `cpu-only`. Either way, quite frustrating.

One other thing, as I've continued to work on this project: using the above `pyproject.toml`, I can't `uv add torchvision` without automatically getting `torchvision+cpu`. Not entirely sure why. What I'm seeing is:
```
uv add torchvision
error: distribution torchvision==0.19.1+cpu @ registry+https://download.pytorch.org/whl/cpu can't be installed because it doesn't have a source distribution or wheel for the current platform
```
This is on my macbook, where I would have expected it to resolve to just `torchvision`, sans `+cpu`.

However, if I manually add `torchvision==0.19.1` to my `pyproject.toml` and `uv sync`, everything behaves as expected.

---

_Referenced in [astral-sh/uv#7859](../../astral-sh/uv/issues/7859.md) on 2024-10-02 14:56_

---

_Comment by @Leon0402 on 2024-10-18 16:57_

Variant 1:

```
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

```
⠼ lit==15.0.7                                                                                                                          × Failed to download and build `lit==15.0.7`
  ├─▶ Failed to resolve requirements from `build-system.requires`
  ├─▶ No solution found when resolving: `setuptools>=40.8.0`, `wheel`
  ╰─▶ Because wheel was not found in the package registry and you require wheel, we can conclude that your requirements are
      unsatisfiable.

      hint: An index URL (https://download.pytorch.org/whl/cu118) could not be queried due to a lack of valid authentication
      credentials (403 Forbidden).
```

Variant 2:

```
uv add torch torchvision torchaudio --default-index https://download.pytorch.org/whl/cu118
```

```
error: Distribution `torchaudio==2.0.0 @ registry+https://download.pytorch.org/whl/cu118` can't be installed because it doesn't have a source distribution or wheel for the current platform
````

Any idea why neither is working? 

---

_Comment by @FishAlchemist on 2024-10-18 17:11_

@Leon0402 I think this isn't a UV issue. I accessed it a few minutes ago through the browser and it didn't have the correct content, but it seems to be working now. Do you want to try it again?

The result of my visit a few minutes ago:
![image](https://github.com/user-attachments/assets/6d104d0d-63ea-48c0-a463-8bdc13b3256d)


---

_Comment by @Leon0402 on 2024-10-18 17:35_

@FishAlchemist Issue persists. I also tried `cu121`, but same issue. I am running the right commands though, right? I am new to uv, so maybe I did not use it correctly. 
I also just created a new fresh project with uv init and then above command, but same issue.

---

_Comment by @FishAlchemist on 2024-10-18 17:42_

@Leon0402 I am unable to replicate this issue on my machine. Would you consider filing a separate issue and providing more specific details based on the prompts?

---

_Comment by @Leon0402 on 2024-10-18 18:05_

> @Leon0402 I am unable to replicate this issue on my machine. Would you consider filing a separate issue and providing more specific details based on the prompts?

Sure: https://github.com/astral-sh/uv/issues/8344, Let me know what other output you need there.

---

_Comment by @fzyzcjy on 2024-10-19 12:24_

Hi I am facing problem in macos for pytorch cpu installation (https://github.com/astral-sh/uv/issues/8358), is there any suggestions? Thanks!

---

_Comment by @amiralik16 on 2024-10-21 17:43_

Hi UV team. First let me thank you for all the work you're doing on the project. I've just started using it and it is FAST and pretty intuitive (especially coming from poetry).

For my use case, I'm trying to generate a lock file that would install either a cpu only or gpu enabled version of pytorch when on a linux platform and just regular pypi pytorch when using a mac.
According to pytorch, one needs to point to different indexes:
1. Linux and cpu: https://download.pytorch.org/whl/cpu
2. Linux + cuda 11.8 + gpu: https://download.pytorch.org/whl/cu118
3. mac: pypi
I have the following pyproject.toml (the idea is I'd be able to do uv sync --extra torch-gpu/torch-cpu) that would install the gpu/cpu version on a linux platform.
```
[project]
name = "uv"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10"
dependencies = [
    # bunch of irrelevant deps
]

[project.optional-dependencies]
torch-gpu = [
    "torch==2.4.1+cu118;sys_platform == 'linux' and extra == 'torch-gpu' and extra != 'torch-cpu'", 
    "torch==2.4.1;sys_platform == 'darwin'",
    "torchmetrics>=1.4.3",
]
torch-cpu = [
    "torch==2.4.1+cpu;sys_platform == 'linux' and extra != 'torch-gpu' and extra == 'torch-cpu'", 
    "torch==2.4.1;sys_platform == 'darwin'",
    "torchmetrics>=1.4.3",
]
classic = [
    "lightgbm>=4.5.0",
    "scikit-learn>=1.5.2",
    "xgboost>=2.1.1",
]


[tool.uv.sources]
torch = [
  { index = "torch-cpu", marker = "sys_platform == 'linux' and extra != 'torch-gpu' and extra == 'torch-cpu'"},
  { index = "torch-cu118", marker = "sys_platform == 'linux' and extra == 'torch-gpu' and extra != 'torch-cpu'"},
]

[[tool.uv.index]]
name = "torch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```
However, I am getting the below error. I'm not sure where the torch{sys_platform == 'linux' and extra != 'torch-gpu'}is coming from since my expectation is that torch2.4.1+cpu is ONLY installed if extra torch-cpu is requested and extra torch-gpu isn't requested.
```
 × No solution found when resolving dependencies for split (sys_platform != 'darwin'):
  ╰─▶ Because there is no version of torch{sys_platform == 'linux' and extra != 'torch-gpu'}==2.4.1+cpu and uv[torch-cpu] depends on torch{sys_platform == 'linux' and extra
      != 'torch-gpu'}==2.4.1+cpu, we can conclude that uv[torch-cpu]'s requirements are unsatisfiable.
      And because your project requires uv[torch-cpu], we can conclude that your projects's requirements are unsatisfiable.
```
I'm mainly looking for any suggestions on how to achieve the end goal and if anyone from the UV team can help me understand why torch2.4.1+cpu is being considered for torch{sys_platform == 'linux' and extra != 'torch-gpu'}

---

_Comment by @laxas on 2024-11-08 11:17_

I got the same promlem but  

```bash
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
``` 
works.

It gets uninstalled after every `uv sync`, but my workaround is
 
```bash
uv sync && uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
``` 
That looks stupid but is, do to caching, realy fast.




---

_Closed by @charliermarsh on 2024-12-17 03:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-17 03:53_

---

_Referenced in [chronicler-ai/chronicle#124](../../chronicler-ai/chronicle/pulls/124.md) on 2025-10-14 04:57_

---
