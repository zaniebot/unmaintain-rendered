---
number: 6880
title: Pytorch downgrades to a lower version
type: issue
state: open
author: thepowerfuldeez
labels:
  - question
assignees: []
created_at: 2024-08-30T16:40:47Z
updated_at: 2025-01-13T10:00:53Z
url: https://github.com/astral-sh/uv/issues/6880
synced_at: 2026-01-10T01:24:07Z
---

# Pytorch downgrades to a lower version

---

_Issue opened by @thepowerfuldeez on 2024-08-30 16:40_

Hi! Is there functionality to ignore dependencies if they are already exist in the system python?
I am using `nvcr.io/nvidia/pytorch:24.08-py3` docker image which has `pytorch==2.5.0a0+872d972e41.nv24.08` installed

I followed guide from [here](https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers) to enable system python via `UV_SYSTEM_PYTHON=1` and I have `pyproject.toml` that includes packages that I've put from requirements.txt and made `uv lock`.

But now I see that it's been resolved with `torch==2.4.0` and when I run `uv sync` my pytorch gets downgraded to `2.4.0` which is unexpected.
I have tried to manually remove all pytorch dependencies from lockfile and run `uv sync` from that state, but for some reason now it suceeds with installing only 4 of 126 packages (and they are just file packages `from file://...` defined in pyproject.toml)
I've tried using `uv sync --frozen` `uv sync --locked` and tried creating `uv sync` with different variations but haven't suceeded yet. To be frank `pip install -r requirements.txt` would also downgrade my `torch` unless I make `pip freeze > requirements.txt` and install with `-r --no-deps` but this is not favorable.

I generally lack understanding of such intricacies and I've noticed that other people stumble with flash-attn installation or CUDA extensions. I wonder what's the best way to handle such conflicts?
Thank you very much for modern replacement of pip!

---

_Comment by @thepowerfuldeez on 2024-08-30 17:08_

Example of downgrading torch (and installing cuda extensions which are not needed)
```
> uv sync
Resolved 127 packages in 567ms
⠧ Preparing packages... (5/13)
nvidia-cufft-cu12 ------------------------------ 58.53 MB/121.64 MB
nvidia-cusolver-cu12 ------------------------------ 58.41 MB/124.16 MB
nvidia-nccl-cu12 ------------------------------ 58.49 MB/176.25 MB
nvidia-cusparse-cu12 ------------------------------ 58.55 MB/195.96 MB
nvidia-cublas-cu12 ------------------------------ 58.29 MB/410.59 MB
nvidia-cudnn-cu12 ------------------------------ 58.35 MB/664.75 MB
torch      ------------------------------ 33.03 MB/797.23 MB
^C```

---

_Comment by @charliermarsh on 2024-09-01 15:54_

This doesn't work today with `uv sync` or `uv lock` -- it won't respect existing environments. The lower-level `uv pip` APIs do, though. You can run `uv pip install` with an active virtual environment, and it will retain existing versions of PyTorch, if they're already installed.

---

_Label `question` added by @charliermarsh on 2024-09-01 15:54_

---

_Comment by @thepowerfuldeez on 2024-09-01 17:31_

@charliermarsh how do I respect pyproject.toml and use project setup features? I thought of switching to uv as a project management tool (similar to poetry or rye if that matter). Will `uv pip install -r pyproject.toml` work?
What's the proposed approach for project management for ML workflows? This is highly requested imo, would glad to have clear guide :)

---

_Comment by @charliermarsh on 2024-09-01 17:43_

Yeah `uv pip install -r pyproject.toml` should work just fine. In general, our goal is for folks to use the "higher-level" project APIs (`uv lock`, `uv sync`, etc.). But in this case, if you're using an nvidia image that has packages pre-installed, it won't play correctly with `uv lock` and `uv sync`. So if you want to build atop that base environment, you'll need to use the "lower-level" `uv pip` APIs.

E.g., activate that virtual environment, then run `uv pip install -r pyproject.toml`.

---

_Comment by @thepowerfuldeez on 2024-09-03 09:16_

@charliermarsh This doesn't work
I have tried
```
> uv venv --python-preference only-system --system-site-packages
source .venv/bin/activate
uv pip install -r pyproject.toml
```

It still installs torch==2.4.0 even though it's not in the dependencies of pyproject.toml

I verified that inside this venv torch is installed

---

_Comment by @thepowerfuldeez on 2024-09-03 09:27_

@charliermarsh I have tried to set

