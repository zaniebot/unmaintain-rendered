```yaml
number: 17000
title: Add a mode for isolated lock without isolated sync
type: issue
state: open
author: zanieb
labels:
  - enhancement
assignees: []
created_at: 2025-12-05T14:54:22Z
updated_at: 2025-12-17T17:23:10Z
url: https://github.com/astral-sh/uv/issues/17000
synced_at: 2026-01-12T16:02:42Z
```

# Add a mode for isolated lock without isolated sync

---

_@zanieb_

### Summary

Right now, we support `uv run --isolated` which creates an isolated virtual environment and uses an in-memory lockfile (i.e., changes to the lockfile are not persisted to disk).

We don't support `uv sync --isolated` because we'd remove the virtual environment on completion.

We should add a mode that uses the project environment, but doesn't update the lockfile. This is useful for cases like https://github.com/dantebben/nox-uv/issues/73 where you want to `uv sync` with a different resolution mode.

### Example

```
uv sync --isolated-lock
```

---

_Label `enhancement` added by @zanieb on 2025-12-05 14:54_

---

_Comment by @konstin on 2025-12-17 17:23_

For me, the main motivating use case is testing `uv run --resolution lowest` without changing files in the git repo. It's also useful for testing excludes-newer cutoffs or inversely `uv run --upgrade` to check if there were any (accidentally) breaking changes recently.

---
