```yaml
number: 7543
title: uv 0.4.12 failing to read distribution cache in github actions
type: issue
state: closed
author: nijel
labels:
  - bug
  - cache
assignees: []
created_at: 2024-09-19T11:42:41Z
updated_at: 2024-09-19T20:31:57Z
url: https://github.com/astral-sh/uv/issues/7543
synced_at: 2026-01-10T04:45:10Z
```

# uv 0.4.12 failing to read distribution cache in github actions

---

_Issue opened by @nijel on 2024-09-19 11:42_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

This seems almost like https://github.com/astral-sh/uv/issues/7485, but I still see it happen with 0.4.12:

```
 error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: mercurial==6.8.1
  Caused by: /home/runner/work/_temp/setup-uv-cache/built-wheels-v3/pypi/mercurial/6.8.1/kYDLbz0owpJdwyMLRFXvG/mercurial-6.8.1.tar.gz does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
```

The cache was created by 0.4.12 as uv version is in GitHub cache key:

```
 uv cache restored from GitHub Actions cache with key: setup-uv-1-x86_64-unknown-linux-gnu-0.4.12
-dc3406fd913fe4622566cb4edc384b2e3b933f9767e05f304e1f3ec73a14f457
```

When executed without a cache, it runs just fine.

PS: Action log can be seen here: https://github.com/WeblateOrg/weblate/actions/runs/10940191066/job/30372045488

---

_Label `bug` added by @zanieb on 2024-09-19 11:56_

---

_Label `cache` added by @zanieb on 2024-09-19 11:56_

---

_Comment by @charliermarsh on 2024-09-19 12:13_

At least on a first pass I can't reproduce that locally, but I'll keep trying.

---

_Comment by @charliermarsh on 2024-09-19 12:20_

Actually, I think I have an idea as to how this could happen if you're sharing that cache between multiple Python versions. Is that possible? I reproduced with some manual cache editing:

```
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: mercurial==6.8.1
  Caused by: /Users/crmarsh/.cache/uv/built-wheels-v3/pypi/mercurial/6.8.1/bZrFqUG39NkQoZTpIu0WD/mercurial-6.8.1.tar.gz does not appear to be a Python project, as neither `pyproject.toml` nor `setup.py` are present in the directory
```

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-19 12:20_

---

_Comment by @nijel on 2024-09-19 12:20_

Ah, that is most likely it. We use multiple Python versions, and I didn't realize that the cache key doesn't include it.

---

_Comment by @zanieb on 2024-09-19 12:21_

I feel like the cache _should_ be robust to multiple Python versions

---

_Comment by @charliermarsh on 2024-09-19 12:21_

Yeah it should.

---

_Comment by @charliermarsh on 2024-09-19 12:22_

In general it _is_ robust to multiple Python versions, platforms, etc. But there's a bug here whereby if you run `uv cache prune --ci` (which runs via the GitHub Action), we perform some modifications that break assumptions.

---

_Comment by @charliermarsh on 2024-09-19 12:25_

I'll fix this today.

---

_Comment by @nijel on 2024-09-19 12:30_

In our case, the GitHub cache really should be per Python version, so I've just changed that. Without that, it is not really useful as compiled wheels are not compatible across Python versions.

---

_Closed by @charliermarsh on 2024-09-19 20:31_

---
