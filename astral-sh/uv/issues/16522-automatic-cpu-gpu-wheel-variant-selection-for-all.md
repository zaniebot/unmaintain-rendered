```yaml
number: 16522
title: Automatic CPU/GPU wheel variant selection for all packages (not just PyTorch)
type: issue
state: open
author: muhammad-fiaz
labels:
  - enhancement
assignees: []
created_at: 2025-10-30T18:49:18Z
updated_at: 2025-11-04T19:50:12Z
url: https://github.com/astral-sh/uv/issues/16522
synced_at: 2026-01-10T03:23:55Z
```

# Automatic CPU/GPU wheel variant selection for all packages (not just PyTorch)

---

_Issue opened by @muhammad-fiaz on 2025-10-30 18:49_

I am requesting a feature that allows `uv` to automatically select the correct **CPU or GPU backend wheel variant** for packages that ship multiple prebuilt versions - not only for PyTorch, but also for ecosystem libraries like **`xformers`** and **`flash-attn`**.

Currently:

* `torch-backend = "auto"` correctly installs CUDA / ROCm / CPU variants for **PyTorch**
* But companion packages like `xformers` and `flash-attn` are **not matched** to the selected backend
* This frequently results in:

  * **CUDA PyTorch**
  * **CPU-only xformers**
  * -> leading to missing CUDA kernels and runtime errors

This happens because the CUDA wheels for `xformers` and `flash-attn` are not served fully on PyPI when cuda enabled , so `uv` (like pip) defaults to CPU wheels.
And because `xformers` is installed without CUDA support, the resolver may downgrade PyTorch to CPU, or install mismatched wheel variants, causing conflicts and inconsistent environments. Avoiding this currently requires optional dependencies, platform markers, or manual wheel URLs, which creates unnecessary overhead.

---

### **Requested Behavior**

Extend `torch-backend` logic into a **general backend-aware resolver**, e.g.:

```toml
# uv.toml
[tool.uv.pip]
backend = "auto"                        # Detect CPU / CUDA / ROCm
auto-match-packages = ["xformers", "flash-attn"]
```

Then:

```bash
uv sync
```

`uv` should:

* Detect the system backend (CPU / CUDA version / ROCm)
* Select the correct wheel sources
* Install **matching** GPU/CPU wheel variants across packages consistently

---

Additionally, I am requesting support for **backend-aware index selection** when multiple CUDA/CPU indexes are defined, e.g.:

```toml
[tool.uv.sources]
torch = [
  { index = "pytorch-cpu" },
  { index = "pytorch-cu128", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
  { index = "pytorch-cu129", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
  { index = "pytorch-cu130", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]

torchvision = [
  { index = "pytorch-cpu" },
  { index = "pytorch-cu128", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
  { index = "pytorch-cu129", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
  { index = "pytorch-cu130", marker = "sys_platform == 'linux' or sys_platform == 'win32'" },
]

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu129"
url = "https://download.pytorch.org/whl/cu129"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu130"
url = "https://download.pytorch.org/whl/cu130"
explicit = true

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

`uv` should automatically select the correct index based on detected backend, and ensure that companion packages (such as `xformers` and `flash-attn`) resolve against the same backend index without requiring manual markers or optional dependency trees.

> Note: this feature may eliminate the need for optional dependencies for simplicity :)


### **Why This Matters**

* `xformers` GPU wheels must match **PyTorch's CUDA version**
* Incorrect fallback to CPU wheels causes:

  * Failed fused attention ops
  * Silent CPU fallback and performance drops
  * Common runtime errors (`No available CUDA kernel`)
* This is currently the **failure point** for new ML users setting up environments

---

### **I am aware of existing docs**

This request **builds upon**:
[https://docs.astral.sh/uv/guides/integration/pytorch/](https://docs.astral.sh/uv/guides/integration/pytorch/)
[https://docs.astral.sh/uv/reference/settings/#pip_torch-backend](https://docs.astral.sh/uv/reference/settings/#pip_torch-backend)
[https://docs.astral.sh/uv/concepts/projects/config/#augmenting-build-dependencies](https://docs.astral.sh/uv/concepts/projects/config/#augmenting-build-dependencies)

But expands automatic backend resolution **beyond PyTorch** to the entire CUDA/ROCm ecosystem.


### Example


Current behavior:

```bash
uv add torch xformers flash-attn
```

Since no CUDA index is provided, this installs:

```
torch (CPU)
xformers (CPU)
flash-attn (CPU)
```

---

Trying to force CUDA manually:

```bash
uv add torch xformers flash-attn --index-url https://download.pytorch.org/whl/cu128
```

Current result with uv:

```
torch (CUDA 12.8)
xformers (CPU-only or mismatched version)
flash-attn (CPU-only or fails to resolve)
```

Because `uv` only applies the index to `torch` and not to related ecosystem packages, leading to conflicts and inconsistent backends.

---

With the requested feature:

```toml
# uv.toml
[tool.uv.pip]
backend = "auto"
auto-match-packages = ["xformers", "flash-attn"]
```

```bash
uv sync
```

Result:

```
torch (CUDA)
xformers (CUDA)
flash-attn (CUDA)
```

All resolved to the **same detected backend**, with **no manual index flags**, **no platform markers**, and **no optional dependency trees**.

Thank you for reviewing this request :)


---

_Label `enhancement` added by @muhammad-fiaz on 2025-10-30 18:49_

---

_Comment by @zanieb on 2025-10-30 19:24_

You may be interested in https://astral.sh/blog/wheel-variants

The problem with doing this for arbitrary packages is that they encode which GPU variant they're compatible with in different ways. We're working to standardize this, so it'll "just work" for the whole ecosystem.

---

_Comment by @muhammad-fiaz on 2025-10-30 19:53_

@zanieb Thanks for reply! yeah that feature is good and as you said it just works! but the problem for me is with conflicts between them based on versions even if its can be ignored by uv settings for isolation it kind of hard to work with for multiple versioned projects such as Library projects (it almost consuming 500+ lines of optional dependencies with its artifacts url) and that why i am requesting i like the `uv` method like `[tool.uv.sources]` and `[tool.uv.index]` which eliminate the needed to multiple versioned urls into one index and also `uv` have features for `universal=true` on option settings `pyproject.toml` i hope that this feature may even added or simply process for optional dependencies in future 

BTW i have some **Example Use Case Scenarios:** with [unsloth pyproject.toml](https://github.com/unslothai/unsloth/blob/main/pyproject.toml)

<details> <summary>Click to Open Example </summary>

```toml

