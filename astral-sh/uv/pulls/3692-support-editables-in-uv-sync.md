```yaml
number: 3692
title: "Support editables in `uv sync`"
type: pull_request
state: merged
author: konstin
labels:
  - preview
assignees: []
merged: true
base: main
head: konsti/support-editables-in-uv-run
created_at: 2024-05-21T11:24:26Z
updated_at: 2024-05-21T13:38:17Z
url: https://github.com/astral-sh/uv/pull/3692
synced_at: 2026-01-10T14:32:20Z
```

# Support editables in `uv sync`

---

_Pull request opened by @konstin on 2024-05-21 11:24_

This is bare-bones support for editables in `uv sync` as basis for workspace support, notably without lockfile integration. It leverages the existing `ResolvedEditables` infrastructure.

---

_Label `preview` added by @konstin on 2024-05-21 11:24_

---

_Renamed from "Support editables in `uv run`" to "Support editables in `uv sync`" by @konstin on 2024-05-21 11:25_

---

_Merged by @konstin on 2024-05-21 11:30_

---

_Closed by @konstin on 2024-05-21 11:30_

---

_Branch deleted on 2024-05-21 11:30_

---

_@charliermarsh reviewed on 2024-05-21 11:46_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:94 on 2024-05-21 11:46_

Wait, but this PR doesn't actually add anything, does it?

---

_@charliermarsh reviewed on 2024-05-21 11:47_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/project/sync.rs`:94 on 2024-05-21 11:47_

I intentionally removed this in favor of a default impl.

---

_@konstin reviewed on 2024-05-21 13:38_

---

_Review comment by @konstin on `crates/uv/src/commands/project/sync.rs`:94 on 2024-05-21 13:38_

I see, i'll revert it until we have editable from the lockfile then: https://github.com/astral-sh/uv/pull/3696

---
