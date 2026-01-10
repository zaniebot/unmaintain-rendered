```yaml
number: 4780
title: Improvements to the Python metadata fetch script
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/download-improvements
created_at: 2024-07-03T16:00:10Z
updated_at: 2024-07-03T16:37:01Z
url: https://github.com/astral-sh/uv/pull/4780
synced_at: 2026-01-10T13:48:28Z
```

# Improvements to the Python metadata fetch script

---

_Pull request opened by @zanieb on 2024-07-03 16:00_

This fell out of my investigation of https://github.com/astral-sh/uv/issues/4774 but the bug was fixed by the reporter in #4775

- Adds support for `GH_TOKEN` authentication again — basically needed to avoid rate limits when hacking on this.
- Clarifies some handling and logging of flavors

---

_Label `internal` added by @zanieb on 2024-07-03 16:00_

---

_Comment by @zanieb on 2024-07-03 16:02_

As a security measure, I also confirmed that #4775 did not include any manual changes to URLs in the generated files.

---

_Marked ready for review by @zanieb on 2024-07-03 16:02_

---

_@charliermarsh approved on 2024-07-03 16:31_

---

_Merged by @zanieb on 2024-07-03 16:36_

---

_Closed by @zanieb on 2024-07-03 16:36_

---

_Branch deleted on 2024-07-03 16:37_

---