```
[tool.uv]
# Always install torch 2.5, regardless of whether transitive dependencies request
# a different version.
override-dependencies = ["torch==2.5.0a0+872d972e41.nv24.08"]
```

but it can't be resolved
```
× No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.5.0a0+872d972e41.nv24.8 and accelerate==0.33.0 depends on torch==2.5.0a0+872d972e41.nv24.8, we can conclude that accelerate==0.33.0 cannot be used.
```

---

_Comment by @charliermarsh on 2024-09-03 12:46_

@thepowerfuldeez -- If you're trying to use packages from the system Python, you'll need to use `uv pip install --system -r pyproject.toml`, and avoid creating a virtual environment at all. Can you try that?

---

_Comment by @thepowerfuldeez on 2024-09-03 14:09_

@charliermarsh Thank you so much! It works now! So the solution is not using lockfile for now, right?

---

_Comment by @charliermarsh on 2024-09-03 14:11_

Unfortunately yes. We'll need to think on how to solve this properly.

---

_Comment by @charliermarsh on 2024-09-03 14:12_

I'm gonna leave the issue open but might tweak the title and add some more details on the underlying problem, if that's ok.

---

_Comment by @awoimbee on 2024-09-04 09:41_

I had the same issue with poetry some time ago: https://github.com/python-poetry/poetry/issues/6035 (resolved by https://github.com/python-poetry/poetry/pull/8359).

Note `uv venv --system-site-packages` exists since #2101, but "we won't take the system site packages into account in subsequent commands".

---

_Comment by @zanieb on 2024-10-21 21:40_

@charliermarsh it seems like there might be a real issue to track here?

---

_Comment by @charliermarsh on 2024-10-21 23:21_

Yeah. The system packages aren't taken into account.

---

_Referenced in [astral-sh/uv#4466](../../astral-sh/uv/issues/4466.md) on 2024-11-21 11:32_

---

_Comment by @zanieb on 2025-01-07 20:13_

I think this is related to

- #4466
- https://github.com/astral-sh/uv/pull/9849

Do we need to re-triage this or can we close in favor of the other issue?

---

_Comment by @charliermarsh on 2025-01-07 20:32_

I think it's a bit different... We don't even provide a way for users to tell us to look at already-installed packages, much less system packages. Whereas #4466 is about respecting `system-site-packags`.

---

_Comment by @zanieb on 2025-01-07 21:59_

@charliermarsh Could you open an issue that describes what change would be needed for this issue? (even if it's brief)


---

_Comment by @thepowerfuldeez on 2025-01-08 10:13_

@zanieb would be great to have a solution for using existing docker images from nvidia (as it contains cuda, cudnn, flash_attn, triton, bitsandbytes, nccl, latest pytorch where some of such libraries need to be compiled from source) and still using lockfile with high level uv sync interface that would install dependencies on top of the existing system packages.
Something like `UV_SYSTEM_PYTHON=1 uv sync --respect-system-packages` would work

---

_Comment by @mlgill on 2025-01-09 13:09_

@zanieb +1 to the suggestion by @thepowerfuldeez. Multiple software teams that I work with have this problem with NVIDIA containers, in particular the PyTorch one. As @thepowerfuldeez notes, these containers contain libraries that are compiled from source to ensure they are optimized for GPUs. Thus, installing a pip version of the same library should be avoided.

---

_Comment by @zanieb on 2025-01-09 14:56_

Yeah we want to solve that problem, but it's not trivial.

---

_Comment by @pstjohn on 2025-01-11 20:21_

Just to chime in with a specific use case, we use `uv pip install` in our `nvcr.io/nvidia/pytorch` derived image ([link](https://github.com/NVIDIA/bionemo-framework/blob/20f4937b6ddc8399b6c2f1c9a71ab8d8099a5500/Dockerfile#L124-L139)), but it would be great to be able to do something similar to https://docs.astral.sh/uv/guides/integration/docker/#intermediate-layers, where we could create a uv.lock file (maybe excluding the system packages), and then run `uv sync --frozen --no-install-workspace` in our docker image to improve caching. 

---

_Comment by @awoimbee on 2025-01-13 10:00_

Just to help ppl working with containers, here's the solution I found some time ago:
I use use `FROM docker.io/pytorch/pytorch` and I just give the conda env to uv via `UV_PROJECT_ENVIRONMENT=/opt/conda`.
`uv` has its venv with torch so it's happy and doesn't reinstall it. 

---
