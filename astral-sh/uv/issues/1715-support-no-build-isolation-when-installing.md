---
number: 1715
title: "Support \"--no-build-isolation\" when installing packages"
type: issue
state: closed
author: Taytay
labels:
  - enhancement
  - compatibility
  - installer
assignees: []
created_at: 2024-02-19T19:58:58Z
updated_at: 2025-05-19T17:56:38Z
url: https://github.com/astral-sh/uv/issues/1715
synced_at: 2026-01-10T01:23:08Z
---

# Support "--no-build-isolation" when installing packages

---

_Issue opened by @Taytay on 2024-02-19 19:58_

`uv --version`: `0.1.5`

This is an official issue to track a separate issue discovered in #1582. It was requested to be tracked separately here: https://github.com/astral-sh/uv/issues/1582#issuecomment-1950984546

## Short version:
Without `--no-build-isolation`, many popular ML libraries, including `flash-attn` can't be `pip installed`.
I want to be able to do this:

`uv pip install flash-attn --no-build-isolation`

But I can't.

If `uv pip install` doesn't support this, I don't think that it will support installing some popular ML and Deep Learning python modules.

### Disclaimer
I just discovered this stuff in about 1 hour of reading, and my python-dependency knowledge is pretty poor. I _think_ this is a good description of the issue, but know that this is NOT coming from an expert.

