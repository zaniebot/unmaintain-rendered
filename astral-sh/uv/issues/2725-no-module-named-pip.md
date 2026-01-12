```yaml
number: 2725
title: "No module named 'pip'"
type: issue
state: closed
author: shenwpo
labels:
  - duplicate
assignees: []
created_at: 2024-03-29T08:52:45Z
updated_at: 2024-03-29T14:23:40Z
url: https://github.com/astral-sh/uv/issues/2725
synced_at: 2026-01-12T15:58:40Z
```

# No module named 'pip'

---

_@shenwpo_

Error when installing certain dependencies in uv.

* A minimal code snippet that reproduces the bug.
`uv pip install pyyaml==5.4.1 --verbose`
`uv pip install chumpy fast-slic --verbose`
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.

```
INFO Found a virtualenv through VIRTUAL_ENV at: /home/xxx/code3/uv_build/.venv
DEBUG Cached interpreter info for Python 3.8.10, skipping probing: .venv/bin/python
DEBUG Using Python 3.8.10 environment at .venv/bin/python
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.8.10
DEBUG Adding direct dependency: chumpy*
DEBUG Adding direct dependency: fast-slic*
DEBUG Found stale response for: https://pypi.org/simple/chumpy/
DEBUG Sending revalidation request for: https://pypi.org/simple/chumpy/
DEBUG No credentials found for: https://pypi.org/simple/chumpy/
DEBUG Found stale response for: https://pypi.org/simple/fast-slic/
DEBUG Sending revalidation request for: https://pypi.org/simple/fast-slic/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/fast-slic/
DEBUG No credentials found for: https://pypi.org/simple/fast-slic/
DEBUG Found not-modified response for: https://pypi.org/simple/fast-slic/
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/15/3b/7dee2167feda67e9cc6877d667939e5a44c42a006b1775b18d42c2fedcee/fast-slic-0.4.0.tar.gz
DEBUG Preparing metadata for: fast-slic==0.4.0
DEBUG No static `PKG-INFO` available for: fast-slic==0.4.0 (DynamicPkgInfo(UnsupportedMetadataVersion("1.2")))
DEBUG No static `pyproject.toml` available for: fast-slic==0.4.0 (MissingPyprojectToml)
DEBUG Solving with target Python version 3.8.10
DEBUG Adding direct dependency: setuptools>=40.8.0
DEBUG Found stale response for: https://pypi.org/simple/setuptools/
DEBUG Sending revalidation request for: https://pypi.org/simple/setuptools/
DEBUG No credentials found for already-seen URL: https://pypi.org/simple/setuptools/
DEBUG No credentials found for: https://pypi.org/simple/setuptools/
DEBUG Found not-modified response for: https://pypi.org/simple/chumpy/
DEBUG Searching for a compatible version of chumpy (*)
DEBUG Selecting: chumpy==0.70 (chumpy-0.70.tar.gz)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/01/f7/865755c8bdb837841938de622e6c8b5cb6b1c933bde3bd3332f0cd4574f1/chumpy-0.70.tar.gz
DEBUG Preparing metadata for: chumpy==0.70
DEBUG No static `PKG-INFO` available for: chumpy==0.70 (DynamicPkgInfo(UnsupportedMetadataVersion("1.1")))
DEBUG No static `pyproject.toml` available for: chumpy==0.70 (MissingPyprojectToml)
DEBUG Found not-modified response for: https://pypi.org/simple/setuptools/
DEBUG Searching for a compatible version of setuptools (>=40.8.0)
DEBUG Selecting: setuptools==69.2.0 (setuptools-69.2.0-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/92/e1/1c8bb3420105e70bdf357d57dd5567202b4ef8d27f810e98bb962d950834/setuptools-69.2.0-py3-none-any.whl.metadata
DEBUG Installing in setuptools==69.2.0 in /home/xxx/.cache/uv/.tmpjo5q8j/.venv
DEBUG Requirement already cached: setuptools==69.2.0
DEBUG Installing build requirement: setuptools==69.2.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
DEBUG Installing in setuptools==69.2.0 in /home/xxx/.cache/uv/.tmpZC7EuQ/.venv
DEBUG Requirement already cached: setuptools==69.2.0
DEBUG Installing build requirement: setuptools==69.2.0
DEBUG Calling `setuptools.build_meta:__legacy__.get_requires_for_build_wheel()`
error: Failed to download and build: chumpy==0.70
  Caused by: Failed to build: chumpy==0.70
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 9, in <module>
ModuleNotFoundError: No module named 'pip'

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/home/xxx/.cache/uv/.tmpZC7EuQ/.venv/lib/python3.8/site-packages/setuptools/build_meta.py", line 325, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=['wheel'])
  File "/home/xxx/.cache/uv/.tmpZC7EuQ/.venv/lib/python3.8/site-packages/setuptools/build_meta.py", line 295, in _get_build_requires
    self.run_setup()
  File "/home/xxx/.cache/uv/.tmpZC7EuQ/.venv/lib/python3.8/site-packages/setuptools/build_meta.py", line 487, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/home/xxx/.cache/uv/.tmpZC7EuQ/.venv/lib/python3.8/site-packages/setuptools/build_meta.py", line 311, in run_setup
    exec(code, locals())
  File "<string>", line 11, in <module>
ModuleNotFoundError: No module named 'pip'
---
```

* The current uv platform.
ubuntu 20.04.6 LTS at intel x86 platform
* The current uv version (`uv --version`).
uv 0.1.26


---

_Comment by @zanieb on 2024-03-29 13:34_

These packages have likely not declared a build dependency on `pip` and just "assume" it is available.

---

_Comment by @zanieb on 2024-03-29 13:35_

See https://github.com/astral-sh/uv/issues/2251#issuecomment-1984797316

---

_Label `duplicate` added by @zanieb on 2024-03-29 13:35_

---

_Comment by @shenwpo on 2024-03-29 14:20_

thank you for your reply.

---

_Closed by @shenwpo on 2024-03-29 14:20_

---

_Comment by @zanieb on 2024-03-29 14:23_

Thank you! I'd recommend reporting this upstream.

---
