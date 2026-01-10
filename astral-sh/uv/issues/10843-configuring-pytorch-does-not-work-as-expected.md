---
number: 10843
title: Configuring PyTorch does not work as expected
type: issue
state: closed
author: Jsakkos
labels:
  - question
assignees: []
created_at: 2025-01-22T05:48:06Z
updated_at: 2025-01-22T16:53:45Z
url: https://github.com/astral-sh/uv/issues/10843
synced_at: 2026-01-10T01:24:58Z
---

# Configuring PyTorch does not work as expected

---

_Issue opened by @Jsakkos on 2025-01-22 05:48_

### Summary

Following the guide here https://docs.astral.sh/uv/guides/integration/pytorch/#configuring-accelerators-with-optional-dependencies. 
Running `uv sync --extra cpu` works, but I can't get the gpu dependencies to work consistently.
```
 uv sync --extra cu124
Resolved 64 packages in 3ms
error: Distribution `torch==2.5.1 @ registry+https://download.pytorch.org/whl/cu124` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Windows (`win_amd64`), but `torch` (v2.5.1) only has wheels for the following platform: `linux_aarch64
```
from my pyproject.toml file:

```
requires-python = ">=3.9"
dependencies = [
    "configparser>=7.1.0",
    "ffmpeg>=1.4",
    "loguru>=0.7.2",
    "openai-whisper>=20240930",
    "opensubtitlescom>=0.1.5",
    "pytesseract>=0.3.13",
    "rapidfuzz>=3.10.1",
    "requests>=2.32.3",
    "tmdb-client>=0.0.1",
    "torch>=2.5.1",
    "torchaudio>=2.5.1",
    "torchvision>=0.20.1",
    "wave>=0.0.2",
]
[project.optional-dependencies]
cpu = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
  "torchaudio>=2.5.1",
]
cu124 = [
  "torch>=2.5.1",
  "torchvision>=0.20.1",
    "torchaudio>=2.5.1",
]

