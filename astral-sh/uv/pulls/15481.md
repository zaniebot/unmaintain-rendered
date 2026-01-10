```yaml
number: 15481
title: Cache PyTorch wheels by default
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/ptc
created_at: 2025-08-24T13:02:33Z
updated_at: 2025-08-24T15:41:18Z
url: https://github.com/astral-sh/uv/pull/15481
synced_at: 2026-01-10T06:44:33Z
```

# Cache PyTorch wheels by default

---

_Pull request opened by @charliermarsh on 2025-08-24 13:02_

## Summary

After chatting with the PyTorch team, it looks like some number of wheels were accidentally uploaded with `no-cache,no-store,must-revalidate` due to https://github.com/pytorch/pytorch/pull/149218. They're going to correct this for the respective wheels. I've encouraged them to set an immutable caching header for these files, and it might happen. But even if this isn't set, by default we only allow these wheels to be cached for 600s, since the other wheels don't include a `Cache-Control` header at all (but do include a `Last-Modified`, so we cache based on our heuristic: `Freshness lifetime heuristically assumed because of presence of last-modified header: 600s`). This probably leads to tons of unnecessary downloads for users over time. Andrey from the PyTorch team agreed that we should do this.

Closes https://github.com/astral-sh/uv/issues/15480.


---

_Review requested from @zanieb by @charliermarsh on 2025-08-24 13:02_

---

_Review requested from @konstin by @charliermarsh on 2025-08-24 13:02_

---

_Label `enhancement` added by @charliermarsh on 2025-08-24 13:02_

---

_Marked ready for review by @charliermarsh on 2025-08-24 13:02_

---

_@zanieb approved on 2025-08-24 15:11_

---

_Merged by @charliermarsh on 2025-08-24 15:41_

---

_Closed by @charliermarsh on 2025-08-24 15:41_

---

_Branch deleted on 2025-08-24 15:41_

---
