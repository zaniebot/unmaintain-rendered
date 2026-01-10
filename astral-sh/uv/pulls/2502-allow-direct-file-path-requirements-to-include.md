```yaml
number: 2502
title: Allow direct file path requirements to include fragments
type: pull_request
state: merged
author: eth3lbert
labels:
  - bug
assignees: []
merged: true
base: main
head: path-fragment
created_at: 2024-03-18T01:30:07Z
updated_at: 2024-03-18T19:47:11Z
url: https://github.com/astral-sh/uv/pull/2502
synced_at: 2026-01-10T14:49:08Z
```

# Allow direct file path requirements to include fragments

---

_Pull request opened by @eth3lbert on 2024-03-18 01:30_

This PR handles the fragment part of the URL path.
It achieves this by splitting the fragment from the path before normalization and parsing. It then sets the fragment back after the URL has been parsed.

Resolve #2501 

---

_Label `bug` added by @charliermarsh on 2024-03-18 01:43_

---

_Comment by @charliermarsh on 2024-03-18 01:44_

Nice, thank you. Looks like there's one test failure on Windows.

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-18 03:15_

---

_Comment by @charliermarsh on 2024-03-18 03:15_

Will review and merge tomorrow, thanks!

---

_Assigned to @charliermarsh by @zanieb on 2024-03-18 14:45_

---

_@charliermarsh approved on 2024-03-18 16:35_

---

_Renamed from "Allow path with fragment" to "Allow direct file path requirements to include fragments" by @charliermarsh on 2024-03-18 16:35_

---

_Merged by @charliermarsh on 2024-03-18 17:06_

---

_Closed by @charliermarsh on 2024-03-18 17:06_

---

_Branch deleted on 2024-03-18 19:47_

---
