```yaml
number: 9651
title: PyTorch nightly index makes uv download all torch versions in a loop
type: issue
state: closed
author: ViRb3
labels:
  - question
assignees: []
created_at: 2024-12-04T23:51:17Z
updated_at: 2025-01-06T00:59:50Z
url: https://github.com/astral-sh/uv/issues/9651
synced_at: 2026-01-10T04:36:21Z
```

# PyTorch nightly index makes uv download all torch versions in a loop

---

_Issue opened by @ViRb3 on 2024-12-04 23:51_

I copied the full PyTorch example from https://docs.astral.sh/uv/guides/integration/pytorch/#using-a-pytorch-index:

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cpu", marker = "platform_system != 'Darwin'" },
]
torchvision = [
    { index = "pytorch-cpu", marker = "platform_system != 'Darwin'" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

Only difference is I swapped the index url to `https://download.pytorch.org/whl/nightly/cpu`

When I run `uv sync`, it downloads all versions of torch in reverse order. It doesn't really stop until I Ctrl + C it. Without nightly, it works fine.

```bash
$ uv version
uv 0.5.6 (Homebrew 2024-12-03)
```

---

_Renamed from "PyTorch guide with nightly index makes uv download all torch versions in a loop" to "PyTorch nightly index makes uv download all torch versions in a loop" by @ViRb3 on 2024-12-04 23:51_

---

_Comment by @zanieb on 2024-12-05 01:06_

We're backtracking on the package because its dependencies can't be solved. Because of the limitations of the torch index, we can't inspect its metadata without downloading the full wheel.

With some verbose logs..

```
TRACE Selecting candidate for pytorch-triton with range ==3.2.0+git35c6c7c6 with 1 remote versions
TRACE Exhausted all candidates for package pytorch-triton with range ==3.2.0+git35c6c7c6 after 0 steps
```

There are `pytorch-triton` [wheels available](https://download.pytorch.org/whl/nightly/pytorch-triton/) with a matching version, but your index is set to `explicit`. You'll want to use the index for it, e.g.

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
  "torch>=2.5.1", "pytorch-triton",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cpu"},
]
torchvision = [
    { index = "pytorch-cpu"},
]
pytorch-triton = [
    { index = "pytorch-cpu"},
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/nightly/cpu"
explicit = true
```

---

_Label `question` added by @zanieb on 2024-12-05 01:06_

---

_Comment by @charliermarsh on 2024-12-05 01:08_

A big part of the problem here (I assume) is that the PyTorch registry has no fast-path for metadata. So you literally have to download every version (in full) to understand the dependencies at each stage.

---

_Comment by @ViRb3 on 2024-12-05 01:11_

@zanieb Thanks for the suggestion! I tried that, but it fails on macOS with the following error:
> error: Distribution `pytorch-triton==3.2.0+git35c6c7c6 @ registry+https://download.pytorch.org/whl/nightly/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform

Removing `pytorch-triton` from dependencies sends it into the torch download loop again...

---

_Comment by @zanieb on 2024-12-05 01:13_

I removed the != Darwin markers to debug, you should restore those (and include it for triton).

---

_Comment by @ViRb3 on 2024-12-05 01:21_

That worked, thanks! Although, why did it work? As far as I understand, we are specifying to use the nightly index ONLY IF `platform_system != 'Darwin'`. However, I _am_ on Darwin, so it should fall back to the default pip index, and the nightly index is left unused?

---

_Comment by @zanieb on 2024-12-05 01:27_

During lock, we'll solve for Darwin / not Darwin so there will be two versions

```
❯ uv lock
Resolved 27 packages in 516ms
Updated pytorch-triton v3.2.0+git35c6c7c6 -> v0.0.1, v3.2.0+git35c6c7c6
Updated torch v2.6.0.dev20241204+cpu -> v2.5.1, v2.6.0.dev20241204+cpu
```

And yeah, the Darwin version will come from PyPI instead of the Pytorch CPU index (which only supports Linux).

During sync, we install a locked version appropriate for the platform.

```
❯ uv sync
...
 + pytorch-triton==0.0.1
 + torch==2.5.1
...
```

---

_Comment by @zanieb on 2024-12-05 01:29_

You can remove the extraneous `pytorch-triton` install (that's just a dummy package) by declaring the dependency as `"pytorch-triton; platform_system != 'Darwin'",`

---

_Comment by @ViRb3 on 2024-12-05 01:35_

Interesting. I was following Apple's guide on installing PyTorch nightly (https://developer.apple.com/metal/pytorch/) and they specify the following command:

```
pip3 install --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/cpu
```

If I run this using `uv pip`, it installs the following:

```
torch==2.6.0.dev20241126
torchaudio==2.5.0.dev20241126
torchvision==0.20.0.dev20241126
```

This definitely looks like nightly, so it surely should exist on the Pytorch CPU index? I was able to reproduce using the following:

```toml
dependencies = [
    "torch",
    "torchaudio",
    "torchvision",
]

[tool.uv]
environments = [
    "platform_system == 'Darwin'",
]

[tool.uv.sources]
torch = { url = "https://download.pytorch.org/whl/nightly/cpu/torch-2.6.0.dev20241126-cp312-none-macosx_11_0_arm64.whl" }
torchaudio = { url = "https://download.pytorch.org/whl/nightly/cpu/torchaudio-2.5.0.dev20241126-cp312-cp312-macosx_11_0_arm64.whl" }
torchvision = { url = "https://download.pytorch.org/whl/nightly/cpu/torchvision-0.20.0.dev20241126-cp312-cp312-macosx_11_0_arm64.whl" }
```

I'm honestly not sure what the `environments` part does, I saw it from another GitHub issue since without it I get this error:

```
  × No solution found when resolving dependencies for split (platform_system != 'Darwin' and python_full_version >= '3.12' and python_full_version < '4'):
  ╰─▶ Because there is no version of pytorch-triton{python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'}==3.2.0+git35c6c7c6 and torch==2.6.0.dev20241126 depends on pytorch-triton{python_full_version < '3.13' and platform_machine
      == 'x86_64' and platform_system == 'Linux'}==3.2.0+git35c6c7c6, we can conclude that torch==2.6.0.dev20241126 cannot be used.
      And because only torch==2.6.0.dev20241126 is available and your project depends on torch, we can conclude that your project's requirements are unsatisfiable.
```

Specifying a dependency `pytorch-triton; platform_system != 'Darwin'` does not help in this case.

Does this error throw because I am hardcoding macOS sources, while uv is trying to solve for all OSes so their dependencies can be included in the lock file?

---

_Comment by @zanieb on 2024-12-05 02:54_

```
environments = [
    "platform_system == 'Darwin'",
]
```

means "only lock for Darwin". https://docs.astral.sh/uv/concepts/projects/config/#limited-resolution-environments

With `uv pip`, we only solve for the current platform. https://docs.astral.sh/uv/concepts/resolution/#platform-specific-resolution

I believe the `+cpu` suffixes on the versions are messing up our resolution. The nightly index seems to have different distributions. I'm not sure what the workaround is for that, @charliermarsh would probably know. I'd try to designate a version without `+cpu` for Darwin and with `+cpu` elsewhere.

---

_Comment by @zanieb on 2024-12-05 03:18_

Okay trial and error, the proper invocation is

```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.12.0"
dependencies = [
  "torch===2.6.0.dev20241204; platform_system == 'Darwin'",
  "torchvision===0.20.0.dev20241205; platform_system == 'Darwin'",
  "torch==2.6.0.dev20241204+cpu; platform_system != 'Darwin'",
  "torchvision==0.20.0.dev20241205+cpu; platform_system != 'Darwin'",
  "pytorch-triton; platform_system != 'Darwin'",
]

[tool.uv.sources]
torch = [
    { index = "pytorch-cpu" },
]
torchvision = [
    { index = "pytorch-cpu" },
]
pytorch-triton = [
    { index = "pytorch-cpu" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/nightly"
explicit = true
```

Sorry it's such a pain, the torch indexes are very inconsistent in their versioning and distribution availability per platform.


---

_Comment by @ViRb3 on 2024-12-07 20:30_

Got it, makes sense. Thanks for the help!

---

_Closed by @ViRb3 on 2024-12-07 20:30_

---

_Comment by @charliermarsh on 2025-01-06 00:59_

This should be _way_ better in the next release.

---
