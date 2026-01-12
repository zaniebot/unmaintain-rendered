```yaml
number: 14155
title: "Clarification needed: Conflicting behavior between automatic sync and optional-dependencies"
type: issue
state: closed
author: Arist12
labels:
  - question
assignees: []
created_at: 2025-06-20T14:33:50Z
updated_at: 2025-06-21T00:46:07Z
url: https://github.com/astral-sh/uv/issues/14155
synced_at: 2026-01-12T16:01:44Z
```

# Clarification needed: Conflicting behavior between automatic sync and optional-dependencies

---

_@Arist12_

### Question

I'm confused about what seems like contradictory behavior between uv's automatic syncing and optional-dependencies handling. The documentation and observed behavior appear to conflict with each other.

From the documentation ([Locking and syncing](https://docs.astral.sh/uv/concepts/projects/sync/)):

From my understanding, commands like `uv run` will automatically invoke `uv lock` and `uv sync` before executing the command. 

Syncing is **"exact"** by default, which means it will remove any packages that are not present in the lockfile, and because uv does not sync extras by default, optional dependencies will all be removed.

However, my observed behavior:
When using uv add --optional <extra-name> <package> to add dependencies to optional-dependencies, the package gets added to the current environment immediately, even though optional-dependencies are not supposed to be synced by default. Moreover, after executing a python script with `uv run xxx.py`, the optional dependencies are still not removed. It's only removed if I explicitly call `uv sync`. (I have some confusions about this command as well. It's mentioned in the document that this command only syncs the environment, but it actually runs `uv lock` first?)


### Platform

macOS 15.5

### Version

0.7.13

---

_Label `question` added by @Arist12 on 2025-06-20 14:33_

---

_Comment by @zanieb on 2025-06-20 15:25_

`uv run` performs an inexact sync by default, unlike `uv sync`. `uv run` doesn't _quite_ invoke `uv lock` and `uv sync` before running the command, it performs a lock and sync using internal abstractions, not that exact public interface.

---

_Comment by @Arist12 on 2025-06-21 00:46_

Thank you!

---

_Closed by @Arist12 on 2025-06-21 00:46_

---
