```yaml
number: 4472
title: "Reapply \"Bump version to 0.2.14\""
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/0214-2
created_at: 2024-06-24T13:50:05Z
updated_at: 2024-06-24T14:14:17Z
url: https://github.com/astral-sh/uv/pull/4472
synced_at: 2026-01-10T13:48:28Z
```

# Reapply "Bump version to 0.2.14"

---

_Pull request opened by @zanieb on 2024-06-24 13:50_

Restores #4431

This reverts commit 9ff6a5ed7429f6fb20114733eefee4ffb88a071a (#4436)


---

_Label `release` added by @zanieb on 2024-06-24 13:50_

---

_Renamed from "Reapply "Bump version to 0.2.14 (#4431)" (#4436)" to "Reapply "Bump version to 0.2.14"" by @zanieb on 2024-06-24 13:50_

---

_Comment by @zanieb on 2024-06-24 13:51_

I think, actually, that we cannot release this easily because the tag will be wrong? Or perhaps it will be applied correctly by cargo-xdist? It seems likely that they store the build commit and apply the tag to that.

---

_Comment by @charliermarsh on 2024-06-24 13:53_

Let's just do a fresh release IMO.

---

_Comment by @zanieb on 2024-06-24 13:58_

I'm reaching out to the Axo team, it's good for my understanding of what happens in these situations.

A fresh release is probably okay too.

---

_Comment by @zanieb on 2024-06-24 14:14_

The release went out fine, going to merge this to restore the changelog.

However, cargo-dist tagged the wrong commit.

---

_Merged by @zanieb on 2024-06-24 14:14_

---

_Closed by @zanieb on 2024-06-24 14:14_

---

_Branch deleted on 2024-06-24 14:14_

---
