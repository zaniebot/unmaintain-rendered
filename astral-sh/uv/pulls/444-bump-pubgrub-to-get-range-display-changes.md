```yaml
number: 444
title: Bump pubgrub to get range display changes
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zanie/pubrub-5
created_at: 2023-11-17T16:12:21Z
updated_at: 2023-11-20T15:12:49Z
url: https://github.com/astral-sh/uv/pull/444
synced_at: 2026-01-12T16:03:57Z
```

# Bump pubgrub to get range display changes

---

_@zanieb_

See https://github.com/zanieb/pubgrub/pull/5


---

_Comment by @zanieb on 2023-11-17 16:24_

This should show `|` in the long version ranges but I guess we don't have any in our test snapshots so there's no diff there.

---

_Comment by @zanieb on 2023-11-17 17:14_

e.g. from #423 

`... And because dependencies of transformers[tensorboard] at version ==4.34.0 are unavailable and dependencies of transformers[tensorboard] at version ==4.34.1 are unavailable, transformers[tensorboard]<4.35.0 | >4.35.0, <4.35.1 | >4.35.1, <4.35.2 | >4.35.2 is forbidden., ...`

---

_Review requested from @charliermarsh by @zanieb on 2023-11-17 17:15_

---

_Comment by @charliermarsh on 2023-11-17 17:26_

Is this an improvement as long as there are still commas between the version pairs?

---

_Comment by @zanieb on 2023-11-17 17:55_

@charliermarsh yes instead of using `,` everywhere this uses `,` for _and_ and `|` for _or_.

---

_@charliermarsh approved on 2023-11-20 11:36_

---

_Merged by @zanieb on 2023-11-20 15:12_

---

_Closed by @zanieb on 2023-11-20 15:12_

---

_Branch deleted on 2023-11-20 15:12_

---
