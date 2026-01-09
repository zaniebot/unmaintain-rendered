---
number: 1884
title: Trying to install torch fails
type: issue
state: closed
author: sachit-menon
labels:
  - question
assignees: []
created_at: 2024-02-22T22:11:19Z
updated_at: 2024-04-07T00:15:28Z
url: https://github.com/astral-sh/uv/issues/1884
synced_at: 2026-01-07T13:12:16-06:00
---

# Trying to install torch fails

---

_Issue opened by @sachit-menon on 2024-02-22 22:11_

Thanks for the great project! First time using uv, trying out my typical first install in a fresh conda environment. The same command nominally works with pip. 

```
conda create -n test python=3.10
```


Command run: 
```
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```

Error message:
```
error: Failed to download and build: lit==15.0.7
  Caused by: Failed to build: lit==15.0.7
  Caused by: Failed to install requirements from setup.py build (resolve)
  Caused by: No solution found when resolving: wheel, setuptools >=40.8.0
  Caused by: Because wheel was not found in the package registry and you require wheel, we can conclude that the requirements are unsatisfiable.
```
Platform: Linux
uv version: 0.1.8

---

_Comment by @zanieb on 2024-02-22 22:24_

Hi! Can you make sure this isn't a duplicate of one of the existing issues for torch?

https://github.com/astral-sh/uv/issues?q=is%3Aissue+is%3Aopen+torch

---

_Comment by @sachit-menon on 2024-02-22 22:26_

I searched for different keywords for the error that this is getting and it doesn't appear in any of the other issues :)

---

_Comment by @zanieb on 2024-02-22 22:33_

It looks like you might have meant to use `--extra-index-url` or, instead, run `uv pip install wheel setuptools` first. They normally comes pre-installed in virtual environments, but we do not seed environments by default (#865). Since something needs `wheel` but it is not provided by the torch index, installation fails.

---

_Label `question` added by @zanieb on 2024-02-22 22:33_

---

_Comment by @sachit-menon on 2024-02-22 22:36_

I copied the install command from the pytorch website with `--index-url`. 
Just tried installing those first and:
```
> uv pip install wheel setuptools                                       
Resolved 2 packages in 539ms
Downloaded 1 package in 10.68s
Installed 1 package in 3.35s
 + setuptools==69.1.0
> uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
error: Failed to download and build: lit==15.0.7
  Caused by: Failed to build: lit==15.0.7
  Caused by: Failed to install requirements from setup.py build (resolve)
  Caused by: No solution found when resolving: wheel, setuptools >=40.8.0
  Caused by: Because wheel was not found in the package registry and you require wheel, we can conclude that the requirements are unsatisfiable.
```

`conda list` also confirms that `wheel` is already installed.

Does that help narrow it down? I created the environment with `conda` (full command is in the first post) so some things were already pre-installed. 

---

_Comment by @zanieb on 2024-02-22 22:40_

Interesting, I presumed we'd use your local builds for these if they were around but it doesn't look like we do. This is basically a limitation of specifying index urls, you've requested `https://download.pytorch.org/whl/cu118` as your sole index URL but we cannot build the `lit` package without dependencies from the normal index. You could also pre-install `lit`.

I'm surprised this would work on `pip` as described (without caching).

---

_Comment by @sachit-menon on 2024-02-22 22:56_

Interesting. I just tried with `--extra-index-url` instead and, unsurprisingly, it nominally succeeds but installs with CUDA 12 instead of 11 (because that's what's in the normal index). 
This might be a separate source of error, but after I tried 
```
uv pip uninstall torch torchvision torchaudio
uv pip install lit
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu118
```
I get 
```
error: Failed to read metadata for: torchaudio==2.2.1
  Caused by: failed to open file `/proj/vondrick4/sachit/miniconda3/envs/test/lib/python3.10/site-packages/torchaudio-2.2.1.dist-info/METADATA`
  Caused by: No such file or directory (os error 2)
```

which seems like it doesn't like that I'm trying to install a package I uninstalled? (I wanted to try pre-installing `lit` like you recommended, which I'll still try in a fresh environment.)

---

_Comment by @sachit-menon on 2024-02-23 03:28_

Manually installing the correct version of lit first in a fresh environment results in the same error. 

```
error: Failed to download and build: lit==15.0.7
  Caused by: Failed to build: lit==15.0.7
  Caused by: Failed to install requirements from setup.py build (resolve)
  Caused by: No solution found when resolving: wheel, setuptools >=40.8.0
  Caused by: Because wheel was not found in the package registry and you require wheel, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @sachit-menon on 2024-03-04 17:04_

Is there any way to install PyTorch with CUDA 11.8 using uv? This is one of my main use cases. 

---

_Comment by @zanieb on 2024-03-08 00:06_

Hi @sachit-menon 

We now support `--no-build-isolation` so you can install `wheel` and `setuptools` beforehand which should unblock you here. See https://github.com/astral-sh/uv/issues/2251#issuecomment-1984797316 for example.

We also changed the order `--extra-index-url` and `--index-url` are prioritized in https://github.com/astral-sh/uv/pull/2083 so just using `--extra-index-url` should work now?

---

_Comment by @charliermarsh on 2024-04-07 00:15_

I think everything here has been resolved, but let me know if you have any outstanding questions and happy to walk through them.

---

_Closed by @charliermarsh on 2024-04-07 00:15_

---
