```yaml
number: 13851
title: Sync latest Python releases
type: pull_request
state: merged
author: github-actions
labels: []
assignees: []
merged: true
base: main
head: sync-python-releases
created_at: 2025-06-05T00:27:23Z
updated_at: 2025-06-05T19:23:42Z
url: https://github.com/astral-sh/uv/pull/13851
synced_at: 2026-01-10T11:10:42Z
```

# Sync latest Python releases

---

_Pull request opened by @github-actions on 2025-06-05 00:27_

Automated update for Python releases.

---

_Comment by @zanieb on 2025-06-05 02:41_

@samypr100 is it clear to you why this wasn't included in the sync that happened earlier today?

---

_Comment by @samypr100 on 2025-06-05 03:14_

> @samypr100 is it clear to you why this wasn't included in the sync that happened earlier today?

Was it a pre-release at the time? If so, GH/latest API doesn't consider it latest when resolving.

---

_Comment by @zanieb on 2025-06-05 13:02_

Ah yes it was. I often run the sync workflow here first a smoke test of the pre-release :) that's good to know though, thanks!

---

_Closed by @zanieb on 2025-06-05 13:02_

---

_Reopened by @zanieb on 2025-06-05 13:02_

---

_Merged by @zanieb on 2025-06-05 16:21_

---

_Closed by @zanieb on 2025-06-05 16:21_

---

_Branch deleted on 2025-06-05 16:21_

---

_Comment by @samypr100 on 2025-06-05 19:23_

> Ah yes it was. I often run the sync workflow here first a smoke test of the pre-release :) that's good to know though, thanks!

Thanks, should I try to find a solution that works with pre-released too?

---
