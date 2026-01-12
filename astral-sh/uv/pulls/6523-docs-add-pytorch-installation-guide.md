```yaml
number: 6523
title: "docs: Add PyTorch installation guide"
type: pull_request
state: merged
author: baggiponte
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs2024/pytorch-guide
created_at: 2024-08-23T14:54:28Z
updated_at: 2024-11-19T01:02:55Z
url: https://github.com/astral-sh/uv/pull/6523
synced_at: 2026-01-12T16:07:24Z
```

# docs: Add PyTorch installation guide

---

_@baggiponte_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

Hello there! First real docs PR for uv.

1. I expect this will be rewritten a gazillion times to have a consistent tone with the rest of the docs, despite me trying to stick to it as best as I could. Feel free to edit!
2. I went super on the verbose mode, while also providing a callout with a TLDR on top. Scrap anything you feel it's redundant!
3. I placed the guide under `integrations` since Charlie added the FastAPI integration there.

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Addresses #5945

## Test Plan

<!-- How was it tested? -->
I just looked at the docs on the dev server of mkdocs if it looked nice.

**I could not test the commands that I wrote work** outside of macOS. If someone among contributors has a Windows/Linux laptop, it should be enough, even for the GPU-supported versions: I expect the installation will just break once torch checks for CUDA (perhaps even at runtime).

---

_Assigned to @zanieb by @zanieb on 2024-08-23 15:05_

---

_Label `documentation` added by @zanieb on 2024-08-23 15:05_

---

_Comment by @FishAlchemist on 2024-08-23 15:50_

Have you considered including a way to specify package versions?
This part is the difference between uv and pip. 
https://github.com/astral-sh/uv/blob/main/docs/pip/compatibility.md#local-version-identifiers

---

_Comment by @baggiponte on 2024-08-23 16:25_

