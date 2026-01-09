---
number: 8863
title: uv resolves to older numba version incompatible with python version
type: issue
state: closed
author: MitchellAcoustics
labels:
  - duplicate
assignees: []
created_at: 2024-11-06T14:12:56Z
updated_at: 2024-11-06T18:00:07Z
url: https://github.com/astral-sh/uv/issues/8863
synced_at: 2026-01-07T13:12:18-06:00
---

# uv resolves to older numba version incompatible with python version

---

_Issue opened by @MitchellAcoustics on 2024-11-06 14:12_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When attempting to add a new dependency to a fresh env which depends on numba, it fails to install with:

```bash
# uv 0.4.28 (debe67ffd 2024-10-28)
# ./.python-version 3.12
uv cache clean
uv add scikit-maad
```

```bash
DEBUG Downloading source distribution: numba==0.53.1
DEBUG Traceback (most recent call last):
DEBUG   File "<string>", line 14, in <module>
DEBUG   File "/Users/mitch/.cache/uv/builds-v0/.tmp6ZFD4B/lib/python3.12/site-packages/setuptools/build_meta.py", line 333, in get_requires_for_build_wheel
DEBUG     return self._get_build_requires(config_settings, requirements=[])
DEBUG            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
DEBUG   File "/Users/mitch/.cache/uv/builds-v0/.tmp6ZFD4B/lib/python3.12/site-packages/setuptools/build_meta.py", line 303, in _get_build_requires
DEBUG     self.run_setup()
DEBUG   File "/Users/mitch/.cache/uv/builds-v0/.tmp6ZFD4B/lib/python3.12/site-packages/setuptools/build_meta.py", line 521, in run_setup
DEBUG     super().run_setup(setup_script=setup_script)
DEBUG   File "/Users/mitch/.cache/uv/builds-v0/.tmp6ZFD4B/lib/python3.12/site-packages/setuptools/build_meta.py", line 319, in run_setup
DEBUG     exec(code, locals())
DEBUG   File "<string>", line 55, in <module>
DEBUG   File "<string>", line 52, in _guard_py_ver
DEBUG RuntimeError: Cannot install on Python version 3.12.5; only versions >=3.6,<3.10 are supported.
DEBUG Released lock at `/Users/mitch/.cache/uv/sdists-v5/pypi/llvmlite/0.36.0/.lock`
DEBUG Released lock at `/Users/mitch/.cache/uv/sdists-v5/pypi/numba/0.53.1/.lock`
error: Failed to prepare distributions
  Caused by: Failed to download and build `llvmlite==0.36.0`
  Caused by: Build backend failed to determine requirements with `build_wheel()` (exit status: 1)

[stderr]
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/Users/mitch/.cache/uv/builds-v0/.tmp6ZFD4B/lib/python3.12/site-packages/setuptools/build_meta.py", line 333, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/mitch/.cache/uv/builds-v0/.tmp6ZFD4B/lib/python3.12/site-packages/setuptools/build_meta.py", line 303, in _get_build_requires
    self.run_setup()
  File "/Users/mitch/.cache/uv/builds-v0/.tmp6ZFD4B/lib/python3.12/site-packages/setuptools/build_meta.py", line 521, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/Users/mitch/.cache/uv/builds-v0/.tmp6ZFD4B/lib/python3.12/site-packages/setuptools/build_meta.py", line 319, in run_setup
    exec(code, locals())
  File "<string>", line 55, in <module>
  File "<string>", line 52, in _guard_py_ver
RuntimeError: Cannot install on Python version 3.12.5; only versions >=3.6,<3.10 are supported.
```

In my case, I am trying to install [`scikit-maad`](https://github.com/scikit-maad/scikit-maad), which (as of v1.4.3) depends on `resampy >=0.4` -> `numba>=0.53`. From a fresh venv with uv cache cleared, this resolves to v0.53.1 with 

```
DEBUG Searching for a compatible version of numba (>=0.53)
DEBUG Selecting: numba==0.53.1 [preference] (numba-0.53.1.tar.gz)
``` 

However, numba v0.53.1 is incompatible with my pinned python v3.12: 

```
# from https://github.com/numba/numba/blob/0.53.1/setup.py
min_python_version = "3.6"
max_python_version = "3.10"  # exclusive
```

I would expect that uv would handle this when resolving - recognise numba 0.53.1 is not compatible with python 3.12 and find a newer version which is, since higher versions are allowed by both resampy and scikit-maad. I'm not sure why it considers 0.53.1 'preferred'?

I can confirm it's not an actual dependency conflict - clearing uv cache and doing `uv add numba` first (which sets the dependency to the latest `numba>=0.60.0`), then `uv add scikit-maad` installs everything fine. 

---

_Comment by @MitchellAcoustics on 2024-11-06 14:16_

Just to be extra sure, I just updated uv to v0.4.30 and still get the same.

---

_Comment by @zanieb on 2024-11-06 16:24_

Probably a duplicate of https://github.com/astral-sh/uv/issues/8157

---

_Comment by @charliermarsh on 2024-11-06 16:26_

Yeah, there's an extensive write-up on this here: https://github.com/astral-sh/uv/issues/6281#issuecomment-2316240724

---

_Closed by @charliermarsh on 2024-11-06 16:26_

---

_Label `duplicate` added by @zanieb on 2024-11-06 16:27_

---

_Comment by @MitchellAcoustics on 2024-11-06 16:38_

Thanks! I saw those other discussions, what I'm confused by is why the Python version constraints are not respected. My assumption would be numba 0.53 wouldn't even be considered because of its python constraints, rather than the other dependency constraints.

---

_Comment by @charliermarsh on 2024-11-06 16:42_

Are you referring to the `RuntimeError: Cannot install on Python version 3.12.5; only versions >=3.6,<3.10 are supported.` error?

---

_Comment by @MitchellAcoustics on 2024-11-06 16:47_

Somewhat, but that happens when trying to install llvmlite. My impression would be that numba v0.53.1 shouldn't even be a candidate for resolving since it places an upper bound on the python version, which is then what leads to that error happening. I'm not sure when they started supporting python 3.12, but I would assume that's where the resolution would then start looking for a version compatible with the other dependencies and v0.53 wouldn't be considered. That said, I don't know anything about how dependency resolution works, so I could have an incorrect understanding!

```
# from https://github.com/numba/numba/blob/0.53.1/setup.py
min_python_version = "3.6"
max_python_version = "3.10"  # exclusive
```

---

_Comment by @charliermarsh on 2024-11-06 16:52_

Yeah, we don't respect upper-bounds on `requires-python`. Our stance is that `requires-python` is intended to indicate minimum Python version compatibility. There's a lot of discussion around that elsewhere -- here's one reference that links to a few more: https://github.com/astral-sh/uv/issues/4022#issuecomment-2148505055

---

_Comment by @MitchellAcoustics on 2024-11-06 18:00_

Thank you, that's very helpful! Now that I think about it, that sort of explicit upper bounding was exactly one of the issues I had with poetry and why I stopped using it even before uv came around.

I suppose I'll just set my own numba version dependency in my package, although it feels like there's a more elegant solution. 



---

_Referenced in [astral-sh/uv#12126](../../astral-sh/uv/issues/12126.md) on 2025-03-12 01:46_

---

_Referenced in [MaartenGr/BERTopic#2311](../../MaartenGr/BERTopic/issues/2311.md) on 2025-03-23 21:17_

---
