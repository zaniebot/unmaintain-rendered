```yaml
number: 3208
title: Bump version to 0.1.37
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: release/0137
created_at: 2024-04-23T14:25:15Z
updated_at: 2024-04-23T14:38:34Z
url: https://github.com/astral-sh/uv/pull/3208
synced_at: 2026-01-12T16:05:29Z
```

# Bump version to 0.1.37

---

_@zanieb_

_No description provided._

---

_Label `release` added by @zanieb on 2024-04-23 14:25_

---

_@charliermarsh approved on 2024-04-23 14:26_

---

_Comment by @charliermarsh on 2024-04-23 14:27_

Thanks Zanie

---

_Review comment by @konstin on `CHANGELOG.md`:7 on 2024-04-23 14:29_

This actually changed with the original http timeout -> read timeout PR, but it's good to make this clear here.

```suggestion
- Change default HTTP read timeout to 30s. Note that uv does not have a timeout for the entire HTTP request, but only between two reads in a request ([#3182](https://github.com/astral-sh/uv/pull/3182))
```

---

_@konstin approved on 2024-04-23 14:30_

---

_Merged by @zanieb on 2024-04-23 14:35_

---

_Closed by @zanieb on 2024-04-23 14:35_

---

_Branch deleted on 2024-04-23 14:35_

---

_@zanieb reviewed on 2024-04-23 14:38_

---

_Review comment by @zanieb on `CHANGELOG.md`:7 on 2024-04-23 14:38_

Sorry didn't see this before the auto-merge. I'll make this note in the GitHub release.

---
