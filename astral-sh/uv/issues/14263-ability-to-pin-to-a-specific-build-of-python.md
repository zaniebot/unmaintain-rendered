---
number: 14263
title: Ability to pin to a specific build of python-build-standalone
type: issue
state: open
author: notatallshaw
labels:
  - enhancement
assignees: []
created_at: 2025-06-25T16:07:00Z
updated_at: 2025-12-04T23:00:29Z
url: https://github.com/astral-sh/uv/issues/14263
synced_at: 2026-01-10T01:25:43Z
---

# Ability to pin to a specific build of python-build-standalone

---

_Issue opened by @notatallshaw on 2025-06-25 16:07_

One reproducibility issue I have with using uv to install Python, when compared to conda, is I can't pin the exact build of Python between uv versions.

This is a problem because it means if I have a script that creates the Python environment between two different users the Python build they get will be based on the uv version they have installed. And that uv version upgrades are coupled to the python-build-standalone build upgrades.

I understand there is a design issue here in how uv maps to python-build-standalone releases, but I would be happy with a verbose escape hatch, as I only need to install for one platform I would be fine with manually providing the url and any metadata uv needs about what Python version is I am installing.

Unless I am missing that some escape hatch already exists?

---

_Label `enhancement` added by @notatallshaw on 2025-06-25 16:07_

---

_Comment by @krazyito65 on 2025-12-04 22:11_

personally, this would be useful for me. the current solution i have to do is make sure i 'pin' which version of `uv` i install, but if i don't it uses the latest version of `python-build-standalone`.

my usecase is that `uv` is in a much faster approval process to bring internally into my company since its a simple pip package, but internally hosting `python-build-standalone` is a much slower, and not as regularly needed occurrence, hence we are usually somewhat behind on which version of `python-build-standalone` we are using with our internal mirror.

---

_Comment by @zanieb on 2025-12-04 22:39_

You can do rudimentary pinning now, see https://github.com/astral-sh/uv/pull/15314

https://github.com/astral-sh/uv/blob/ed63be5dab467f370b92f9bc0658418780b1a5c7/crates/uv/tests/it/python_install.rs#L3627


---

_Comment by @krazyito65 on 2025-12-04 22:41_

oh, this is perfect, im going to try it out, thank you!

---

_Comment by @krazyito65 on 2025-12-04 22:52_

```

[2025-12-04T22:49:20.825Z] DEBUG Skipping interpreter at `/usr/bin/python3` from search path: does not satisfy request `3.13.9`

[2025-12-04T22:49:20.825Z] DEBUG Skipping download `cpython-3.13.9-linux-x86_64-gnu`: requested build version `20251014` does not match download build version `20251120`

[2025-12-04T22:49:20.825Z] DEBUG Skipping download `cpython-3.13.9-linux-x86_64_v2-gnu`: requested build version `20251014` does not match download build version `20251120`

[2025-12-04T22:49:20.826Z] DEBUG Skipping download `cpython-3.13.9-linux-x86_64_v3-gnu`: requested build version `20251014` does not match download build version `20251120`

[2025-12-04T22:49:20.826Z] DEBUG Skipping download `cpython-3.13.9-linux-x86_64_v4-gnu`: requested build version `20251014` does not match download build version `20251120`

[2025-12-04T22:49:20.826Z] DEBUG Skipping download `cpython-3.13.9-linux-x86_64-gnu`: requested build version `20251014` does not match download build version `20251120`

[2025-12-04T22:49:20.826Z] DEBUG Skipping download `cpython-3.13.9-linux-x86_64_v2-gnu`: requested build version `20251014` does not match download build version `20251120`

[2025-12-04T22:49:20.826Z] DEBUG Skipping download `cpython-3.13.9-linux-x86_64_v3-gnu`: requested build version `20251014` does not match download build version `20251120`

[2025-12-04T22:49:20.826Z] DEBUG Skipping download `cpython-3.13.9-linux-x86_64_v4-gnu`: requested build version `20251014` does not match download build version `20251120`

[2025-12-04T22:49:20.826Z] DEBUG No downloads are available for Python 3.13.9
```

did i do something wrong?
i have
`UV_PYTHON_CPYTHON_BUILD = "20251014"`
and 
`UV_PYTHON_INSTALL_MIRROR=`https://......./python-build-standalone/`

within that mirror there is a folder marked: 20251014/ and has the binaries within.

---

_Comment by @zanieb on 2025-12-04 22:54_

It doesn't work that way, it requires a `build` field in the download metadata and a `BUILD` file in the distribution. It's not set up to make it easy to fetch arbitrary specific builds yet though, it's set up to fail if your pinned build doesn't match what's available.

---

_Comment by @krazyito65 on 2025-12-04 23:00_

ah i see, i misunderstood, then my previous statement i guess still stands as a feature request, but at least with this you can ensure that people are using the same build

---
