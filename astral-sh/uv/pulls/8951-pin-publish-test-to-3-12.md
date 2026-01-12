```yaml
number: 8951
title: Pin publish test to 3.12
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/publish-test-timeout
created_at: 2024-11-08T17:22:20Z
updated_at: 2024-11-10T14:43:49Z
url: https://github.com/astral-sh/uv/pull/8951
synced_at: 2026-01-12T16:08:34Z
```

# Pin publish test to 3.12

---

_@konstin_

The bump to 3.13 broke the test

---

_Label `internal` added by @konstin on 2024-11-08 17:22_

---

_Comment by @ranjithrajv on 2024-11-08 17:56_

How about introducing a command or flag to uv publish that allows setting a custom timeout value? 
This would offer greater flexibility by letting users specify the timeout according to their specific requirements instead of relying on a default fixed value.

---

_Comment by @konstin on 2024-11-08 17:57_

This is applicable to the test script only

---

_Comment by @zanieb on 2024-11-08 18:14_

Oof 20 minutes is really long for the test to run. Why is it taking so long?

---

_Comment by @zanieb on 2024-11-08 19:32_

And it still failed here — I think there needs to be a different solution.

---

_Renamed from "Increase publish test timeout to 20min" to "Pin publish test to 3.12" by @konstin on 2024-11-10 11:31_

---

_Merged by @konstin on 2024-11-10 14:43_

---

_Closed by @konstin on 2024-11-10 14:43_

---

_Branch deleted on 2024-11-10 14:43_

---
