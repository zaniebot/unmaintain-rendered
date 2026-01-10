```yaml
number: 46
title: Include the raw tag in Docker publish labels
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/docker-raw
created_at: 2025-05-05T23:42:41Z
updated_at: 2025-07-08T10:38:35Z
url: https://github.com/astral-sh/ty/pull/46
synced_at: 2026-01-10T02:34:10Z
```

# Include the raw tag in Docker publish labels

---

_Pull request opened by @zanieb on 2025-05-05 23:42_

In https://github.com/astral-sh/ty/actions/runs/14848358920/job/41687152688, we've published a Python PEP 440 tag but we are trying to consume a Rust version tag later. We could transform the Rust tag into a Python tag, but since `ty version` will report the Rust version tag for pre-releases (and so will the Git tag), we should just _also_ publish a tag for matching Rust pre-release styling.

Follows #44 

---

_Merged by @zanieb on 2025-05-05 23:45_

---

_Closed by @zanieb on 2025-05-05 23:45_

---

_Comment by @zanieb on 2025-05-05 23:49_

Finally, success https://github.com/astral-sh/ty/actions/runs/14848531602/job/41687606862

---

_Branch deleted on 2025-07-08 10:38_

---
