---
number: 10556
title: A possible security issue
type: issue
state: closed
author: mpagni12
labels: []
assignees: []
created_at: 2025-01-13T07:39:31Z
updated_at: 2025-01-14T08:45:13Z
url: https://github.com/astral-sh/uv/issues/10556
synced_at: 2026-01-10T01:24:55Z
---

# A possible security issue

---

_Issue opened by @mpagni12 on 2025-01-13 07:39_

I saved my PyPI API token in a local file located in the git root directory (possibly not a great idea), which is not tracked by Git, but located in the git root directory.

`uv publish` published this untracked file and I can be retrieve from https://github.com/pypi-data/... This issue has been detected automatically by a github-action bot and my key was revoked. This is nevertheless annoying with respect to security. 

Thank you for making `uv`, which is a great tool

  Marco



---

_Comment by @nbaju1 on 2025-01-13 07:49_

As far as I know, `uv publish` does not interact with git related stuff directly, so don't see why the file not being tracked by git should matter. Don't blame uv for your insecure handling of your credentials.

---

_Comment by @mpagni12 on 2025-01-13 08:11_

Sorry for blaming`uv`, this was clearly my fault . 

I was naively assuming that `uv publish` was relying on the content of `pyproject.toml` exclusively.

---

_Closed by @mpagni12 on 2025-01-13 08:29_

---

_Comment by @zanieb on 2025-01-13 18:56_

What build backend were you using? It's a build backend responsibility to include and exclude untracked files.

---

_Comment by @mpagni12 on 2025-01-14 08:45_

hatchling.build


---
