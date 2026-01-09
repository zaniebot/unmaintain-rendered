---
number: 2566
title: HTTP status client error (401 Unauthorized) / (403 Forbidden) for packages hosted on a JFrog PyPI repo
type: issue
state: closed
author: ealap
labels:
  - bug
  - registry
assignees: []
created_at: 2024-03-20T15:13:53Z
updated_at: 2024-03-21T17:10:44Z
url: https://github.com/astral-sh/uv/issues/2566
synced_at: 2026-01-07T13:12:17-06:00
---

# HTTP status client error (401 Unauthorized) / (403 Forbidden) for packages hosted on a JFrog PyPI repo

---

_Issue opened by @ealap on 2024-03-20 15:13_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Similar to an issue reported at #2444, the authentication fails when trying to install packages from extra index URL pointing to a JFrog PyPI repo
```shell
export UV_EXTRA_INDEX_URL="https://<JFROG_USERNAME>:<JFROG_ACCESS_TOKEN>@<JFROG_ARTIFACTORY_URL>/api/pypi/simple"
uv pip install --verbose --compile --no-cache --requirement="requirements.txt" --override="override.txt"

# Output
# A bunch logs like these
DEBUG No cache entry for: https://<JFROG_ARTIFACTORY_URL>/api/pypi/packages/xxx/xxx/packageAAA-1.0.0.whl#sha256=xxx
DEBUG Adding authentication to already-seen URL: https://<JFROG_ARTIFACTORY_URL>/api/pypi/packages/xxx/xxx/packageAAA-1.0.0.whl#sha256=xxx

# then finally 403 error
error: Failed to download: packageBBB==31.4.0
  Caused by: HTTP status client error (403 Forbidden) for url (https://<JFROG_ARTIFACTORY_URL>/api/pypi/packages/xxx/xxx/packageBBB==31.4.0.whl#sha256=xxx)

# sometimes 401 error
error: Failed to download: packageCCC==2.3.0
  Caused by: HTTP status client error (401 Unauthorized) for url (https://<JFROG_ARTIFACTORY_URL>/api/pypi/packages/xxx/xxx/packageCCC==2.3.0.whl#sha256=xxx)
```

uv versions where it doesn't work:
```shell
uv==0.1.19
uv==0.1.22 # current latest at the time of this issue
```

The issue was fixed by on v0.1.20 likely because it has the same root cause as #2444 that was fixed by #2446). But it got broken again on v0.1.22 likely due to #2449. 

---

_Label `bug` added by @zanieb on 2024-03-20 16:01_

---

_Label `registry` added by @zanieb on 2024-03-20 16:01_

---

_Assigned to @zanieb by @zanieb on 2024-03-20 16:01_

---

_Comment by @zanieb on 2024-03-21 04:24_

Can you confirm that the `JFROG_ARTIFACTORY_URL` part of the URL is consistent?

Are there logs like `No credentials found for: ...` and `Request already has an authorization header: ...`? Do you think you can share the full logs?

---

_Comment by @ealap on 2024-03-21 10:18_

Yes, `JFROG_ARTIFACTORY_URL` is the same for all packages.

<details>

<summary>Here's the install log (with redacted info)</summary>

```
DEBUG Found a virtualenv through VIRTUAL_ENV at: /home/ealap/.cache/pypoetry/virtualenvs/sample-project-L4q7N5gd-py3.10
DEBUG Probing interpreter info for: /home/ealap/.cache/pypoetry/virtualenvs/sample-project-L4q7N5gd-py3.10/bin/python
DEBUG Found Python 3.10.13 for: /home/ealap/.cache/pypoetry/virtualenvs/sample-project-L4q7N5gd-py3.10/bin/python
DEBUG Using Python 3.10.13 environment at [36m/home/ealap/.cache/pypoetry/virtualenvs/sample-project-L4q7N5gd-py3.10/bin/python[39m
DEBUG Using registry request timeout of 300s
DEBUG Solving with target Python version 3.10.13
DEBUG Adding direct dependency: sympy==1.11.0
DEBUG No cache entry for: https://<JFROG_ARTIFACTORY_URL>/api/pypi/build/simple/sympy/
DEBUG Request already has an authorization header: https://<JFROG_ARTIFACTORY_URL>/api/pypi/build/simple/sympy/
DEBUG Searching for a compatible version of sympy (==1.11.0)
DEBUG Selecting: sympy==1.11.0 (sympy-1.11.0-py3-none-any.whl)
DEBUG No cache entry for: https://<JFROG_ARTIFACTORY_URL>/api/pypi/build/packages/packages/d0/04/66be21ceb305c66a4b326b0ae44cc4f027a43bc08cac204b48fb45bb3653/sympy-1.11.0-py3-none-any.whl#sha256=df75d738930f6fe9ebe7034e59d56698f29e85f443f743e51e47df0caccc2130
DEBUG Adding authentication to already-seen URL: https://<JFROG_ARTIFACTORY_URL>/api/pypi/build/packages/packages/d0/04/66be21ceb305c66a4b326b0ae44cc4f027a43bc08cac204b48fb45bb3653/sympy-1.11.0-py3-none-any.whl#sha256=df75d738930f6fe9ebe7034e59d56698f29e85f443f743e51e47df0caccc2130
error: Failed to download: sympy==1.11.0
  Caused by: HTTP status client error (403 Forbidden) for url (https://<JFROG_ARTIFACTORY_URL>/api/pypi/build/packages/packages/d0/04/66be21ceb305c66a4b326b0ae44cc4f027a43bc08cac204b48fb45bb3653/sympy-1.11.0-py3-none-any.whl#sha256=df75d738930f6fe9ebe7034e59d56698f29e85f443f743e51e47df0caccc2130)
```

</details>

---

_Comment by @zanieb on 2024-03-21 14:45_

Thanks!

---

_Referenced in [astral-sh/uv#2592](../../astral-sh/uv/pulls/2592.md) on 2024-03-21 16:23_

---

_Closed by @zanieb on 2024-03-21 17:10_

---
