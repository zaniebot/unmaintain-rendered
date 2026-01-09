---
number: 8082
title: uv pip install failes for wheel URLs with placeholder
type: issue
state: closed
author: mil-ad
labels:
  - compatibility
  - needs-decision
assignees: []
created_at: 2024-10-10T11:21:35Z
updated_at: 2025-09-29T23:54:27Z
url: https://github.com/astral-sh/uv/issues/8082
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip install failes for wheel URLs with placeholder

---

_Issue opened by @mil-ad on 2024-10-10 11:21_

I was following the `vllm` [nightly installation guide](https://docs.vllm.ai/en/latest/getting_started/installation.html#install-the-latest-code) where they have this one-liner for installing their nightly wheels:

```
pip install https://vllm-wheels.s3.us-west-2.amazonaws.com/nightly/vllm-1.0.0.dev-cp38-abi3-manylinux1_x86_64.whl
```

Their documentation specifically says:

> The version string in the wheel file name (1.0.0.dev) is just a placeholder to have a unified URL for the wheels. The actual versions of wheels are contained in the wheel metadata.

when using `uv pip install` I get:

```
error: Failed to install: vllm-1.0.0.dev0-cp38-abi3-manylinux1_x86_64.whl (vllm==1.0.0.dev0 (from https://vllm-wheels.s3.us-west-2.amazonaws.com/nightly/vllm-1.0.0.dev-cp38-abi3-manylinux1_x86_64.whl))
  Caused by: Wheel version does not match filename: 0.6.3.dev155+gf3a507f1.d20241010 != 1.0.0.dev0
```

but doing vanilla `pip install` works.



---

_Comment by @charliermarsh on 2024-10-10 11:28_

My _initial_ reaction is that it seems correct to fail here (I'm somewhat surprised this recommend this), and pip's lack of enforcement seems more like a bug than a feature.

---

_Comment by @mil-ad on 2024-10-10 11:30_

I think I agree. I hadn't seen this pattern before.

---

_Referenced in [vllm-project/vllm#9244](../../vllm-project/vllm/issues/9244.md) on 2024-10-10 12:00_

---

_Comment by @konstin on 2024-10-10 12:12_

It violates the Python packaging specs (the binary distribution spec requires the version to be in the wheel name), so i consider this a bug in their packaging system - Thanks for filing the upstream bug!

---

_Label `compatibility` added by @charliermarsh on 2024-10-11 14:46_

---

_Label `needs-decision` added by @charliermarsh on 2024-10-11 14:46_

---

_Comment by @gaardhus on 2024-12-04 11:02_

I just ran into the same issue with the same package. Are you guys thinking of adding a flag to circumvent this, or should I try to push for them to change their practice? FWIW I don't think it's a bug as in unintended on their part, but an "easy" way to let people install the latest nightly version without having to update the url accordingly?

---

_Comment by @konstin on 2024-12-04 11:05_

> but an "easy" way to let people install the latest nightly version without having to update the url accordingly?

This can be done by using a `--find-links` index (basically a flat html page with links to source distributions and wheels) or a full index with the nightly package. The wheel version of a filename needs to be fixed, otherwise resolvers such as uv process the file properly in the dependencies graph.

---

_Comment by @gaardhus on 2024-12-04 11:08_

Roger that, will turn to the vllm repo then

---

_Comment by @ywang96 on 2024-12-22 09:53_

A few of us from the vLLM project are addressing this issue so that you can use `uv` to install nightly wheels - See https://github.com/vllm-project/vllm/pull/11404

---

_Referenced in [astral-sh/uv#10099](../../astral-sh/uv/issues/10099.md) on 2024-12-22 17:58_

---

_Comment by @connorblack on 2024-12-26 04:31_

Running into this with the [huggingface.co/docs/bitsandbytes](https://huggingface.co/docs/bitsandbytes/installation?backend=AMD+ROCm#multi-backend-pip) package as well.

The wheel it fails with:
- https://github.com/bitsandbytes-foundation/bitsandbytes/releases/download/continuous-release_multi-backend-refactor/bitsandbytes-0.45.0.dev0-py3-none-manylinux_2_24_x86_64.whl

---

_Comment by @charliermarsh on 2024-12-27 14:34_

I believe this has been (or is being) fixed with vLLM specifically, but in general I think this is the correct behavior for uv.

For bits-and-bytes, it seems like they _should_ be including the local tag in the wheel filename... Though we can make it optional.

---

_Comment by @dsantiago on 2025-01-05 04:59_

> Running into this with the [huggingface.co/docs/bitsandbytes](https://huggingface.co/docs/bitsandbytes/installation?backend=AMD+ROCm#multi-backend-pip) package as well.
> 
> The wheel it fails with:
> 
> * https://github.com/bitsandbytes-foundation/bitsandbytes/releases/download/continuous-release_multi-backend-refactor/bitsandbytes-0.45.0.dev0-py3-none-manylinux_2_24_x86_64.whl

+1 Same problem here with `bitsandbytes`

<img width="1438" alt="Image" src="https://github.com/user-attachments/assets/d08f1b61-faa7-455f-92f1-618d72133002" />


---

_Comment by @Canahmetozguven on 2025-03-16 04:45_

> I believe this has been (or is being) fixed with vLLM specifically, but in general I think this is the correct behavior for uv.
> 
> For bits-and-bytes, it seems like they _should_ be including the local tag in the wheel filename... Though we can make it optional.

@charliermarsh any update still getting the same error with the bitsandbytes 

---

_Comment by @dtrifiro on 2025-05-06 10:34_

Although I agree with Charlie about the correctness of `uv` failing on these kind of issues, I'll add another example of a failing package (torch xla).

```bash
uv pip install https://storage.googleapis.com/pytorch-xla-releases/wheels/tpuvm/torch-2.8.0.dev20250408-cp310-cp310-linux_x86_64.whl
```

I found this out by trying to install vllm's `tpu` requirements:

```bash
# Using vllm tpu requirements as an example: https://github.com/vllm-project/vllm/blob/main/requirements/tpu.txt
curl -L https://github.com/vllm-project/vllm/raw/refs/heads/main/requirements/tpu.txt > req_tpu.txt 

uv pip install --pre -r req_tpu.txt
```

```
...
error: Failed to install: torch-2.8.0.dev20250408-cp310-cp310-linux_x86_64.whl (torch==2.8.0.dev20250408 (from https://storage.googleapis.com/pytorch-xla-releases/wheels/tpuvm/torch-2.8.0.dev20250408-cp310-cp310-linux_x86_64.whl))
  Caused by: Wheel version does not match filename: 2.8.0 != 2.8.0.dev20250408
..
```


---

_Referenced in [vllm-project/vllm#24126](../../vllm-project/vllm/issues/24126.md) on 2025-09-02 22:17_

---

_Comment by @vadimkantorov on 2025-09-02 22:18_

Seems related:
- https://github.com/vllm-project/vllm/issues/24126

But this time hit with `uv sync`, not with `uv pip install`

Any there any workarounds for 

```
error: Failed to install: vllm-1.0.0.dev0-cp38-abi3-manylinux1_x86_64.whl (vllm==1.0.0.dev0 (from https://vllm-wheels.s3.amazonaws.com/038e9be4eb7a63189c8980845d80cb96957b9919/vllm-1.0.0.dev-cp38-abi3-manylinux1_x86_64.whl))
  Caused by: Wheel version does not match filename: 0.10.1rc2.dev397+g038e9be4e != 1.0.0.dev0
```

?

Is there a way to force-disable uv checking for filename<>metadata consistency?

Btw after getting I'm starting to get this error when running `uv sync` again:
```
error: Failed to parse `uv.lock`
  Caused by: The entry for package `vllm` v0.10.1rc2.dev397+g038e9be4e has wheel `vllm-1.0.0.dev0-cp38-abi3-manylinux1_x86_64.whl` with inconsistent version: v1.0.0.dev0
```

---

Agreed that it's a vllm isue, but would it be possible to have a switch for uv ignoring metadata consistency, at least when explicit wheel URL is set? E.g. as in:

```toml
# pyproject.toml - a single file in curdir

[build-system]
requires = ["setuptools>=65", "wheel"]
build-backend = "setuptools.build_meta"

[project]
name =  "test_vllm"
version = "0.0.0.1"
requires-python = ">=3.12"
dependencies = [
    "vllm",
    "torch>=2.8",
    "flash-attn==2.8.2",
]

[tool.uv.sources]
vllm = { url = "https://vllm-wheels.s3.amazonaws.com/038e9be4eb7a63189c8980845d80cb96957b9919/vllm-1.0.0.dev-cp38-abi3-manylinux1_x86_64.whl" }

[tool.uv]
override-dependencies = ["outlines-core==0.2.10"]
```

---

_Referenced in [astral-sh/uv#15647](../../astral-sh/uv/issues/15647.md) on 2025-09-03 00:22_

---

_Comment by @DenysFrasinich on 2025-09-06 17:42_

Same error with 
`uv add "torch @ https://developer.download.nvidia.com/compute/redist/jp/v61/pytorch/torch-2.5.0a0+872d972e41.nv24.08.17622132-cp310-cp310-linux_aarch64.whl"`
`error: Failed to install: torch-2.5.0a0+872d972e41.nv24.8.17622132-cp310-cp310-linux_aarch64.whl (torch==2.5.0a0+872d972e41.nv24.8.17622132 (from https://developer.download.nvidia.com/compute/redist/jp/v61/pytorch/torch-2.5.0a0+872d972e41.nv24.08.17622132-cp310-cp310-linux_aarch64.whl))
  Caused by: Wheel version does not match filename: 2.5.0a0+872d972e41.nv24.8 != 2.5.0a0+872d972e41.nv24.8.17622132
`
pip has its architectural issues, but the lack of backward compatibility in uv makes it unusable in so-called "enterprise projects" — like on NVIDIA Jetson devices, for example, where using their redistributable is the recommended way to install PyTorch, and this redistributable package is built in an inaccurate manner.
For details about PyTorch on Jetson devices, see: https://docs.nvidia.com/deeplearning/frameworks/install-pytorch-jetson-platform/index.html


---

_Comment by @jsermeno on 2025-09-10 01:22_

Same issue for `xformers`.

```
uv add xformers                                                                       ✔  10154  02:23:30
warning: The `extra-build-dependencies` option is experimental and may change without warning. Pass `--preview-features extra-build-dependencies` to disable this warning.
Resolved 81 packages in 1.26s
      xx
Prepared 1 package in 537ms
Uninstalled 1 package in 0.32ms
error: Failed to install: xformers-0.0.33.dev20250908+cu128-cp39-abi3-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl (xformers==0.0.33.dev20250908+cu128)
  Caused by: Wheel version does not match filename: 0.0.33+5d4b92a5.d20250909 != 0.0.33.dev20250908+cu128
```

It's good to be principled, but at the moment it would seem that the only option to avoid this is to stop using `uv`.

---

_Comment by @charliermarsh on 2025-09-10 01:25_

Yeah, I guess we'll have to loosen the constraints here (even though those wheels are clearly not valid).

---

_Comment by @mfordiani-cosmohc on 2025-09-12 10:24_

Same issue with NVIDIA's torch builds for tegra: https://developer.download.nvidia.cn/compute/redist/jp/v61/pytorch/torch-2.5.0a0+872d972e41.nv24.08.17622132-cp310-cp310-linux_aarch64.whl

Steps to reproduce:

1. `wget https://developer.download.nvidia.cn/compute/redist/jp/v61/pytorch/torch-2.5.0a0+872d972e41.nv24.08.17622132-cp310-cp310-linux_aarch64.whl`
2. `uv add ./torch-2.5.0a0+872d972e41.nv24.08.17622132-cp310-cp310-linux_aarch64.whl`


---

_Comment by @notatallshaw on 2025-09-14 15:15_

I have proposed this pip also deprecate accepting such wheels: https://github.com/pypa/pip/issues/13580

---

_Comment by @vadimkantorov on 2025-09-14 15:18_

In my view, in principle it would be good, but also there must be an explicit opt-in switch to ignore the mismatch (globally or for a particular wheel) - to workaround  existing whl files named with mismatch...

---

_Comment by @notatallshaw on 2025-09-14 15:21_

@vadimkantorov I assume that comment is toward me and pip, if so please post such feedback on the pip issue, I would want to go into more detail on the use cases and the complexity such code would add to pip, but I don't want to do it on the uv issue tracker.

---

_Comment by @vadimkantorov on 2025-09-14 15:23_

My comment is for uv as well - because currently pip is already permissive, but uv requires manual wheel downloading and renaming step. But I'll repost it to pip too, thanks for suggesting!

---

_Comment by @konstin on 2025-09-14 16:14_

A key question here is, why are these projects producing these broken wheels? Is it just that pip didn't check those, so it should never showed up that those wheels were broken until now, and if so, can we fix that upstream by ensuring coherence between the METADATA version and the wheel filename version?

---

_Referenced in [pytorch/pytorch#163232](../../pytorch/pytorch/pulls/163232.md) on 2025-09-18 01:16_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-09-27 23:47_

---

_Referenced in [astral-sh/uv#16046](../../astral-sh/uv/pulls/16046.md) on 2025-09-28 00:08_

---

_Closed by @charliermarsh on 2025-09-29 23:54_

---
