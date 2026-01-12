```yaml
number: 15773
title: Avoid initiating login flow for invalid API keys
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/x
created_at: 2025-09-10T18:39:32Z
updated_at: 2025-09-10T19:07:29Z
url: https://github.com/astral-sh/uv/pull/15773
synced_at: 2026-01-12T16:11:56Z
```

# Avoid initiating login flow for invalid API keys

---

_@charliermarsh_

## Summary

If the login flow fails, and the user provided an API key, it's unintuitive to initiate the login flow.

---

_Label `bug` added by @charliermarsh on 2025-09-10 18:39_

---

_Review requested from @zanieb by @charliermarsh on 2025-09-10 18:39_

---

_Marked ready for review by @charliermarsh on 2025-09-10 18:39_

---

_@zanieb reviewed on 2025-09-10 18:40_

---

_Review comment by @zanieb on `crates/uv/src/commands/auth/token.rs`:114 on 2025-09-10 18:40_

"authenticate with" maybe?

---

_@zanieb approved on 2025-09-10 18:40_

---

_Merged by @charliermarsh on 2025-09-10 19:07_

---

_Closed by @charliermarsh on 2025-09-10 19:07_

---

_Branch deleted on 2025-09-10 19:07_

---
