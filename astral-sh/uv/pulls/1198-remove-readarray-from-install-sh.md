```yaml
number: 1198
title: "Remove readarray from `install.sh`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/readarray
created_at: 2024-01-31T00:21:52Z
updated_at: 2024-01-31T15:22:15Z
url: https://github.com/astral-sh/uv/pull/1198
synced_at: 2026-01-10T15:39:03Z
```

# Remove readarray from `install.sh`

---

_Pull request opened by @charliermarsh on 2024-01-31 00:21_

## Summary

This isn't available on macOS (see, e.g., https://stackoverflow.com/questions/23842261/alternative-to-readarray-because-it-does-not-work-on-mac-os-x), but this version works both on macOS and Linux.

Closes https://github.com/astral-sh/puffin/issues/1196. (Verified locally and on CI.)

---

_Review requested from @zanieb by @charliermarsh on 2024-01-31 00:21_

---

_Marked ready for review by @charliermarsh on 2024-01-31 00:21_

---

_Label `bug` added by @charliermarsh on 2024-01-31 00:21_

---

_Renamed from "Remove readarray from install.sh" to "Remove readarray from `install.sh`" by @charliermarsh on 2024-01-31 00:22_

---

_Comment by @zanieb on 2024-01-31 14:44_

Interesting.. I mean it is available on my macOS machine it just requires a newer version of `bash`? That stackoverflow issue is from 2014. This is fine though. I find IFS hard to read but it's relatively common.

---

_@zanieb approved on 2024-01-31 14:44_

---

_Merged by @charliermarsh on 2024-01-31 15:22_

---

_Closed by @charliermarsh on 2024-01-31 15:22_

---

_Branch deleted on 2024-01-31 15:22_

---
