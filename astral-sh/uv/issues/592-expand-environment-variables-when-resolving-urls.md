---
number: 592
title: Expand environment variables when resolving URLs
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2023-12-08T16:06:19Z
updated_at: 2023-12-14T15:09:15Z
url: https://github.com/astral-sh/uv/issues/592
synced_at: 2026-01-10T01:23:04Z
---

# Expand environment variables when resolving URLs

---

_Issue opened by @charliermarsh on 2023-12-08 16:06_

Apparently pip supports, e.g., `file://${PWD}/...`. We need to do a bit more research into this.


---

_Label `enhancement` added by @charliermarsh on 2023-12-08 16:06_

---

_Added to milestone `Feature complete` by @charliermarsh on 2023-12-08 16:06_

---

_Comment by @konstin on 2023-12-08 16:28_

Pip expand all environment variables on the string contents of the requirements file before parsing it: https://github.com/pypa/pip/blob/aebc0c5fc321141ede837c80572427ab7b795c3f/src/pip/_internal/req/req_file.py#L156-L165, https://github.com/pypa/pip/blob/aebc0c5fc321141ede837c80572427ab7b795c3f/src/pip/_internal/req/req_file.py#L503

---

_Comment by @charliermarsh on 2023-12-08 16:33_

Wow okay. I guess that sort of makes life easier...

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-08 19:57_

---

_Unassigned @charliermarsh by @charliermarsh on 2023-12-08 22:08_

---

_Removed from milestone `Feature complete` by @charliermarsh on 2023-12-08 22:08_

---

_Added to milestone `Future` by @charliermarsh on 2023-12-08 22:08_

---

_Comment by @charliermarsh on 2023-12-08 22:08_

Moving to future, this seems extremely rare based on GitHub Code Search, I can barely find any usages.

---

_Comment by @mitsuhiko on 2023-12-10 21:21_

It's in some ways the only way to get editable installs for local dependencies going. It's also what PDM recommends: https://pdm-project.org/latest/usage/advanced/#use-pdm-to-manage-a-monorepo

One hilarious challenge here is that this feeds into a URL usually, which actually causes issues very quickly (eg: backslashes vs slashes, spaces, drive letters etc.).

In Rye I hacked around this by special casing `PROJECT_ROOT`. Ideally none of this is needed. https://github.com/mitsuhiko/rye/blob/92b571bfd42e5748d2e535174d78fc7311a889a3/rye/src/lock.rs#L413-L421

---

_Comment by @charliermarsh on 2023-12-10 21:29_

Ah yeah, maybe we'll do this too. I think Hatch does something like that as well.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-10 22:31_

---

_Comment by @charliermarsh on 2023-12-13 15:40_

Doing this today. The only hard part is ensuring that we preserve the environment variable when writing the lockfile. (I'm also going to limit this to URLs for now intentionally.)

---

_Referenced in [astral-sh/uv#640](../../astral-sh/uv/pulls/640.md) on 2023-12-13 21:52_

---

_Closed by @charliermarsh on 2023-12-14 15:09_

---

_Referenced in [pdm-project/pdm#2641](../../pdm-project/pdm/issues/2641.md) on 2024-02-21 23:48_

---

_Referenced in [pypa/pip#12597](../../pypa/pip/issues/12597.md) on 2024-03-25 20:45_

---
