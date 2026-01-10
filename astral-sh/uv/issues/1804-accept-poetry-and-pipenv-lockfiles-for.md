---
number: 1804
title: Accept poetry and pipenv lockfiles for installation
type: issue
state: closed
author: abondrn
labels:
  - enhancement
  - wontfix
assignees: []
created_at: 2024-02-21T12:42:47Z
updated_at: 2024-08-23T15:28:06Z
url: https://github.com/astral-sh/uv/issues/1804
synced_at: 2026-01-10T01:23:09Z
---

# Accept poetry and pipenv lockfiles for installation

---

_Issue opened by @abondrn on 2024-02-21 12:42_

When `uv pip install` is passed `poetry.lock` or `Pipfile.lock` (and any optional dependency groups, or extras) instead of `requirements.txt`, it could attempt to install these in the uv environment. I can only speak for myself when I say this will speed up adoption of uv in some contexts we are using it, such as executing local tests or Docker pipelines.

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Label `enhancement` added by @AlexWaygood on 2024-02-21 13:04_

---

_Comment by @bschoenmaeckers on 2024-03-12 09:24_

Should we consider re-implementing the [poetry-export-plugin](https://github.com/python-poetry/poetry-plugin-export) or calling it as a subprocess?

---

_Comment by @zanieb on 2024-03-12 14:50_

Similar to our policy on reading [pip's configuration](https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#configuration-files-and-environment-variables), I don't think we can do this. I think the bullets there are a good summary of why. 

---

_Label `wontfix` added by @zanieb on 2024-03-12 14:51_

---

_Closed by @zanieb on 2024-04-03 04:29_

---

_Comment by @michaelmior on 2024-08-23 13:52_

It might not be perfect, but it would be really helpful to have at least an attempt to import from pipenv configuration. You can get close with `uv add -r <(pipenv requirements)` but then all package versions are pinned because `pipenv requirements` is based on the lock file. The other piece would be reading from the Pipfile to specify the dependencies.

---

_Comment by @zanieb on 2024-08-23 15:27_

Related

- #5200 
- #6520 
- #6005 

---