[project.optional-dependencies]
triton = [
    "triton>=3.0.0 ; ('linux' in sys_platform)",
    "triton-windows ; (sys_platform == 'win32') and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
]
huggingfacenotorch = [
    "wheel>=0.42.0",
    "packaging",
    "numpy",
    "tqdm",
    "psutil",
    "tyro",
    "protobuf",
    "sentencepiece>=0.2.0",
    "datasets>=3.4.1,!=4.0.*,!=4.1.0",
    "accelerate>=0.34.1",
    "peft>=0.7.1,!=0.11.0",
    "huggingface_hub>=0.34.0",
    "hf_transfer",
    "diffusers",
    "transformers>=4.51.3,!=4.52.0,!=4.52.1,!=4.52.2,!=4.52.3,!=4.53.0,!=4.54.0,!=4.55.0,!=4.55.1,!=4.57.0,<=4.57.2",
    "trl>=0.18.2,!=0.19.0,<=0.23.0",
]
huggingface = [
    "unsloth[huggingfacenotorch]",
    "unsloth_zoo>=2025.10.13",
    "torchvision",
    "unsloth[triton]",
]
windows = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0 ; (sys_platform == 'win32')",
    "xformers>=0.0.22.post7 ; (sys_platform == 'win32')",
]
base = [
    "unsloth[huggingface]",
]
cu118only = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.22.post7%2Bcu118-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.22.post7%2Bcu118-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.22.post7%2Bcu118-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
]
cu121only = [
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.22.post7-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.22.post7-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.22.post7-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
]
cu118onlytorch211 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.23%2Bcu118-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.23%2Bcu118-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.23%2Bcu118-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
]
cu121onlytorch211 = [
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.23-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.23-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.23-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
]
cu118onlytorch212 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.23.post1%2Bcu118-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.23.post1%2Bcu118-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.23.post1%2Bcu118-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
]
cu121onlytorch212 = [
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.23.post1-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.23.post1-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.23.post1-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
]
cu118onlytorch220 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.24%2Bcu118-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.24%2Bcu118-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.24%2Bcu118-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
]
cu121onlytorch220 = [
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.24-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.24-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.24-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
]
cu118onlytorch230 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.27%2Bcu118-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.27%2Bcu118-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.27%2Bcu118-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.27%2Bcu118-cp312-cp312-manylinux2014_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu121onlytorch230 = [
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.27-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.27-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.27-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.27-cp312-cp312-manylinux2014_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu118onlytorch240 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.27.post2%2Bcu118-cp39-cp39-manylinux2014_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.27.post2%2Bcu118-cp310-cp310-manylinux2014_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.27.post2%2Bcu118-cp311-cp311-manylinux2014_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.27.post2%2Bcu118-cp312-cp312-manylinux2014_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu121onlytorch240 = [
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.28.post1-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.28.post1-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.28.post1-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.28.post1-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu124onlytorch240 = [
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post1-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post1-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post1-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post1-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post1-cp39-cp39-win_amd64.whl ; python_version=='3.9' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post1-cp310-cp310-win_amd64.whl ; python_version=='3.10' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post1-cp311-cp311-win_amd64.whl ; python_version=='3.11' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post1-cp312-cp312-win_amd64.whl ; python_version=='3.12' and (sys_platform == 'win32')",
]
cu118onlytorch250 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.28.post2-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.28.post2-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.28.post2-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.28.post2-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu121onlytorch250 = [
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.28.post2-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.28.post2-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.28.post2-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.28.post2-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu124onlytorch250 = [
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post2-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post2-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post2-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post2-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post2-cp39-cp39-win_amd64.whl ; python_version=='3.9' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post2-cp310-cp310-win_amd64.whl ; python_version=='3.10' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post2-cp311-cp311-win_amd64.whl ; python_version=='3.11' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.28.post2-cp312-cp312-win_amd64.whl ; python_version=='3.12' and (sys_platform == 'win32')",
]
cu118onlytorch251 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.29.post1-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.29.post1-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.29.post1-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.29.post1-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu121onlytorch251 = [
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.29.post1-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.29.post1-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.29.post1-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu121/xformers-0.0.29.post1-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu124onlytorch251 = [
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post1-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post1-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post1-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post1-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post1-cp39-cp39-win_amd64.whl ; python_version=='3.9' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post1-cp310-cp310-win_amd64.whl ; python_version=='3.10' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post1-cp311-cp311-win_amd64.whl ; python_version=='3.11' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post1-cp312-cp312-win_amd64.whl ; python_version=='3.12' and (sys_platform == 'win32')",
]
cu118onlytorch260 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.29.post3-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.29.post3-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.29.post3-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.29.post3-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
]
cu124onlytorch260 = [
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post3-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post3-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post3-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post3-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post3-cp39-cp39-win_amd64.whl ; python_version=='3.9' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post3-cp310-cp310-win_amd64.whl ; python_version=='3.10' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post3-cp311-cp311-win_amd64.whl ; python_version=='3.11' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu124/xformers-0.0.29.post3-cp312-cp312-win_amd64.whl ; python_version=='3.12' and (sys_platform == 'win32')",
]
cu126onlytorch260 = [
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.29.post3-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.29.post3-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.29.post3-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.29.post3-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.29.post3-cp39-cp39-win_amd64.whl ; python_version=='3.9' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.29.post3-cp310-cp310-win_amd64.whl ; python_version=='3.10' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.29.post3-cp311-cp311-win_amd64.whl ; python_version=='3.11' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.29.post3-cp312-cp312-win_amd64.whl ; python_version=='3.12' and (sys_platform == 'win32')",
]
cu118onlytorch270 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.30-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.30-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.30-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.30-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.30-cp39-cp39-win_amd64.whl ; python_version=='3.9' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.30-cp310-cp310-win_amd64.whl ; python_version=='3.10' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.30-cp311-cp311-win_amd64.whl ; python_version=='3.11' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.30-cp312-cp312-win_amd64.whl ; python_version=='3.12' and (sys_platform == 'win32')",
]
cu126onlytorch270 = [
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.30-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.30-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.30-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.30-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.30-cp39-cp39-win_amd64.whl ; python_version=='3.9' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.30-cp310-cp310-win_amd64.whl ; python_version=='3.10' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.30-cp311-cp311-win_amd64.whl ; python_version=='3.11' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.30-cp312-cp312-win_amd64.whl ; python_version=='3.12' and (sys_platform == 'win32')",
]
cu128onlytorch270 = [
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.30-cp39-cp39-manylinux_2_28_x86_64.whl ; python_version=='3.9' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.30-cp310-cp310-manylinux_2_28_x86_64.whl ; python_version=='3.10' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.30-cp311-cp311-manylinux_2_28_x86_64.whl ; python_version=='3.11' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.30-cp312-cp312-manylinux_2_28_x86_64.whl ; python_version=='3.12' and ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.30-cp39-cp39-win_amd64.whl ; python_version=='3.9' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.30-cp310-cp310-win_amd64.whl ; python_version=='3.10' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.30-cp311-cp311-win_amd64.whl ; python_version=='3.11' and (sys_platform == 'win32')",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.30-cp312-cp312-win_amd64.whl ; python_version=='3.12' and (sys_platform == 'win32')",
]
cu118onlytorch271 = [
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.31.post1-cp39-abi3-manylinux_2_28_x86_64.whl ; ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu118/xformers-0.0.31.post1-cp39-abi3-win_amd64.whl ; (sys_platform == 'win32')",
]
cu126onlytorch271 = [
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.31.post1-cp39-abi3-manylinux_2_28_x86_64.whl ; ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.31.post1-cp39-abi3-win_amd64.whl ; (sys_platform == 'win32')",
]
cu128onlytorch271 = [
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.31.post1-cp39-abi3-manylinux_2_28_x86_64.whl ; ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.31.post1-cp39-abi3-win_amd64.whl ; (sys_platform == 'win32')",
]
cu118onlytorch280 = [
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.32.post2-cp39-abi3-manylinux_2_28_x86_64.whl ; ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu126/xformers-0.0.32.post2-cp39-abi3-win_amd64.whl ; (sys_platform == 'win32')",
]
cu126onlytorch280 = [
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.32.post2-cp39-abi3-manylinux_2_28_x86_64.whl ; ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu128/xformers-0.0.32.post2-cp39-abi3-win_amd64.whl ; (sys_platform == 'win32')",
]
cu128onlytorch280 = [
    "xformers @ https://download.pytorch.org/whl/cu129/xformers-0.0.32.post2-cp39-abi3-manylinux_2_28_x86_64.whl ; ('linux' in sys_platform)",
    "xformers @ https://download.pytorch.org/whl/cu129/xformers-0.0.32.post2-cp39-abi3-win_amd64.whl ; (sys_platform == 'win32')",
]
cu130onlytorch280 = [
]
cu126onlytorch290 = [
]
cu128onlytorch290 = [
]
cu130onlytorch290 = [
]
cu118 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118only]",
]
cu121 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121only]",
]
cu118-torch211 = [
    "unsloth[huggingface]",
    "bitsandbytes==0.45.5",
    "unsloth[cu118onlytorch211]",
]
cu121-torch211 = [
    "unsloth[huggingface]",
    "bitsandbytes==0.45.5",
    "unsloth[cu121onlytorch211]",
]
cu118-torch212 = [
    "unsloth[huggingface]",
    "bitsandbytes==0.45.5",
    "unsloth[cu118onlytorch212]",
]
cu121-torch212 = [
    "unsloth[huggingface]",
    "bitsandbytes==0.45.5",
    "unsloth[cu121onlytorch212]",
]
cu118-torch220 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch220]",
]
cu121-torch220 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch220]",
]
cu118-torch230 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch230]",
]
cu121-torch230 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch230]",
]
cu118-torch240 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch240]",
]
cu121-torch240 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch240]",
]
cu124-torch240 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu124onlytorch240]",
]
cu118-torch250 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch250]",
]
cu121-torch250 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch250]",
]
cu124-torch250 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu124onlytorch250]",
]
cu118-torch251 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch251]",
]
cu121-torch251 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch251]",
]
cu124-torch251 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu124onlytorch251]",
]
cu118-torch260 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch260]",
]
cu124-torch260 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu124onlytorch260]",
]
cu126-torch260 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch260]",
]
cu118-torch270 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch270]",
]
cu126-torch270 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch270]",
]
cu128-torch270 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu128onlytorch270]",
]
cu118-torch271 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch271]",
]
cu126-torch271 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch271]",
]
cu128-torch271 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu128onlytorch271]",
]
cu118-torch280 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch280]",
]
cu126-torch280 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch280]",
]
cu128-torch280 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu128onlytorch280]",
]
cu130-torch280 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu130onlytorch280]",
]
cu126-torch290 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch290]",
]
cu128-torch290 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu128onlytorch290]",
]
cu130-torch290 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu130onlytorch290]",
]
kaggle = [
    "unsloth[huggingface]",
]
kaggle-new = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
]
conda = [
    "unsloth[huggingface]",
]
colab-torch211 = [
    "unsloth[huggingface]",
    "bitsandbytes==0.45.5",
    "unsloth[cu121onlytorch211]",
]
colab-ampere-torch211 = [
    "unsloth[huggingface]",
    "bitsandbytes==0.45.5",
    "unsloth[cu121onlytorch211]",
    "packaging",
    "ninja",
    "flash-attn>=2.6.3 ; ('linux' in sys_platform)",
]
colab-torch220 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch220]",
]
colab-ampere-torch220 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch220]",
    "packaging",
    "ninja",
    "flash-attn>=2.6.3 ; ('linux' in sys_platform)",
]
colab-new = [
    "unsloth_zoo>=2025.10.13",
    "packaging",
    "tyro",
    "transformers>=4.51.3,!=4.52.0,!=4.52.1,!=4.52.2,!=4.52.3,!=4.53.0,!=4.54.0,!=4.55.0,!=4.55.1,!=4.57.0,<=4.57.2",
    "datasets>=3.4.1,!=4.0.*,!=4.1.0",
    "sentencepiece>=0.2.0",
    "tqdm",
    "psutil",
    "wheel>=0.42.0",
    "numpy",
    "protobuf",
    "huggingface_hub>=0.34.0",
    "hf_transfer",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[triton]",
]
colab-no-deps = [
    "accelerate>=0.34.1",
    "trl>=0.18.2,!=0.19.0,<=0.23.0",
    "peft>=0.7.1",
    "xformers ; ('linux' in sys_platform or sys_platform == 'win32') and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "protobuf",
]
colab = [
    "unsloth[cu121]",
]
flashattention = [
    "packaging ; ('linux' in sys_platform)",
    "ninja ; ('linux' in sys_platform)",
    "flash-attn>=2.6.3 ; ('linux' in sys_platform)",
]
colab-ampere = [
    "unsloth[colab-ampere-torch220]",
    "unsloth[flashattention]",
]
cu118-ampere = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118only]",
    "unsloth[flashattention]",
]
cu121-ampere = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121only]",
    "unsloth[flashattention]",
]
cu118-ampere-torch211 = [
    "unsloth[huggingface]",
    "bitsandbytes==0.45.5",
    "unsloth[cu118onlytorch211]",
    "unsloth[flashattention]",
]
cu121-ampere-torch211 = [
    "unsloth[huggingface]",
    "bitsandbytes==0.45.5",
    "unsloth[cu121onlytorch211]",
    "unsloth[flashattention]",
]
cu118-ampere-torch220 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch220]",
    "unsloth[flashattention]",
]
cu121-ampere-torch220 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch220]",
    "unsloth[flashattention]",
]
cu118-ampere-torch230 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch230]",
    "unsloth[flashattention]",
]
cu121-ampere-torch230 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch230]",
    "unsloth[flashattention]",
]
cu118-ampere-torch240 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch240]",
    "unsloth[flashattention]",
]
cu121-ampere-torch240 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch240]",
    "unsloth[flashattention]",
]
cu124-ampere-torch240 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu124onlytorch240]",
    "unsloth[flashattention]",
]
cu118-ampere-torch250 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch250]",
    "unsloth[flashattention]",
]
cu121-ampere-torch250 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch250]",
    "unsloth[flashattention]",
]
cu124-ampere-torch250 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu124onlytorch250]",
    "unsloth[flashattention]",
]
cu118-ampere-torch251 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch251]",
    "unsloth[flashattention]",
]
cu121-ampere-torch251 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu121onlytorch251]",
    "unsloth[flashattention]",
]
cu124-ampere-torch251 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu124onlytorch251]",
    "unsloth[flashattention]",
]
cu118-ampere-torch260 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch260]",
    "unsloth[flashattention]",
]
cu124-ampere-torch260 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu124onlytorch260]",
    "unsloth[flashattention]",
]
cu126-ampere-torch260 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch260]",
    "unsloth[flashattention]",
]
cu118-ampere-torch270 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch270]",
    "unsloth[flashattention]",
]
cu126-ampere-torch270 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch270]",
    "unsloth[flashattention]",
]
cu128-ampere-torch270 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu128onlytorch270]",
    "unsloth[flashattention]",
]
cu118-ampere-torch271 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch271]",
    "unsloth[flashattention]",
]
cu126-ampere-torch271 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch271]",
    "unsloth[flashattention]",
]
cu128-ampere-torch271 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu128onlytorch271]",
    "unsloth[flashattention]",
]
cu118-ampere-torch280 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu118onlytorch280]",
    "unsloth[flashattention]",
]
cu126-ampere-torch280 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch280]",
    "unsloth[flashattention]",
]
cu128-ampere-torch280 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu128onlytorch280]",
    "unsloth[flashattention]",
]
cu130-ampere-torch280 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu130onlytorch280]",
    "unsloth[flashattention]",
]
cu126-ampere-torch290 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu126onlytorch290]",
]
cu128-ampere-torch290 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu128onlytorch290]",
]
cu130-ampere-torch290 = [
    "unsloth[huggingface]",
    "bitsandbytes>=0.45.5,!=0.46.0,!=0.48.0",
    "unsloth[cu130onlytorch290]",
]
flashattentiontorch260abiFALSEcu12x = [
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiFALSE-cp39-cp39-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.9'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiFALSE-cp310-cp310-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.10'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiFALSE-cp311-cp311-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.11'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiFALSE-cp312-cp312-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.12'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiFALSE-cp313-cp313-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.13'",
]
flashattentiontorch260abiTRUEcu12x = [
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiTRUE-cp39-cp39-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.9'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiTRUE-cp310-cp310-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.10'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiTRUE-cp311-cp311-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.11'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiTRUE-cp312-cp312-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.12'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.6cxx11abiTRUE-cp313-cp313-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.13'",
]
flashattentiontorch250abiFALSEcu12x = [
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiFALSE-cp39-cp39-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.9'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiFALSE-cp310-cp310-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.10'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiFALSE-cp311-cp311-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.11'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiFALSE-cp312-cp312-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.12'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiFALSE-cp313-cp313-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.13'",
]
flashattentiontorch250abiTRUEcu12x = [
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiTRUE-cp39-cp39-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.9'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiTRUE-cp310-cp310-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.10'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiTRUE-cp311-cp311-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.11'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiTRUE-cp312-cp312-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.12'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.5cxx11abiTRUE-cp313-cp313-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.13'",
]
flashattentiontorch240abiFALSEcu12x = [
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiFALSE-cp39-cp39-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.9'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiFALSE-cp310-cp310-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.10'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiFALSE-cp311-cp311-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.11'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiFALSE-cp312-cp312-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.12'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiFALSE-cp313-cp313-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.13'",
]
flashattentiontorch240abiTRUEcu12x = [
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiTRUE-cp39-cp39-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.9'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiTRUE-cp310-cp310-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.10'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiTRUE-cp311-cp311-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.11'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiTRUE-cp312-cp312-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.12'",
    "flash-attn @ https://github.com/Dao-AILab/flash-attention/releases/download/v2.7.4.post1/flash_attn-2.7.4.post1+cu12torch2.4cxx11abiTRUE-cp313-cp313-linux_x86_64.whl ; ('linux' in sys_platform) and python_version == '3.13'",
]
intelgputorch260 = [
    "unsloth_zoo[intelgpu]",
    "unsloth[huggingfacenotorch]",

    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.2.0-cp39-cp39-linux_x86_64.whl#sha256=147607f190a7d7aa24ba454def5977fbbfec792fdae18e4ed278cfec29b69271 ; ('linux' in sys_platform) and python_version == '3.9' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.2.0-cp310-cp310-linux_x86_64.whl#sha256=23aa423fa1542afc34f67eb3ba8ef20060f6d1b3a4697eaeab22b11c92b30f2b ; ('linux' in sys_platform) and python_version == '3.10' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.2.0-cp311-cp311-linux_x86_64.whl#sha256=bcfa995229bbfd9ffd8d6c8d9f6428d393e876fa6e23ee3c20e3c0d73ca75ca5 ; ('linux' in sys_platform) and python_version == '3.11' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.2.0-cp312-cp312-linux_x86_64.whl#sha256=bd340903d03470708df3442438acb8b7e08087ab9e61fbe349b2872bf9257ab0 ; ('linux' in sys_platform) and python_version == '3.12' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.2.0-cp313-cp313-linux_x86_64.whl#sha256=814dccc8a07159e6eca74bed70091bc8fea2d9dd87b0d91845f9f38cde62f01c ; ('linux' in sys_platform) and python_version == '3.13' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",

    "torch @ https://download.pytorch.org/whl/xpu/torch-2.6.0%2Bxpu-cp39-cp39-linux_x86_64.whl#sha256=6a8adf6dc4c089406e8b3a7e58ab57a463bddf9b07130d2576e76eced43e92af ; ('linux' in sys_platform) and python_version == '3.9' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.6.0%2Bxpu-cp310-cp310-linux_x86_64.whl#sha256=ff4561cbf07c83bbccaa0f6e9bb0e6dcf721bacd53c9c43c4eb0e7331b4792f9 ; ('linux' in sys_platform) and python_version == '3.10' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.6.0%2Bxpu-cp311-cp311-linux_x86_64.whl#sha256=12005f66b810ddd3ab93f86c4522bcfdd412cbd27fc9d189b661ff7509bc5e8a ; ('linux' in sys_platform) and python_version == '3.11' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.6.0%2Bxpu-cp312-cp312-linux_x86_64.whl#sha256=c4c5c67625cdacf35765c2b94e61fe166e3c3f4a14521b1212a59ad1b3eb0f2e ; ('linux' in sys_platform) and python_version == '3.12' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.6.0%2Bxpu-cp313-cp313-linux_x86_64.whl#sha256=e6864f7a60a5ecc43d5d38f59a16e5dd132384f73dfd3a697f74944026038f7b ; ('linux' in sys_platform) and python_version == '3.13' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
]
intel-gpu-torch260 = [
    "unsloth[intelgputorch260]"
]
intelgputorch270 = [
    "unsloth_zoo[intelgpu]",
    "unsloth[huggingfacenotorch]",

    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.3.0-cp39-cp39-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=749a7098492c6a27b356c97149a4a62973b953eae60bc1b6259260974f344913 ; ('linux' in sys_platform) and python_version == '3.9' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.3.0-cp310-cp310-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=44362e80abd752471a08341093321955b066daa2cfb4810e73b8e3b240850f93 ; ('linux' in sys_platform) and python_version == '3.10' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.3.0-cp311-cp311-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=faa6b8c945a837a080f641bc8ccc77a98fa66980dcd7e62e715fd853737343fd ; ('linux' in sys_platform) and python_version == '3.11' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.3.0-cp312-cp312-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=40f6fb65b345dc9a61813abe7ac9a585f2c9808f414d140cc2a5f11f53ee063c ; ('linux' in sys_platform) and python_version == '3.12' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.3.0-cp313-cp313t-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=9821fe059de58e827ffc6aa10d69369b16c2f8c2a988b86bef9c2c6e396ab3aa ; ('linux' in sys_platform) and python_version == '3.13' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",

    "torch @ https://download.pytorch.org/whl/xpu/torch-2.7.0%2Bxpu-cp39-cp39-linux_x86_64.whl#sha256=f8ee75e50fcbb37ed5b498299ca2264da99ab278a93fae2358e921e4a6e28273 ; ('linux' in sys_platform) and python_version == '3.9' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.7.0%2Bxpu-cp310-cp310-linux_x86_64.whl#sha256=d6fdc342961d98fdcd9d03dfd491a3208bb5f7fbb435841f8f72ce9fdcd2d026 ; ('linux' in sys_platform) and python_version == '3.10' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.7.0%2Bxpu-cp311-cp311-linux_x86_64.whl#sha256=74d07f9357df5cf2bf223ad3c84de16346bfaa0504f988fdd5590d3e177e5e86 ; ('linux' in sys_platform) and python_version == '3.11' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.7.0%2Bxpu-cp312-cp312-linux_x86_64.whl#sha256=c806d44aa2ca5d225629f6fbc6c994d5deaac2d2cde449195bc8e3522ddd219a ; ('linux' in sys_platform) and python_version == '3.12' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.7.0%2Bxpu-cp313-cp313-linux_x86_64.whl#sha256=25d8277b7f01d42e2e014ccbab57a2692b6ec4eff8dcf894eda1b297407cf97a ; ('linux' in sys_platform) and python_version == '3.13' and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
]
intel-gpu-torch270 = [
    "unsloth[intelgputorch270]"
]
intelgputorch280 = [
    "unsloth_zoo[intelgpu]",
    "unsloth[huggingfacenotorch]",

    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.4.0-cp39-cp39-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=ac4d8e33986b1c3c5e48151640539272b2187e83016985853111b46fb82c3c94 ; platform_system == 'Linux' and python_version == '3.9' and platform_machine == 'x86_64'",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.4.0-cp310-cp310-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=999fef4c1f711092b9d3086525920545df490de476ecebe899ffc777019ae17f ; platform_system == 'Linux' and python_version == '3.10' and platform_machine == 'x86_64'",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.4.0-cp311-cp311-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=57b09c8c492985ff6a27cd3a22b08e8f7b96b407bd8030967b6efbb9f63b80cf ; platform_system == 'Linux' and python_version == '3.11' and platform_machine == 'x86_64'",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.4.0-cp312-cp312-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=df4bb3282bac9a3b90231700077110d8680b338416de03c2b7c6133c9b602649 ; platform_system == 'Linux' and python_version == '3.12' and platform_machine == 'x86_64'",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.4.0-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=60da63c99ca827bdcb0df28e0298bf7d066dc607454c6d6176783cb4e79d838b ; platform_system == 'Linux' and python_version == '3.13' and platform_machine == 'x86_64'",

    "torch @ https://download.pytorch.org/whl/xpu/torch-2.8.0%2Bxpu-cp39-cp39-linux_x86_64.whl ; platform_system == 'Linux' and python_version == '3.9' and platform_machine == 'x86_64'",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.8.0%2Bxpu-cp310-cp310-linux_x86_64.whl ; platform_system == 'Linux' and python_version == '3.10' and platform_machine == 'x86_64'",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.8.0%2Bxpu-cp311-cp311-linux_x86_64.whl ; platform_system == 'Linux' and python_version == '3.11' and platform_machine == 'x86_64'",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.8.0%2Bxpu-cp312-cp312-linux_x86_64.whl ; platform_system == 'Linux' and python_version == '3.12' and platform_machine == 'x86_64'",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.8.0%2Bxpu-cp313-cp313-linux_x86_64.whl ; platform_system == 'Linux' and python_version == '3.13' and platform_machine == 'x86_64'",

    "bitsandbytes @ https://github.com/bitsandbytes-foundation/bitsandbytes/releases/download/continuous-release_main/bitsandbytes-1.33.7.preview-py3-none-manylinux_2_24_x86_64.whl ; ('linux' in sys_platform) and (platform_machine == 'x86_64')",

    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.23.0%2Bxpu-cp39-cp39-manylinux_2_28_x86_64.whl#sha256=6e981c192045fc249c008441179ff237bb00174d818b875b0475730b63f0eaca ; platform_system == 'Linux' and python_version == '3.9' and platform_machine == 'x86_64'",
    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.23.0%2Bxpu-cp310-cp310-manylinux_2_28_x86_64.whl#sha256=e5ba4805969277175ebfd59cc717093528cc6e3ada89ac2725fc7a3c1fee6169 ; platform_system == 'Linux' and python_version == '3.10' and platform_machine == 'x86_64'",
    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.23.0%2Bxpu-cp311-cp311-manylinux_2_28_x86_64.whl#sha256=74c39c144104416bc4c5ad8c26ab0c169dc5cc6be58059e01bc3665dd0ef676f ; platform_system == 'Linux' and python_version == '3.11' and platform_machine == 'x86_64'",
    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.23.0%2Bxpu-cp312-cp312-manylinux_2_28_x86_64.whl#sha256=0acec355b80c3899841184084f365df336c508602812e34a44007b8b60d53af4 ; platform_system == 'Linux' and python_version == '3.12' and platform_machine == 'x86_64'",
    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.23.0%2Bxpu-cp313-cp313-manylinux_2_28_x86_64.whl#sha256=e2109ae773dad27b98ca17681044b4f876563c37f2382b75de3a371399edcff8 ; platform_system == 'Linux' and python_version == '3.13' and platform_machine == 'x86_64'",
]
intel-gpu-torch280 = [
    "unsloth[intelgputorch280]"
]
intelgputorch290 = [
    "unsloth_zoo[intelgpu]",
    "unsloth[huggingfacenotorch]",

    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.5.0-cp310-cp310-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=c169a1de14c19673b17c751290d467fa282fc90fa5da4314b2e5cdab1f553146 ; platform_system == 'Linux' and python_version == '3.10' and platform_machine == 'x86_64'",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.5.0-cp311-cp311-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=013d9dd5d6479bd22983161f462e61c8dbe1d82e6730624a7a8d5945507eaa61 ; platform_system == 'Linux' and python_version == '3.11' and platform_machine == 'x86_64'",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.5.0-cp312-cp312-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=afc8cabfbf7ed51fd278d1e0f88d6afc157b0201bad4b99d681e4d542f9e66d4 ; platform_system == 'Linux' and python_version == '3.12' and platform_machine == 'x86_64'",
    "pytorch_triton_xpu @ https://download.pytorch.org/whl/pytorch_triton_xpu-3.5.0-cp313-cp313-manylinux_2_27_x86_64.manylinux_2_28_x86_64.whl#sha256=0d24c1716088f2764d0d24c64227732195b6a42706c3c5fc89eeb4904bfa0818 ; platform_system == 'Linux' and python_version == '3.13' and platform_machine == 'x86_64'",

    "torch @ https://download.pytorch.org/whl/xpu/torch-2.9.0%2Bxpu-cp310-cp310-linux_x86_64.whl#sha256=5afbe860ce991825a36b75706a523601087e414b77598ef0d9d3d565741c277d ; platform_system == 'Linux' and python_version == '3.10' and platform_machine == 'x86_64'",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.9.0%2Bxpu-cp311-cp311-linux_x86_64.whl#sha256=607fe419c32d6e8e0556f745742e7cff1d0babce51f54be890e0c1422359c442 ; platform_system == 'Linux' and python_version == '3.11' and platform_machine == 'x86_64'",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.9.0%2Bxpu-cp312-cp312-linux_x86_64.whl#sha256=376bae584d89980b8e59934d248c38d5fa3b7d4687a4df1a19f4bc1d23dcc8c1 ; platform_system == 'Linux' and python_version == '3.12' and platform_machine == 'x86_64'",
    "torch @ https://download.pytorch.org/whl/xpu/torch-2.9.0%2Bxpu-cp313-cp313-linux_x86_64.whl#sha256=98d6a06dd7fb185874367b18bd609f05f16fdce4142a5980ca94461949965cd2 ; platform_system == 'Linux' and python_version == '3.13' and platform_machine == 'x86_64'",

    "bitsandbytes @ https://github.com/bitsandbytes-foundation/bitsandbytes/releases/download/continuous-release_main/bitsandbytes-1.33.7.preview-py3-none-manylinux_2_24_x86_64.whl ; ('linux' in sys_platform) and (platform_machine == 'x86_64')",

    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.24.0%2Bxpu-cp310-cp310-manylinux_2_28_x86_64.whl#sha256=cbfae2b79b7549fd368c2462fc8e94f8f26cc450782ee72138e908077c09a519 ; platform_system == 'Linux' and python_version == '3.10' and platform_machine == 'x86_64'",
    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.24.0%2Bxpu-cp311-cp311-manylinux_2_28_x86_64.whl#sha256=044fa36ef4b6b43edcd490b75c853fa4b3eb033c2bded29f8fbcf27734713c67 ; platform_system == 'Linux' and python_version == '3.11' and platform_machine == 'x86_64'",
    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.24.0%2Bxpu-cp312-cp312-manylinux_2_28_x86_64.whl#sha256=4b91e4bec1d740a6211f02578a79888550b73f3a4e1383035f8f6d72f587212c ; platform_system == 'Linux' and python_version == '3.12' and platform_machine == 'x86_64'",
    "torchvision @ https://download.pytorch.org/whl/xpu/torchvision-0.24.0%2Bxpu-cp313-cp313-manylinux_2_28_x86_64.whl#sha256=88239e73ca37254bec84f29cd5887e10ff712de7edbbda3fbb3609cd6190d99e ; platform_system == 'Linux' and python_version == '3.13' and platform_machine == 'x86_64'",
]
intel-gpu-torch290 = [
    "unsloth[intelgputorch290]"
]
intel = [
    "unsloth[intelgputorch280]",
]
amd = [
    "unsloth[huggingfacenotorch]",
    "bitsandbytes @ https://github.com/bitsandbytes-foundation/bitsandbytes/releases/download/continuous-release_main/bitsandbytes-1.33.7.preview-py3-none-manylinux_2_24_x86_64.whl ; ('linux' in sys_platform) and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "bitsandbytes @ https://github.com/bitsandbytes-foundation/bitsandbytes/releases/download/continuous-release_main/bitsandbytes-1.33.7.preview-py3-none-win_amd64.whl ; (sys_platform == 'win32') and (platform_machine == 'AMD64' or platform_machine == 'x86_64')",
    "bitsandbytes @ https://github.com/bitsandbytes-foundation/bitsandbytes/releases/download/continuous-release_main/bitsandbytes-1.33.7.preview-py3-none-manylinux_2_24_aarch64.whl ; ('linux' in sys_platform) and (platform_machine == 'aarch64')",
]
```
</details>

as you can see there is huge optional dependencies which are hard manage 





---

_Comment by @mrs83 on 2025-11-04 19:12_

it may help also for bitsandbytes https://github.com/bitsandbytes-foundation/bitsandbytes/releases/tag/continuous-release_multi-backend-refactor

---

_Comment by @muhammad-fiaz on 2025-11-04 19:42_

> it may help also for bitsandbytes https://github.com/bitsandbytes-foundation/bitsandbytes/releases/tag/continuous-release_multi-backend-refactor

Hello @mrs83 Yes also i tried many ways but some works by forced for one platform alone but not for universal platform

below is example workflow i tried for `multiple versioned dependencies support`!
```toml
[build-system]
requires = ["uv_build>=0.9.6,<0.10.0"]
build-backend = "uv_build"
 
