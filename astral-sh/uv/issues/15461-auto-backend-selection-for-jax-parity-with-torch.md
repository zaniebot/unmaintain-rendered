```yaml
number: 15461
title: Auto backend selection for JAX (parity with --torch-backend=auto)
type: issue
state: open
author: nkiyohara
labels:
  - enhancement
assignees: []
created_at: 2025-08-22T18:24:38Z
updated_at: 2025-08-26T15:19:44Z
url: https://github.com/astral-sh/uv/issues/15461
synced_at: 2026-01-12T16:02:11Z
```

# Auto backend selection for JAX (parity with --torch-backend=auto)

---

_@nkiyohara_

### Summary

Hi Astral team, thanks for the amazing work on uv!

## Motivation

uv’s PyTorch integration supports automatic backend selection via `--torch-backend=auto` and `UV_TORCH_BACKEND`, which transparently installs the appropriate wheels (CPU / CUDA / ROCm / XPU) and switches to the correct PyTorch index. This has been a game-changer for reproducible installs across heterogeneous machines.

- Docs: [Using uv with PyTorch](https://docs.astral.sh/uv/guides/integration/pytorch/)
- CLI flag reference: [`--torch-backend`](https://docs.astral.sh/uv/reference/cli/#torch-backend)
- Env var: `UV_TORCH_BACKEND`

The JAX community is growing quickly, and many projects need to “do the right thing” depending on user hardware (CPU vs NVIDIA CUDA). Today this requires manually choosing extras like `jax[cuda12]` or `jax[cuda12-local]`, which is error-prone for non-experts and complicates distribution.

## Proposal

Provide an auto backend selection for JAX, analogous to PyTorch:

- **CLI**
  ```bash
  uv pip install "jax" --jax-backend=auto
  uv add jax --jax-backend=auto
  ```

- **Config**
  ```toml
  [tool.uv.pip]
  jax-backend = "auto"   # cpu | cuda12 | cuda12-local | auto
  ```

- **Env var**
  ```bash
  UV_JAX_BACKEND=auto
  ```

## Expected behavior

- Inspect the environment (e.g., presence of NVIDIA drivers / `libcuda`, compatible CUDA runtime) and select the appropriate JAX variant.
- If compatible CUDA is available → install `jax[cuda12]` (or `jax[cuda12-local]`) according to JAX’s release mapping; otherwise fall back to CPU.
- Ensure reproducibility: the resolved variant should be captured in `uv.lock` with index/marker annotations similar to PyTorch.

## Notes

- JAX release notes indicate current CUDA baseline (e.g., CUDA 12.8) and extras naming conventions (recently `cuda12_local` → `cuda12-local`). This variability reinforces the need for an “auto” path that stays aligned with upstream.

Thanks for considering JAX parity with the fantastic PyTorch integration!

### Example

_No response_

---

_Label `enhancement` added by @nkiyohara on 2025-08-22 18:24_

---

_Comment by @jorgehermo9 on 2025-08-22 19:15_

Should we have a more generic env var/config to control this bevavior instead? To me, it seems that adding an env var per package does not scale well. Just my opinion, I don't know if this would grow to many more packages (maybe not)

---

_Comment by @nkiyohara on 2025-08-26 10:45_

Thanks, that’s a good point. I also feel like having one env var per package doesn’t really scale.

Maybe we could think about a **generic backend knob** (with per-package overrides) + some kind of small “resolver” registry inside uv, so detection logic lives in one place?

### 1. Generic knobs (with overrides)

- **Env**
  - `UV_BACKENDS_DEFAULT=auto|cpu|prefer-gpu|require-gpu`
  - Overrides like:  
    - `UV_BACKEND_JAX=auto|cpu|cuda12|cuda12-local`  
    - `UV_BACKEND_TORCH=auto|cpu|cuda|rocm|xpu`

- **CLI**
  - `uv add jax --backend auto` (just uses the generic policy)  
  - Scoped form like: `--backend jax=auto torch=auto`

- **pyproject**
  ```toml
  [tool.uv.backends]
  default = "prefer-gpu"
  jax = "auto"
  torch = "auto"
  ```

This way UX stays consistent and we don’t end up with a zoo of env vars. The old `--torch-backend` flag could just stay around as an alias.

### 2. Resolver registry

- uv exposes some “backend resolver” interface:  
  input = (package name, detected env like cuda/driver/rocm/cpu)  
  output = (exact requirement, index url, extras etc)

- uv ships resolvers for torch and jax, more can be added later.  
- Detection logic (nvml, libcuda, `nvidia-smi`) reused so we don’t duplicate.

### 3. Repro / locking

- Whatever decision the resolver makes is written into `uv.lock` (extras + index markers).  
- If detection is fuzzy, just fall back to CPU and warn.

---

That feels like it keeps things tidy + flexible, and would avoid one-off hacks for each ML framework.

---

_Comment by @notatallshaw on 2025-08-26 15:19_

> some kind of small “resolver” registry inside uv,

Btw, for those not aware, this is being discussed as a standard:

https://wheelnext.dev/proposals/pepxxx_wheel_variant_support/

https://discuss.python.org/t/wheelnext-wheel-variants-an-update-and-a-request-for-feedback/102383


---
