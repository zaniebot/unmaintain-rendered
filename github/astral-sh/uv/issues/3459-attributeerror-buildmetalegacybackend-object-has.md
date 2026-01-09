---
number: 3459
title: "AttributeError: '_BuildMetaLegacyBackend' object has no attribute 'build_editable'"
type: issue
state: open
author: adminy
labels:
  - needs-mre
assignees: []
created_at: 2024-05-08T13:20:28Z
updated_at: 2025-01-20T09:03:42Z
url: https://github.com/astral-sh/uv/issues/3459
synced_at: 2026-01-07T13:12:17-06:00
---

# AttributeError: '_BuildMetaLegacyBackend' object has no attribute 'build_editable'

---

_Issue opened by @adminy on 2024-05-08 13:20_

I have a pyproject.toml with a couple of optional dependencies.

When running:
```bash
uv pip install -e '.[dep1,dep2,dep3,dep4]' --exclude-newer 2020-12-02

error: Failed to build editables
  Caused by: Failed to build editable: file:///root/repos/MyProject
  Caused by: Build backend failed to build wheel through `build_editable()` with exit status: 1
--- stdout:

--- stderr:
Traceback (most recent call last):
  File "<string>", line 11, in <module>
AttributeError: '_BuildMetaLegacyBackend' object has no attribute 'build_editable'
```


---

_Label `error messages` added by @konstin on 2024-05-08 13:39_

---

_Comment by @charliermarsh on 2024-05-08 14:30_

@konstin - do you know what the root cause is here?

---

_Comment by @konstin on 2024-05-08 15:07_

I'm pretty sure the backend doesn't not support editable builds (PEP 660). We should catch this and show a better error message. We may also need to implement extra behavior for setuptools, needs to be investigated

---

_Comment by @charliermarsh on 2024-07-28 00:46_

@adminy - Do you know what build backend this is using?

---

_Comment by @kimdwkimdw on 2024-10-29 04:08_

Any updates?

---

_Comment by @scotgopal on 2024-12-18 13:52_

> [@adminy](https://github.com/adminy) - Do you know what build backend this is using?

I faced this issue as well. The build backend is `setuptools`. With pip v24.0, doing `pip install -e .` has no issues, but using `uv pip install --no-build-isolation -e .` shows this error. 

---

_Comment by @konstin on 2024-12-18 13:56_

What version of setuptools are you using? What does your `pyproject.toml` look like?

---

_Comment by @scotgopal on 2024-12-19 23:37_

> What version of setuptools are you using? What does your `pyproject.toml` look like?

I am currently using `setup.py`, not a pyproject.toml. The version of setuptools is v59.8.0. I don't have the setup.py with me, atm. Will update this comment when available.

---

_Comment by @konstin on 2025-01-02 12:12_

Can you try with the latest setuptools version?

---

_Comment by @ayjayt on 2025-01-20 01:18_

nb I'm also having issues with this, can't reproduce, only occurs for one project where i tried to run mypy w/ an editable source (using the LEGACY env var that doesn't work anymore), and now no sync works at all... doing a full re-install

---

_Label `needs-mre` added by @konstin on 2025-01-20 09:03_

---

_Label `error messages` removed by @konstin on 2025-01-20 09:03_

---
