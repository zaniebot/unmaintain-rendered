```yaml
number: 4622
title: "Add retries to `system-test-opensuse` setup which flakes during database updates"
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/opensuse-retries
created_at: 2024-06-28T15:18:19Z
updated_at: 2024-06-28T20:24:45Z
url: https://github.com/astral-sh/uv/pull/4622
synced_at: 2026-01-12T16:06:21Z
```

# Add retries to `system-test-opensuse` setup which flakes during database updates

---

_@zanieb_

e.g. failure at https://github.com/astral-sh/uv/actions/runs/9714929523/job/26815318110

Seems to be some sort of issue where they're updating their package repository / database causing a spurious failure. I've seen this fail a lot lately.

---

_Label `internal` added by @zanieb on 2024-06-28 15:18_

---

_Label `testing` added by @zanieb on 2024-06-28 15:18_

---

_Comment by @zanieb on 2024-06-28 15:29_

Looks like my retry had to do it's thing here?? (and it worked!) https://github.com/astral-sh/uv/actions/runs/9715171143/job/26816007480?pr=4622

---

_Review requested from @charliermarsh by @zanieb on 2024-06-28 15:29_

---

_Review requested from @konstin by @zanieb on 2024-06-28 15:29_

---

_Review requested from @BurntSushi by @zanieb on 2024-06-28 15:29_

---

_Comment by @zanieb on 2024-06-28 15:29_

Doesn't need three reviewers, if someone else reviewed it just move on :)

---

_Marked ready for review by @zanieb on 2024-06-28 15:29_

---

_@charliermarsh approved on 2024-06-28 18:46_

---

_Merged by @zanieb on 2024-06-28 20:24_

---

_Closed by @zanieb on 2024-06-28 20:24_

---

_Branch deleted on 2024-06-28 20:24_

---
