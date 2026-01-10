---
number: 16804
title: uv sync CI secrets leak
type: issue
state: closed
author: matyx44
labels:
  - question
assignees: []
created_at: 2025-11-21T09:35:44Z
updated_at: 2025-12-03T00:43:02Z
url: https://github.com/astral-sh/uv/issues/16804
synced_at: 2026-01-10T01:26:10Z
---

# uv sync CI secrets leak

---

_Issue opened by @matyx44 on 2025-11-21 09:35_

### Summary

Hope this is not a duplicate, sorry if that is the case. 

**Description**
When running uv sync  in GitLab CI, we hit a case where a source build of a dependency printed the entire os.environ, and that output was passed through by uv into the CI logs. This resulted in multiple sensitive environment variables (DB passwords, test user credentials, CI/infra/repository tokens, etc.) being exposed in the GitLab job log.

In our case the dependency was pymupdf==1.26.6 on CPython 3.13.9 / Alpine (no wheel available), whose setup.py prints detailed environment information including os.environ. Because uv streams the build output directly, all secrets in the environment were logged.

**Environment**
uv image: astral/uv:0.9-python3.13-alpine
Python: CPython 3.13.9
OS: Alpine Linux
CI: GitLab CI
Dependency: pymupdf==1.26.6 (no prebuilt wheel for this platform/runtime)


**Steps to Reproduce**
Run GitLab CI job using astral/uv:0.9-python3.13-alpine.
Configure environment with sensitive variables (DB password, tokens, etc.).
Add pymupdf==1.26.6 (or any package whose setup.py prints os.environ) to pyproject.toml.
Run uv sync.
Inspect CI job log.


**Actual Behavior**
During the source build of pymupdf, its setup.py prints the full os.environ.
uv forwards this output to stdout.
CI logs now contain all environment variables, including secrets.
The build then fails with Failed to build pymupdf==1.26.6, but the secrets are already exposed.


**Expected Behavior**
Can uv prevent this somehow? Or can we prevent this ourselves?
The --quiet / -q flag does not hide this.



Screenshot from log:

<img width="767" height="349" alt="Image" src="https://github.com/user-attachments/assets/62cecef7-eb5e-4ecd-b327-08ac12addcd2" />

### Platform

astral/uv:0.9-python3.13-alpine

### Version

0.9

### Python version

3.13

---

_Label `bug` added by @matyx44 on 2025-11-21 09:35_

---

_Comment by @konstin on 2025-11-21 09:54_

There's not really much uv can do here, iirc all build frontends show build backend output on failure which is needed for debugging. Instead, secrets should be redacted from logging by the CI provider, e.g. with https://docs.gitlab.com/ci/variables/#mask-a-cicd-variable. GitHub Actions has a Secrets feature, where all secrets are added through a dedicated interface and automatically redacted, but I couldn't find the same mechanism in GitLab CI during a quick search.

---

_Label `bug` removed by @konstin on 2025-11-21 09:54_

---

_Label `question` added by @konstin on 2025-11-21 09:54_

---

_Comment by @zanieb on 2025-11-21 16:43_

I'd also report this to that package, they shouldn't dump all of your environment.

---

_Comment by @zanieb on 2025-11-21 16:43_

I wouldn't be opposed to a `UV_HIDE_BUILD_LOGS` option, I guess?

---

_Referenced in [pymupdf/PyMuPDF#4801](../../pymupdf/PyMuPDF/issues/4801.md) on 2025-11-24 09:14_

---

_Comment by @matyx44 on 2025-11-25 08:08_

@zanieb 
Something like that would be great, I reported it to the pymupdf package also, as you suggested.
I am not sure how to mask secrets loaded into env variables from external secret manager such as Google Cloud, I don't think that is possible in gitlab sadly.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-11-28 17:01_

---

_Unassigned @charliermarsh by @charliermarsh on 2025-11-28 17:25_

---

_Referenced in [astral-sh/uv#16885](../../astral-sh/uv/pulls/16885.md) on 2025-11-28 18:35_

---

_Closed by @charliermarsh on 2025-12-03 00:43_

---
