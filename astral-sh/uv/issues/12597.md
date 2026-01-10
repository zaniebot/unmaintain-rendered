```yaml
number: 12597
title: Markers for Glibc/manylinux version
type: issue
state: open
author: PhilipVinc
labels:
  - enhancement
assignees: []
created_at: 2025-04-01T09:11:33Z
updated_at: 2025-10-28T14:55:34Z
url: https://github.com/astral-sh/uv/issues/12597
synced_at: 2026-01-10T03:23:53Z
```

# Markers for Glibc/manylinux version

---

_Issue opened by @PhilipVinc on 2025-04-01 09:11_

### Summary

I am trying to install `jax[cuda12]` on a system with an older GLIBC version 2.17.

Until a few months ago, this was all good, however recent `jax[cuda12]` versions have a lower bound requirement on `nvidia-cublas-cu12` (and many other cuda packages) which require glibc 2.27.

In short, on this machine installing with uv `jax[cuda12]` will fail with the error below:

```bash
(base) [filippo.vicentini@cholesky-login02 test]$ uv init --bare
Initialized project `test`
(base) [filippo.vicentini@cholesky-login02 test]$ uv add 'jax[cuda12]>=0.4.35'
Using CPython 3.12.7
Creating virtual environment at: .venv
Resolved 19 packages in 597ms
error: Distribution `nvidia-cublas-cu12==12.8.4.1 @ registry+https://pypi.org/simple` can't be installed because it doesn't have a source distribution or wheel for the current platform

hint: You're on Linux (`manylinux_2_17_x86_64`), but `nvidia-cublas-cu12` (v12.8.4.1) only has wheels for the following platforms: `manylinux_2_27_aarch64`, `manylinux_2_27_x86_64`, `win_amd64`
```
(verbose output at [this gist](https://gist.github.com/PhilipVinc/408c84fa53ee8843aa65f77a54b9502c) )

However, I would have expected uv to manually to some backtracking and find a valid version.
Indeed, if I do it by hand, I was able to identify this set of valid dependency constraints:

```bash
uv add 'jax[cuda12]>=0.4.35' 'nvidia-cublas-cu12<=12.6.3.3' 'nvidia-cudnn-cu12<9.5.1.17' 'nvidia-cusolver-cu12<=11.7.1.2'
```

--

If I understand correctly, uv is locking the environment correctly, but then it cannot sync/install the locked dependencies because of the old glibc version.

While this seems from one point of view reasonable especially if ran as `uv add --no-sync`, I suspect that `uv add` could behave differently when used directly as `uv add`, and attempt to resolve for an older glibc.

I also suspect that uv has options to automatically use the good platform, but could not find this information in the documentation. 
Moreover, while I can understand the error and that it stems from glibc, some of my 'less knowledgeable' colleagues where puzzled and had no idea how to solve this (for example the need to manually look for working versions). 

I suspect a better more explicative error message might help


### Platform

Linux 3.10.0-1062.el7.x86_64 x86_64 GNU/Linux

### Version

uv 0.6.10

### Python version

Python 3.12.7

---

_Label `bug` added by @PhilipVinc on 2025-04-01 09:11_

---

_Renamed from "Failed resolution" to "UV and GlibC requirement, unexpected error and no backtracking" by @PhilipVinc on 2025-04-01 09:12_

---

_Comment by @charliermarsh on 2025-04-01 12:53_

There's a fairly extensive write-up on "packages without a source distribution" here: https://docs.astral.sh/uv/concepts/resolution/#required-environments

---

_Label `bug` removed by @charliermarsh on 2025-04-01 12:55_

---

_Label `question` added by @charliermarsh on 2025-04-01 12:55_

---

_Comment by @PhilipVinc on 2025-04-03 08:17_

Sorry, I don't understand how to specify in the pyproject.toml a 'platform marker' that requires `manylinux_2_17`/`manylinux2014`. 

I see that I can require a given OS or other [environment markers](https://packaging.python.org/en/latest/specifications/dependency-specifiers/#environment-markers) but I don't understand how to require a specific manylinux version

---

_Comment by @konstin on 2025-10-24 13:44_

Currently, we don't have a way to force backtracking for glibc (or musl) versions.

---

_Label `question` removed by @konstin on 2025-10-24 13:44_

---

_Label `enhancement` added by @konstin on 2025-10-24 13:44_

---

_Renamed from "UV and GlibC requirement, unexpected error and no backtracking" to "Markers for Glibc/manylinux version" by @konstin on 2025-10-28 14:55_

---
