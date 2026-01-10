---
number: 5409
title: Document best practices for maintaining the cache
type: issue
state: closed
author: zanieb
labels:
  - documentation
assignees: []
created_at: 2024-07-24T14:30:38Z
updated_at: 2024-08-20T20:10:17Z
url: https://github.com/astral-sh/uv/issues/5409
synced_at: 2026-01-10T01:23:48Z
---

# Document best practices for maintaining the cache

---

_Issue opened by @zanieb on 2024-07-24 14:30_

e.g. as prompted in https://github.com/astral-sh/uv/issues/5222#issuecomment-2247510996

How to remove old entries? (`uv cache prune`?)
How to remove specific packages? (`uv cache clean <name>`)
How to optimize size in CI?



---

_Referenced in [astral-sh/uv#5222](../../astral-sh/uv/issues/5222.md) on 2024-07-24 14:31_

---

_Label `documentation` added by @zanieb on 2024-07-24 14:31_

---

_Comment by @zanieb on 2024-07-24 14:32_

See also:

- https://github.com/hynek/setup-cached-uv/issues/7
- https://github.com/actions/setup-python/issues/822

---

_Comment by @zanieb on 2024-07-24 19:31_

See also:
- https://github.com/astral-sh/uv/pull/5391#p

---

_Comment by @MarcusJellinghaus on 2024-07-29 08:03_

Maybe you also want to add information related to
- running several CI pipelines in parallel (eg several GitHub runners on one Windows machine) - should I have one cache for all or several caches?
- incorrect treatment of new packages - maybe also add this [comment](https://github.com/astral-sh/uv/issues/5351#issuecomment-2252911773)
- best practices for running CI pipelines on Windows servers ( i.e. where the CI server might stay the same for years, while kubernetes-based pods might get recreated from template regularly).

---

_Referenced in [astral-sh/uv#5663](../../astral-sh/uv/pulls/5663.md) on 2024-07-31 16:34_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-20 20:10_

---

_Closed by @charliermarsh on 2024-08-20 20:10_

---