> Have you considered including a way to specify package versions? This part is the difference between uv and pip. [`main`/docs/pip/compatibility.md#local-version-identifiers](https://github.com/astral-sh/uv/blob/main/docs/pip/compatibility.md?rgh-link-date=2024-08-23T15%3A50%3A15Z#local-version-identifiers)

Like `uv add -- "torch==2.4.0+cpu"`? Didn't think about that: having tried on macOS, it simply fails. I just went with "let's just port to uv the pip commands that the torch docs recommend". Any suggestion on how I could try that?

---

_Comment by @zanieb on 2024-08-23 16:27_

This will definitely require validation from folks on other platforms. Thanks for starting though!

---

_Comment by @FishAlchemist on 2024-08-23 16:43_

> > Have you considered including a way to specify package versions? This part is the difference between uv and pip. [`main`/docs/pip/compatibility.md#local-version-identifiers](https://github.com/astral-sh/uv/blob/main/docs/pip/compatibility.md?rgh-link-date=2024-08-23T15%3A50%3A15Z#local-version-identifiers)
> 
> Like `uv add -- "torch==2.4.0+cpu"`? Didn't think about that: having tried on macOS, it simply fails. I just went with "let's just port to uv the pip commands that the torch docs recommend". Any suggestion on how I could try that?

On macOS, packages are obtained directly from PyPI, thus eliminating the need for local version identifiers.
![image](https://github.com/user-attachments/assets/ae44cb81-cf44-4430-9cc5-7ffbcd1d1149)
Therefore, only on macOS, we cannot test whether the usage within the project(uv add/remove) is correct.
I don't know how to correctly provide the PyTorch version with CUDA to UV.

---

_Comment by @baggiponte on 2024-08-23 17:10_

> > > Have you considered including a way to specify package versions? This part is the difference between uv and pip. [`main`/docs/pip/compatibility.md#local-version-identifiers](https://github.com/astral-sh/uv/blob/main/docs/pip/compatibility.md?rgh-link-date=2024-08-23T16%3A43%3A46Z#local-version-identifiers)
> > 
> > 
> > Like `uv add -- "torch==2.4.0+cpu"`? Didn't think about that: having tried on macOS, it simply fails. I just went with "let's just port to uv the pip commands that the torch docs recommend". Any suggestion on how I could try that?
> 
> On macOS, packages are obtained directly from PyPI, thus eliminating the need for local version identifiers. ![image](https://private-user-images.githubusercontent.com/48265002/361001169-ae44cb81-cf44-4430-9cc5-7ffbcd1d1149.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjQ0MzI2OTAsIm5iZiI6MTcyNDQzMjM5MCwicGF0aCI6Ii80ODI2NTAwMi8zNjEwMDExNjktYWU0NGNiODEtY2Y0NC00NDMwLTljYzUtN2ZmYmNkMWQxMTQ5LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA4MjMlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwODIzVDE2NTk1MFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWE3ZTI2MmRmNmMxZmVlZDRiODgxZDExMmY3ZGIzZTNkNGE0OGZkZGU1ZTEyNDRhOWFmZmQ4MmQyNmZmYzgwYTcmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.C9VaT_ZuKf-oN4ifbp6nZBWe--t3dY6zeFgShdwoYk0) Therefore, only on macOS, we cannot test whether the usage within the project(uv add/remove) is correct. I don't know how to correctly provide the PyTorch version with CUDA to UV.

Exactly! That's what I wrote on the macOS section. Sorry if I wasn't being clear.

---

I realised I can test this out on Colab for Linux 😈

1. Go on `colab.new`
2. Run:

```
!curl -LsSf https://astral.sh/uv/install.sh | sh
!source $HOME/.cargo/env bash
!/root/.cargo/bin/uv --version # doesn't find it on the $PATH, I guess I should restart the shell? idk
!/root/.cargo/bin/uv venv
```

Then:

```
# installs GPU cu12
!/root/.cargo/bin/uv pip install -- torch # works

# fails
!/root/.cargo/bin/uv pip install -- "torch==2.4.0+cpu" # fails

# works
!/root/.cargo/bin/uv pip install --extra-index-url=https://download.pytorch.org/whl/cpu -- torch
!/root/.cargo/bin/uv pip install --extra-index-url=https://download.pytorch.org/whl/cu118 -- torch
!/root/.cargo/bin/uv pip install --extra-index-url=https://download.pytorch.org/whl/cu121 -- torch
!/root/.cargo/bin/uv pip install --extra-index-url=https://download.pytorch.org/whl/cu124 -- torch
```

---

_Comment by @FishAlchemist on 2024-08-23 17:16_

Have uv considered adding the index-url to the pyproject.toml file when using uv add?
After adding PyTorch, when I added another package from PyPI, the lock file modified PyTorch to be installed from PyPI.
[lockfile.zip](https://github.com/user-attachments/files/16731853/lockfile.zip)
**command**
```powershell
 uv add --extra-index-url=https://download.pytorch.org/whl/cu121 torch torchvision torchaudio --no-sync
 uv add deep-translator
 ```

---

_Comment by @zanieb on 2024-08-23 18:02_

@FishAlchemist yes, it's on the roadmap https://github.com/astral-sh/uv/issues/171

---

_Comment by @FishAlchemist on 2024-08-23 18:10_

> @FishAlchemist yes, it's on the roadmap #171

@zanieb 
If it's not yet supported, it seems like we can't include the project API part in the PR's document. 
After all, when the source is not PyPI, the lock file might be unexpected.

---

_Comment by @baggiponte on 2024-08-23 18:15_

> > @FishAlchemist yes, it's on the roadmap #171
> 
> @zanieb If it's not yet supported, it seems like we can't include the project API part in the PR's document. After all, when the source is not PyPI, the lock file might be unexpected.

Uh yeah, just pushed a commit to remove all the mentions to modifying `pyproject.toml`.

So:

1. We might want to give this a spin on a Windows machine to make sure it works
2. Given that currently there's no mechanism to bind a specific package to a specific source, the only thing that can be documented in the docs is to run `uv pip install --extra-index-url=...` or `uv add --extra-index-url=...`, am I right?

---

_Comment by @FishAlchemist on 2024-08-24 04:13_

@baggiponte 
I think there's no problem with downloading PyTorch using "uv pip install" on Windows. Although I've only run CUDA 12.1, I was able to do simple tests using the installation method provided by PyTorch, just with the difference of using "uv pip". 
For more complex tasks, I switched to Linux because my Windows computer has insufficient memory.
As for the project, although the command can run on Windows, the locked file results are not what I expected.

For PyTorch, I still recommend including the specific version in the documentation. I remember seeing some issues in the past where problems only occurred when a specific version was specified, and I'm not sure if they have been fixed.

---

_Comment by @baggiponte on 2024-08-24 08:43_

> For more complex tasks, I switched to Linux because my Windows computer has insufficient memory.

Do you think I should try and/or cover some of those?

> As for the project, although the command can run on Windows, the locked file results are not what I expected.

Uhm, I guess this deserves an issue of its own?

> For PyTorch, I still recommend including the specific version in the documentation. I remember seeing some issues in the past where problems only occurred when a specific version was specified, and I'm not sure if they have been fixed.

Were those issues uv-related or just generic torch version problems? Because otherwise I would not be super inclined to add this kind of recommendation to the docs.

---

Unrelated: perhaps I could create a new repo and use github actions on various runners to see if everything works, if we need more complex installation tests.

---

_Comment by @FishAlchemist on 2024-08-24 09:46_

@baggiponte  If  lock file doesn't have a mac wheel, I'm unsure if ``uv sync`` can successfully execute on a Mac.
### Command (uv 0.3.3 (deea6025a 2024-08-23))
```powershell
uv init torch_uv -p 3.10
# Remember to enter the directory
uv python pin 3.10
uv add --extra-index-url=https://download.pytorch.org/whl/cu121 torch --no-sync
```
**Note:** Create on windows 11 (x86-64)
### Part of uv.lock for torch
```
[[package]]
name = "torch"
version = "2.4.0+cu121"
source = { registry = "https://download.pytorch.org/whl/cu121" }
dependencies = [
    { name = "filelock" },
    { name = "fsspec" },
    { name = "jinja2" },
    { name = "networkx" },
    { name = "nvidia-cublas-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-cupti-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-nvrtc-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cuda-runtime-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cudnn-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cufft-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-curand-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusolver-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-cusparse-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nccl-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "nvidia-nvtx-cu12", marker = "platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "sympy" },
    { name = "triton", marker = "python_full_version < '3.13' and platform_machine == 'x86_64' and platform_system == 'Linux'" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://download.pytorch.org/whl/cu121/torch-2.4.0%2Bcu121-cp310-cp310-linux_x86_64.whl", hash = "sha256:28bfba084dca52a06c465d7ad0f3cc372c35fc503f3eab881cc17a5fd82914e7" },
    { url = "https://download.pytorch.org/whl/cu121/torch-2.4.0%2Bcu121-cp310-cp310-win_amd64.whl", hash = "sha256:9244bdc160d701915ae03e14cc25c085aa11e30d711a0b64bef0ee427e04632c" },
    { url = "https://download.pytorch.org/whl/cu121/torch-2.4.0%2Bcu121-cp311-cp311-linux_x86_64.whl", hash = "sha256:a9fff32d365e0c74b6909480548b2e291314a204adb29b6bb6f2c6d33f8be26c" },
    { url = "https://download.pytorch.org/whl/cu121/torch-2.4.0%2Bcu121-cp311-cp311-win_amd64.whl", hash = "sha256:bada31485e04282b9f099da39b774484d3e4c431b7ea0df3663817295ae764e4" },
    { url = "https://download.pytorch.org/whl/cu121/torch-2.4.0%2Bcu121-cp312-cp312-linux_x86_64.whl", hash = "sha256:49ac55a6497ddd6d0cdd51b5ea27d8ebe20c9273077855e9c96eb0dc289f07c3" },
    { url = "https://download.pytorch.org/whl/cu121/torch-2.4.0%2Bcu121-cp312-cp312-win_amd64.whl", hash = "sha256:b5c27549daf5f3209da6e07607f2bb8d02712555734fcd8cd7a23703a6e7d639" },
]
```
[project_source.zip](https://github.com/user-attachments/files/16736078/project_source.zip)

According to the document: 
> uv.lock is a universal or cross-platform lockfile that captures the packages that would be installed across all possible Python markers such as operating system, architecture, and Python version.

If a uv.lock generated on Windows cannot be used on other platforms, then it is not a uv.lock as documented.
Therefore, when the documentation mentions using the Project API and depending on PyTorch, the uv.lock should conform to the documentation's specifications.
Or note that the generated lock file is not a universal file?

---

_Comment by @FishAlchemist on 2024-08-24 10:51_

@baggiponte 
As for describing the installation method for a specific version, it's because UV installs PyTorch from sources other than PyPI, and it requires not only the version number but also the [local version identifiers](https://docs.astral.sh/uv/pip/compatibility/#local-version-identifiers).
Or, mentioning  ``Local version identifiers`` in the document might be another way to help people understand how to install a specific version.

### pip
![image](https://github.com/user-attachments/assets/781df16f-ab46-47da-acff-307d276675c7)
### uv pip
* **2.4.0**
![image](https://github.com/user-attachments/assets/b17322df-59c0-4cb2-825f-40515ff3c29b)
* **2.4.0+cu121**
![image](https://github.com/user-attachments/assets/1d784833-e3d4-4662-8b2a-1b20e22c1558)


---

_Comment by @baggiponte on 2024-08-26 08:44_

Hey there, was away for the weekend. Thank you very much for the explanation 😊 Will get back to this after work, later today.

In the meanwhile, to recap:

1. I should investigate lockfiles generated by torch installation and document, at least to say that they might not be cross-platform, in this case.
2. Cover the local version identifiers differences between pip and uv.

Did I get everything?

Thank you again for taking the time to steer me through this!

---

_Comment by @FishAlchemist on 2024-08-26 09:59_

> Hey there, was away for the weekend. Thank you very much for the explanation 😊 Will get back to this after work, later today.
> 
> In the meanwhile, to recap:
> 
> 1. I should investigate lockfiles generated by torch installation and document, at least to say that they might not be cross-platform, in this case.
> 2. Cover the local version identifiers differences between pip and uv.
> 
> Did I get everything?
> 
> Thank you again for taking the time to steer me through this!

While this is generally correct, there's a potential issue when using uv add to install PyTorch.
If not configured properly, uv add might overwrite your existing PyTorch installation with a version from PyPI that lacks CUDA support, even if you previously had a GPU-accelerated version.
As I mentioned from this comment:  https://github.com/astral-sh/uv/pull/6523#issuecomment-2307497266

**Note:** Since [PyPI's PyTorch](https://pypi.org/project/torch/) offers wheels for macOS, Linux, and Windows, if we switch the source to PyPI and remove the Local version identifiers, there will be no errors. However, the version will possibly switch from CUDA to CPU only.
~~**Note:**  The Linux version of PyTorch ``CUDA 12.1`` on PyPI already supports CUDA.~~
**Note:**  The Linux version of PyTorch ``CUDA 12.4`` on PyPI already supports CUDA.
![image](https://github.com/user-attachments/assets/ea7420cd-9fe1-4c5a-83a8-5dd1982118ba)

You're welcome. I know how frustrating these issues can be, so I wanted to save other users some time.
Providing good documentation is a great service to users, and I appreciate you taking the time to do so.

---

_Comment by @inflation on 2024-08-27 00:52_

It's pretty annoying since somehow `extra-index-url` overwrites the default index when a package with the same name but some versions missing. `uv` simply does not look at the default index for the missing version.

---

_Comment by @zanieb on 2024-08-27 00:54_

@inflation there are details on that behavior in the [documentation](https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes). Please don't complain about it in someone's pull request.

---

_Comment by @inflation on 2024-08-27 01:00_

> @inflation there are details on that behavior in the [documentation](https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes). Please don't complain about it in someone's pull request.

This is precisely where it happens the most. Installing `pytorch` using the its index introduce the problem. [`pixi`](https://pixi.sh/latest/advanced/channel_priority/#use-case-pytorch-and-nvidia-with-conda-forge) has a similar issue and contains a nice example and explanation. 

---

_@seemethere approved on 2024-09-11 20:54_

+1 from the PyTorch side!

---

_Review comment by @albanD on `docs/guides/integration/pytorch.md`:17 on 2024-09-12 17:22_

Would we be able to do this with `--index-url` instead of `--extra-index-url` throughout? 
The extra version of this flag was the cause of a bad security event already for PyTorch and we would not want to repeat that here: https://pytorch.org/blog/compromised-nightly-dependency/

Because we do use `--index-url` for all the pip commands, you can rely on the fact that the url will contain all depedencies needed to install torch.

---

_@albanD reviewed on 2024-09-12 17:24_

---

_@zanieb reviewed on 2024-09-12 19:29_

---

_Review comment by @zanieb on `docs/guides/integration/pytorch.md`:17 on 2024-09-12 19:29_

Note we have different behavior than pip to try to avoid such issues https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes

I think the real answer is that we need #171 though

---

_@baggiponte reviewed on 2024-09-13 06:21_

---

_Review comment by @baggiponte on `docs/guides/integration/pytorch.md`:17 on 2024-09-13 06:21_

Uh, he has a point. I'll add a link to explain this behaviour.

---

_Comment by @baggiponte on 2024-09-13 07:14_

Hello there! Sorry for disappearing, but as if it was not enough already, we got a sparkle of floods here too.

I edited a couple of things with the last commit I pushed.

1. Since @albanD rightfully pointed out, I added a small callout to point to the relevant bits of the uv docs.
2. I reworked a bit the TL;DR section to make it a bit more straightforward.
3. I added a mention to #171
4. I tried to explain there might be issues with the cross-compatible lockfile. I am not sure I explained correctly what @FishAlchemist meant, though. I guess what they mean is:
    * If someone does `uv add torch --extra-index-url=...`
    * Then `uv add foobar`
    * Then the pytorch version might be replaced with the PyPI one?
    If so, how can I phrase this correctly?

@zanieb let me know if it makes sense, suggest edits or make them directly.

---

_Comment by @FishAlchemist on 2024-09-13 11:49_

@baggiponte 
The primary issue with file locking is that the extra-index-url specified on the CLI is not written to pyproject.toml (Nor should it be written automatically). As a result, the next time you lock your dependencies, it won't remember to search the extra-index-url. Therefore, before adding PyTorch using the project API, it's recommended to manually add the extra-index-url to pyproject.toml instead of providing it on the CLI.

---

_Review comment by @albanD on `docs/guides/integration/pytorch.md`:17 on 2024-09-13 15:59_

I agree the default is better. But it still sounds quite dangerous if we make a mistake (don't realize we forgot to push a version of that package on our index). Looking forward to the pinned package index!

Thanks for the update.

---

_@albanD reviewed on 2024-09-13 15:59_

---

_Comment by @baggiponte on 2024-09-13 16:06_

> @baggiponte The primary issue with file locking is that the extra-index-url specified on the CLI is not written to pyproject.toml (Nor should it be written automatically). As a result, the next time you lock your dependencies, it won't remember to search the extra-index-url. Therefore, before adding PyTorch using the project API, it's recommended to manually add the extra-index-url to pyproject.toml instead of providing it on the CLI.

Makes perfect sense!

I guess it might be a good idea to mention that you should add `[tool.uv.sources]` to your pyproject. What do you think?

---

_Comment by @FishAlchemist on 2024-09-14 04:17_

> > @baggiponte The primary issue with file locking is that the extra-index-url specified on the CLI is not written to pyproject.toml (Nor should it be written automatically). As a result, the next time you lock your dependencies, it won't remember to search the extra-index-url. Therefore, before adding PyTorch using the project API, it's recommended to manually add the extra-index-url to pyproject.toml instead of providing it on the CLI.
> 
> Makes perfect sense!
> 
> I guess it might be a good idea to mention that you should add `[tool.uv.sources]` to your pyproject. What do you think?

According to the documentation, version 0.4.10, [tool.uv.sources] only supports these sources.
![image](https://github.com/user-attachments/assets/4dc75ff7-4db5-4be3-8669-855381d481ba)
Therefore, using [tool.uv.sources] requires you to find the sources yourself.

 I'm trying to use extra-index-url, but as a result, there are no macOS wheels available.
 ```toml
 [tool.uv]
extra-index-url = ["https://download.pytorch.org/whl/cu121"]
```

I've yet to find a solution for using [tool.uv.sources] that can support Windows, Linux, macOS, and CUDA.


---

_Comment by @baggiponte on 2024-09-14 15:50_

> > > @baggiponte The primary issue with file locking is that the extra-index-url specified on the CLI is not written to pyproject.toml (Nor should it be written automatically). As a result, the next time you lock your dependencies, it won't remember to search the extra-index-url. Therefore, before adding PyTorch using the project API, it's recommended to manually add the extra-index-url to pyproject.toml instead of providing it on the CLI.
> > 
> > 
> > Makes perfect sense!
> > I guess it might be a good idea to mention that you should add `[tool.uv.sources]` to your pyproject. What do you think?
> 
> According to the documentation, version 0.4.10, [tool.uv.sources] only supports these sources. ![image](https://private-user-images.githubusercontent.com/48265002/367486491-4dc75ff7-4db5-4be3-8669-855381d481ba.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjYzMjg5MDAsIm5iZiI6MTcyNjMyODYwMCwicGF0aCI6Ii80ODI2NTAwMi8zNjc0ODY0OTEtNGRjNzVmZjctNGRiNS00YmUzLTg2NjktODU1MzgxZDQ4MWJhLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDA5MTQlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQwOTE0VDE1NDMyMFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWE5NTNhZjU4YTc3MzZlNzE4MmU2NzA2YjU2OWViNmU2NDZiMjUzNmI4NmIzOGQ3NTJhNzg1MjhlNzhjOGNjZGUmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JmFjdG9yX2lkPTAma2V5X2lkPTAmcmVwb19pZD0wIn0.Iwv_6ivjZ1J14oJfZqrug_IJlhZujhYR_5CW0dguXlw) Therefore, using [tool.uv.sources] requires you to find the sources yourself.
> 
> I'm trying to use extra-index-url, but as a result, there are no macOS wheels available.
> 
> ```toml
> [tool.uv]
> extra-index-url = ["https://download.pytorch.org/whl/cu121"]
> ```
> 
> I've yet to find a solution for using [tool.uv.sources] that can support Windows, Linux, macOS, and CUDA.

Very clear. Pushed another minor edit mentioning this. Would love to hear your feedback on the phrasing.

---

_Comment by @FishAlchemist on 2024-09-15 09:29_

> For example: suppose a Windows user runs uv add torch and then a Linux user runs uv sync to synchronise the lockfile. The Windows user will get CPU-only PyTorch, while the Linux user will get CUDA 12.1 PyTorch.

@baggiponte I'm a bit worried this could be confusing, since it assumes the user hasn't given PyTorch package indexes to uv anywhere. However, this thing was never explicitly mentioned in the narrative. I'm unsure whether it's a good idea to imply it

As for the [tool.uv.sources] issue, it's just that I personally don't know how to use it to make PyTorch's lock files cross-platform. Therefore, I'm not sure if the current uv can make PyTorch cross-platform through it.
Therefore, it feels inappropriate that the document mentions the possibility of it not working.

@zanieb Can the current UV create cross-platform lockfiles for projects that depend on PyTorch? 
(Capable of running across Windows, Linux, and macOS, at least under the specified CUDA version)

---

_Comment by @zanieb on 2024-09-20 00:13_

I think this might be unblocked by #7481 or some of the work following that.

---

_Comment by @FishAlchemist on 2024-10-17 16:25_

Hi everyone,

Since the release of [uv 0.4.23](https://github.com/astral-sh/uv/releases/tag/0.4.23), we might be able to start this work. Because I'm not very familiar with how to use the new features, I asked @charliermarsh  on Discord about how to write a pyproject.toml for PyTorch that supports Windows CUDA 12.1 and Linux (from PyPI) in this version. I got two examples, and I see that the lockfile can support three major platforms, but I only asked about Windows and Linux. I'm not sure if I need to write more details about macOS.

This is the question I asked, and it's also a personal use case for me.

## Conversation 1:
> I need PyTorch 2.1.2, which requires CUDA 12.1 on my Windows system. On my Linux system, I need to install it from PyPI. I'm wondering how to write a pyproject.toml file to fulfill these requirements.
Moreover, installing PyTorch from non-PyPI sources using uv requires specifying Local version identifiers. Therefore, the PyTorch version should be 2.1.2+cu121 on Windows and 2.1.2 on Linux. 

https://discord.com/channels/1039017663004942429/1207998321562619954/1296492153576489030
```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
  "torch==2.1.2+cu121 ; platform_system == 'Windows'",
  "torch==2.1.2 ; platform_system != 'Windows'",
]

[tool.uv.sources]
torch = [
  { index = "torch-cu121", marker = "platform_system == 'Windows'"},
  { index = "pypi", marker = "platform_system != 'Windows'"},
]

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"

[[tool.uv.index]]
name = "torch-cu121"
url = "https://download.pytorch.org/whl/cu121"
```
## Conversation 2 (Continue conversation 1):
> Thank you for providing me with the example. I originally thought that if no marker was matched, UV would go to PyPI to search, but I didn't expect that I needed to explicitly specify to go to PyPI.

https://discord.com/channels/1039017663004942429/1207998321562619954/1296500150600077322
```toml
[project]
name = "project"
version = "0.1.0"
requires-python = ">=3.10"
dependencies = [
  "torch==2.1.2 ; platform_system != 'Windows'",
  "torch==2.1.2+cu121 ; platform_system == 'Windows'",
]

[tool.uv.sources]
torch = [
  { index = "torch-cu121", marker = "platform_system == 'Windows'"},
]

[[tool.uv.index]]
name = "torch-cu121"
url = "https://download.pytorch.org/whl/cu121"
explicit = true
```

The above examples can be used as a reference for this PR.




---

_Comment by @baggiponte on 2024-10-17 17:42_

Hello there! Saw the latest release with extreme joy! Might hop in the Discord to edge out the last details. I am not 100% sure I have the time to do this tonight; might be tomorrow or over the weekend. If anyone has the time, feel free to pick up where I left and bring this over the finish line.

---

_Comment by @fzyzcjy on 2024-10-19 12:24_

Hi I am facing problem in macos for pytorch cpu installation (https://github.com/astral-sh/uv/issues/8358), is there any suggestions? Thanks!

---

_Comment by @baggiponte on 2024-10-20 08:54_

Hello there! Finally have some time.

> ## Conversation 2 (Continue conversation 1):
> > Thank you for providing me with the example. I originally thought that if no marker was matched, UV would go to PyPI to search, but I didn't expect that I needed to explicitly specify to go to PyPI.
> 
> [discord.com/channels/1039017663004942429/1207998321562619954/1296500150600077322](https://discord.com/channels/1039017663004942429/1207998321562619954/1296500150600077322)
> 
> ```toml
> [project]
> name = "project"
> version = "0.1.0"
> requires-python = ">=3.10"
> dependencies = [
>   "torch==2.1.2 ; platform_system != 'Windows'",
>   "torch==2.1.2+cu121 ; platform_system == 'Windows'",
> ]
> 
> [tool.uv.sources]
> torch = [
>   { index = "torch-cu121", marker = "platform_system == 'Windows'"},
> ]
> 
> [[tool.uv.index]]
> name = "torch-cu121"
> url = "https://download.pytorch.org/whl/cu121"
> explicit = true
> ```
> 
> The above examples can be used as a reference for this PR.

This works for me on Python 3.10 and 3.11, but not 3.12. If I just do `uv add torch`, everything works as usual on all supported Python versions. I am on an ARM mac.

---

_Comment by @FishAlchemist on 2024-10-20 09:16_

@baggiponte I noticed that PyPI's torch package is now using CUDA 12.4, but I'm unsure of the exact date and version when this change was made.
![image](https://github.com/user-attachments/assets/958f8cc6-367d-42d4-94b5-9a658bb840ac)


---

_Comment by @baggiponte on 2024-10-20 09:30_

> @baggiponte I noticed that PyPI's torch package is now using CUDA 12.4, but I'm unsure of the exact date and version when this change was made. ![image](https://private-user-images.githubusercontent.com/48265002/378172621-958f8cc6-367d-42d4-94b5-9a658bb840ac.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3Mjk0MTY2MDMsIm5iZiI6MTcyOTQxNjMwMywicGF0aCI6Ii80ODI2NTAwMi8zNzgxNzI2MjEtOTU4ZjhjYzYtMzY3ZC00MmQ0LTk0YjUtOWE2NThiYjg0MGFjLnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNDEwMjAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjQxMDIwVDA5MjUwM1omWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTQ3ZjgzYzM4NDc0Mjk0NmYxMWI3NDM2YmJjNThiMDg0ZmVhMTJlNWE4NWQ4NTIwOGJiZGIwYzRlZTFjZTIzNjMmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.FQGhH6t6Ob1NF4fMvQXLXyvLahJki6K3p8kPq2GGOpw)

No actually it's dumber; torch 2.12 does not support python 3.12.

---

_Comment by @FishAlchemist on 2024-10-20 09:35_

> No actually it's dumber; torch 2.12 does not support python 3.12.

Version 2.1.x is not compatible with Python 3.12, therefore this behavior is expected.
https://github.com/pytorch/pytorch/blob/main/RELEASE.md#release-compatibility-matrix
![image](https://github.com/user-attachments/assets/a94095e0-05ec-43f2-af2f-23185a0b2320)


---

_Comment by @fzyzcjy on 2024-10-20 10:01_

So is there anyone who successfully install macos torch cpu version, and could you please share your script? Thanks!

---

_Comment by @baggiponte on 2024-10-20 10:13_

> So is there anyone who successfully install macos torch cpu version, and could you please share your script? Thanks!

Ciao, this discussion is dedicated to the PR implementation. Please keep the discussion to the issue you created 😊 (I could not reproduce your issue because I am on an ARM mac, btw.)

---

_Comment by @baggiponte on 2024-10-20 10:53_

Rewrote the thing from scratch. I am not 100% sure if the solution works in every usecase.

With the sample pyproject.toml that was provided, one can ensure that (for example) a macOS user installs from PyPI while Windows and Linux from _either_ torch+cpu _or_ torch+cu{whatever}. IIUC, there's no way to automatically install CUDA if there are drivers on the machine. Am I wrong?

---

_Comment by @fzyzcjy on 2024-10-20 13:29_

Sorry for disturbing (I originally thought this PR is about torch installation, thus also includes torch installation on macos)!

---

_Comment by @FishAlchemist on 2024-10-20 13:53_

> Rewrote the thing from scratch. I am not 100% sure if the solution works in every usecase.
> 
> With the sample pyproject.toml that was provided, one can ensure that (for example) a macOS user installs from PyPI while Windows and Linux from _either_ torch+cpu _or_ torch+cu{whatever}. IIUC, there's no way to automatically install CUDA if there are drivers on the machine. Am I wrong?

I'm not quite sure what you're referring to. But based on my experience with Windows, a package manager simply downloads and installs packages from the specified index and considers its job done. Installing and configuring the  drivers and system dependencies is outside its scope.

---

_Comment by @baggiponte on 2024-10-20 14:43_

> > Rewrote the thing from scratch. I am not 100% sure if the solution works in every usecase.
> > With the sample pyproject.toml that was provided, one can ensure that (for example) a macOS user installs from PyPI while Windows and Linux from _either_ torch+cpu _or_ torch+cu{whatever}. IIUC, there's no way to automatically install CUDA if there are drivers on the machine. Am I wrong?
> 
> I'm not quite sure what you're referring to. But based on my experience with Windows, a package manager simply downloads and installs packages from the specified index and considers its job done. Installing and configuring the drivers and system dependencies is outside its scope.

Ops, sorry, was not clear enough. I did not mean to imply that uv should configure cuda drivers and such.

Let me know if the guide covers enough or there should be some more warnings/clarifications/recommendations :)

---

_Comment by @FishAlchemist on 2024-10-20 14:57_

@baggiponte Should we mention https://github.com/astral-sh/uv/issues/8358#issuecomment-2424808369?
https://github.com/pytorch/pytorch/blob/main/RELEASE.md#operating-systems
However, I haven't executed the content of the document yet. I'm only browsing it on GitHub at the moment.

---

_Comment by @FishAlchemist on 2024-10-21 04:01_

@fzyzcjy Since you're already here, is there anything missing from the current documents, based on your usage experience?
When using UV to process PyTorch, what kind of information would you like to know?

---

_Comment by @fzyzcjy on 2024-10-21 05:05_

> Since you're already here, is there anything missing from the current documents, based on your usage experience?
When using UV to process PyTorch, what kind of information would you like to know?

Your comment in #8358 - the "`uv add torch<2.3.0`" and the "2.3.0 does not support intel chip anymore" - is really helpful. If someone is using intel chip mac, I guess he or she may be beneficial from such a line in doc. 

On the other hand, I agree it is not a `uv` issue, so not saying this in doc is also reasonable.

---

_Comment by @baggiponte on 2024-10-21 09:46_

Added the mention! :)

---

_@fzyzcjy reviewed on 2024-10-21 09:57_

---

_Review comment by @fzyzcjy on `docs/guides/integration/pytorch.md`:167 on 2024-10-21 09:57_

Looks great! Just a nit: seems it is 2.2.x that is supported and 2.3.0 does not support (if I understand correctly)

---

_@FishAlchemist reviewed on 2024-10-22 03:06_

---

_Review comment by @FishAlchemist on `docs/guides/integration/pytorch.md`:167 on 2024-10-22 03:06_

While the new PyTorch version isn't available on PyPI or PyTorch Index for macOS Intel, you can still get it by installing from conda.
https://anaconda.org/conda-forge/pytorch/files
![image](https://github.com/user-attachments/assets/d2823f47-f132-4f47-827c-ddcf32986f4f)


---

_@baggiponte reviewed on 2024-10-22 05:13_

---

_Review comment by @baggiponte on `docs/guides/integration/pytorch.md`:167 on 2024-10-22 05:13_

> Just a nit: seems it is 2.2.x that is supported and 2.3.0 does not support

You are correct! Thanks for spotting this

> you can still get it by installing from conda.

Interesting. I'll edit accordingly. Do we need to link to conda, and if so, what's the most useful link?

---

_Comment by @baggiponte on 2024-10-22 07:21_

I also added torchvision to the example in the docs given #8344.

---

_Assigned to @charliermarsh by @zanieb on 2024-10-22 11:44_

---

_Comment by @zanieb on 2024-10-22 11:45_

I believe that @charliermarsh plans to look into PyTorch this month so I've assigned him. Great to see all the collaboration here!

---

_Comment by @vvuk on 2024-10-30 20:38_

I'm running into the same original issue as #7202 following these docs (the "Distribution .. can't be installed because it doesn't have a source distribution...") error. My setup:

workspace pyproject.toml:
```
[project]
name = "workspace"
version = "0.0.0"
requires-python = "==3.12.*"
dependencies = []

[tool.uv.workspace]
members = [
    "foo",
]

[tool.uv]
managed = true
override-dependencies = [
  # mediapipe wrongly depends on protobuf<5
    "protobuf==5.27.5",
    "mediapipe==0.10.14",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu124", marker = "platform_system != 'Darwin'"},
]
torchvision = [
  { index = "pytorch-cu124", marker = "platform_system != 'Darwin'"},
]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true
```

And `foo/pyproject.toml`:
```
[project]
name = "foo"
version = "0.0.0"
dependencies = [
    "torch",
    "torchsde",
    "torchvision",
    "torchaudio",
    "einops",
    "transformers>=4.28.1",
    "tokenizers>=0.13.3",
    "sentencepiece",
    "safetensors>=0.4.2",
    "aiohttp",
    "pyyaml",
    "Pillow",
    "scipy",
    "tqdm",
    "psutil",
    "kornia>=0.7.1",
    "spandrel",
    "soundfile",
]
requires-python = ">=3.12"
readme = "README.md"
```

Trying to run `uv sync --project foo` results in:

```
error: Distribution `torch==2.5.1 @ registry+https://download.pytorch.org/whl/cu124` can't be installed because it doesn't have a source distribution or wheel for the current platform```


---

_Comment by @charliermarsh on 2024-10-30 20:43_

@vvuk -- Are you running on macOS or non-macOS (per the markers)?

---

_Comment by @vvuk on 2024-10-30 20:49_

non-macos (Windows). I've also noticed some docs (e.g. here) have `platform_system != 'Darwin'` and others have `sys_platform != 'Darwin'` -- which is correct? Or are they interchangeable? Also perhaps odd is that a `uv sync` in the workspace root succeeds, and a lock file gets created.. but now that I look in the lock file, there's no resolution for `"torch"` for Windows. Just for Darwin, and for.. hmm. Not sure how to interpret this:

```
[[package]]
name = "torch"
version = "2.5.1"
source = { registry = "https://download.pytorch.org/whl/cu124" }
resolution-markers = [
    "platform_machine == 'aarch64' and platform_system == 'Linux'",
    "(platform_machine != 'aarch64' and platform_system != 'Darwin') or (platform_system != 'Darwin' and platform_system
 != 'Linux')",
]
...
wheels = ... linux_aarch64 ...

[[package]]
name = "torch"
version = "2.5.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "platform_system == 'Darwin'",
]
...
wheels = .. macosx_11_arm64 ...
```

So looks like there was only a resolution for linux-aarch64 and for macos-arm64

---

_Comment by @charliermarsh on 2024-10-30 20:52_

That's discussed at length in a few places: https://github.com/astral-sh/uv/issues/5182#issuecomment-2237858277, https://github.com/astral-sh/uv/issues/8536#issuecomment-2436123242. (There's no way for us to know if the set of wheels covers the entire space you care about.)

---

_Comment by @charliermarsh on 2024-10-30 20:52_

You probably need to define your dependencies like:

```
torch==2.5.1+cu124 ; platform_system != 'Darwin'
torch==2.5.1 ; platform_system == 'Darwin'
```

Confusingly, the wheels you want from the PyTorch index are tagged as `2.5.1+cu124` -- there are some wheels that are just `2.5.1`, but not for Windows: https://download.pytorch.org/whl/torch/


---

_Comment by @vvuk on 2024-10-30 21:02_

Yeah, I was hoping to not have to declare explicit versions; that seems to be the only workable solution though. Maybe one way to resolve this mess would be a uv config for "prioritize these local tags in this order for resolution". e.g.:

```
local-priority = [
   "cu124 ; platform_system != 'Darwin'",
   "foo ; platform_system == 'Darwin'",
   "bar"
]
```

To state that on non-macos, prioritize `+cu124` above any other matching version. On macOS, prioritize `+foo`. And on all platforms prioritize `+bar` above anything else. The above would also be in order, so on windows `+cu124` would be picked before `+bar`. (Maybe an optional syntax to say "require cu124" vs "prioritize cu124", i.e. whether to pick `2.6.0` over `2.5.5+cu124` or not)

---

_Comment by @charliermarsh on 2024-10-30 21:03_

Yeah, it's not a great situation. I'd really love to just fix / improve the local version handling entirely.

---

_Comment by @vvuk on 2024-10-30 23:21_

I'm trying with this in my workspace pyproject.toml:

```
[tool.uv]
override-dependencies = [
  "torch==2.5.1+cu124 ; platform_system != 'Darwin'",
  "torch==2.5.1 ; platform_system == 'Darwin'",

  "torchvision==0.20.1+cu124 ; platform_system != 'Darwin'",
  "torchvision==0.20.1 ; platform_system == 'Darwin'",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu124", marker = "platform_system != 'Darwin'" }
]
torchvision = [
  { index = "pytorch-cu124", marker = "platform_system != 'Darwin'" }
]
```

A workspace member has a dependency on just `torch`. Running sync on windows:

```
  × No solution found when resolving dependencies for split (python_full_version == '3.12.*' and platform_machine == 'aarch64' and platform_system ==
  │ 'Linux'):
  ╰─▶ Because there is no version of torch{platform_system != 'Darwin'}==2.5.1+cu124 and comfyui depends on torch{platform_system !=
      'Darwin'}==2.5.1+cu124, we can conclude that comfyui's requirements are unsatisfiable.
      And because your workspace requires comfyui, we can conclude that your workspace's requirements are unsatisfiable.
```

okay, I can believe there's no cuda version for aarch64. I've tried every variant I can think of and can't get a resolution, including this which I think should 100% work:

```
override-dependencies = [
  "torch==2.5.1+cu124 ; platform_system == 'Windows'",
  "torch==2.5.1 ; platform_system != 'Windows'",
  "torchvision==0.20.1+cu124 ; platform_system == 'Windows'",
  "torchvision==0.20.1 ; platform_system != 'Windows'",
]

[tool.uv.sources]
torch = [
  { index = "pytorch-cu124", marker = "platform_system == 'Windows'" },
  # added these because I wasn't sure if sources was constraining the indexes completely, i.e.
  # it wouldn't consider any additional ones if explicit ones are specified
  { index = "pytorch-cpu", marker = "platform_system != 'Windows'" },
]
torchvision = [
  { index = "pytorch-cu124", marker = "platform_system == 'Windows'" },
  { index = "pytorch-cpu", marker = "platform_system != 'Windows'" },
]

[[tool.uv.index]]
name = "pytorch-cu124"
url = "https://download.pytorch.org/whl/cu124"
explicit = true

[[tool.uv.index]]
name = "pytorch-cpu"
url = "https://download.pytorch.org/whl/cpu"
explicit = true
```

I get:

```
  × No solution found when resolving dependencies for split (python_full_version == '3.12.*' and platform_system == 'Windows'):
  ╰─▶ Because there is no version of torch{platform_system == 'Windows'}==2.5.1+cu124 and comfyui depends on torch{platform_system ==
      'Windows'}==2.5.1+cu124, we can conclude that comfyui's requirements are unsatisfiable.
      And because your workspace requires comfyui, we can conclude that your workspace's requirements are unsatisfiable.
```

If I do `uv sync --no-cache -vv`, I actually see zero hits to the pytorch index URLs. If I remove `explicit = true`, I get further, but then I get into a mess because I'm picking up random packages from the pytorch sources that are too old, and versions inside pypi's aren't considered. If I use `--index-strategy unsafe-best-match` then I end up picking up a random package that is _too new_, and somehow exists for only one platform and so can't be found anywhere else.

---

_Comment by @vvuk on 2024-10-30 23:41_

This seems to be an interaction with dependency overrides and sources; it seems like `tool.uv.sources` is not considered when something is in `override-dependencies`. If I turn my workspace into a non-virtual one:

```
[project]
name = "foo"
version = "0.0.0"
dependencies = [
  "torch==2.5.1+cu124 ; platform_system == 'Windows'",
  "torch==2.5.1 ; platform_system != 'Windows'",
  "torchaudio==2.5.1+cu124 ; platform_system == 'Windows'",
  "torchaudio==2.5.1 ; platform_system != 'Windows'",
  "torchvision==0.20.1+cu124 ; platform_system == 'Windows'",
  "torchvision==0.20.1 ; platform_system != 'Windows'",
]
```
and don't put that in override-dependencies, and keep the uv sources to point to the cpu repo for non-windows, I can resolve. If I make dependencies generic ("torch", "torchaudio", "torchvision") and restore the overrides, I get the same error about not being able to resolve +cu124. 

... but if I do this (dependencies), then I'm back to a non-virtual workspace, and then I run into #5727. So, I created a dummy `pin-pytorch` package that contains just a `pyproject.toml` like above and added that to my (virtual) workspace. This seems to work!


---

_Comment by @Leon0402 on 2024-11-01 21:15_

I am missing the following use cases in the docs: 

1. What happens if we have transitive dependencies that specify different versions of torch. Imagine one dependency with a dependency on torch CPU and one on torch CudaX. But you actually need CudaY. I think it would be quite valuable to give some detail here how the resolver behaves in such cases (even though that is not torch specific) and how to resolve potential conflicts -> I have not tried that use case and I am no UV expert, so I don't really know what happens here. But I guess it goes in the direction of `override-dependencies` mentioned earlier here.
2. How can we deal with the situation that project members need different torch versions for the same platform. Imagine one person only has a CPU, but another one a GPU. Or two people need different cuda versions on the same platform. AFAIK there is no super nice solution here as we do not have cuda markers, because python cannot realiably detect this or something like this. So I imagine this needs to be solved somehow with extras or something similar. Mentioning limitations and possible solutions would be beneficial. 

And perhaps it would be nice to have some sort of FAQ section with common mistakes, for instance the docs specify in the beginning `!!! tip "Supported Python versions"`, but I think people might miss that or cannot relate the error message directly with this. So it might make sense to show explicitly the kind of error that happens, when you use an incompatible python version. I guess there are more examples for common mistakes / error messages. 

---

_Comment by @zanieb on 2024-11-01 23:51_

Perhaps a useful example https://github.com/astral-sh/uv/issues/8746#issuecomment-2452438775

---

_Comment by @FishAlchemist on 2024-11-02 03:04_

Hi everyone,
I think we need to confirm the minimum supported PyTorch version in this PR's document, and whether we should support all its target architectures.

# Operating Systems
https://github.com/pytorch/pytorch/blob/main/RELEASE.md#operating-systems
![image](https://github.com/user-attachments/assets/04df15fb-0b5e-4a1c-82bd-a573869d2e8f)
If we want to support versions below 2.3.0, such as 2.2.2, then we need to be able to install it on macOS Intel (x86-64).
Alternatively, is it outside the scope of our project API to accommodate PyTorch execution on macOS Intel(x86-64)?
# Release Compatibility Matrix
https://github.com/pytorch/pytorch/blob/main/RELEASE.md#release-compatibility-matrix
![image](https://github.com/user-attachments/assets/5cd0391a-10d1-42e3-9255-d7ea07124985)
According to the diagram above, since UV currently supports Python 3.8 as the minimum version, we only need to ensure support for PyTorch 2.0 or above.

---

_@bryant1410 reviewed on 2024-11-12 13:43_

---

_Review comment by @bryant1410 on `docs/guides/integration/pytorch.md`:3 on 2024-11-12 13:43_

```suggestion
[PyTorch](https://pytorch.org/) is a popular open-source deep learning framework that has first-class support for acceleration via GPUs. Installation, however, can be complex, as you won't find all the wheels for PyTorch on PyPI and you have to manage this through external indexes. This guide aims to help you set up a `pyproject.toml` file using `uv` [indexes features](../../configuration/indexes.md).
```

---

_Comment by @charliermarsh on 2024-11-18 18:23_

Now that we have all the pieces in place I'm going to invest in comprehensive PyTorch docs. I will fold this work into those docs -- thank you so much for writing it up. I may even merge this first, then make my PR atop it so that this is preserved.

---

_Comment by @charliermarsh on 2024-11-18 18:57_

(These docs are really great, thank you @baggiponte.)

---

_Comment by @charliermarsh on 2024-11-18 19:21_

I've built on this work in https://github.com/astral-sh/uv/pull/9210. This PR will merge first, then https://github.com/astral-sh/uv/pull/9210 would follow.

---

_@charliermarsh approved on 2024-11-19 01:00_

---

_Merged by @charliermarsh on 2024-11-19 01:02_

---

_Closed by @charliermarsh on 2024-11-19 01:02_

---