[tool.uv]
conflicts = [
  [
    { extra = "cpu" },
    { extra = "cu124" },
  ],
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchvision = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]
torchaudio = [
  { index = "pytorch-cpu", extra = "cpu" },
  { index = "pytorch-cu124", extra = "cu124" },
]

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```


### Platform

Windows 10 AMD

### Version

uv 0.5.22 (4574ced37 2025-01-21)

### Python version

Python 3.9.20

---

_Label `bug` added by @Jsakkos on 2025-01-22 05:48_

---

_Comment by @zanieb on 2025-01-22 06:35_

It looks like they only publish torch cu124 wheels for Windows on Python 3.10+

[torch-2.5.1+cu124-cp310-cp310-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp310-cp310-linux_x86_64.whl#sha256=9dde30f399ca22137455cca4d47140dfb7f4176e2d16a9729fc044eebfadb13a)
[torch-2.5.1+cu124-cp310-cp310-win_amd64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp310-cp310-win_amd64.whl#sha256=6f99d8459369cfd6661c2aee14787592fe50156a33faf9ef643ba04e42d6543f)
[torch-2.5.1+cu124-cp311-cp311-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-linux_x86_64.whl#sha256=6b2966ede9affe2fd69e0765691ca723ec870e0c34c7761f4d5b8e318383fdaf)
[torch-2.5.1+cu124-cp311-cp311-win_amd64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-win_amd64.whl#sha256=6c8a7003ef1327479ede284b6e5ab3527d3900c2b2d401af15bcc50f2245a59f)
[torch-2.5.1+cu124-cp312-cp312-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-linux_x86_64.whl#sha256=bf6484bfe5bc4f92a4a1a1bf553041505e19a911f717065330eb061afe0e14d7)
[torch-2.5.1+cu124-cp312-cp312-win_amd64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-win_amd64.whl#sha256=3c3f705fb125edbd77f9579fa11a138c56af8968a10fc95834cdd9fdf4f1f1a6)
[torch-2.5.1+cu124-cp313-cp313-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp313-cp313-linux_x86_64.whl#sha256=e9bebf91ede89267577911da4b0709ac6113a0cff6a1c2202c046b1ec2a51601)
[torch-2.5.1+cu124-cp39-cp39-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp39-cp39-linux_x86_64.whl#sha256=d681b8be3fdc2cd41112310db3c3904f7c6a09a7ae28d042ae0af3af01c8fcda)
[torch-2.5.1+cu124-cp39-cp39-win_amd64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp39-cp39-win_amd64.whl#sha256=9036c4372dec409842a80965d94b7b0fb4298e0967ceb03336a42c83778faa6f)
[torch-2.5.1-cp310-cp310-linux_aarch64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1-cp310-cp310-linux_aarch64.whl#sha256=d468d0eddc188aa3c1e417ec24ce615c48c0c3f592b0354d9d3b99837ef5faa6)
[torch-2.5.1-cp311-cp311-linux_aarch64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1-cp311-cp311-linux_aarch64.whl#sha256=e080353c245b752cd84122e4656261eee6d4323a37cfb7d13e0fffd847bae1a3)
[torch-2.5.1-cp312-cp312-linux_aarch64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1-cp312-cp312-linux_aarch64.whl#sha256=302041d457ee169fd925b53da283c13365c6de75c6bb3e84130774b10e2fbb39)
[torch-2.5.1-cp39-cp39-linux_aarch64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1-cp39-cp39-linux_aarch64.whl#sha256=012887a6190e562cb266d2210052c5deb5113f520a46dc2beaa57d76144a0e9b)

cc @charliermarsh this hint seems to highlight the wrong thing; fixable?

---

_Label `bug` removed by @zanieb on 2025-01-22 06:35_

---

_Label `question` added by @zanieb on 2025-01-22 06:35_

---

_Comment by @Jsakkos on 2025-01-22 06:48_

Isn't it at the bottom of the other cu124 options (see bolded link)?
> It looks like they only publish torch cu124 wheels for Windows on Python 3.10+
> 
[torch-2.5.1+cu124-cp310-cp310-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp310-cp310-linux_x86_64.whl#sha256=9dde30f399ca22137455cca4d47140dfb7f4176e2d16a9729fc044eebfadb13a)
[torch-2.5.1+cu124-cp310-cp310-win_amd64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp310-cp310-win_amd64.whl#sha256=6f99d8459369cfd6661c2aee14787592fe50156a33faf9ef643ba04e42d6543f)
[torch-2.5.1+cu124-cp311-cp311-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-linux_x86_64.whl#sha256=6b2966ede9affe2fd69e0765691ca723ec870e0c34c7761f4d5b8e318383fdaf)
[torch-2.5.1+cu124-cp311-cp311-win_amd64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp311-cp311-win_amd64.whl#sha256=6c8a7003ef1327479ede284b6e5ab3527d3900c2b2d401af15bcc50f2245a59f)
[torch-2.5.1+cu124-cp312-cp312-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-linux_x86_64.whl#sha256=bf6484bfe5bc4f92a4a1a1bf553041505e19a911f717065330eb061afe0e14d7)
[torch-2.5.1+cu124-cp312-cp312-win_amd64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp312-cp312-win_amd64.whl#sha256=3c3f705fb125edbd77f9579fa11a138c56af8968a10fc95834cdd9fdf4f1f1a6)
[torch-2.5.1+cu124-cp313-cp313-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp313-cp313-linux_x86_64.whl#sha256=e9bebf91ede89267577911da4b0709ac6113a0cff6a1c2202c046b1ec2a51601)
[torch-2.5.1+cu124-cp39-cp39-linux_x86_64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp39-cp39-linux_x86_64.whl#sha256=d681b8be3fdc2cd41112310db3c3904f7c6a09a7ae28d042ae0af3af01c8fcda)
**[torch-2.5.1+cu124-cp39-cp39-win_amd64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1%2Bcu124-cp39-cp39-win_amd64.whl#sha256=9036c4372dec409842a80965d94b7b0fb4298e0967ceb03336a42c83778faa6f)**
[torch-2.5.1-cp310-cp310-linux_aarch64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1-cp310-cp310-linux_aarch64.whl#sha256=d468d0eddc188aa3c1e417ec24ce615c48c0c3f592b0354d9d3b99837ef5faa6)
[torch-2.5.1-cp311-cp311-linux_aarch64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1-cp311-cp311-linux_aarch64.whl#sha256=e080353c245b752cd84122e4656261eee6d4323a37cfb7d13e0fffd847bae1a3)
[torch-2.5.1-cp312-cp312-linux_aarch64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1-cp312-cp312-linux_aarch64.whl#sha256=302041d457ee169fd925b53da283c13365c6de75c6bb3e84130774b10e2fbb39)
[torch-2.5.1-cp39-cp39-linux_aarch64.whl](https://download.pytorch.org/whl/cu124/torch-2.5.1-cp39-cp39-linux_aarch64.whl#sha256=012887a6190e562cb266d2210052c5deb5113f520a46dc2beaa57d76144a0e9b)


---

_Comment by @zanieb on 2025-01-22 06:56_

Oh! Classic sorting confusion — thanks for the correction.

It looks like we're selecting 2.5.1 instead of 2.5.1+cpu124, I'm not sure if this is a problem with the resolver or torch's metdata. The relevant lock section (if I resolve locally) looks like:


```
    { name = "torch", version = "2.5.1", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "(platform_machine == 'aarch64' and sys_platform == 'linux' and extra == 'extra-7-example-cpu') or (platform_machine != 'aarch64' and extra == 'extra-7-example-cpu' and extra == 'extra-7-example-cu124') or (sys_platform == 'darwin' and extra == 'extra-7-example-cpu') or (sys_platform != 'linux' and extra == 'extra-7-example-cpu' and extra == 'extra-7-example-cu124')" },
    { name = "torch", version = "2.5.1", source = { registry = "https://download.pytorch.org/whl/cu124" }, marker = "(platform_machine == 'aarch64' and sys_platform == 'linux' and extra == 'extra-7-example-cu124') or (platform_machine != 'aarch64' and extra == 'extra-7-example-cpu' and extra == 'extra-7-example-cu124') or (sys_platform != 'linux' and extra == 'extra-7-example-cpu' and extra == 'extra-7-example-cu124')" },
    { name = "torch", version = "2.5.1", source = { registry = "https://pypi.org/simple" }, marker = "(extra == 'extra-7-example-cpu' and extra == 'extra-7-example-cu124') or (extra != 'extra-7-example-cpu' and extra != 'extra-7-example-cu124')" },
    { name = "torch", version = "2.5.1+cpu", source = { registry = "https://download.pytorch.org/whl/cpu" }, marker = "(platform_machine != 'aarch64' and sys_platform == 'linux' and extra == 'extra-7-example-cpu') or (sys_platform != 'darwin' and sys_platform != 'linux' and extra == 'extra-7-example-cpu') or (sys_platform == 'darwin' and extra == 'extra-7-example-cpu' and extra == 'extra-7-example-cu124') or (sys_platform == 'linux' and extra == 'extra-7-example-cpu' and extra == 'extra-7-example-cu124')" },
    { name = "torch", version = "2.5.1+cu124", source = { registry = "https://download.pytorch.org/whl/cu124" }, marker = "(platform_machine != 'aarch64' and extra == 'extra-7-example-cu124') or (sys_platform != 'linux' and extra == 'extra-7-example-cu124') or (extra == 'extra-7-example-cpu' and extra == 'extra-7-example-cu124')" },
