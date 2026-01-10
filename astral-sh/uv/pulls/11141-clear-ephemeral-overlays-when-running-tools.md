```yaml
number: 11141
title: Clear ephemeral overlays when running tools
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/overlay
created_at: 2025-01-31T19:15:13Z
updated_at: 2025-02-04T22:45:46Z
url: https://github.com/astral-sh/uv/pull/11141
synced_at: 2026-01-10T11:10:34Z
```

# Clear ephemeral overlays when running tools

---

_Pull request opened by @charliermarsh on 2025-01-31 19:15_

## Summary

This PR removes the ephemeral `.pth` overlay when using a cached environment. This solution isn't _completely_ safe, since we could remove the `.pth` file just as another process is starting the environment... But that risk already exists today, since we could _overwrite_ the `.pth` file just as another process is starting the environment, so I think what I've added here is a strict improvement.

Ideally, we wouldn't write this file at all, and we'd instead somehow (e.g.) pass a file to the interpreter to run at startup? Or find some other solution that doesn't require poisoning the cache like this.

Closes https://github.com/astral-sh/uv/issues/11117.

# Test Plan

Ran through the great reproduction steps from the linked issue.

Before:

![Screenshot 2025-01-31 at 2 11 31 PM](https://github.com/user-attachments/assets/d36e1db5-27b1-483a-9ced-bec67bd7081d)

After:

![Screenshot 2025-01-31 at 2 11 39 PM](https://github.com/user-attachments/assets/1f963ce0-7903-4acd-9fd6-753374c31705)


---

_Review requested from @zanieb by @charliermarsh on 2025-01-31 19:20_

---

_Review requested from @konstin by @charliermarsh on 2025-01-31 19:20_

---

_Label `bug` added by @charliermarsh on 2025-01-31 19:20_

---

_@zanieb approved on 2025-02-03 19:58_

---

_@konstin approved on 2025-02-04 11:09_

lgtm

---

_Merged by @charliermarsh on 2025-02-04 22:45_

---

_Closed by @charliermarsh on 2025-02-04 22:45_

---

_Branch deleted on 2025-02-04 22:45_

---
