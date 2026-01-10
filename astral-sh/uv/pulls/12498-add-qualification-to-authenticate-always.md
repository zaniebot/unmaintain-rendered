```yaml
number: 12498
title: Add qualification to authenticate always documentation
type: pull_request
state: merged
author: jtfmumm
labels:
  - documentation
assignees: []
merged: true
base: main
head: jtfm/auth-default-doc
created_at: 2025-03-26T21:21:15Z
updated_at: 2025-04-02T17:31:21Z
url: https://github.com/astral-sh/uv/pull/12498
synced_at: 2026-01-10T11:10:40Z
```

# Add qualification to authenticate always documentation

---

_Pull request opened by @jtfmumm on 2025-03-26 21:21_

It might not be obvious to some users that authenticate always will not prevent uv from consulting other indexes.


---

_Label `documentation` added by @jtfmumm on 2025-03-26 21:21_

---

_@zanieb reviewed on 2025-03-26 21:23_

---

_Review comment by @zanieb on `docs/configuration/indexes.md`:211 on 2025-03-26 21:23_

Does it always? Or is this only with the `unsafe-best-match` index strategy? Do we fallback to other indexes on failed authentication with the default strategy?

---

_@jtfmumm reviewed on 2025-03-26 21:24_

---

_Review comment by @jtfmumm on `docs/configuration/indexes.md`:211 on 2025-03-26 21:24_

It does in my experiments. But I can explore further before merging this

---

_@jtfmumm reviewed on 2025-03-28 15:44_

---

_Review comment by @jtfmumm on `docs/configuration/indexes.md`:211 on 2025-03-28 15:44_

In the registry client, [we call](https://github.com/astral-sh/uv/blob/ab3bab14217d75c06c5d86e2ef1ba65c48091b23/crates/uv-client/src/registry_client.rs#L340) `simple_single_index` with the first index strategy. And in that method [we treat both 401s and 403s](https://github.com/astral-sh/uv/blob/ab3bab14217d75c06c5d86e2ef1ba65c48091b23/crates/uv-client/src/registry_client.rs#L500) as equivalent to "package not found in index" (which is what returning `Ok(None)` signals there).


---

_@zanieb approved on 2025-04-02 14:25_

---

_Merged by @jtfmumm on 2025-04-02 17:31_

---

_Closed by @jtfmumm on 2025-04-02 17:31_

---

_Branch deleted on 2025-04-02 17:31_

---