````

Can you share verbose logs for `uv lock` and `uv sync`? It'd also be helpful to share your lockfile.

---

_Comment by @Jsakkos on 2025-01-22 07:05_

Here is uv sync --extra cu124 --verbose:
```
←[34mDEBUG←[39m uv 0.5.22 (4574ced37 2025-01-21)
←[34mDEBUG←[39m Found project root: `D:\mkv-episode-matcher`
←[34mDEBUG←[39m No workspace root found, using project root
←[34mDEBUG←[39m Reading Python requests from version file at `D:\mkv-episode-matcher\.python-version`
←[34mDEBUG←[39m Using Python request `3.9` from version file at `.python-version`
←[34mDEBUG←[39m The virtual environment's Python version satisfies `3.9`
←[34mDEBUG←[39m Using request timeout of 30s
←[34mDEBUG←[39m Found static `requires-dist` for: D:\mkv-episode-matcher\
←[34mDEBUG←[39m No workspace root found, using project root
←[34mDEBUG←[39m Existing `uv.lock` satisfies workspace requirements
Resolved 64 packages in 4ms
error: Distribution `torch==2.5.1 @ registry+https://download.pytorch.org/whl/cu124` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Windows (`win_amd64`), but `torch` (v2.5.1) only has wheels for the following platform: `linux_aarch64`
```
I've already run uv sync --extra cpu (successfully), but here is the output if I run it again:

```
(base) PS D:\mkv-episode-matcher> uv sync --extra cpu --verbose
←[34mDEBUG←[39m uv 0.5.22 (4574ced37 2025-01-21)
←[34mDEBUG←[39m Found project root: `D:\mkv-episode-matcher`
←[34mDEBUG←[39m No workspace root found, using project root
←[34mDEBUG←[39m Reading Python requests from version file at `D:\mkv-episode-matcher\.python-version`
←[34mDEBUG←[39m Using Python request `3.9` from version file at `.python-version`
←[34mDEBUG←[39m The virtual environment's Python version satisfies `3.9`
←[34mDEBUG←[39m Using request timeout of 30s
←[34mDEBUG←[39m Found static `requires-dist` for: D:\mkv-episode-matcher\
←[34mDEBUG←[39m No workspace root found, using project root
←[34mDEBUG←[39m Existing `uv.lock` satisfies workspace requirements
Resolved 64 packages in 4ms
DEBUG Using request timeout of 30s
DEBUG Requirement already installed: mkv-episode-matcher==0.3.6.post1.dev2+ged28bc0.d20250122 (from file:///D:/mkv-episode-matcher)
DEBUG Requirement already installed: chardet==5.2.0
DEBUG Requirement already installed: pytest==8.3.3
DEBUG Requirement already installed: pytest-cov==6.0.0
DEBUG Requirement already installed: ruff==0.8.0
DEBUG Requirement already installed: configparser==7.1.0
DEBUG Requirement already installed: ffmpeg==1.4
DEBUG Requirement already installed: loguru==0.7.2
DEBUG Requirement already installed: openai-whisper==20240930
DEBUG Requirement already installed: opensubtitlescom==0.1.5
DEBUG Requirement already installed: pytesseract==0.3.13
DEBUG Requirement already installed: rapidfuzz==3.10.1
DEBUG Requirement already installed: requests==2.32.3
DEBUG Requirement already installed: tmdb-client==0.0.1
DEBUG Identified uncached distribution: torch==2.5.1+cpu
DEBUG Requirement already installed: torchaudio==2.5.1+cpu
DEBUG Requirement already installed: torchvision==0.20.1+cpu
DEBUG Requirement already installed: wave==0.0.2
DEBUG Requirement already installed: colorama==0.4.6
DEBUG Requirement already installed: exceptiongroup==1.2.2
DEBUG Requirement already installed: iniconfig==2.0.0
DEBUG Requirement already installed: packaging==24.2
DEBUG Requirement already installed: pluggy==1.5.0
DEBUG Requirement already installed: tomli==2.1.0
DEBUG Requirement already installed: coverage==7.6.8
DEBUG Requirement already installed: win32-setctime==1.1.0
DEBUG Requirement already installed: more-itertools==10.5.0
DEBUG Requirement already installed: numba==0.60.0
DEBUG Requirement already installed: numpy==2.0.2
DEBUG Requirement already installed: tiktoken==0.8.0
DEBUG Requirement already installed: tqdm==4.67.1
DEBUG Requirement already installed: prettytable==3.12.0
DEBUG Requirement already installed: pillow==11.0.0
DEBUG Requirement already installed: certifi==2024.8.30
DEBUG Requirement already installed: charset-normalizer==3.4.0
DEBUG Requirement already installed: idna==3.10
DEBUG Requirement already installed: urllib3==2.0.7
DEBUG Requirement already installed: pydantic==2.9.2
DEBUG Requirement already installed: python-dateutil==2.9.0.post0
DEBUG Requirement already installed: typing-extensions==4.12.2
DEBUG Requirement already installed: filelock==3.16.1
DEBUG Requirement already installed: fsspec==2024.10.0
DEBUG Requirement already installed: jinja2==3.1.4
DEBUG Requirement already installed: networkx==3.2.1
DEBUG Requirement already installed: sympy==1.13.1
DEBUG Requirement already installed: llvmlite==0.43.0
DEBUG Requirement already installed: regex==2024.11.6
DEBUG Requirement already installed: wcwidth==0.2.13
DEBUG Requirement already installed: annotated-types==0.7.0
DEBUG Requirement already installed: pydantic-core==2.23.4
DEBUG Requirement already installed: six==1.16.0
DEBUG Requirement already installed: markupsafe==3.0.2
DEBUG Requirement already installed: mpmath==1.3.0
DEBUG No cache entry for: https://download.pytorch.org/whl/cpu/torch-2.5.1%2Bcpu-cp39-cp39-win_amd64.whl
Prepared 1 package in 20.53s
warning: Failed to uninstall package at .venv\Lib\site-packages\torch-2.5.1+cu124.dist-info due to missing `RECORD` file. Installation may result in an incomplete environment.
DEBUG Uninstalled torch (10785 files, 449 directories)
Uninstalled 2 packages in 2.03s
DEBUG Failed to hardlink `D:\mkv-episode-matcher\.venv\Lib\site-packages\functorch\compile\__init__.py` to `C:\Users\Jonathan\AppData\Local\uv\cache\archive-v0\kyemyoyFliIs819VqENlI\functorch\compile\__init__.py`, attempting to copy files as a fallback
warning: Failed to hardlink files; falling back to full copy. This may lead to degraded performance.
         If the cache and target directories are on different filesystems, hardlinking may not be supported.
         If this is intentional, set `export UV_LINK_MODE=copy` or use `--link-mode=copy` to suppress this warning.
Installed 1 package in 9.16s
 - torch==2.5.1
 - torch==2.5.1+cu124
 + torch==2.5.1+cpu
```
uv.lock file attached.
[uv.txt](https://github.com/user-attachments/files/18501181/uv.txt)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-22 13:31_

---

_Comment by @charliermarsh on 2025-01-22 13:31_

I can take a look.

---

_Comment by @charliermarsh on 2025-01-22 15:58_

Your lockfile does not look like what I get if I run `uv lock` from scratch. Can you just confirm that `rm uv.lock` and `uv lock` does _not_ resolve this?

---

_Comment by @Jsakkos on 2025-01-22 16:05_

Thanks @charliermarsh! Deleting the lock file and regenerating it resolved my issue.

---

_Closed by @charliermarsh on 2025-01-22 16:53_

---

_Comment by @charliermarsh on 2025-01-22 16:53_

Okay, great. I think this was likely a bug that we fixed in v0.5.22, but it didn't trigger a re-resolution since it was just a behavior change on our side (rather than a change in the input dependencies).

---
