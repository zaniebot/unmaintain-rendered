```yaml
number: 8930
title: uv failing to create multi-platform env for torch
type: issue
state: closed
author: sharmuz
labels:
  - question
assignees: []
created_at: 2024-11-08T10:27:40Z
updated_at: 2024-11-25T23:34:51Z
url: https://github.com/astral-sh/uv/issues/8930
synced_at: 2026-01-12T15:59:38Z
```

# uv failing to create multi-platform env for torch

---

_@sharmuz_

**uv version:** ~~0.5.0~~ 0.4.28
**OS/platform:** macOS Sonoma 14.7.1 (on a MBP 2020 Intel)

Hey folks - YAO pytorch issue :)

I'm attempting to create an env which will install a given `torch` and `torchvision` version from a specified index, depending on whether the OS is darwin/win/linux. 

I'm going off of the info in #6523 and [#8746](https://github.com/astral-sh/uv/issues/8746#issuecomment-2452438775).

Here's a minimal excerpt of my `pyproject.toml`:

```toml
[project]
name = "minimal"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.10.*"
dependencies = [
  "torch==1.13.1 ; platform_system == 'darwin'",
  "torch==1.13.1+cpu ; platform_system == 'win32'",
  "torch==1.13.1+cu116 ; platform_system == 'linux'",
  "torchvision==0.14.1 ; platform_system == 'darwin'",
  "torchvision==0.14.1+cpu ; platform_system == 'win32'",
  "torchvision==0.14.1+cu116 ; platform_system == 'linux'",
]

[tool.uv.sources]
torch = [
  { index = "torch-cpu", marker = "platform_system != 'linux'" },
  { index = "torch-cuda", marker = "platform_system == 'linux'" },
]
torchvision = [
  { index = "torch-cpu", marker = "platform_system != 'linux'" },
  { index = "torch-cuda", marker = "platform_system == 'linux'" },
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-cuda"
url = "https://download.pytorch.org/whl/cu116"
explicit = true
```

`uv sync` runs without error but the generated `uv.lock` looks off:

```toml
[[package]]
name = "torch"
version = "1.13.1"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "platform_system == 'darwin'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'darwin'" },
]

[[package]]
name = "torch"
version = "1.13.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "platform_system != 'darwin' and platform_system != 'linux' and platform_system != 'win32'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system != 'darwin' and platform_system != 'linux' and platform_system != 'win32'" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/81/58/431fd405855553af1a98091848cf97741302416b01462bbf9909d3c422b3/torch-1.13.1-cp310-cp310-manylinux1_x86_64.whl", hash = "sha256:fd12043868a34a8da7d490bf6db66991108b00ffbeecb034228bfcbbd4197143", size = 887450534 },
    { url = "https://files.pythonhosted.org/packages/2c/45/43233d36e8e7ec5588fa802bb098337ae73c314863190c68797287f2fbdd/torch-1.13.1-cp310-cp310-manylinux2014_aarch64.whl", hash = "sha256:d9fe785d375f2e26a5d5eba5de91f89e6a3be5d11efb497e76705fdf93fa3c2e", size = 60541101 },
    { url = "https://files.pythonhosted.org/packages/33/bd/e174e6737daba03f8eaa7c051b9971d361022eb37b86cbe5db0b08cab00e/torch-1.13.1-cp310-cp310-win_amd64.whl", hash = "sha256:98124598cdff4c287dbf50f53fb455f0c1e3a88022b39648102957f3445e9b76", size = 162597315 },
    { url = "https://files.pythonhosted.org/packages/82/d8/0547f8a22a0c8aeb7e7e5e321892f1dcf93ea021829a99f1a25f1f535871/torch-1.13.1-cp310-none-macosx_10_9_x86_64.whl", hash = "sha256:393a6273c832e047581063fb74335ff50b4c566217019cc6ace318cd79eb0566", size = 135290591 },
    { url = "https://files.pythonhosted.org/packages/24/45/61e41ef8a84e1d6200ff10b7cb87e23e211599ab62420396a363295f973c/torch-1.13.1-cp310-none-macosx_11_0_arm64.whl", hash = "sha256:0122806b111b949d21fa1a5f9764d1fd2fcc4a47cb7f8ff914204fd4fc752ed5", size = 53188434 },
]

[[package]]
name = "torch"
version = "1.13.1+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "platform_system == 'win32'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'win32'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1%2Bcpu-cp310-cp310-linux_x86_64.whl", hash = "sha256:11692523b87c45b79ddfb5148b12a713d85235d399915490d94e079521f7e014" },
]

[[package]]
name = "torch"
version = "1.13.1+cu116"
source = { registry = "https://download.pytorch.org/whl/cu116" }
resolution-markers = [
    "platform_system == 'linux'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'linux'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu116/torch-1.13.1%2Bcu116-cp310-cp310-linux_x86_64.whl", hash = "sha256:51d5870cdf05b6208b1c739fe0ba511b977eca37f9507829675596acc11b6ca4" },
]
```
(equivalent output for torchvision)

