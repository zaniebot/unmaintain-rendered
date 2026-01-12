```yaml
number: 4456
title: Add standalone CLI crate
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/cli
created_at: 2024-06-23T23:51:59Z
updated_at: 2024-06-24T10:16:23Z
url: https://github.com/astral-sh/uv/pull/4456
synced_at: 2026-01-12T16:06:14Z
```

# Add standalone CLI crate

---

_@charliermarsh_

## Summary

This PR moves all the CLI code into its own crate, separate from the `uv` crate. The `uv` crate is iterated on frequently, and the CLI code comprises a significant portion of it but rarely changes. Removing the CLI code reduces the `uv` crate size from 1.4MiB to 1.0MiB.


---

_Label `internal` added by @charliermarsh on 2024-06-23 23:52_

---

_@konstin approved on 2024-06-24 06:58_

---

_Merged by @charliermarsh on 2024-06-24 10:16_

---

_Closed by @charliermarsh on 2024-06-24 10:16_

---

_Branch deleted on 2024-06-24 10:16_

---
