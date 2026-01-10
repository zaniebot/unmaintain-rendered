```yaml
number: 4487
title: Use shared client in Git fetch implementation
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/git-client
created_at: 2024-06-24T19:34:45Z
updated_at: 2024-06-24T21:09:30Z
url: https://github.com/astral-sh/uv/pull/4487
synced_at: 2026-01-10T13:48:28Z
```

# Use shared client in Git fetch implementation

---

_Pull request opened by @charliermarsh on 2024-06-24 19:34_

## Summary

It turns out that the Git fetch implementation is initializing its own client, which can be really expensive on macOS (due to loading native certificates) _and_ bypasses any of our middleware. This PR modifies the Git implementation to accept a shared client.


---

_Label `performance` added by @charliermarsh on 2024-06-24 19:34_

---

_Review requested from @ibraheemdev by @charliermarsh on 2024-06-24 19:34_

---

_@ibraheemdev approved on 2024-06-24 19:48_

LGTM.

---

_Merged by @charliermarsh on 2024-06-24 21:09_

---

_Closed by @charliermarsh on 2024-06-24 21:09_

---

_Branch deleted on 2024-06-24 21:09_

---