I see two problems:

1. `torch==1.13.1+cpu` is marked as for win32 but the binary listed is for linux
2. The entry for `torch==1.13.1`for darwin lists no binary, even though this is definitely present (I've tested with [the url](https://download.pytorch.org/whl/cpu/torch-1.13.1-cp310-none-macosx_10_9_x86_64.whl) supplied directly instead of index

I've tried:

- Same as above but with torch 2.2.2 and torchvision 0.17.2 --> same result
- Specifying `index = "torch-cpu"` as only for win32 (so darwin should use pypi) --> 1) still a prob
- Specifying the direct url for win32 and using `index = "torch-cpu"` as only for darwin --> 2) still a prob

Am I missing something? Ideally I'd like `pyproject.toml` to not need any direct url sources and work for mac-x86/mac-arm/win/linux-cuda

---

_Comment by @FishAlchemist on 2024-11-08 14:02_

In reality, it should be quite rare to see ``win32``  for ``platform_system``, as it's not difficult to replace ``win32`` with ``Windows`` in its judgment.
![image](https://github.com/user-attachments/assets/1bbf1706-5391-4c9f-86fa-808e1035fef4)

https://docs.python.org/3.10/library/platform.html#platform.system
![image](https://github.com/user-attachments/assets/02c94699-fac4-4e3d-85b1-352fd5920855)
https://docs.astral.sh/uv/concepts/dependencies/#dependency-specifiers-pep-508

To generate more precise results, I recommend using ``Linux``, ``Windows``, or ``Darwin`` as the marker value. Please note that these values are **case-sensitive**.

The value ``win32`` is associated with the marker ``sys_platform``.
https://docs.python.org/3.10/library/sys.html#sys.platform
![image](https://github.com/user-attachments/assets/e9b5ca33-66ce-44fd-bce3-256fd2b84f63)

Edit:
I'm unsure if ``platform_system`` is case-sensitive, but using the correct capitalization for the lock file is usually  best.

---

_Comment by @sharmuz on 2024-11-08 17:35_

Thanks @FishAlchemist - that did the trick!

Looks like **case-sensitivity is important for platform_system**. FWIW I originally was using sys_platform instead (hence the win32) but still had issues (can't recall if were same).

Might be worth updating the [dependency docs](https://docs.astral.sh/uv/concepts/dependencies) to use platform_system? Just a thought

Anyway here's my working pyproject.toml in case is of use:

```toml
[project]
name = "minimal"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.10.*"
dependencies = [
  "torch==1.13.1 ; platform_system == 'Darwin'",
  "torch==1.13.1+cpu ; platform_system == 'Windows'",
  "torch==1.13.1+cu116 ; platform_system == 'Linux'",
  "torchvision==0.14.1 ; platform_system == 'Darwin'",
  "torchvision==0.14.1+cpu ; platform_system == 'Windows'",
  "torchvision==0.14.1+cu116 ; platform_system == 'Linux'",
]

[tool.uv.sources]
torch = [
  { index = "torch-cpu", marker = "platform_system != 'Linux'" },
  { index = "torch-cuda", marker = "platform_system == 'Linux'" },
]
torchvision = [
  { index = "torch-cpu", marker = "platform_system != 'Linux'" },
  { index = "torch-cuda", marker = "platform_system == 'Linux'" },
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-cuda"
url = "https://download.pytorch.org/whl/cu116"
explicit = true
```

(Btw I'm aware pytorch advise installing from pypi for macos - I just prefer this way)

---

_Closed by @sharmuz on 2024-11-08 17:35_

---

_Comment by @sharmuz on 2024-11-13 11:15_

Sorry to say I'm getting a new issue with this with 0.5.1 :(

uv fails to install the env, saying it cannot find a compatible file for `torch==1.13.1+cpu` for my platform (Mac/Darwin).

However, as you can see above it should not be attempting to install that version of torch on Mac. Scanning the lockfile I see that it has indeed erroneously added `platform_system == 'Darwin'` to the entry for `torch==1.13.1+cpu` instead of `torch==1.13.1`.

Downgrading to 0.5.0 did not resolve it. (I must've been mistaken with the ver I wrote at the top).

Changing the tool.uv.sources entry for torch so that `index = "torch-cpu"` is only for Windows does resolve it. I.e. Darwin installs from pypi. But before it worked from torch-cpu.

Downgrading to 0.4.28 *does* resolve it fully - it creates a lockfile consistent with the toml and installs the correct torch packages from pytorch.org for Mac.

---

_Reopened by @sharmuz on 2024-11-13 11:15_

---

_Comment by @FishAlchemist on 2024-11-13 11:26_

@sharmuz Can you provide a pyproject.toml file that can reproduce the issue you're facing?
Will this issue occur in version 0.4.29? Since 0.5.0 is a major update, confirming this issue in 0.4.29, the last version in the 0.4.X series, will help narrow down the scope of uv's bug-hunting efforts.

---

_Comment by @charliermarsh on 2024-11-13 15:44_

@sharmuz -- You likely need to do `torch==1.13.1, !==1.13.1+cpu ; platform_system == 'Darwin'`, or `torch===1.13.1 ; platform_system == 'Darwin'`.

`1.13.1+cpu` _is_ a valid version for `torch==1.13.1`, per the Python standards.

---

_Comment by @charliermarsh on 2024-11-13 15:49_

I believe this is a duplicate of https://github.com/astral-sh/uv/issues/9036.

---

_Comment by @sharmuz on 2024-11-18 16:22_

Okay using the following pyproject.toml:

```toml
[project]
name = "minimal"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = "==3.10.*"
dependencies = [
  "torch==1.13.1 ; platform_system == 'Darwin'",
  "torch==1.13.1+cpu ; platform_system == 'Windows'",
  "torch==1.13.1+cu116 ; platform_system == 'Linux'",
  "torchvision==0.14.1 ; platform_system == 'Darwin'",
  "torchvision==0.14.1+cpu ; platform_system == 'Windows'",
  "torchvision==0.14.1+cu116 ; platform_system == 'Linux'",
]

[project.scripts]
minimal = "minimal:main"

[tool.uv]
package = true

[tool.uv.sources]
torch = [
  { index = "torch-cpu", marker = "platform_system != 'Linux'" },
  { index = "torch-cuda", marker = "platform_system == 'Linux'" },
]
torchvision = [
  { index = "torch-cpu", marker = "platform_system != 'Linux'" },
  { index = "torch-cuda", marker = "platform_system == 'Linux'" },
]

[[tool.uv.index]]
name = "torch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "torch-cuda"
url = "https://download.pytorch.org/whl/cu116"
explicit = true

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

```

I get this in uv.lock for 0.4.28 **and** 0.4.29:

```toml
[[package]]
name = "torch"
version = "1.13.1"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "platform_system == 'Darwin'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'Darwin'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1-cp310-none-macosx_10_9_x86_64.whl", hash = "sha256:393a6273c832e047581063fb74335ff50b4c566217019cc6ace318cd79eb0566" },
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1-cp310-none-macosx_11_0_arm64.whl", hash = "sha256:0122806b111b949d21fa1a5f9764d1fd2fcc4a47cb7f8ff914204fd4fc752ed5" },
]

[[package]]
name = "torch"
version = "1.13.1+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "platform_system == 'Windows'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'Windows'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1%2Bcpu-cp310-cp310-linux_x86_64.whl", hash = "sha256:11692523b87c45b79ddfb5148b12a713d85235d399915490d94e079521f7e014" },
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1%2Bcpu-cp310-cp310-win_amd64.whl", hash = "sha256:207ab3700cd9c4349f4fd1892597eb3d385eb78221c0f2974ec54b8ea903aa00" },
]

[[package]]
name = "torch"
version = "1.13.1+cu116"
source = { registry = "https://download.pytorch.org/whl/cu116" }
resolution-markers = [
    "platform_system == 'Linux'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'Linux'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu116/torch-1.13.1%2Bcu116-cp310-cp310-linux_x86_64.whl", hash = "sha256:51d5870cdf05b6208b1c739fe0ba511b977eca37f9507829675596acc11b6ca4" },
]
```
(With equivalent entries for torchvision). This is as I would expect.

Attempting with uv 0.5.2:
```bash
error: Distribution `torch==1.13.1+cpu @ registry+https://download.pytorch.org/whl/cpu` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

If I alter the first dependency, per Charlie's suggestion to:
```toml
"torch==1.13.1, !=1.13.1+cpu ; platform_system == 'Darwin'"
```
(and equiv. for torchvision)

 I then get this in uv.lock for 0.5.2:

```toml
[[package]]
name = "torch"
version = "1.13.1"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "platform_system == 'Darwin'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'Darwin'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1-cp310-none-macosx_10_9_x86_64.whl", hash = "sha256:393a6273c832e047581063fb74335ff50b4c566217019cc6ace318cd79eb0566" },
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1-cp310-none-macosx_11_0_arm64.whl", hash = "sha256:0122806b111b949d21fa1a5f9764d1fd2fcc4a47cb7f8ff914204fd4fc752ed5" },
]

[[package]]
name = "torch"
version = "1.13.1+cpu"
source = { registry = "https://download.pytorch.org/whl/cpu" }
resolution-markers = [
    "platform_system == 'Windows'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'Windows'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1%2Bcpu-cp310-cp310-linux_x86_64.whl", hash = "sha256:11692523b87c45b79ddfb5148b12a713d85235d399915490d94e079521f7e014" },
    { url = "https://download.pytorch.org/whl/cpu/torch-1.13.1%2Bcpu-cp310-cp310-win_amd64.whl", hash = "sha256:207ab3700cd9c4349f4fd1892597eb3d385eb78221c0f2974ec54b8ea903aa00" },
]

[[package]]
name = "torch"
version = "1.13.1+cu116"
source = { registry = "https://download.pytorch.org/whl/cu116" }
resolution-markers = [
    "platform_system == 'Linux'",
]
dependencies = [
    { name = "typing-extensions", marker = "platform_system == 'Linux'" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu116/torch-1.13.1%2Bcu116-cp310-cp310-linux_x86_64.whl", hash = "sha256:51d5870cdf05b6208b1c739fe0ba511b977eca37f9507829675596acc11b6ca4" },
]
```
Which is good 😎 

So I guess now we need to explicitly forbid the +cpu version?

---

_Comment by @charliermarsh on 2024-11-20 00:14_

Yeah you have to explicitly forbid `+cpu` on macOS, since there is no `+cpu` wheel for macOS (all the macOS builds are assumed to be `+cpu`). I just published some docs around this with a few example configurations: https://docs.astral.sh/uv/guides/integration/pytorch/.

---

_Closed by @charliermarsh on 2024-11-20 00:14_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 00:14_

---

_Label `question` added by @charliermarsh on 2024-11-20 00:14_

---

_Comment by @ianmobbs on 2024-11-25 23:26_

Hi, thank you for the detailed response here. I am also seeing this behavior on macOS:
```
ianmobbs@ians-mbp code % mkdir testtorch
ianmobbs@ians-mbp code % cd testtorch
ianmobbs@ians-mbp testtorch % uv init
Initialized project `testtorch`
ianmobbs@ians-mbp testtorch % uv add torch
Resolved 23 packages in 4ms
error: Distribution `torch==2.5.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform
```

It seems like this is intended behavior based on the above discussion, but the same steps had been used to create a previous project. I attempted to follow the `uv pip` alias at the bottom of the blog post, but that errors too:
```ianmobbs@ians-mbp testtorch % uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cpu
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of torch are available:
          ...
          torch==2.5.1
          torch==2.5.1+cpu
      and torch<=0.4.1 has no wheels with a matching Python implementation tag, we can conclude that torch<0.1.6.post17 cannot be used.
      And because torch==0.4.1.post2 has no wheels with a matching Python ABI tag and torch>=1.0.0,<=1.2.0+cpu has no wheels with a matching Python
      implementation tag, we can conclude that torch<1.0.1 cannot be used.
      And because torch==1.3.0 has no wheels with a matching Python ABI tag and torch>=1.3.0+cpu,<=1.5.0+cpu has no wheels with a matching Python
      implementation tag, we can conclude that torch<1.3.0.post2 cannot be used.
      And because torch>=1.5.1 has no wheels with a matching Python ABI tag and you require torch, we can conclude that your requirements are
      unsatisfiable.
```
(This occurs even with a fresh `uv venv`).

Additionally, this `pyproject.toml` that was previously working for a different project is now broken:
```
[project]
name = "test"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
    "torch>=2.5.1",
]
```

Totally understand that this seems to be a quirk with Pytorch distribution and not necessarily a uv-specific issue. However, I am curious what the 'golden path' is for spinning up a greenfield project using uv and torch on macOS.

---

_Comment by @charliermarsh on 2024-11-25 23:34_

Do you know what Python version you're on? You may need to constraint to Python 3.12, since PyTorch doesn't ship Python 3.13 wheels.

There's an example configuration here: https://docs.astral.sh/uv/guides/integration/pytorch/#using-a-pytorch-index

---