## Long version
The "newer" versions of pip enable build-isolation by default when modules are being installed. (BIG discussion here for those interested in the history: https://github.com/pypa/pip/issues/8437)
This is generally pretty nice, because it allows a python module to request a newer version of `setuptools` or something at install time, without polluting a user's global environment by permanently installing an undesired newer version of a module.
The downside is that it hides all existing installed modules from the module's setup.py script.
The typical solution to this is to tell `pyproject.toml` about the modules required at setup time. It will install them in the isolated environment, and then proceed with the installation.

There is a bigger downside though:

A number of machine learning-related modules [Flash-attn](https://github.com/Dao-AILab/flash-attention), [NVidia Apex](https://github.com/NVIDIA/apex) need pytorch to be installed before they are installed, so that they can do things like compile against version of pytorch currently installed in the user's environment. 

You might think that these modules should declare that they depend upon Pytorch at setup time in their pyproject.toml file, but that isn't a good solution. No one wants a newly installed Pytorch version, and they might have installed pytorch using Conda or some other mechanism that pip isn't aware of. And these modules want to just work with whatever version of pytorch is already there. It would cause a LOT more problems than it would solve.

So, modules like these simply instruct users to disable this build isolation by running `pip install` with `--no-build-isolation`. Problem solved! But `uv` doesn't support this. (As discovered in https://github.com/astral-sh/uv/issues/1582).

(This issue _generally_ manifests as an error when setup.py can't being find the `packaging` module, and users being confused because they already ran `pip install packaging`. See: https://github.com/Dao-AILab/flash-attention/issues/453 
That's exactly the error I get when I `uv pip install flash-attn`: `ModuleNotFoundError: No module named 'packaging'`


---

_Referenced in [astral-sh/uv#1582](../../astral-sh/uv/issues/1582.md) on 2024-02-19 19:59_

---

_Referenced in [Dao-AILab/flash-attention#833](../../Dao-AILab/flash-attention/issues/833.md) on 2024-02-19 20:17_

---

_Label `enhancement` added by @zanieb on 2024-02-19 21:00_

---

_Label `compatibility` added by @zanieb on 2024-02-19 21:00_

---

_Label `installer` added by @zanieb on 2024-02-19 21:00_

---

_Comment by @ilkersigirci on 2024-02-20 19:16_

I hope this is fixed soon since `flash-attn` is the barebone for my ML projects :(

---

_Assigned to @charliermarsh by @charliermarsh on 2024-02-20 19:17_

---

_Comment by @charliermarsh on 2024-02-20 19:30_

We'll support it. Assigning to myself.

---

_Comment by @gaborbernat on 2024-02-20 19:32_

> So, modules like these simply instruct users to disable this build isolation by running `pip install` with `--no-build-isolation`. Problem solved!

I'd call this a workaround, not a problem solved... and note this approach would not be endorsed by the PyPA. I wonder though why these tools can't do ensure pytorch is installed inside the build wheel isolated environment, and they _must_ have access to the target Python instead.

---

_Comment by @hmc-cs-mdrissi on 2024-02-20 21:57_

Numpy is more foundational library with similar interesting workarounds (oldest-supported-numpy). What build isolated environment has doesn't really matter because today there's no way to say that build environment and runtime environment for library must be same. These libraries care about runtime version of pytorch that will be used. They may support older pytorch versions, but build process can do optimizations specific to actual pytorch used at runtime. The pytorch present at build time does not matter to them. So build isolation here is in direct conflict with this use case.

A packaging standard here would ideally be a way for specific dependencies to specify that their build vs runtime version are same. Most dependencies isolation is fine, only rare few that are sensitive to this. 

edit: If your intent is access to target python to dynamically inspect runtime pytorch that may work, but also seems like violation of intent of build isolation and would still leave a separate pytorch in build environment as not needed.

---

_Comment by @hauntsaninja on 2024-02-20 22:51_

Yeah, +1 to the problem mdrissi mentions. `--no-build-isolation` is really useful to allow us to ensure that a build of PyTorch in the build environment is the exact same build that is in the runtime environment. Without this you get linker issues.

The PyPI ecosystem doesn't really work well for this kind of thing. We have custom logic that wraps and handles all of this stuff at work (e.g. it uses things like https://github.com/astral-sh/uv/issues/1415 as an escape hatch). Although note we don't actually use `pip install --no-build-isolation`, we do `python -m build --no-isolation` since we prebuild all this stuff. Our custom logic actually gets quite interesting, if you're curious I'd be happy to talk more about it when y'all are less swamped.

---

_Comment by @konstin on 2024-02-22 11:27_

If we could guarantee that we install the install the same pytorch version in the build env as in the target env (assuming we manage the target env torch version) and we reflink/hardlink so installing is fast and doesn't take disk space, would that work for you? I don't think we can cover all use cases that `--no-build-isolation` supports, but i'd like to support isolation for as many cases as possible

---

_Referenced in [drivendataorg/zamba#311](../../drivendataorg/zamba/pulls/311.md) on 2024-03-02 01:10_

---

_Comment by @marcelroed on 2024-03-04 18:57_

> I hope this is fixed soon since `flash-attn` is the barebone for my ML projects :(

This is the only issue keeping me from using uv for everything.

---

_Referenced in [astral-sh/uv#2258](../../astral-sh/uv/pulls/2258.md) on 2024-03-07 02:19_

---

_Comment by @charliermarsh on 2024-03-07 02:34_

PR is up: https://github.com/astral-sh/uv/pull/2258

---

_Comment by @marcelroed on 2024-03-07 02:38_

Wow, thanks @charliermarsh! Will test this on `flash-attn` as soon as it lands in main.

---

_Closed by @charliermarsh on 2024-03-07 14:04_

---

_Comment by @charliermarsh on 2024-03-07 23:51_

This just went out in v0.1.16. Feedback welcome as always.

---

_Referenced in [astral-sh/rye#552](../../astral-sh/rye/issues/552.md) on 2024-03-11 11:06_

---

_Referenced in [AutoGPTQ/AutoGPTQ#535](../../AutoGPTQ/AutoGPTQ/issues/535.md) on 2024-03-28 10:05_

---

_Comment by @shiyangcao on 2024-04-24 19:07_

Hi, 

I'm finding that --no-build-isolation makes it so that nothing in the pyproject.toml is processed.

For example, we have this pyproject.toml

```
  1
  2 [build-system]
  3 requires = [ "setuptools>=41", "wheel", "setuptools-git-versioning<2", "packaging"]
  4 build-backend = "setuptools.build_meta"
  5
  6 [tool.setuptools-git-versioning]
  7 enabled = true
  8 dev_template = "{tag}.{ccount}+g{sha}"
  9
```

When we use `uv pip install -e .` the version is something like 1.2.3. However, when we use --no-build-isolation, it goes to `0.0.0`

Is there a way to assume that the dependencies are already installed, but still run setuptools-git-versioning etc?

Thanks


---

_Referenced in [ModelCloud/GPTQModel#108](../../ModelCloud/GPTQModel/issues/108.md) on 2024-06-28 11:47_

---

_Referenced in [NVIDIA/TransformerEngine#981](../../NVIDIA/TransformerEngine/pulls/981.md) on 2024-07-03 22:50_

---

_Comment by @hcoona on 2024-08-14 04:42_

May I know how to install `flash-attn` with uvx?

I tried `uvx --with torch --with flash-attn --no-build-isolation insanely-fast-whisper` but got the error: (uv 0.2.36)

```
warning: `uvx` is experimental and may change without warning
⠙ nvidia-cusparse-cu12==12.1.0.106                                                                                      error: Failed to download and build `flash-attn==2.6.3`
  Caused by: Failed to build: `flash-attn==2.6.3`
  Caused by: Build backend failed to determine metadata through `prepare_metadata_for_build_wheel` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/usr/lib/python3/dist-packages/setuptools/build_meta.py", line 174, in prepare_metadata_for_build_wheel
    self.run_setup()
  File "/usr/lib/python3/dist-packages/setuptools/build_meta.py", line 267, in run_setup
    super(_BuildMetaLegacyBackend,
  File "/usr/lib/python3/dist-packages/setuptools/build_meta.py", line 158, in run_setup
    exec(compile(code, __file__, 'exec'), locals())
  File "setup.py", line 21, in <module>
    import torch
ModuleNotFoundError: No module named 'torch'
---
  Caused by: This error likely indicates that flash-attn==2.6.3 depends on torch, but doesn't declare it as a build dependency. If flash-attn==2.6.3 is a first-party package, consider adding torch to its `build-system.requires`. Otherwise, `uv pip install torch` into the environment and re-run with `--no-build-isolation`.
```

---

_Comment by @charliermarsh on 2024-08-14 13:23_

@hcoona -- I think this isn't possible right now. I'll file an issue about it. I think you'll have better luck using `uv tool install`, like:

```
uv venv
uv pip install torch
uv tool install flash-attn --no-build-isolation --python .venv
```

---

_Referenced in [astral-sh/uv#6083](../../astral-sh/uv/issues/6083.md) on 2024-08-14 13:35_

---

_Referenced in [rusty1s/pytorch_scatter#455](../../rusty1s/pytorch_scatter/issues/455.md) on 2024-09-04 20:32_

---

_Referenced in [NVIDIA/TransformerEngine#1324](../../NVIDIA/TransformerEngine/pulls/1324.md) on 2024-11-08 19:47_

---

_Referenced in [astral-sh/uv#10138](../../astral-sh/uv/issues/10138.md) on 2024-12-24 08:24_

---

_Comment by @dsantiago on 2025-01-11 23:34_

> [@hcoona](https://github.com/hcoona) -- I think this isn't possible right now. I'll file an issue about it. I think you'll have better luck using `uv tool install`, like:
> 
> ```
> uv venv
> uv pip install torch
> uv tool install flash-attn --no-build-isolation --python .venv
> ```

@charliermarsh Could you explain why install `flash-attn` via `uv tool` ?

---

_Comment by @tmm1 on 2025-01-15 23:45_

How can I build flash-attn against a specific version of pytorch? I added `tool.tv.sources` and `tools.tv.index` entries, and confirmed its downloading the right version of pytorch. But the other packages I've listed as dependencies are not getting built against that version of torch?

---

_Comment by @CorentinJ on 2025-01-29 20:10_

So `uv add flash-attn --no-build-isolation` works great but the `--no-build-isolation` is not saved to the pyproject.toml! So the installation fails when syncing without flash-attn present because it installs without the flag

---

_Comment by @tmm1 on 2025-01-29 20:22_

Sure you can add it like so:

```toml
[tool.uv]
no-build-isolation-package = ['flash-attn']
```

---

_Comment by @CorentinJ on 2025-01-30 07:50_

Ah I see, so the problem with syncing an env from scratch with `--no-build-isolation-package flash-attn` is that the packages required for building might not be installed at the time `flash-attn` is installed. I've had to do
```
uv sync --no-install-package flash-attn
uv sync
```
To install from scratch (with `no-build-isolation-package` set in the toml)

---

_Comment by @ananth1996 on 2025-01-30 10:09_

> Ah I see, so the problem with syncing an env from scratch with `--no-build-isolation-package flash-attn` is that the packages required for building might not be installed at the time `flash-attn` is installed. I've had to do
> 
> ```
> uv sync --no-install-package flash-attn
> uv sync
> ```
> 
> To install from scratch (with `no-build-isolation-package` set in the toml)

This approach worked for me to install `torch-scatter`, `torch-sparse` and `torch-cluster` for PyTorch Geometric. 
This was an open issue in  https://github.com/rusty1s/pytorch_scatter/issues/455

---

_Referenced in [bulletmark/pipxu#19](../../bulletmark/pipxu/issues/19.md) on 2025-03-10 10:13_

---

_Comment by @doctorpangloss on 2025-05-19 17:55_

this should be reopened - we practically need to specify torch as an override for build requirements

---

_Comment by @charliermarsh on 2025-05-19 17:56_

Please open a separate issue with a clear ask and MRE, and avoid commenting on closed issues. Thanks! 

---

_Referenced in [astral-sh/uv#15248](../../astral-sh/uv/issues/15248.md) on 2025-08-13 03:03_

---

_Referenced in [Machine-Learning-for-Medical-Language/cnlp_llm#43](../../Machine-Learning-for-Medical-Language/cnlp_llm/issues/43.md) on 2025-08-24 03:46_

---

_Referenced in [NVlabs/earth2grid#57](../../NVlabs/earth2grid/issues/57.md) on 2025-10-28 23:08_

---
