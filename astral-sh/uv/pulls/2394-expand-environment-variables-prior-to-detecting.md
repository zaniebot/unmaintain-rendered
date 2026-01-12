```yaml
number: 2394
title: Expand environment variables prior to detecting scheme
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/expand
created_at: 2024-03-12T20:57:27Z
updated_at: 2024-03-12T23:17:42Z
url: https://github.com/astral-sh/uv/pull/2394
synced_at: 2026-01-12T16:05:00Z
```

# Expand environment variables prior to detecting scheme

---

_@charliermarsh_

## Summary

This PR ensures that we expand environment variables _before_ sniffing for the URL scheme (e.g., `file://` vs. `https://` vs. something else).

Closes https://github.com/astral-sh/uv/issues/2375.


---

_Label `bug` added by @charliermarsh on 2024-03-12 20:57_

---

_@charliermarsh reviewed on 2024-03-12 20:57_

---

_Review comment by @charliermarsh on `crates/pep508-rs/src/verbatim_url.rs`:203 on 2024-03-12 20:57_

I believe this isn't necessary. If the root is in a `file://`, we already strip that as a special case and parse this as a path, so spaces are fine.

---

_@zanieb approved on 2024-03-12 21:00_

---

_Marked ready for review by @charliermarsh on 2024-03-12 23:17_

---

_Merged by @charliermarsh on 2024-03-12 23:17_

---

_Closed by @charliermarsh on 2024-03-12 23:17_

---

_Branch deleted on 2024-03-12 23:17_

---
