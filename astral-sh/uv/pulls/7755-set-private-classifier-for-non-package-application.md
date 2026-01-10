```yaml
number: 7755
title: Set private classifier for non-package application
type: pull_request
state: closed
author: j178
labels: []
assignees: []
base: main
head: private-classifier
created_at: 2024-09-28T10:07:44Z
updated_at: 2024-10-13T09:13:56Z
url: https://github.com/astral-sh/uv/pull/7755
synced_at: 2026-01-10T12:53:54Z
```

# Set private classifier for non-package application

---

_Pull request opened by @j178 on 2024-09-28 10:07_

## Summary

For non-package applications (mostly private projects), set classifier to `Private :: Do Not Upload` to prevent it from being uploaded to PyPI by accident.



---

_Review requested from @zanieb by @charliermarsh on 2024-09-28 12:36_

---

_Assigned to @zanieb by @charliermarsh on 2024-09-28 12:37_

---

_Comment by @zanieb on 2024-10-01 18:33_

I'm a little worried about this as a default. There's not even a build system for these projects so they can't be published already. It seems it just creates hurdle towards taking something you started as a personal project and publishing it later?

---

_Comment by @zanieb on 2024-10-08 19:19_

Let's see if there's more demand for this first. Thanks though.

---

_Closed by @zanieb on 2024-10-08 19:19_

---

_Branch deleted on 2024-10-13 09:13_

---
