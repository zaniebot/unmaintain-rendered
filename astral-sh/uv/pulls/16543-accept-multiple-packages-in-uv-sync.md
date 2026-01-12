```yaml
number: 16543
title: "Accept multiple packages in `uv sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/multi-sync
created_at: 2025-11-01T01:33:49Z
updated_at: 2025-11-04T14:18:00Z
url: https://github.com/astral-sh/uv/pull/16543
synced_at: 2026-01-12T16:12:18Z
```

# Accept multiple packages in `uv sync`

---

_@charliermarsh_

## Summary

Closes https://github.com/astral-sh/uv/issues/12130.


---

_Review requested from @zanieb by @charliermarsh on 2025-11-01 01:34_

---

_Review requested from @konstin by @charliermarsh on 2025-11-01 01:34_

---

_Label `enhancement` added by @charliermarsh on 2025-11-01 01:34_

---

_Marked ready for review by @charliermarsh on 2025-11-01 01:34_

---

_Comment by @zanieb on 2025-11-03 17:39_

Whats your plan for other commands?

---

_@zanieb approved on 2025-11-03 17:39_

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:129 on 2025-11-04 14:00_

```suggestion
                    return Err(anyhow::anyhow!("Package `{name}` not found in workspace"));
```

---

_@konstin approved on 2025-11-04 14:01_

Great addition!

---

_Merged by @charliermarsh on 2025-11-04 14:17_

---

_Closed by @charliermarsh on 2025-11-04 14:17_

---

_Branch deleted on 2025-11-04 14:18_

---