dependencies = [
    "torch>=2.5.0",
    "torchvision>=0.20.0",
]


[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu118"
url = "https://download.pytorch.org/whl/cu118"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu126"
url = "https://download.pytorch.org/whl/cu126"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu130"
url = "https://download.pytorch.org/whl/cu130"
explicit = true

[[tool.uv.index]]
name = "pytorch-rocm"
url = "https://download.pytorch.org/whl/rocm6.2"
explicit = true

[[tool.uv.index]]
name = "pytorch-xpu"
url = "https://download.pytorch.org/whl/xpu"
explicit = true
[tool.uv.sources]

torch = [
    { index = "pytorch-cu130", marker = "extra == 'cuda-130'" },
    { index = "pytorch-cu128", marker = "extra == 'cuda-128' and extra != 'cuda-130'" },
    { index = "pytorch-cu126", marker = "extra == 'cuda-126' and extra != 'cuda-130' and extra != 'cuda-128'" },
    { index = "pytorch-cu124", marker = "extra == 'cuda-124' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126'" },
    { index = "pytorch-cu121", marker = "extra == 'cuda-121' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124'" },
    { index = "pytorch-cu118", marker = "extra == 'cuda-118' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124' and extra != 'cuda-121'" },
    { index = "pytorch-rocm", marker = "extra == 'rocm' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124' and extra != 'cuda-121' and extra != 'cuda-118'" },
    { index = "pytorch-xpu", marker = "extra == 'xpu' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124' and extra != 'cuda-121' and extra != 'cuda-118' and extra != 'rocm'" },
]

torchvision = [
    { index = "pytorch-cu130", marker = "extra == 'cuda-130'" },
    { index = "pytorch-cu128", marker = "extra == 'cuda-128' and extra != 'cuda-130'" },
    { index = "pytorch-cu126", marker = "extra == 'cuda-126' and extra != 'cuda-130' and extra != 'cuda-128'" },
    { index = "pytorch-cu124", marker = "extra == 'cuda-124' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126'" },
    { index = "pytorch-cu121", marker = "extra == 'cuda-121' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124'" },
    { index = "pytorch-cu118", marker = "extra == 'cuda-118' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124' and extra != 'cuda-121'" },
    { index = "pytorch-rocm", marker = "extra == 'rocm' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124' and extra != 'cuda-121' and extra != 'cuda-118'" },
    { index = "pytorch-xpu", marker = "extra == 'xpu' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124' and extra != 'cuda-121' and extra != 'cuda-118' and extra != 'rocm'" },
]

xformers = [
    { index = "pytorch-cu130", marker = "extra == 'cuda-130'" },
    { index = "pytorch-cu128", marker = "extra == 'cuda-128' and extra != 'cuda-130'" },
    { index = "pytorch-cu126", marker = "extra == 'cuda-126' and extra != 'cuda-130' and extra != 'cuda-128'" },
    { index = "pytorch-cu124", marker = "extra == 'cuda-124' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126'" },
    { index = "pytorch-cu121", marker = "extra == 'cuda-121' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124'" },
    { index = "pytorch-cu118", marker = "extra == 'cuda-118' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124' and extra != 'cuda-121'" },
    { index = "pytorch-rocm", marker = "extra == 'rocm' and extra != 'cuda-130' and extra != 'cuda-128' and extra != 'cuda-126' and extra != 'cuda-124' and extra != 'cuda-121' and extra != 'cuda-118'" },
]


[tool.uv.extra-build-dependencies]
# xformers requires torch and ninja for compilation
xformers = ["torch", "ninja"]

# flash-attn requires torch, packaging, and ninja for compilation
flash-attn = ["torch>=2.0", "packaging", "ninja"]

[project.optional-dependencies]

# Flash Attention support (Linux only) - separate extra
flash-attn = [
    "flash-attn>=2.6.3 ; sys_platform == 'linux'",
]

# CUDA 11.8 support
cuda-118 = [
    "xformers>=0.0.22",
]

# CUDA 12.1 support  
cuda-121 = [
    "xformers>=0.0.22",
]

# CUDA 12.4 support
cuda-124 = [
    "xformers>=0.0.28",
]

# CUDA 12.6 support
cuda-126 = [
    "xformers>=0.0.28",
]

# CUDA 12.8 support
cuda-128 = [
    "xformers>=0.0.28",
]

# CUDA 13.0 support (latest)
cuda-130 = [
    "xformers>=0.0.28",
]

# AMD ROCm support (PyTorch routed via index)
rocm = []

# Intel XPU support (works on both Windows and Linux x86_64)
# Note: Only installed when explicitly requesting the xpu extra
xpu = [
    "pytorch-triton-xpu",
]

```
i tried like this also it does not work as intented and always have any conflicts output by `uv`
also for my i don't know why it does not even installing `pytorch`  BTW! worked when adding individually pytorch to each `optional-dependencies` but not installing correct extra version like `uv sync --extra cuda-130` or `uv sync --extra cuda-128` like this but it always install fixed on one version and also conflict occurs!, as for `single versioned` for multiple platform usecase with xformer i found one solution #16532

---
