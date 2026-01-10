---
number: 9077
title: uv.lock contains entries for unused/incompatible python versions
type: issue
state: closed
author: idlsoft
labels:
  - bug
assignees: []
created_at: 2024-11-13T05:58:30Z
updated_at: 2024-11-13T16:16:40Z
url: https://github.com/astral-sh/uv/issues/9077
synced_at: 2026-01-10T01:24:36Z
---

# uv.lock contains entries for unused/incompatible python versions

---

_Issue opened by @idlsoft on 2024-11-13 05:58_

I created a simple project using `uv init`, and added one dependency:
```toml
[project]
name = "myapp"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.12"
dependencies = [
  "frozendict ~= 2.4.6",
]
```
`.python-version` is 
```
3.12
```

If I run `uv lock` I get a lock file with entries for python 3.11, 3.12, 3.13:
```toml
version = 1
requires-python = ">=3.12"

[[package]]
name = "frozendict"
version = "2.4.6"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/bb/59/19eb300ba28e7547538bdf603f1c6c34793240a90e1a7b61b65d8517e35e/frozendict-2.4.6.tar.gz", hash = "sha256:df7cd16470fbd26fc4969a208efadc46319334eb97def1ddf48919b351192b8e", size = 316416 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/04/13/d9839089b900fa7b479cce495d62110cddc4bd5630a04d8469916c0e79c5/frozendict-2.4.6-py311-none-any.whl", hash = "sha256:d065db6a44db2e2375c23eac816f1a022feb2fa98cbb50df44a9e83700accbea", size = 16148 },
    { url = "https://files.pythonhosted.org/packages/ba/d0/d482c39cee2ab2978a892558cf130681d4574ea208e162da8958b31e9250/frozendict-2.4.6-py312-none-any.whl", hash = "sha256:49344abe90fb75f0f9fdefe6d4ef6d4894e640fadab71f11009d52ad97f370b9", size = 16146 },
    { url = "https://files.pythonhosted.org/packages/a5/8e/b6bf6a0de482d7d7d7a2aaac8fdc4a4d0bb24a809f5ddd422aa7060eb3d2/frozendict-2.4.6-py313-none-any.whl", hash = "sha256:7134a2bb95d4a16556bb5f2b9736dceb6ea848fa5b6f3f6c2d6dba93b44b4757", size = 16146 },
]

[[package]]
name = "myapp"
version = "0.1.0"
source = { virtual = "." }
dependencies = [
    { name = "frozendict" },
]

[package.metadata]
requires-dist = [{ name = "frozendict", specifier = "~=2.4.6" }]
```

Running `uv 0.5.1` on a Mac.
If I had to guess it's the `-none-any` part that throws it off, because it doesn't seem to be happening for other packages.

---

_Comment by @charliermarsh on 2024-11-13 14:58_

The 3.11 wheel could perhaps be omitted, but the 3.13 wheel is correct -- we're locking for all versions later than 3.12. You could use `requires-python = ">=3.12, <3.13"` if you wanted to omit the 3.13 wheel.

However, I think `py311-none ` is pretty strange? It's not being omitted because the `none` indicates it's compatible with other Python versions, IIUC. \cc @konstin

---

_Label `question` added by @charliermarsh on 2024-11-13 14:58_

---

_Comment by @idlsoft on 2024-11-13 15:02_

> The 3.11 wheel could perhaps be omitted, but the 3.13 wheel is correct -- we're locking for all versions later than 3.12. You 

That's exactly what drew my attention. 3.13 makes sense to me but not 3.11.

---

_Comment by @konstin on 2024-11-13 15:29_

`py311-none` is an odd tag (i don't think it should be used), though it's something we should be able to handle: https://github.com/astral-sh/uv/blob/15ef807c80c2a6d8945a9fe53707e5c0551a87b1/crates/uv-resolver/src/requires_python/tests.rs#L76

---

_Label `question` removed by @charliermarsh on 2024-11-13 15:40_

---

_Label `bug` added by @charliermarsh on 2024-11-13 15:40_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-13 16:11_

---

_Comment by @charliermarsh on 2024-11-13 16:11_

I can fix.

---

_Comment by @charliermarsh on 2024-11-13 16:14_

Actually, _is_ this wrong? Can't Python 3.12 run a `py311-none` wheel...?

---

_Comment by @charliermarsh on 2024-11-13 16:15_

We have tests today that assert that this behavior is _correct_.

---

_Comment by @charliermarsh on 2024-11-13 16:16_

Okay yeah, I think per spec it's actually correct to include this. See: https://packaging.python.org/en/latest/specifications/platform-compatibility-tags/#use. There, they say that `py32-none-any` is a match for `CPython 3.3` (but `cp32-none-any` is not).

---

_Closed by @charliermarsh on 2024-11-13 16:16_

---
