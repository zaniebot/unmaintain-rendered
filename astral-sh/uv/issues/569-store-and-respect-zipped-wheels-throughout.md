---
number: 569
title: Store and respect zipped wheels throughout caching pipeline
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2023-12-05T18:17:12Z
updated_at: 2024-02-02T20:18:54Z
url: https://github.com/astral-sh/uv/issues/569
synced_at: 2026-01-10T01:23:04Z
---

# Store and respect zipped wheels throughout caching pipeline

---

_Issue opened by @charliermarsh on 2023-12-05 18:17_

If we download a bunch of wheels, then fail while unzipping them, we should be left with some subset of zipped wheels in the cache. If we then re-run the command, we _should_ just unzip those zipped wheels rather than re-downloading them.

After #568, this will require re-adding zipped wheels under their own cache shard, then modifying our `InstallPlan` and downstream consumers to respect that there could be zipped or unzipped wheels in the cache.


---

_Label `enhancement` added by @charliermarsh on 2023-12-05 18:17_

---

_Added to milestone `Initial release` by @charliermarsh on 2023-12-05 18:17_

---

_Referenced in [astral-sh/uv#571](../../astral-sh/uv/issues/571.md) on 2023-12-05 18:20_

---

_Renamed from "Store and respect built wheels throughout caching pipeline" to "Store and respect zipped wheels throughout caching pipeline" by @charliermarsh on 2023-12-13 15:11_

---

_Comment by @konstin on 2024-02-02 20:00_

Can this be closed?

---

_Closed by @charliermarsh on 2024-02-02 20:18_

---
