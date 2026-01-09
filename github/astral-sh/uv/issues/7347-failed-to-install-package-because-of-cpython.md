---
number: 7347
title: Failed to install package because of CPython 
type: issue
state: closed
author: AmrMKayid
labels:
  - question
assignees: []
created_at: 2024-09-12T23:48:44Z
updated_at: 2024-09-13T15:49:03Z
url: https://github.com/astral-sh/uv/issues/7347
synced_at: 2026-01-07T13:12:17-06:00
---

# Failed to install package because of CPython 

---

_Issue opened by @AmrMKayid on 2024-09-12 23:48_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag. `uv add madmom`
* The current uv platform. `x86_64`
* The current uv version (`uv --version`): `uv 0.4.9`
-->

Unable to install [madmom](https://github.com/CPJKU/madmom) because of missing `CPython`

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag. `uv add madmom`
* The current uv platform. `x86_64`
* The current uv version (`uv --version`): `uv 0.4.9`

```shell
uv add madmom==0.16.1
Resolved 537 packages in 534ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: madmom==0.16.1
  Caused by: Build backend failed to determine extra requires with `build_wheel()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 14, in <module>
  File "/root/.cache/uv/builds-v0/.tmppCaaWn/lib/python3.10/site-packages/setuptools/build_meta.py", line 332, in get_requires_for_build_wheel
    return self._get_build_requires(config_settings, requirements=[])
  File "/root/.cache/uv/builds-v0/.tmppCaaWn/lib/python3.10/site-packages/setuptools/build_meta.py", line 302, in _get_build_requires
    self.run_setup()
  File "/root/.cache/uv/builds-v0/.tmppCaaWn/lib/python3.10/site-packages/setuptools/build_meta.py", line 503, in run_setup
    super().run_setup(setup_script=setup_script)
  File "/root/.cache/uv/builds-v0/.tmppCaaWn/lib/python3.10/site-packages/setuptools/build_meta.py", line 318, in run_setup
    exec(code, locals())
  File "<string>", line 12, in <module>
ModuleNotFoundError: No module named 'Cython'
---
```

---

_Comment by @zanieb on 2024-09-13 01:48_

Hi! It looks like it says [Cython](https://cython.org) without the "P" which is quite important! It's actually a separate Python compiler that I guess this package needs?

---

_Label `question` added by @zanieb on 2024-09-13 01:48_

---

_Comment by @notatallshaw on 2024-09-13 15:46_

There hasn't been a release of the project on pypi in over 5 years: https://pypi.org/project/madmom/#history

So while they have a correctly defined build system: https://github.com/CPJKU/madmom/blob/main/pyproject.toml, you can only use it by installing from source.

I would contact the project authors on where exactly you're supposed to install the project from.

---

_Comment by @charliermarsh on 2024-09-13 15:49_

Yeah you'll need to install this without build isolation in order to get it working (see, e.g., https://docs.astral.sh/uv/concepts/projects/#build-isolation). It's an issue with the package itself.

For example:
```
❯ uv venv
Using Python 3.12.1
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

❯ uv pip install cython setuptools numpy
Resolved 3 packages in 6ms
Installed 3 packages in 22ms
 + cython==3.0.11
 + numpy==2.1.1
 + setuptools==74.1.2

❯ uv pip install madmom==0.16.1 --no-build-isolation
Resolved 6 packages in 8ms
Installed 4 packages in 23ms
 + madmom==0.16.1
 + mido==1.3.2
 + packaging==23.2
 + scipy==1.14.1
```

---

_Closed by @charliermarsh on 2024-09-13 15:49_

---

_Referenced in [astral-sh/uv#7450](../../astral-sh/uv/issues/7450.md) on 2024-09-17 08:32_

---

_Referenced in [aboutcode-org/cyseq#7](../../aboutcode-org/cyseq/issues/7.md) on 2025-10-29 15:33_

---
