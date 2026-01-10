```yaml
number: 2541
title: Support nightly PyTorch installs
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-03-19T15:14:10Z
updated_at: 2024-11-20T00:38:48Z
url: https://github.com/astral-sh/uv/issues/2541
synced_at: 2026-01-10T04:36:19Z
```

# Support nightly PyTorch installs

---

_Issue opened by @charliermarsh on 2024-03-19 15:14_

See: https://github.com/astral-sh/uv/issues/2008.

Commands like `pip install -U --prerelease allow torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu121` currently fail, and it's inconvenient to add the `+cu121` specifiers where necessary because the user is intentionally pulling in the "latest" release, and so doesn't want to add a dedicated version specifier.

---

_Label `enhancement` added by @charliermarsh on 2024-03-19 15:14_

---

_Label `compatibility` added by @charliermarsh on 2024-03-19 15:14_

---

_Comment by @ewhauser on 2024-03-22 17:31_

Here's a resolution issue I'm running into that `pip install` handles:

```
 whisperX @ https://github.com/m-bain/whisperX/archive/befe2b242eb59dcd7a8a122d127614d5c63d36e9.zip
 pyannote-audio @ git+https://github.com/pyannote/pyannote-audio@11b56a137a578db9335efc00298f6ec1932e6317
 torch @ https://download.pytorch.org/whl/cu118/torch-2.2.1%2Bcu118-cp311-cp311-linux_x86_64.whl#sha256=84328a35621cc6a67a182a327baaab67e5f5869981c4b1677ed05f92c15cceb1
 torchaudio @ https://download.pytorch.org/whl/cu118/torchaudio-2.2.1%2Bcu118-cp311-cp311-linux_x86_64.whl#sha256=cd3b1c3582b17792c6d7a367dea0459b123e54d7a4242809ea87ccc10fa220e5
 pytorch_triton @ https://download.pytorch.org/whl/nightly/pytorch_triton-2.1.0%2B7d1a95b046-cp311-cp311-linux_x86_64.whl
```

```
> ./uv pip install -r requirements.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because only whisperx==3.1.1 is available and whisperx==3.1.1 depends on torchaudio>=2, we can conclude that all versions of whisperx depend on torchaudio>=2.
      And because only the following versions of torchaudio are available:
          torchaudio<2
          torchaudio==2.2.1+cu118
      and torchaudio==2.2.1+cu118 depends on torch==2.2.1, we can conclude that all versions of whisperx depend on torch==2.2.1.
      And because there is no version of torch==2.2.1 and you require whisperx, we can conclude that the requirements are unsatisfiable.
```

I believe this is documented [here](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers) but unclear if there is a way to override?

---

_Comment by @charliermarsh on 2024-03-22 17:57_

I think it's possible we could support that specific case by extracting the name and version from the wheel filename (i.e., the URL). Can I ask why you prefer to use direct references for these?

---

_Comment by @ewhauser on 2024-03-22 19:55_

This is an existing requirements file so it is possible there is no good reason under `uv`. However, in order for this to work without direct references, you have to specify:

```
 --extra-index-url https://download.pytorch.org/whl/cu118
torch==2.2.1+cu118
torchaudio==2.2.1+cu118
```

which I assume has a performance hit for large requirements files? It does for `pip-tools`. 

I did try that with `uv` and it started to choke on some of our dependencies that were valid:

```
 certifi==2024.2.2

 --extra-index-url https://download.pytorch.org/whl/cu118

 whisperX @ https://github.com/m-bain/whisperX/archive/befe2b242eb59dcd7a8a122d127614d5c63d36e9.zip
 pyannote-audio @ git+https://github.com/pyannote/pyannote-audio@11b56a137a578db9335efc00298f6ec1932e6317
 torch==2.2.1+cu118
 torchaudio==2.2.1+cu118
 pytorch_triton @ https://download.pytorch.org/whl/nightly/pytorch_triton-2.1.0%2B7d1a95b046-cp311-cp311-linux_x86_64.whl
```

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of certifi==2024.2.2 and you require certifi==2024.2.2, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @charliermarsh on 2024-03-22 20:03_

Ah yeah, that's because certifi is published on https://download.pytorch.org/whl/cu118, but only at a different version. We don't allow dependencies to pull "mixed" versions from across indexes as a security measure.

---

_Comment by @charliermarsh on 2024-03-22 20:03_

Anyway, I think we can support this specific thing with the URLs.

---

_Comment by @charliermarsh on 2024-03-22 20:24_

I created a new issue to track that here: https://github.com/astral-sh/uv/issues/2623

---

_Comment by @fernandogrd on 2024-09-06 16:15_

> Ah yeah, that's because certifi is published on https://download.pytorch.org/whl/cu118, but only at a different version. We don't allow dependencies to pull "mixed" versions from across indexes as a security measure.

Hi, just experienced this today, specially the different versions of packages being available in the pytorch.

The `@` workaround worked, but wondering if there would be other workarounds by now as it is a bit tedious to copy the link of each package specially when there a number of them.

Edit.: Nvm, just found about UV_INDEX_STRATEGY in the docs. I wish pytorch would not keep the copied versions available on pypi though..

---

_Comment by @charliermarsh on 2024-11-20 00:38_

This works now!

```
uv pip install -U --prerelease allow torch torchvision torchaudio --index-url https://download.pytorch.org/whl/nightly/cu121
```

---

_Closed by @charliermarsh on 2024-11-20 00:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-20 00:38_

---
