```yaml
number: 16210
title: Ignore pre-release Python versions when a patch version is requested
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: zb/python-pre
head: zb/pre-2
created_at: 2025-10-09T16:17:19Z
updated_at: 2025-10-09T16:39:29Z
url: https://github.com/astral-sh/uv/pull/16210
synced_at: 2026-01-10T06:36:15Z
```

# Ignore pre-release Python versions when a patch version is requested

---

_Pull request opened by @zanieb on 2025-10-09 16:17_

I think this is ostensively breaking, though I think the impact would be small given this will go out in 0.9.1 and should only affect people using pre-release Python versions.

When `3.14.0` is requested (opposed to `3.14`), we treat this as a request for a final / stable version and ignore pre-releases.

I think this is a fairly clean way to allow users to explicitly request the stable version.

Closes https://github.com/astral-sh/uv/issues/16175
Follows #16208 

---

_Label `bug` added by @zanieb on 2025-10-09 16:17_

---

_Marked ready for review by @zanieb on 2025-10-09 16:19_

---

_@charliermarsh approved on 2025-10-09 16:24_

---

_Merged by @zanieb on 2025-10-09 16:39_

---

_Closed by @zanieb on 2025-10-09 16:39_

---

_Branch deleted on 2025-10-09 16:39_

---
