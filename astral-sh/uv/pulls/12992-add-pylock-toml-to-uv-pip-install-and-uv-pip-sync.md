```yaml
number: 12992
title: "Add `pylock.toml` to `uv pip install` and `uv pip sync`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/pep-751-sync
created_at: 2025-04-20T22:51:43Z
updated_at: 2025-04-22T07:42:32Z
url: https://github.com/astral-sh/uv/pull/12992
synced_at: 2026-01-10T11:10:40Z
```

# Add `pylock.toml` to `uv pip install` and `uv pip sync`

---

_Pull request opened by @charliermarsh on 2025-04-20 22:51_

## Summary

We accept `pylock.toml` as a requirements file (e.g., `uv sync pylock.toml` or `uv pip install -r pylock.toml`). When you provide a `pylock.toml` file, we don't allow you to provide other requirements, or constraints, etc. And you can only provide one `pylock.toml` file, not multiple.

We might want to remove this from `uv pip install` for now, since `pip` may end up with a different interface (whereas `uv pip sync` is already specific to uv), and most of the arguments aren't applicable (like `--resolution`, etc.). Regardless, it's behind `--preview` for both commands.


---

_Review requested from @Gankra by @charliermarsh on 2025-04-21 00:17_

---

_Review requested from @zanieb by @charliermarsh on 2025-04-21 00:17_

---

_Label `enhancement` added by @charliermarsh on 2025-04-21 00:17_

---

_Marked ready for review by @charliermarsh on 2025-04-21 00:17_

---

_Comment by @zanieb on 2025-04-21 17:49_

> We might want to remove this from uv pip install for now, since pip may end up with a different interface (whereas uv pip sync is already specific to uv), and most of the arguments aren't applicable (like --resolution, etc.). Regardless, it's behind --preview for both commands.

Keeping it behind preview works for me!

---

_@zanieb approved on 2025-04-21 17:50_

---

_Merged by @charliermarsh on 2025-04-21 22:10_

---

_Closed by @charliermarsh on 2025-04-21 22:10_

---

_Branch deleted on 2025-04-21 22:10_

---

_Review comment by @konstin on `crates/uv-resolver/src/lock/export/pylock_toml.rs`:1126 on 2025-04-22 07:42_

PEP 751 requires UTC, but if we want to support this path, we should use `from_seconds`, there are half-hour timezone offsets.

---

_@konstin reviewed on 2025-04-22 07:42_

---
