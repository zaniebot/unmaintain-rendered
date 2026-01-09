---
number: 9842
title: "'uv publish' for mixed python-rust code fails due to unsupported platform tag 'linux_x86_64'"
type: issue
state: closed
author: giacomopiccinini
labels:
  - question
assignees: []
created_at: 2024-12-12T15:43:55Z
updated_at: 2025-01-28T11:48:41Z
url: https://github.com/astral-sh/uv/issues/9842
synced_at: 2026-01-07T13:12:18-06:00
---

# 'uv publish' for mixed python-rust code fails due to unsupported platform tag 'linux_x86_64'

---

_Issue opened by @giacomopiccinini on 2024-12-12 15:43_

Hi! 

We are experiencing the following issue when building and publishing a mixed python-rust codebase to PyPi. 

Prior to introducing the rust part, in the same project, we were able to successfully build and publish. No issues whatsoever. 

Then, we introduced the rust part with the bindings via pyo3 and maturin. No problem at all with the build part. However, we can't get the code to be published on PyPi due to a "wrong" tag. This is the output we get

```
Successfully built dist/gemmoai-0.1.19.tar.gz
Successfully built dist/gemmoai-0.1.19-cp38-abi3-linux_x86_64.whl
Please enter your PyPI token:

Publishing package...
warning: `uv publish` is experimental and may change without warning
Publishing 2 files https://upload.pypi.org/legacy/
Uploading gemmoai-0.1.19-cp38-abi3-linux_x86_64.whl (2.5MiB)
error: Failed to publish `dist/gemmoai-0.1.19-cp38-abi3-linux_x86_64.whl` to https://upload.pypi.org/legacy/
  Caused by: Upload failed with status code 400 Bad Request. Server says: 400 Binary wheel 'gemmoai-0.1.19-cp38-abi3-linux_x86_64.whl' has an unsupported platform tag 'linux_x86_64'.
```

I suppose the issue is that PyPi requires some flavour of "manylinux" in the tag, but it's unclear where we should enforce this. Our current pyproject.toml reads

```
[project]
name = "gemmoai"
version = "0.1.19"
description = "GemmoAI Python Library"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "maturin>=1.7.6",
    "pymupdf>=1.24.14",
    "tomli>=2.2.1",
]

[tool.maturin]
python-packages = ["gemmoai"]
python-source = "src"
features = ["pyo3/extension-module"]
module-name = "gemmoai.rust" 

[build-system]
requires = ["maturin>=1.0,<2.0"]
build-backend = "maturin"
```

We have tried some solutions based on LLMs suggestion, e.g. adding

```
[tool.maturin]
manylinux = "manylinux2014"
```
or 

```
[tool.maturin]
python-platform = "x8664-manylinux240"
```
or 
```
[tool.uv.pip]
python-platform = "x8664-manylinux240"
```

The final tag never changes, and hence the upload fails. What is the right solution for this? 

Many thanks in advance, and let us know if we can contribute in any way. 

**Details**
```
uv 0.5.8
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04.5 LTS
Release:	22.04
Codename:	jammy
```

---

_Comment by @giacomopiccinini on 2024-12-13 17:52_

After digging in more I was able to debug this using maturin instead of `uv build`. 

First, I tried to run `maturin build` but I got this error

_"Your library is not manylinux_2_17 (aka manylinux2014) compliant because of the presence of too-recent versioned symbols"_

After digging a little more, I was able to find that I should have run the build inside a container to avoid this issue, i.e. run

```
docker run --rm \
	-v "${pwd}:/io" \
	-w /io \
	ghcr.io/pyo3/maturin \
	build --release \
	-o /io/dist 
```
This created indeed a manylinux wheel that I was then able to upload with `uv publish`. So basically I replaced in my CI pipeline the `uv build` with the docker thing above. It's more of a patch I guess, but it's also the patch maturin people suggest.  

At this point I am not sure this is an issue (if it ever was), or if you'd need to have some other Docker image built on top of the maturin one with uv installed to replicate the maturin patch? 

**Bonus**
In trying to replicate the error message _"Your library [...]"_ I re-ran `maturin build` but this time compiled correctly to manylinux. Not sure why is that. However, `uv build` keeps creating the "wrong" thing. 

---

_Assigned to @konstin by @zanieb on 2024-12-13 18:15_

---

_Label `bug` added by @konstin on 2024-12-17 12:44_

---

_Referenced in [astral-sh/uv#9970](../../astral-sh/uv/pulls/9970.md) on 2024-12-17 12:46_

---

_Closed by @konstin on 2024-12-17 13:35_

---

_Reopened by @konstin on 2024-12-17 13:48_

---

_Label `bug` removed by @konstin on 2025-01-28 11:36_

---

_Label `question` added by @konstin on 2025-01-28 11:36_

---

_Unassigned @konstin by @konstin on 2025-01-28 11:48_

---

_Comment by @konstin on 2025-01-28 11:48_

`uv build` doesn't handle anything manylinux-related, that must be done by the build backend, in this case maturin. The maturin containers are convenience wrappers around the containers from https://github.com/pypa/manylinux, which are the official solution for that. If you want to use `uv build`, you need to install uv in one of those containers for your build, otherwise you've correctly described the `maturin build` setup.

---

_Closed by @konstin on 2025-01-28 11:48_

---
