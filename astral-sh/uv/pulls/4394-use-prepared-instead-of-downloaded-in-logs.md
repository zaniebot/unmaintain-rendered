```yaml
number: 4394
title: "Use \"Prepared\" instead of \"Downloaded\" in logs"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/download-prepare
created_at: 2024-06-18T18:15:18Z
updated_at: 2024-06-18T18:38:19Z
url: https://github.com/astral-sh/uv/pull/4394
synced_at: 2026-01-12T16:06:12Z
```

# Use "Prepared" instead of "Downloaded" in logs

---

_@zanieb_

We download, build, and unzip packages in this stage. The current name is very misleading.

Closes https://github.com/astral-sh/uv/issues/4011

---

_Comment by @ibraheemdev on 2024-06-18 18:17_

Should the "Downloading .." spinner text be changed too?

---

_Comment by @zanieb on 2024-06-18 18:20_

Yep, did I miss one -.-

---

_@ibraheemdev approved on 2024-06-18 18:30_

---

_Merged by @zanieb on 2024-06-18 18:38_

---

_Closed by @zanieb on 2024-06-18 18:38_

---

_Branch deleted on 2024-06-18 18:38_

---
