```yaml
number: 2849
title: "Remove additional 'because'"
type: pull_request
state: merged
author: charliermarsh
labels:
  - error messages
assignees: []
merged: true
base: main
head: charlie/because
created_at: 2024-04-06T00:29:43Z
updated_at: 2024-04-06T14:58:29Z
url: https://github.com/astral-sh/uv/pull/2849
synced_at: 2026-01-10T14:43:31Z
```

# Remove additional 'because'

---

_Pull request opened by @charliermarsh on 2024-04-06 00:29_

## Summary

Is this, perhaps, not totally necessary? It doesn't show up in any fixtures beyond those that I added recently.

Closes https://github.com/astral-sh/uv/issues/2846.


---

_Review requested from @zanieb by @charliermarsh on 2024-04-06 00:29_

---

_Label `error messages` added by @charliermarsh on 2024-04-06 00:29_

---

_Marked ready for review by @charliermarsh on 2024-04-06 00:29_

---

_Comment by @zanieb on 2024-04-06 14:58_

It's hard to find cases where the dependencies are unusable — there would have to be something conflicting in them.

This could be confusing in the future but we'll just need to more verbose in the reason we attach when necessary.

---

_@zanieb approved on 2024-04-06 14:58_

---

_Merged by @charliermarsh on 2024-04-06 14:58_

---

_Closed by @charliermarsh on 2024-04-06 14:58_

---

_Branch deleted on 2024-04-06 14:58_

---

_Comment by @charliermarsh on 2024-04-06 14:58_

Makes sense, thanks.

---
