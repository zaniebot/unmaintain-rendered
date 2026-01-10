```yaml
number: 10694
title: "Two-stage `uv sync` is annoying to use in pytorch projects"
type: issue
state: closed
author: tmm1
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2025-01-16T20:33:28Z
updated_at: 2025-08-18T21:15:36Z
url: https://github.com/astral-sh/uv/issues/10694
synced_at: 2026-01-10T03:32:45Z
```

# Two-stage `uv sync` is annoying to use in pytorch projects

---

_Issue opened by @tmm1 on 2025-01-16 20:33_

the [docs on build isolation](https://github.com/astral-sh/uv/blob/45455b33c0a6dfb358345a79abd79ee593253887/docs/concepts/projects/config.md?plain=1#L214) explain how to setup a two-stage build/compile `uv sync` setup to deal with packages which depend on torch at build time.

i'm glad this is documented and it is possible to use `uv` for these packages, but it's quite an annoying manual step. will it be possible to make this automatic in a single `uv sync` in the future?

in the torch ecosystem, it is very common to encounter cuda kernels and other cuda/cutlass libraries which have to be built against the version of pytorch being used. it is also common to need to use nightly for specific bug fixes.

i'm providing one example from my project which illustrates some of the challenges, i.e. custom env vars and cli flags currently required. i would love to be able to move as much of this configuration directly into the `pyproject.toml` so all a user needs is `uv sync`

```toml
[project]
name = "torchnightly"
requires-python = ">=3.11.9"
dependencies = [
    "torch",
    "torchao",
    "torchaudio",
    "torchvision",
    "setuptools", # for grouped_gemm
    "nvidia-cusparselt-cu12", # workaround #10693
]
dynamic = ["version"]

[project.optional-dependencies]
compile = [
    "grouped_gemm @ git+https://github.com/tgale96/grouped_gemm",
]

[tool.uv]
no-build-isolation-package = ['grouped_gemm']

[tool.uv.sources]
torch = [{ index = "pytorch-nightly-cu126" }]
torchvision = [{ index = "pytorch-nightly-cu126" }]
torchaudio = [{ index = "pytorch-nightly-cu126" }]
torchao = [{ index = "pytorch-nightly-cu126" }]

[[tool.uv.index]]
name = "pytorch-nightly-cu126"
url = "https://download.pytorch.org/whl/nightly/cu126"
explicit = true
```

```console
$ uv sync
Using CPython 3.11.9
Creating virtual environment at: .venv
⠦ Resolving dependencies...
warning: Missing version constraint (e.g., a lower bound) for `torch`
warning: Missing version constraint (e.g., a lower bound) for `torchao`
warning: Missing version constraint (e.g., a lower bound) for `torchaudio`
warning: Missing version constraint (e.g., a lower bound) for `torchvision`
 Updated https://github.com/tgale96/grouped_gemm (ebeae0b)
Resolved 20 packages in 7.79s
Installed 14 packages in 39.34s
 + filelock==3.16.1
 + fsspec==2024.12.0
 + jinja2==3.1.5
 + markupsafe==3.0.2
 + mpmath==1.3.0
 + networkx==3.4.2
 + numpy==2.2.1
 + pillow==11.1.0
 + sympy==1.13.1
 + torch==2.7.0.dev20250116+cu126
 + torchao==0.8.0.dev20250116+cu126
 + torchaudio==2.6.0.dev20250116+cu126
 + torchvision==0.22.0.dev20250116+cu126
 + typing-extensions==4.12.2
```

then second stage:

```console
$ TORCH_CUDA_ARCH_LIST=9.0 GROUPED_GEMM_CUTLASS=1 uv sync --extra compile --no-binary-package grouped_gemm
Resolved 21 packages in 5ms
   Built grouped-gemm @ git+https://github.com/tgale96/grouped_gemm@ebeae0bb3ded459886309b2a30410deb16937af4
Prepared 1 package in 13.65s
Installed 1 package in 60ms
 + grouped-gemm==0.1.6 (from git+https://github.com/tgale96/grouped_gemm@ebeae0bb3ded459886309b2a30410deb16937af4)

$ uv run python -c 'import grouped_gemm' ; echo $?
0
```

so to recap, I'm looking for a way to specify the following in `pyproject.toml`:

- `--no-binary-package grouped_gemm`
- env vars that should be used during grouped_gemm build
- explicit build time dependency between grouped_gemm and torch, so the build waits for and uses the torch-nightly package automatically
- not have to pollute my deps with `setuptools` just because grouped_gemm needs it

---

_Comment by @tmm1 on 2025-01-16 21:45_

Tangentially related, it would be nice to include a note on https://docs.astral.sh/uv/guides/integration/pytorch/ about the two-stage trick with a link to it on https://docs.astral.sh/uv/concepts/projects/config/#build-isolation

---

_Comment by @charliermarsh on 2025-01-17 05:10_

Generally agree -- improving this is on our roadmap. Thanks for the concrete example -- very helpful.

---

_Label `enhancement` added by @charliermarsh on 2025-01-17 05:10_

---

_Label `needs-design` added by @charliermarsh on 2025-01-17 05:10_

---

_Comment by @OldaKo on 2025-03-17 18:35_

@charliermarsh I'd like to add another vote for this as the two-step method is directly blocking one of my use cases. 
I'm using [clearml-agent](https://github.com/clearml/clearml-agent) which supports uv by automatically using `uv sync` on a uv lockfile (if present) instead of default automated package discovery. However, `sync` will fail due to `ModuleNotFound` during build if any package has build dependencies (pytorch3d in my case).
I could personally live with tracking the build dependencies in the environment myself but it would be great if `sync` installed packages listed in 
```
[tool.uv] 
no-build-isolation-package = []
```
only after everything else is installed or if there was a second round retry for failed package installations, which is the way [pdm](https://pdm-project.org/en/latest/) does it as far as I understand.

---

_Comment by @tobiasdiez on 2025-05-13 02:20_

Another use case are editable install of meson-python (tracked at https://github.com/astral-sh/uv/issues/10214): You need to have the build dependencies (say cython) installed at runtime as well, since meson-python may need to rebuild the project on-the-fly if you make changes to the source files. The usual way to do this is by specifying `no-build-isolation`. The two-stage solution works but is quite cumbersome - either one has to duplicate the build dependencies in an extra, or manual pip install these dependencies.

It would be simpler if uv could track build dependencies in the lock file as well, and automatically install those if you specify "no-build-isolation".

Another related problem that came up recently is the following: Cython released a new version that broke the build of some packages. While `uv.lock` locks the version of a dependency, it doesn't lock the version of the build dependencies used to build the dependency. So the build now actually broke although the lock file didn't change at all. Moreover, there currently doesn't seem to be an easy way to say "use Cython xyz to build all dependencies. Again, this would be fixed by locking all build dependencies (also those of dependencies) in `uv.lock`.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-16 15:52_

---

_Comment by @charliermarsh on 2025-08-16 16:59_

We've implemented a bunch of improvements here and I just drafted some updated documentation: https://github.com/astral-sh/uv/pull/15326.

For `grouped_gemm`, as an example, you should be able to do this in the latest release, which still uses an isolated build for `grouped_gemm`, but adds `torch` to the environment (and ensures that it's the same version of `torch` as is used at runtime, in the lockfile, etc.):
```toml
[project]
name = "torchnightly"
requires-python = ">=3.11.9"
version = "0.1.0"
dependencies = [
    "grouped_gemm",
    "torch>=2.7.0, <2.8",
    "torchao",
    "torchaudio",
    "torchvision",
    "nvidia-cusparselt-cu12",
]

[tool.uv.extra-build-dependencies]
grouped_gemm = [{ requirement = "torch", match-runtime = true }]

[tool.uv.sources]
torch = [{ index = "cu128" }]
torchvision = [{ index = "cu128" }]
torchaudio = [{ index = "cu128" }]
torchao = [{ index = "cu128" }]

[[tool.uv.index]]
name = "cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true
```

Alternatively, if you do:

```toml
[project]
name = "torchnightly"
requires-python = ">=3.11.9"
version = "0.1.0"
dependencies = [
    "grouped_gemm",
    "torch>=2.7.0, <2.8",
    "torchao",
    "torchaudio",
    "torchvision",
    "setuptools", # for grouped_gemm
    "nvidia-cusparselt-cu12",
]

[tool.uv]
no-build-isolation-package = ['grouped_gemm']

[tool.uv.sources]
torch = [{ index = "cu128" }]
torchvision = [{ index = "cu128" }]
torchaudio = [{ index = "cu128" }]
torchao = [{ index = "cu128" }]

[[tool.uv.index]]
name = "cu128"
url = "https://download.pytorch.org/whl/cu128"
explicit = true
```

This should also work with a single `uv sync` in the next release (not yet out), because by default, we now install anything marked as `no-build-isolation-package` in a second install step (at which point, `torch` will already be included in the environment).

We're generally recommending the first approach, since it retains build isolation everywhere (and doesn't require that the build dependencies, like `setuptools`, are included as project dependencies), but either should work with a single `uv sync`.


---

_Closed by @zanieb on 2025-08-18 21:15_

---
