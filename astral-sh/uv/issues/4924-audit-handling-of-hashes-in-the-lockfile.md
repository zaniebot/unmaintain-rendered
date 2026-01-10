---
number: 4924
title: Audit handling of hashes in the lockfile
type: issue
state: closed
author: konstin
labels:
  - preview
assignees: []
created_at: 2024-07-09T11:26:31Z
updated_at: 2024-07-17T23:02:55Z
url: https://github.com/astral-sh/uv/issues/4924
synced_at: 2026-01-10T01:23:43Z
---

# Audit handling of hashes in the lockfile

---

_Issue opened by @konstin on 2024-07-09 11:26_

Audit our handling of hashes. Some hashes are generated. Some are retrieved from registries. Sometimes hashes should not be used (path dependencies) where as sometimes they should (registry dependencies).

I think there are three main issues here:

First is that hashes come from two places: one place is the registry itself. Another place is by hashes we compute ourselves. Which hashes do we use in the lock file? Does it matter? Which _should_ we use?

Second is that we often have multiple hashes available for any given artifact. Do we need to store all of them in the lock file? Or can we just pick the "best" one?

Third is whether we are doing any hash checking. I don't _think_ we are today. But we probably should be.

---

_Referenced in [astral-sh/uv#3611](../../astral-sh/uv/issues/3611.md) on 2024-07-09 11:26_

---

_Renamed from "Audit our handling of hashes. Some hashes are generated. Some are retrieved from registries. Sometimes hashes should not be used (path dependencies) where as sometimes they should (registry dependencies)." to "Audit handling of hashes in the lockfile" by @konstin on 2024-07-09 11:26_

---

_Label `preview` added by @konstin on 2024-07-09 11:26_

---

_Referenced in [astral-sh/uv#3347](../../astral-sh/uv/issues/3347.md) on 2024-07-09 11:29_

---

_Referenced in [astral-sh/uv#5120](../../astral-sh/uv/issues/5120.md) on 2024-07-16 18:21_

---

_Comment by @charliermarsh on 2024-07-16 18:23_

I can answer all of these questions (regarding what we do today) if helpful.

---

_Comment by @konstin on 2024-07-16 19:14_

Do you expect that we have to change the lockfile (schema) for this?

---

_Comment by @charliermarsh on 2024-07-16 19:17_

I think we need to make hashes optional for registry-based distributions. I can't remember if it's optional today.

---

_Comment by @BurntSushi on 2024-07-17 01:25_

If we make them optional in the lock file, then we won't be able to do hash checking. Are we okay with that?

---

_Comment by @charliermarsh on 2024-07-17 01:28_

We can still hash-check the distributions for which we _do_ have hashes.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 14:33_

---

_Comment by @charliermarsh on 2024-07-17 16:57_

> First is that hashes come from two places: one place is the registry itself. Another place is by hashes we compute ourselves. Which hashes do we use in the lock file? Does it matter? Which should we use?

I think we should follow the strategy we already use today: if from the registry, we use the registry-provided hashes; if from a direct URL, we compute the hash ourselves. If the registry lacks hashes, we don't include any hashes (and warn the user).

> Second is that we often have multiple hashes available for any given artifact. Do we need to store all of them in the lock file? Or can we just pick the "best" one?

I think we should just store the "best" one to save space. I'll verify that we're doing this today.

> Third is whether we are doing any hash checking. I don't think we are today. But we probably should be.

We aren't. I think we should add "verify any hashes that exist, but don't require them" (so that we can still interoperate with registries that lack hashes).


---

_Referenced in [astral-sh/uv#5166](../../astral-sh/uv/pulls/5166.md) on 2024-07-17 20:09_

---

_Referenced in [astral-sh/uv#5167](../../astral-sh/uv/pulls/5167.md) on 2024-07-17 20:12_

---

_Closed by @charliermarsh on 2024-07-17 23:02_

---
