```yaml
number: 11510
title: Split changelog into separate files
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/cl-split
created_at: 2025-02-14T15:16:57Z
updated_at: 2025-02-14T19:20:08Z
url: https://github.com/astral-sh/uv/pull/11510
synced_at: 2026-01-10T11:10:38Z
```

# Split changelog into separate files

---

_Pull request opened by @zanieb on 2025-02-14 15:16_

GitHub consistently crashes while rendering our existing `CHANGELOG.md` — we got a lot of changes!

Here I add a new directory and split the changelog on our "breaking" versions.

See https://github.com/astral-sh/uv/tree/zb/cl-split/changelogs and https://github.com/astral-sh/uv/blob/zb/cl-split/CHANGELOG.md#05x

The downside is that we'll break some links. I don't think there's much to be done about it though.

---

_Label `documentation` added by @zanieb on 2025-02-14 15:16_

---

_Marked ready for review by @zanieb on 2025-02-14 15:23_

---

_@charliermarsh approved on 2025-02-14 19:14_

This seems somewhat inconvenient since you can no long search a single file to find the relevant entry? But I defer to you, I see why it's necessary.

---

_Comment by @zanieb on 2025-02-14 19:20_

Agree it's a bit of a bummer, but I don't see an alternative for now. The rendering breaks >75% of the time for me.

---

_Merged by @zanieb on 2025-02-14 19:20_

---

_Closed by @zanieb on 2025-02-14 19:20_

---

_Branch deleted on 2025-02-14 19:20_

---
